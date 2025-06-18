# -IOT-SECURITY-SYSTEM
**COMPANY**: CODTECH IT SOLUTIONS
**NAME**: NELAVAI USHA
**INTERN ID**: CT06DM957
**DOMAIN**: INTERNET OF THINGS
**DURATION**: 6 WEEKS
**MENTOR**: NEELA SANTHOSH
Here's a complete explanation and build guide for a security system that detects motion, captures images, and sends alerts to a mobile app using Arduino, PIR sensor, ESP32-CAM, and Blynk IoT platform.
An IoT security system utilizes internet-connected devices to enhance safety and security. These systems often incorporate sensors, cameras, and alarms, with notifications sent to users via mobile apps or other connected devices. This allows for remote monitoring and control of security measures, such as door locks and surveillance cameras.
Here's a more detailed look at how these systems work:
Core Components:
Sensors:
Motion detectors (PIR sensors), door/window sensors, smoke detectors, and cameras are commonly used to detect potential threats or hazards.
Microcontrollers:
Devices like Arduino or NodeMCU act as the brain of the system, processing data from sensors and controlling other components.
Connectivity:
WiFi or other wireless modules (like ESP8266) enable communication between the system's components and the user's mobile device or a cloud platform.
Actuators:
Alarms, sirens, and smart locks can be activated based on sensor data and user commands.
User Interface:
Mobile apps or web interfaces provide a way for users to monitor the system, receive alerts, and control connected devices.
How it works:
1. Sensing:
Sensors detect events like motion, door opening, or smoke.
2. Processing:
The microcontroller processes the sensor data, potentially triggering an alarm or sending a notification.
3. Communication:
The system transmits information about the event to the user through the connected network (e.g., via a mobile app).
4. User Action:
The user can then take appropriate action, such as viewing live camera footage, remotely locking a door, or alerting authorities.
Benefits of IoT Security Systems:
Remote Monitoring and Control: Users can monitor their property from anywhere with an internet connection.
Real-time Alerts: Immediate notifications of potential security breaches or hazards.
Increased Awareness: Provides a more comprehensive view of the property's security status.
Cost-effective: Can be more affordable than traditional security systems, especially when utilizing readily available components.
Customization: Systems can be tailored to specific needs and preferences, incorporating various sensors and features. 
✅ Objective
Build a security system that:
1. Detects motion using a PIR sensor.
2. Captures images using ESP32-CAM.
3. Sends an alert to a mobile app (Blynk) with the captured image.
Components Required
Component Quantity
ESP32-CAM 1
PIR Motion Sensor (HC-SR501) 1
Jumper Wires As needed
Breadboard 1
5V Power Supply 1
Smartphone with Blynk app 1
🔌 Circuit Diagram
PIR VCC → 5V (ESP32-CAM)
PIR GND → GND (ESP32-CAM)
PIR OUT → GPIO13 (ESP32-CAM)
📲 Mobile App Setup using Blynk (New Blynk Platform)
1. Download Blynk IoT app (iOS or Android).
2. Create a new project.
3. Add:
o Notification widget
o Image gallery or Web URL widget
4. Get Auth Token from the Blynk dashboard
💻 Arduino Code (ESP32-CAM)
#include "esp_camera.h"
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
char auth[] = "Your_Blynk_Auth_Token";
char ssid[] = "Your_WiFi_Name";
char pass[] = "Your_WiFi_Password";
#define PIR_PIN 13
BlynkTimer timer;
bool motionDetected = false;
// ESP32-CAM camera config
void startCamera() {
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  config.pin_d0 = 5;
  config.pin_d1 = 18;
  config.pin_d2 = 19;
  config.pin_d3 = 21;
  config.pin_d4 = 36;
  config.pin_d5 = 39;
  config.pin_d6 = 34;
  config.pin_d7 = 35;
  config.pin_xclk = 0;
  config.pin_pclk = 22;
  config.pin_vsync = 25;
  config.pin_href = 23;
  config.pin_sscb_sda = 26;
  config.pin_sscb_scl = 27;
  config.pin_pwdn = 32;
  config.pin_reset = -1;
  config.pin_xclk = 0;
  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;
  config.frame_size = FRAMESIZE_VGA;
  config.jpeg_quality = 10;
  config.fb_count = 1;
  // Initialize camera
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("Camera init failed with error 0x%x", err);
    return;
  }
}
void sendCapturedImage() {
  camera_fb_t *fb = esp_camera_fb_get();
  if (!fb) {
    Serial.println("Camera capture failed");
    return;
  }
  // Send notification
  Blynk.logEvent("motion_detected", "Motion detected! Sending image...");
  // Upload image to image hosting (like Imgur or web server)
  // For this demo, assume image sent externally and we send a placeholder URL
  Blynk.virtualWrite(V1, "https://your-server.com/captured_image.jpg");
  esp_camera_fb_return(fb);
}
void detectMotion() {
  if (digitalRead(PIR_PIN) == HIGH) {
    if (!motionDetected) {
      motionDetected = true;
      sendCapturedImage();
    }
  } else {
    motionDetected = false;
  }
}
void setup() {
  Serial.begin(115200);
  pinMode(PIR_PIN, INPUT);
  WiFi.begin(ssid, pass);
  Blynk.begin(auth, ssid, pass);
  startCamera();
  timer.setInterval(1000L, detectMotion);
}
void loop() {
  Blynk.run();
  timer.run();
}
📤 How the System Works
1. PIR sensor detects motion.
2. ESP32-CAM captures an image.
3. Image gets uploaded to a server or hosted link.
4. Mobile user receives:
o Notification alert
o Link or preview of the image
📸 Enhancement Ideas
• Use Google Firebase or Imgur API to upload and store images.
• • Add face recognition using OpenCV (advanced).
• • Integrate with Telegram bot or WhatsApp alerts
