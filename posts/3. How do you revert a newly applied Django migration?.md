# How do you revert a newly applied Django migration?

So you created a new migration and applied it to your database however after some testing you realize it still needs some work and you need to revert it. With Django this is pretty simple with some built in commands 

## The Traditional Way
For this example we will pretend we have a `polls` app and this is the app we wish to manage our migrations in. Django has a lot of great [documentation](https://docs.djangoproject.com/en/4.1/topics/migrations/) on this topic as well.

Let's first check out which what our current migrations look like using the `showmigrations` command. 
```
python manage.py showmigrations polls
polls
 [X] 0001_initial
 [X] 0002_question1
 [X] 0003_person
```
The previous migration that was applied is `0002_question1` so we simply need to tell Django to migrate back to that. This can be done by the `migrate` command

```
python manage.py migrate polls 0002_question1 
Operations to perform:
  Target specific migration: 0002_question1, from polls
Running migrations:
  Rendering model states... DONE
  Unapplying polls.0003_person... OK
```
Now our database should now have reverted back to the state in migration `0002_question1` and we can verify this by using `showmigrations` again.

```
python manage.py showmigrations polls
polls
 [X] 0001_initial
 [X] 0002_question1
 [ ] 0003_person
```
If you want to reset the whole database as well you can use `python manage.py migrate polls zero` which will revert all the way back to `0001_initial`. 

## But how do I just go back to a previous migration without checking what it is?
And this question is something I asked myself as well. The Django framework does not provide this functionality out of the box. 

Let's say you commit migrations to you git repository and are a small team sharing a few servers for other teams to test with. A problem you may run into is different teams having different migrations which are not present in the git main branch. This can lead to some frustrating issues during a deployment and can cause a database to get out of sync with the migration files very quickly (especially if the database is not rebuilt every time). 

This is why I made the [django-migration-rollback](https://pypi.org/project/django-migration-rollback/) app. With some custom Django commands one can now easily migrate back to a previous migration and even sync with a specific branch in a repository quite easily without having to manually check what the migrations are. 

In our previous example, instead of having to do `showmigrations` and manually input the migration in `migrate` you can simply use `migrateprevious`.
```
python manage.py migrateprevious polls
Attempting to go back to rollback polls to previous migration
Operations to perform:
    Target specific migration: 0002_question1, from polls
Running migrations:
    Rendering model states...
DONE
    Unapplying polls.0003_person...
OK
```

And if I had deployed my changes to a test server and had forgotten to rollback the migration the deployment sequence can simply use `migraterollback` before deploying the next branch. 

```
python manage.py migraterollback polls main
Attempting to go back to rollback polls to latest migration on branch main
Operations to perform:
    Target specific migration: 0002_question1, from polls
Running migrations:
    Rendering model states...
DONE
    Unapplying polls.0003_person...
OK
```
However just note this does require a .git file present to be able to call the git commands. 

## Conclusion 
I hope you found this article useful and if you wish to check out the package I talked about it is open source and the repository can be found [here](https://github.com/jdboisvert/django-migration-rollback). Happy coding!

