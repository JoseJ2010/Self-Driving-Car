
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
| Sunfounder Kit | Start the project | $59.99 | <a href="https://www.amazon.com/SunFounder-Compatible-Tutorials-Including-Controller/dp/B0B778L1DZ/ref=sr_1_1?crid=3JQTX3SPFIY9Z&dib=eyJ2IjoiMSJ9.D9LrCZJnua_keVMLJz2FWi87-vYq5Z0c0hghVjdTqVV5SxTVgutlUut8NIgJpkDha5RIUUEOd8ZL_9-liu4TuIX3Y5c9E3mrmlKMD_2d9cnuKu55yBqRD35FcNSR2oUIVkT7byKksfuqXVAx34A8gUuPMYKaM3Jepu1QA3uOutR5sR0O3bugifITwp4OocPwYE4ZDNZaCae7Y3Ydd5zuneo_8PLiYwbdyVH9QvcGEwg.-iuZvwFJywFFRggszeNpXLuAEE8nPtLKbqmhVUOfLc0&dib_tag=se&keywords=sunfounder+3+in+1+starter+kit+for+arduino+uno&qid=1718980379&sprefix=3+in+1+ard%2Caps%2C120&sr=8-1"> Link </a> |
| Motors | To move the car | $10.00 | <a href="https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_4?crid=1JP29NIWBLH2M&dib=eyJ2IjoiMSJ9.Wq3jKgOLbqtEP772vMD4pV5f-w3PLBdEpKqguykXOb0JFO14f4Dq0m_VDVUMUFtR8WFINUEticI3GXcoGqwXPqK9yIh04PhCktgccMz9zAUiKXMJPwmOTUp_6av3XuFD0lXo9WngN9iKI6YgZrhEEs9qnqbcB1GnvgntCdKz8Q1dFuNu61NgSE6Z8vBk3FRpaNcr1lCI7FApTiNi0Qce8gbfmMn6oUggZQHpIOKKZ6s.M7WsZ_ZZtm3rm93kKgw0NOxt1McVBYX6m55oGxu1xxI&dib_tag=se&keywords=dc+motor+with+gearbox&qid=1715911706&sprefix=dc+motor+with+gearbox%2Caps%2C126&sr=8-4">link</a> |
| USB-USBC | To upload code to Arduino UNO | $2.99 | <a href="[ https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ/ref=sxin_17_pa_sp_se](https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93%3Aamzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93&crid=2XKXL9JJ62FRH&cv_ct_cx=usba%2Bto%2Busbc&keywords=usba%2Bto%2Busbc&pd_rd_i=B0D3T2QDVJ&pd_rd_r=4d705a77-7d1c-4543-b61d-c95f071f99c3&pd_rd_w=zydwI&pd_rd_wg=Ra4PI&pf_rd_p=70fcaece-2dd2-4653-bf00-fb6af1af1b93&pf_rd_r=GA7XX674ZRQ1VKTH3HWX&qid=1750358432&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=usba%2Bto%2Busb%2Caps%2C106&sr=1-1-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1)">link</a>|
| 9V Batteries | External Power Source | $8.88 | <a href="https://www.amazon.com/Amazon-Basics-Performance-All-Purpose-Batteries/dp/B00MH4QM1S/ref=sr_1_5?crid=3T(">link</a> |
| DMM | Power Check | $9.99 | <a href="https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic">link</a>|
| Ultrasonic Sensor | Need a replacement | $9.99 |<a href="https://www.amazon.com/dp/B0C166NX3Z?psc=1&smid=A1YZW40LYQY3L1&ref_=chk_typ_imgToDp">link</a>|
| Mecanum wheels | Modification | $27.59 |<a href="https://www.amazon.com/DWWTKL-Omnidirectional-Educational-Raspberry-Unassembled/dp/B0CBRBCFMZ/ref=sr_1_7?crid=2SBS88QZ6V821&dib=eyJ2IjoiMSJ9.W_KC8xv258Ah7gyGa6PFzud7PVQLQmIoci-CiujLUV4A9RWPRd-F1xRWC477kqvU8FWmi-erIdoIvir1nYAcKkG9o0kbj0nGgH48wyJHG5cZFz8hQhAybE7gYEVtkBb7JopgCNQY6JY7oww5-26WBFWHDOwr7wciHdmDYATQcVRBGNqNIqiJ96IKlR4fG4ftGrh8jrdau44oxhn4De3xjd0bv4N7bbgaQnM1kD5jVb73-CkJWJlUIRmT5fWpZSwRI4MxCpCvOFXeRq9IYuuaRPL9AzvO0n1796j2ssQvNw0.diWdT7b0hUVzVw-d8ry8iLk8J7XXsBNBvX236Gzy0zE&dib_tag=se&keywords=Mecanum%2Bwheel%2Bkit&qid=1753043524&sprefix=mecanum%2Bwheel%2Bkit%2Caps%2C124&sr=8-7&th=1">link</a>|
| HC-05 Bluetooth | For Wireless Connection | $9.99 |<a href="https://www.amazon.com/dp/B01G9KSAF6?ref=nb_sb_ss_w_as-reorder_k1_1_5&amp=&crid=11UESFVVLK3WH&amp=&sprefix=dsd+t">link</a>|
| Small Breadboard | To reorganize Wire | $9.99 |<a href="https://www.amazon.com/BOJACK-Values-Solderless-Breadboard-Flexible/dp/B08Y59P6D1/ref=sr_1_1_sspa?crid=X7L66DN6SKJ8&dib=eyJ2IjoiMSJ9.PdOczz4qMoffnA2C_rK-kJlFCYAHSDmrD65DBq3unlgOZbG-fwywsmV7CrH31Jh5H4feTM47GG5tutODFBfXin1AY21hi400AZl-AtrT1tqAFw8e3fp2feioi9xKMXH6OYJNO8Eb4fvovE7fUHNbZvMMjUbfeD0un_fMwUt02bdoVw5SRJ5-EkzfYGd1hpRrGRcO5XKl_M0RdVlHi5kjCeHvq2ASvhSYkpjQlf7ItSs.Qnt-ThhUkgGrjxpK9xCazsx9pr1RLastAU13H77ANm8&dib_tag=se&keywords=small+breadboard&qid=1753043693&sprefix=small+breadboard%2Caps%2C122&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1">link</a> |
| Motor Drivers | To connect motors to Arduino UNO | $7.79 | <a href="https://www.amazon.com/dp/B00M0F243E?ref=cm_sw_r_cso_cp_apin_dp_Y6MVD5JD7K0TYMNX5RJ0&ref_=cm_sw_r_cso_cp_apin_dp_Y6MVD5JD7K0TYMNX5RJ0&social_share=cm_sw_r_cso_cp_apin_dp_Y6MVD5JD7K0TYMNX5RJ0&csmig=1">link</a> |
