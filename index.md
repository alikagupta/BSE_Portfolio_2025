# Knee Rehabilitation Device
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Alika G | Woodside Priory | Engineering | Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
<!-- # Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone -->

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/xQuDBH7mC_g?si=Wa5gteVYbsMu8dR0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My project is a knee rehabilitation device. It has two main sensors: an accelerometer and a flex sensor. An accelerometer keeps track of the static acceleration values with an internal mesh-like component, and the flex sensor uses variable resistance to measure the angle of bend. It uses these sensors to alert the user (with a beeper) if their knee is bending inwards or once it passes 90 degrees. 

In my first milestone, I was able to put together the flex sensor, accelerometer, beeper, and ESP-32 Arduino with a breadboard. I was able to get data from the flex sensor and accelerometer, and code the buzzer to respond differently to both instruments crossing certain thresholds. I was then able to connect the data to my phone through Bluetooth so I can see the real-time values even when the device is connected to a battery pack.

The main challenge that I faced in this step was getting the accelerometer to connect and send values to my computer. Originally, the accelerometer would just spit out various error codes and junk. In the end, there were a few issues with the initialization of the accelerometer object, which needed to be found in the library since there wasn't any documentation for it.

Next, I hope to place all of this onto the knee compression sleeve and change the thresholds for the flex sensor and accelerometer so the beeping is as precise as possible. I hope to perform linear regression to get the values of the angles from the flex sensor instead of just the regular flex sensor output, which is usually in the 1000s. How I place the accelerometer will also define what the if statement for when the knee is turning inward looks like.


# Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/CEAYpI-ZN_E?si=CcBUxAGMJ-E9xZPf" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Description:**
My starter project was the Retro Arcade Console. It is a roughly hand-sized gaming console with 5 vintage games, brightness and sound settings, and 6 buttons: one for each direction, one for exiting, and one for selecting. I first soldered all the components, then I put the battery pack on part of the casing, and lastly, I assembled the casing. It was pretty straightforward and a fun way for me to revive my soldering skills. I didn't face any large challenges making this. 

**Next Steps:**
Next, I will be moving on to my intensive project: the knee rehabilitation device. This project taught me how to use a multimeter to test for short circuits, which will be very helpful as I move on to my larger, more complex project.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resources to create professional schematic diagrams, though BSE recommends Tinkercad because it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

First Milestone:
```
//adding needed libraries
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_LSM6DS33.h>
#include <BleSerial.h>

// creating the Accelerometer and Bluetooth object
Adafruit_LSM6DS33 lsm6ds33 {};
BleSerial ble;


// delaring and initializing variables
const int FlexPin = 33;
const int BuzzerPin = 23;
int FlexValue = 0;


void setup() {
  Serial.begin(115200);
  ble.begin("Alika'sKneeRehabValues");
  pinMode(FlexPin, INPUT);
  pinMode(BuzzerPin, OUTPUT);

  //Accelerometer set up
  lsm6ds33.begin_I2C ();
  Serial.print("Accelerometer range set to: ");
  switch (lsm6ds33.getAccelRange()) {
  case LSM6DS_ACCEL_RANGE_2_G:
    Serial.println("+-2G");
    break;
  case LSM6DS_ACCEL_RANGE_4_G:
    Serial.println("+-4G");
    break;
  case LSM6DS_ACCEL_RANGE_8_G:
    Serial.println("+-8G");
    break;
  case LSM6DS_ACCEL_RANGE_16_G:
    Serial.println("+-16G");
    break;
  }

  Serial.print("Accelerometer data rate set to: ");
  lsm6ds33.setAccelDataRate(LSM6DS_RATE_52_HZ);
  switch (lsm6ds33.getAccelDataRate()) {
  case LSM6DS_RATE_SHUTDOWN:
    Serial.println("0 Hz");
    break;
  case LSM6DS_RATE_12_5_HZ:
    Serial.println("12.5 Hz");
    break;
  case LSM6DS_RATE_26_HZ:
    Serial.println("26 Hz");
    break;
  case LSM6DS_RATE_52_HZ:
    Serial.println("52 Hz");
    break;
  case LSM6DS_RATE_104_HZ:
    Serial.println("104 Hz");
    break;
  case LSM6DS_RATE_208_HZ:
    Serial.println("208 Hz");
    break;
  case LSM6DS_RATE_416_HZ:
    Serial.println("416 Hz");
    break;
  case LSM6DS_RATE_833_HZ:
    Serial.println("833 Hz");
    break;
  case LSM6DS_RATE_1_66K_HZ:
    Serial.println("1.66 KHz");
    break;
  case LSM6DS_RATE_3_33K_HZ:
    Serial.println("3.33 KHz");
    break;
  case LSM6DS_RATE_6_66K_HZ:
    Serial.println("6.66 KHz");
    break;
  }

}

void loop() {   

  //get and print flex sensor data
  FlexValue = analogRead(FlexPin);
  Serial.print("Flex Sensor Value: ");
  Serial.println(FlexValue);
  ble.println("Flex Sensor Value: ");
  ble.println(FlexValue);


  //get and print accelerometer data
  sensors_event_t accel;
  sensors_event_t gyro;
  sensors_event_t temp;
  lsm6ds33.getEvent(&accel, &gyro, &temp);

  Serial.print("\t\tAccel X: ");
  Serial.print(accel.acceleration.x);
  Serial.print(" \tY: ");
  Serial.print(accel.acceleration.y);
  Serial.print(" \tZ: ");
  Serial.print(accel.acceleration.z);
  Serial.println(" m/s^2 ");

  ble.print("\t\tAccel X: ");
  ble.print(accel.acceleration.x);
  ble.print(" \tY: ");
  ble.print(accel.acceleration.y);
  ble.print(" \tZ: ");
  ble.print(accel.acceleration.z);
  ble.println(" m/s^2 ");


  //Check and Buzz if needed 
  if(accel.acceleration.z < 0){
    tone(BuzzerPin, 1000);  
  }  else if (FlexValue > 3200) {
    tone(BuzzerPin, 200);  
    delay(200);
    tone(BuzzerPin, 0); 
  }else{
    tone(BuzzerPin, 0);  
  }
  
  delay(200);

}

```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
