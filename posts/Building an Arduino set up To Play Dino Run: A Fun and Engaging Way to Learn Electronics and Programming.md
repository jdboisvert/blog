# Building an Arduino set up To Play Dino Run: A Fun and Engaging Way to Learn Electronics and Programming

The Dino Run game is one of Google's the most popular games around, and it's no surprise why: it's simple, engaging, and addictive. But you know what is even more fun? Getting an Arduino to do it for you. In this blog post, we'll explore how to do just that using the open-source code available on [GitHub](https://github.com/jdboisvert/arduino-t-rex-run).

## What is an Arduino?

Arduino is an open-source platform for building electronic devices and projects. It consists of a physical programmable board and an Integrated Development Environment (IDE) that allows you to write code and upload it to the board. Arduino boards are commonly used in a wide range of projects, including robotics, home automation, and Internet of Things (IoT) applications.

## What is Dino Run?

Dino Run is a popular game that is built into the Google Chrome browser. It's a simple game where the player controls a T-Rex who must jump over obstacles to avoid crashing. The game is played by pressing the space bar or up arrow to jump and the down arrow to duck. You can play the game by typing chrome://dino/ into your Chromium browser (ex: Chrome, Brave, etc.)

## How to make an Arduino Play Dino Run Game

If you want to keep it simple feel free to pull down the code I wrote [here](https://github.com/jdboisvert/arduino-t-rex-run) which also shows how to get set up. 

You will need the following: 

- An Arduino UNO (or really any Arduino should be fine)
- Two Servos (one to be tapped onto the keyboard key to jump and one to be tapped onto the keyboard key to duck).
- One Light Sensor (Photoresistor/LDR) which will be used to detect objects to know when to jump or duck
- Eight wires
- One 10k OHM resistor

Here is how you will will set everything up  
![Diagram showing how to wire up the servos, Arduino and sensors](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pe9kd2ztdxhx6gyu9480.png)

Once you have everything hooked up ensure to tape the jump servo to the up arrow key and the down servo to the down arrow key (note the code in my project will always be pressing either key and will always default to ducking aka the down key). 

Tape the light sensor (used to know when to jump) on the computer's screen (about 1/4 from the left hand side this can vary). The position is important since if it detects an object too late our little Dino won't have time to jump and if it detects it too soon it will have jumped into the object. 

Open up your [Arduino IDE](https://www.arduino.cc/en/software) and paste in the following code (feel free to look at the comments left if you want to better understand the code):

```c++
/**+---
 * Used to play Google's Dino run game 
 * with just servos and a LDR sensor. 
 */

// This is just importing the Servo object you will need. 
#include <Servo.h>

// You need to declare you Servo 
// objects here to be able to attach them later on. 
Servo jumpServo; 
Servo duckServo; 

//Pins
const int JUMP_SERVO_PIN = 2; // TODO Change to your pin
const int DUCK_SERVO_PIN = 4; // TODO Change to your pin 
const int JUMP_SENSOR_PIN = A0; // TODO Change to your pin 

//Servo positions
const int JUMP_OFF_POSITION = 10; //Change this based on how your servos are set up 
const int JUMP_ON_POSITION = 20; //Change this based on how your servos are set up 
const int DUCK_OFF_POSITION = 105; //Change this based on how your servos are set up 
const int DUCK_ON_POSITION = 120; //Change this based on how your servos are set up 

//Sensor object values
const int MIN_OBJECT_RANGE = 532; //Change based on your screen
const int MAX_OBJECT_RANGE = 695; //Change based on your screen

int jumpSensorValue = 0; 
const int TIME_BETWEEN_JUMP = 290; //Change based on your placement

// This runs upon turning on the Arduino and is usually where 
// you tell the Arduino where certain electronics are and here 
// we ensure the servos are in the "start position" meaning to 
// always duck. 
void setup() {
  jumpServo.attach(JUMP_SERVO_PIN); 
  duckServo.attach(DUCK_SERVO_PIN); 
  startPosition();
  Serial.begin(9600);
}

// This will run forever as the Arduino is on.
// This is where the actual game is happening and essentially 
// our Light Sensor is going to return a number based on how 
// much light is present (meaning a darker object returns a 
// higher number and a lighter object returns a lower number). 
void loop() {
  jumpSensorValue = analogRead(JUMP_SENSOR_PIN);
  Serial.println(jumpSensorValue); // Just for testing purposes and how you will know what your sensor returns all sensors can be different :) 
  if(jumpSensorValue > MIN_OBJECT_RANGE && jumpSensorValue < MAX_OBJECT_RANGE){
    // It is always good to have a range since as the object will move across the screen the number can change and there will be a delay from when the servo pressing the key and when the Arduino runs the code to do it. 
    jump();   
  }
  else {
    duck();
  }
}

/**
 * Put both keys off the trigger
 */
void startPosition(){
  jumpServo.write(JUMP_OFF_POSITION); 
  duckServo.write(DUCK_OFF_POSITION); 
}

/**
 * Used to press the key to jump
 */
void jump(){
  duckServo.write(DUCK_OFF_POSITION); 
  jumpServo.write(JUMP_ON_POSITION); 
  delay(TIME_BETWEEN_JUMP); 
  jumpServo.write(JUMP_OFF_POSITION); 
}

/**
 * Used to press the key to duck 
 */
void duck(){
  duckServo.write(DUCK_ON_POSITION); 
}
```

You may need to play around with the variables I indicated with some `TODO` but once all is set up give it a try and hopefully you will end up with something like this.


![An animated gif showing the set up of the author's Arduino plying Dino run](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/em3kp1kmf571ns1gpdzp.gif)


## Conclusion

Making an Arduino play Dino Run for you is a fun and engaging way to learn about electronics and programming. It's a great project for beginners who want to learn about Arduino and electronics. By following the instructions on GitHub and using the Dino Run code provided, you can build your own Dino Run set up to play the game in no time. 

My best score was 1393 can you beat it? Let me know in the comments below!