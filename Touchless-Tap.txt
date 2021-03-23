#include <HCSR04.h>
#include <HCSR04.h>
#include <Servo.h>
#define echoPin 35
#define trigPin 33
#define Grove_Water_Sensor 7 // Attach Water sensor to Arduino Digital Pin 7
#define LED 31// Attach an LED to Digital Pin 9 (or use onboard LED)
#include <LiquidCrystal.h> 

// initialize the library with the numbers of the interface pins

LiquidCrystal lcd(12,11,10,9,8,7,6,5,4,3,2);  
int pumpPin = 37; // water pump
int IRSensor = 28; // connect ir sensor to arduino pin 2
int distance;
long duration;

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position


void setup() {
  pinMode(trigPin,OUTPUT);
  pinMode (echoPin, INPUT);
  Serial.begin(9600);
  myservo.attach(24);  // attaches the servo on pin 24 to the servo object
   pinMode(LED, OUTPUT); // The LED is an Output
   pinMode(pumpPin, OUTPUT); //The pump is an Output
   pinMode (IRSensor, INPUT); // sensor pin INPUT
    lcd.begin(16,2);      // set up the LCD's number of columns and rows:

}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration= pulseIn(echoPin, HIGH);
  distance= duration * 0.034 / 2;

  Serial.print( "Distance: ");
  Serial.print( distance);
  Serial.println ("cm");
 lcd.clear();
  lcd.setCursor(0,0);   // set the cursor to column 0, line 0
  lcd.print("Welcome, Please "); //// Print a message to the LCD.
  lcd.setCursor(0,2);   // set the cursor to column 0, line 2
  lcd.print("wash Your hands");  //Print a message to the LCD.
  /* The water sensor will switch LOW when water is detected.
   Get the Arduino to illuminate the LED and activate the buzzer
   when water is detected, and switch both off when no water is present */
   if(distance <= 5)
   {
      digitalWrite(LED,HIGH);
      digitalWrite(pumpPin,HIGH);
      
  //delay(15);
  lcd.clear();
  lcd.setCursor(0,0);   // set the cursor to column 0, line 0
  lcd.print("Washing hands  "); //// Print a message to the LCD.
  lcd.setCursor(0,2);   // set the cursor to column 0, line 2
  lcd.print("please wait.....");  //Print a message to the LCD.
   delay (2000);
      
   }else {
      digitalWrite(LED,LOW);
      digitalWrite(pumpPin,LOW);
      delay (1000);
  
   }

int statusSensor = digitalRead (IRSensor);
  
 // if (statusSensor == 1)
  if (distance >5 && distance <= 25)
   { myservo.write(180);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
 lcd.clear() ; 
  lcd.setCursor(0,0);   // set the cursor to column 0, line 0
  lcd.print("please proceed"); //// Print a message to the LCD.
  lcd.setCursor(0,2);   // set the cursor to column 0, line 2
  lcd.print("Now proceed...");  //Print a message to the LCD.
  delay(2000);
   }
    
//  for (pos = 180; pos >= 0; pos -= 1) { // goes from 180 degrees to 0 degrees
 
 else{
 myservo.write(0);              // tell servo to go to position in variable 'pos'
 delay(15);                       // waits 15ms for the servo to reach the position
 }   
 //   myservo.write(0);
 
 // lcd.clear();
 // lcd.setCursor(0,0);   // set the cursor to column 0, line 0
 // lcd.print("Welcome, Please "); //// Print a message to the LCD.
  lcd.setCursor(0,2);   // set the cursor to column 0, line 2
  lcd.print("wash Your hands");  //Print a message to the LCD.
}


    
