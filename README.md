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
void loop() {
if ( ! mfrc522.PICC_IsNewCardPresent()) {
return 0;
}
if ( ! mfrc522.PICC_ReadCardSerial()) {
return 0;
}

lcd.clear();
lcd.setCursor(0, 0);
lcd.print(“Scanned UID”);
a = 0;
Serial.println(F(“Scanned PICC’s UID:”));
for ( uint8_t i = 0; i < 4; i++) { //
readCard[i] = mfrc522.uid.uidByte[i];
Serial.print(readCard[i], HEX);
Serial.print(” “);
lcd.setCursor(a, 1);
lcd.print(readCard[i], HEX);
lcd.print(” “);
delay(500);
a += 3;
}
Serial.println(“”);
mfrc522.PICC_HaltA();
return 1;
}
