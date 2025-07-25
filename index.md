
<center>
<H1>Self-Driving Car</H1>
</center>
 <h5>
  An Arduino UNO self-driving car is a simple, small robot that uses an Arduino UNO board as its control center to navigate autonomously. It relies on sensors like ultrasonic distance sensors, infrared sensors, or sometimes cameras to detect objects around it and avoid collisions. These sensors continuously feed data to the Arduino UNO, which processes the information and determines the car’s movements.

The Arduino UNO microcontroller interprets the sensor data and sends commands to the motors to control the car's speed and direction. For example, if the car detects an obstacle in front of it, the Arduino can trigger the car to stop, reverse, or turn to avoid a collision. This allows the car to drive on its own without human input, making it ideal for learning about robotics, automation, and basic principles of autonomous navigation.

In essence, the Arduino UNO self-driving car combines sensors, programming, and basic mechanical systems to allow for autonomous movement, offering a fun and educational way to understand the basics of robotics and self-driving technology. </h5>



| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Jose J | KIPP NYC CPHS | Technical & Civil Engineering | Incoming Sophomore |

![Final project](https://github.com/user-attachments/assets/bda05022-1c0b-443d-8bed-40a2b52d993d)
  
# Final Milestone


<iframe width="399" height="225" src="https://www.youtube.com/embed/o88Zk7bv2zw" title="Tony J. Milestone 3" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<p>
  I was able to add mecanum wheels to my self driving car because the are omnidirectional (being able to move or operate in all directions, without needing to change the orientation of the object itself.) I wasn't able to make the bluetooth module to work so had to move on with the mencanum wheel and currenlty the IR sensor are not working. I learned how a breadboard and ultra sonic worked but I wasn't able to make I hope to learn how to expand my knowledge on schematics .

</p>


# Second Milestone

<iframe width="791" height="445" src="https://www.youtube.com/embed/uZ8DTV_WV1Y" title="Tony J. Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

 I managed to successfully add a joystick to the project and got the ultrasonic sensor working as well, which was a nice achievement. What really surprised me, though, is how many modifications I can still add to the project—there’s a lot of potential for customization.

Next, I plan to integrate a speaker for sound effects and experiment with using a wireless controller for more flexibility. I did run into some issues getting the remote control to work, even after debugging, but I’m not giving up on it and will tackle that part again later. It’s exciting to see the progress and all the possibilities for improvement!


# First Milestone

<iframe width="791" height="445" src="https://www.youtube.com/embed/_Zn2GJUbHg0" title="Tony J. Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I successfully added the Line Following Module and the Obstacle Avoidance Module to my project, and I was able to make the car move both with and without code. The integration of these components went relatively smoothly, and it was satisfying to see the car react to different environments and follow lines or avoid obstacles as intended.

However, I did encounter some challenges, particularly with the ultrasonic module, which needed to be replaced due to some issues. Despite this setback, I’m optimistic about finishing the coding portion and plan to incorporate further modifications to enhance the performance and functionality of the car.


# Schematics 
![Mecanum Wheel Robot Setup](https://i.imgur.com/B4IwBZu.png)
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
| Arduino UNO | Store and Uploads Code | $27.60 | <a href=" "> Link </a> |
| Ultrasonic Sensors | checks distance via sound |  $9.99  | <a href="https://www.google.com/url?q=https://www.amazon.com/dp/B0C166NX3Z?psc%3D1%26smid%3DA1YZW40LYQY3L1%26ref_%3Dchk_typ_imgToDp&sa=D&source=editors&ust=1753470315068377&usg=AOvVaw13Oz2mK3QNG1NenPwKERam"> Link </a> |
| Mecanum wheel kit | For omnidirectional wheels | $27.59 | <a href=" "> Link </a> |
|  |  |  | <a href=" "> Link </a> |
|  |  |  | <a href=" "> Link </a> |
|  |  |  | <a href=" "> Link </a> |
|  |  |  | <a href=" "> Link </a> |


