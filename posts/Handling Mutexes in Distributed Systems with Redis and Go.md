# Handling Mutexes in Distributed Systems with Redis and Go

A mutual exclusion lock (or Mutex which is an abbreviation of **Mut**ual **Ex**clusion) is a very important and sometimes overlooked concept when building a thread-safe access to a system's resources. On top of it being overlooked it can be quite challenging to handle and can lead to race conditions in your system if not done correctly. 

In this post I will go over how you can use [Redis](https://redis.io/) (a popular and widely used key-value store) to help ensure your Go application has mutex on important operations and resources using [Redis's implementation of distributed locks](https://redis.io/docs/manual/patterns/distributed-locks/). 

## How does Redis support distributed locks?

Redis supports distributed locks through the use of its [`SET`](https://redis.io/commands/set/) command. When used with certain options, the `SET` command can be used to implement a locking algorithm (ideally you make the key something unique so no only 1 thread/request can have the lock at once like a user id for example). 

The idea is the `SET` command used will only succeed if the given key does not already exist in Redis, effectively creating a lock. Once the lock is acquired, other processes will be unable to acquire the lock since they are unable to set that same key being used as described in the [distributed locks](https://redis.io/docs/manual/patterns/distributed-locks/) page. 

To release the lock, the process that acquired the lock can use the [DEL](https://redis.io/commands/del/) command to delete the Redis key. Once the key is deleted, the lock is released and other processes can acquire the lock if they need to by setting the key again.

## Implementing distributed locks in Go using Redis

In this example we will use the Redis client library [github.com/go-redis/redis](https://github.com/redis/go-redis) which is a common library many developers use to interact with a Redis instance in Go. 

Make a new [Go project](https://go.dev/doc/code) or pull down the code [I wrote](https://github.com/jdboisvert/redis-distributed-locks). 

Ensure to have a [local Redis instance running](https://redis.io/docs/getting-started/). 

```go
package main

import (
	"fmt"
	"time"

	"github.com/go-redis/redis"
)

func acquireLock(client *redis.Client, key string, expiration time.Duration) (bool, error) {
	// Use the SET command to try to acquire the lock
	result, err := client.SetNX(key, "lock", expiration).Result()
	if err != nil {
		return false, err
	}
	return result, nil
}

func releaseLock(client *redis.Client, key string) error {
	// Use the DEL command to release the lock
	_, err := client.Del(key).Result()
	return err
}

func main() {
	// Create a new Redis client
	client := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "", // no password set
		DB:       0,  // use default DB
	})

	mutexKey := "my_lock"

	// Try to acquire the first lock
	isFirstLockSet, err := acquireLock(client, mutexKey, time.Second*10)
	if err != nil {
		fmt.Println("Error acquiring lock:", err)
		return
	}
	if !isFirstLockSet {
		fmt.Println("Failed to acquire lock")
		return
	}

	// Do some work while holding the lock
	fmt.Println("First lock acquired!")

	isSecondLockSet, _ := acquireLock(client, mutexKey, time.Second*10)

	if !isSecondLockSet {
		fmt.Println("Could not get a second lock which is as expected this is where you would force the request out.")
	} else {
		fmt.Println("Second Lock acquired! This should not happen :)")
	}

	// Simulate some work by sleeping and try to acquire the lock again to see that it fails
	time.Sleep(time.Second * 5)

	isThirdLockSet, _ := acquireLock(client, mutexKey, time.Second*10)
	if !isThirdLockSet {
		fmt.Println("Still could not get the third lock since the first lock is still set.")
	} else {
		fmt.Println("Third Lock acquired! This should not happen :)")
	}

	// Release the lock
	err = releaseLock(client, mutexKey)
	if err != nil {
		fmt.Println("Error releasing lock:", err)
		return
	}
	fmt.Println("First lock released!")

	// Try to acquire the lock again to show it has been released
	isForthLockSet, _ := acquireLock(client, mutexKey, time.Second*10)
	if !isForthLockSet {
		fmt.Println("Failed to acquire lock")
		return
	} else {
		fmt.Println("Forth Lock acquired!")
	}

	// Release the forth lock
	err = releaseLock(client, mutexKey)
	if err != nil {
		fmt.Println("Error releasing lock:", err)
		return
	}
	fmt.Println("Forth Lock released!")
}

```

This when ran will print out the following in the terminal 

```bash 
First lock acquired!
Could not get a second lock which is as expected this is where you would force the request out.
Still could not get the third lock since the first lock is still set.
First lock released!
Forth Lock acquired!
Forth Lock released!
```

## Taking a look at what the code above does
The `acquireLock` function attempts to acquire a lock on a Redis key using the `SET` command with the `NX` option. The NX option tells Redis to only set the key if it does not already exist, effectively creating a lock if our key `my_lock` does not already exist. We set the value to `lock` but this can really be anything. 

This becomes more obvious as we try to acquire a second and third lock which shown above fails since the `acquireLock` returns `false`. 

The `releaseLock` function uses the `DEL` command to release the lock once done. The `DEL` command deletes the `my_lock` key that was created to acquire the lock, effectively releasing the lock.

An important note something that this example highlights is Redis will not raise an exception when doing this so it is up to the application layer to ensure the logic of the locks are applied. Also, it is good to put a default "time to live" on the key to ensure if the application crashes it won't leave the key set forever (in this example we put the `expiration` time as 10 seconds). 

## Conclusion

Mutex is an important concept for coordinating access to shared resources in distributed systems and Redis provides a simple, fast and effective way to implement distributed locks. 

We have explored how to use Redis to implement distributed locks in Go which can be a great combination if you need a small and quick way to restrict access to certain resources/business flows. 