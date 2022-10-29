#include <LiquidCrystal_I2C.h>
#include <SPI.h>
#include <MFRC522.h>

Secondly, the RST and SDA pins are defined. Also, two variables have been created to help the program.

#define RST_PIN 9
#define SS_PIN 10
byte readCard[4];
byte a = 0;

Thirdly, objects are created for I2C and RFID libraries.

LiquidCrystal_I2C lcd(0x27, 16, 2);
MFRC522 mfrc522(SS_PIN, RST_PIN);

In the setup function, the Serial monitor, LCD, SPI bus and RFID module are started. Also, “Put your card” is printed on the LCD.

void setup() {
Serial.begin(9600);
lcd.init();
lcd.backlight();
while (!Serial);
SPI.begin();
mfrc522.PCD_Init();
delay(4);
mfrc522.PCD_DumpVersionToSerial();
lcd.setCursor(2, 0);
lcd.print(“Put your card”);
}
