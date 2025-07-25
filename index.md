<H1>Self-Driving Car</H1>

<H3>An Arduino UNO self-driving car is a simple, small robot that uses an Arduino UNO board as its control center to navigate autonomously. It relies on sensors like ultrasonic distance sensors, infrared sensors, or sometimes cameras to detect objects around it and avoid collisions. These sensors continuously feed data to the Arduino UNO, which processes the information and determines the car’s movements.

The Arduino UNO microcontroller interprets the sensor data and sends commands to the motors to control the car's speed and direction. For example, if the car detects an obstacle in front of it, the Arduino can trigger the car to stop, reverse, or turn to avoid a collision. This allows the car to drive on its own without human input, making it ideal for learning about robotics, automation, and basic principles of autonomous navigation.

In essence, the Arduino UNO self-driving car combines sensors, programming, and basic mechanical systems to allow for autonomous movement, offering a fun and educational way to understand the basics of robotics and self-driving technology.</H3>



| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Jose J | KIPP NYC CPHS | Technical & Civil Engineering | Incoming Sophomore |

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE
<p>
+ I was able to add mecanum wheels to my self driving car because the are omnidirectional(being able to move or operate in all directions, without needing to change the orientation of the object itself. 

) </p>


# Second Milestone

<iframe width="791" height="445" src="https://www.youtube.com/embed/uZ8DTV_WV1Y" title="Tony J. Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

 I managed to successfully add a joystick to the project and got the ultrasonic sensor working as well, which was a nice achievement. What really surprised me, though, is how many modifications I can still add to the project—there’s a lot of potential for customization.

Next, I plan to integrate a speaker for sound effects and experiment with using a wireless controller for more flexibility. I did run into some issues getting the remote control to work, even after debugging, but I’m not giving up on it and will tackle that part again later. It’s exciting to see the progress and all the possibilities for improvement!


# First Milestone

<iframe width="791" height="445" src="https://www.youtube.com/embed/_Zn2GJUbHg0" title="Tony J. Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I successfully added the Line Following Module and the Obstacle Avoidance Module to my project, and I was able to make the car move both with and without code. The integration of these components went relatively smoothly, and it was satisfying to see the car react to different environments and follow lines or avoid obstacles as intended.

However, I did encounter some challenges, particularly with the ultrasonic module, which needed to be replaced due to some issues. Despite this setback, I’m optimistic about finishing the coding portion and plan to incorporate further modifications to enhance the performance and functionality of the car.


# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code

<div style="
  height: 200px;
  overflow-y: auto; /* For vertical scrolling if content exceeds height */
  border: 1px solid #ccc;
  padding: 10px;
  background-color: #f6f8fa; /* Light background for code/preformatted look */
  font-family: monospace; /* Optional: Use a monospaced font like code */
  white-space: pre-wrap; /* THIS IS THE KEY PROPERTY */
">

const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 9;
const int B_1A = 10;

const int echoPin = 4;
const int trigPin = 3;

const int rightIR = 7;
const int leftIR = 8;

float readSensorData() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  float distance = pulseIn(echoPin, HIGH) / 58.00; //Equivalent to (340m/s*1us)/2
  return distance;
}


void moveForward(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void moveBackward(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}


void backLeft(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

void backRight(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void stopMove() {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

void setup() {
  Serial.begin(9600);

  //motor
  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  //ultrasonic
  pinMode(echoPin, INPUT);
  pinMode(trigPin, OUTPUT);

  //IR obstacle
  pinMode(leftIR, INPUT);
  pinMode(rightIR, INPUT);
}

void loop() {

  int left = digitalRead(leftIR);  // 0: Obstructed   1: Empty
  int right = digitalRead(rightIR);

  if (!left && right) {
    backLeft(150);
  } else if (left && !right) {
    backRight(150);
  } else if (!left && !right) {
    moveBackward(150);
  } else {
    float distance = readSensorData();
    Serial.println(distance);
    if (distance > 50) { // Safe
      moveForward(200);
    } else if (distance < 10 && distance > 2) { // Attention
      moveBackward(200);
      delay(1000);
      backLeft(150);
      delay(500);
    } else {
      moveForward(150);
    }
  }
}

</div>

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| SunFounder 3 in 1 Starter Kit for Arduino Uno R3 | Start the project | $69.99 | <a href="https://www.sunfounder.com/products/sunfounder-3-in-1-iot-smart-car-learning-ultimate-starter-kit"> Link </a> |
| Arduino UNO | Store and Uploads Code | $27.60 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
