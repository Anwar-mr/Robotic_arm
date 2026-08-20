#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

Adafruit_PWMServoDriver pwm = Adafruit_PWMServoDriver();


#define BASE_SERVO      0
#define SHOULDER_SERVO  1
#define ELBOW_SERVO     2
#define WRIST_SERVO     3
#define GRIPPER_SERVO   4

#define SERVOMIN 150
#define SERVOMAX 600

const unsigned long SPEED_MS_PER_DEG = 20UL;

int currentAngle[5] = {90, 65, 80, 150, 0};  


void setup() {
  Serial.begin(9600);
  pwm.begin();
  pwm.setPWMFreq(50);
  delay(1000); 
  pwm.sleep();
  delay(1000);
  pwm.wakeup();
  // for (int i = 0; i < 5; i++) {
  //   moveServoSmooth(i, currentAngle[i], 30);
  //   delay(100);
  // }
  moveToHome();
  delay(1000);
}

void setServoAngle(uint8_t servoNum, int angle) {
  if (servoNum < 0 || servoNum > 4) return;
  angle = constrain(angle, 0, 180);
  int pulse = map(angle, 0, 180, SERVOMIN, SERVOMAX);
  pwm.setPWM(servoNum, 0, pulse);
  currentAngle[servoNum] = angle; 
}

void moveServoSmooth(uint8_t servoNum, int endAngle, unsigned long msPerDeg) {
  int startAngle = currentAngle[servoNum];
  if (startAngle == endAngle) {
    Serial.print("Servo ");
    Serial.print(servoNum);
    Serial.println(" - No movement needed.");
    return;
  }

  int step = (endAngle > startAngle) ? 1 : -1;
  int pos = startAngle;
  unsigned long lastStepTime = millis();

  while (pos != endAngle) {
    unsigned long now = millis();
    if ((now - lastStepTime) >= msPerDeg) {
      pos += step;
      setServoAngle(servoNum, pos);
      lastStepTime = now;
    }
    // delay(1);
  }

}

void moveToHome() {
  moveServoSmooth(BASE_SERVO, 90, 5);
  delay(500);
  moveServoSmooth(SHOULDER_SERVO, 65, SPEED_MS_PER_DEG);
  delay(500);
  moveServoSmooth(ELBOW_SERVO, 80, SPEED_MS_PER_DEG);
  delay(500);
  moveServoSmooth(WRIST_SERVO, 150, SPEED_MS_PER_DEG);
  delay(500);
  moveServoSmooth(GRIPPER_SERVO, 0, SPEED_MS_PER_DEG);
  delay(500);

}

void pickAndPlace() {
  moveServoSmooth(BASE_SERVO, 90, 5);
  // delay(300);

  moveServoSmooth(GRIPPER_SERVO, 0, 10);
  // delay(300);

  moveServoSmooth(SHOULDER_SERVO, 65, SPEED_MS_PER_DEG);
  // delay(300);

  moveServoSmooth(ELBOW_SERVO, 80, SPEED_MS_PER_DEG);
  // delay(300);

  // moveServoSmooth(WRIST_SERVO, 150, SPEED_MS_PER_DEG);
  // delay(300);

  moveServoSmooth(GRIPPER_SERVO, 70, 10);  
  // delay(300);

  moveServoSmooth(SHOULDER_SERVO, 50, SPEED_MS_PER_DEG);
  // delay(300);

  moveServoSmooth(BASE_SERVO, 140, 5);
  // delay(300);

  moveServoSmooth(SHOULDER_SERVO, 60, SPEED_MS_PER_DEG);
  // delay(300);

  moveServoSmooth(GRIPPER_SERVO, 0, 10);   
  // delay(300);

  moveServoSmooth(SHOULDER_SERVO, 50, SPEED_MS_PER_DEG);
  // delay(300);

  moveToHome();
  delay(500);
}

void loop() {

  pickAndPlace();
  delay(500);
}
