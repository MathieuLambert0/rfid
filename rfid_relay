#include <EEPROM.h>
#include <Servo.h>
#include <Wire.h>
#include <Adafruit_NFCShield_I2C.h>
#include <FastLED.h>
#include "FastLED.h"


////LEDs*************************************
//Define number of LEDs
#define NUM_LEDS (1)
#define DATA_PIN (13)
// Define the array of leds
CRGB leds[NUM_LEDS];

#define IRQ   (2)
#define RESET (3)
Adafruit_NFCShield_I2C nfc(IRQ, RESET);

int master = 0;
int verification = 0;
int led = 13;
int relay = 12;

void setup(void) {
FastLED.addLeds<NEOPIXEL, DATA_PIN>(leds, NUM_LEDS);

Serial.begin(115200);                                                              
Serial.println("Mathieu Lambert IMCA 321, Dec. 2015"); 
pinMode(relay, OUTPUT);
nfc.begin();
uint32_t versiondata = nfc.getFirmwareVersion();
 if (! versiondata) {
    Serial.print("PN53x Not Found !");
    while (1);
  }

  Serial.print("Carte PN5 trouvee"); Serial.println((versiondata>>24) & 0xFF, HEX); 
  Serial.print("Version du logiciel "); Serial.print((versiondata>>16) & 0xFF, DEC); 
  Serial.print('.'); Serial.println((versiondata>>8) & 0xFF, DEC);

  nfc.SAMConfig(); 
  
  Serial.println("Waiting for ISO14443A ...");
  Serial.println("Scan Card !");


}

void ouverture()
{ int valeur=0;
  
  leds[0] = CRGB::Green;
  digitalWrite(relay,HIGH);
  delay(5000);
  digitalWrite(relay,LOW);
}

void attente()
{
  
}

void enregistrement()
{
 
}  

void loop(void) {

  int test = 0;
  master = 0;
  verification = 0;

leds[0] = CRGB::White;
  FastLED.show();  
  
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };                                      
  uint8_t uidLength;                                                                 
  uint8_t uid_m[] = { 0xE1, 0x7E, 0x22, 0xFC, 0, 0, 0};             
  uint8_t uid_e[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uid_dec[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uid_r[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uid_y[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uid_u[] = { 0, 0, 0, 0, 0, 0, 0 };

  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength);

if (success) { 
  Serial.println("Carte trouvee");
    Serial.print("  UID Length: ");Serial.print(uidLength, DEC);Serial.println(" bytes");
    Serial.print("  UID: ");
    nfc.PrintHex(uid, uidLength);
    Serial.println("");


  
  EEPROM.write(9,uid[0]);                                                                     
  EEPROM.write(10,uid[1]);
  EEPROM.write(11,uid[2]);
  EEPROM.write(12,uid[3]);

  uid_dec[0] = EEPROM.read(9);                                                                
  uid_dec[1] = EEPROM.read(10);
  uid_dec[2] = EEPROM.read(11);
  uid_dec[3] = EEPROM.read(12);
 
  
  uid_e[0] = EEPROM.read(1);
  uid_e[1] = EEPROM.read(2);
  uid_e[2] = EEPROM.read(3);
  uid_e[3] = EEPROM.read(4);
  
  uid_r[0] = EEPROM.read(5);
  uid_r[1] = EEPROM.read(6);
  uid_r[2] = EEPROM.read(7);
  uid_r[3] = EEPROM.read(8);
  
  uid_y[0] = EEPROM.read(13);
  uid_y[1] = EEPROM.read(14);
  uid_y[2] = EEPROM.read(15);
  uid_y[3] = EEPROM.read(16);
  
  uid_u[0] = EEPROM.read(17);
  uid_u[1] = EEPROM.read(18);
  uid_u[2] = EEPROM.read(19);
  uid_u[3] = EEPROM.read(20);




   for (int i=0; i < 4; i++)                                                
   
    {   
        if (uid[i] == uid_m[i])
        {
          master = master + 1;
        }
        
       /*else if (uid[i] == uid_t[i])
        {
        verification = verification + 1;
       
        } */
      /*  else if (uid[i] == uid_n[i])
        {
        verification = verification + 1;
       
        } */

        else if (uid_e[i] == uid_dec[i])
        {
        verification = verification + 1;
      
        }
        else if (uid_r[i] == uid_dec[i])
        {
        verification = verification + 1;
      
        }
        else if (uid_y[i] == uid_dec[i])
        {
        verification = verification + 1;
      
        }
        else if (uid_u[i] == uid_dec[i])
        {
        verification = verification + 1;
      
        }
        
     else                                                                  
     {
       break;
     }
   }



      
  if (master == 4)                                                                                         // Si la carte mere est scannee
  {
                                                                                          // Jouer hungergames
  master = 0;
  verification = 8;
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;
  Serial.println("  Scannez la    ");
  Serial.println(" nouvelle carte ");
  
  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength); // On lit la carte a enregistrer
  
  if (success)
  {
   for (int i=0; i < 4; i++)                                                  // Comparaison entre l'UID lu et la carte maitre
    {   
        if (uid[i] == uid_m[i])
        {
          master = master + 1;
        }
    }
  if (master == 4)                                                          // Si la care maitre est scannee une deuxieme fois
  {
                                                                  // Jouer la marche imperiale
  EEPROM.write(1,0);                                                       // Remise a 0 de la carte enregistree
  EEPROM.write(2,0);
  EEPROM.write(3,0);
  EEPROM.write(4,0);
  EEPROM.write(5,0);                                                       // Remise a 0 de la carte enregistree
  EEPROM.write(6,0);
  EEPROM.write(7,0);
  EEPROM.write(8,0);
  EEPROM.write(13,0);                                                       // Remise a 0 de la carte enregistree
  EEPROM.write(14,0);
  EEPROM.write(15,0);
  EEPROM.write(16,0);
  EEPROM.write(17,0);                                                       // Remise a 0 de la carte enregistree
  EEPROM.write(18,0);
  EEPROM.write(19,0);
  EEPROM.write(20,0);
  delay(2000);
  Serial.println("Suppression cartes");
  delay(2000);

  }
  
  else if (master != 4)                                                    // Si la carte qui a ete scannee une deuxieme fois est pas la carte maitre
  {  
                                                                    // Jouer la melodie
  delay(1);
  if (uid_e[0] == 0)
  {
   enregistrement();
   EEPROM.write(1,uid[0]);                                                  // Enregistrer le code de la carte sur la memoire morte
   EEPROM.write(2,uid[1]);
   EEPROM.write(3,uid[2]);
   EEPROM.write(4,uid[3]);
 
   Serial.println("Enregistrement 1");
   delay(4000);
   test=1;
  } 
  
  else if (uid_r[0] == 0 && test == 0)
  { 
   enregistrement(); 
   EEPROM.write(5,uid[0]);                                                  // Enregistrer le code de la carte sur la memoire morte
   EEPROM.write(6,uid[1]);
   EEPROM.write(7,uid[2]);
   EEPROM.write(8,uid[3]);
   
   Serial.println("Enregistrement 2");
   delay(4000);
   test=1;
  }
  else if (uid_y[0] == 0 && test == 0)
  { 
   enregistrement(); 
   EEPROM.write(13,uid[0]);                                                  // Enregistrer le code de la carte sur la memoire morte
   EEPROM.write(14,uid[1]);
   EEPROM.write(15,uid[2]);
   EEPROM.write(16,uid[3]);
   
   Serial.println("Enregistrement 3");
   delay(4000);
   test=1;
  }
    else if (uid_u[0] == 0 && test == 0)
  { 
   enregistrement(); 
   EEPROM.write(17,uid[0]);                                                  // Enregistrer le code de la carte sur la memoire morte
   EEPROM.write(18,uid[1]);
   EEPROM.write(19,uid[2]);
   EEPROM.write(20,uid[3]);
   
   Serial.println("Enregistrement 4");
   delay(4000);
  }
  
  else
  {
  
   Serial.println("Plus de place");
   delay(4000);
  }  
  }
  }
  }                                                                         // Si l'UID est connu, on apelle la fonction d'ouverture de porte

else if (verification == 4)
  {

  leds[0] = CRGB::Green;
  FastLED.show();
  Serial.println(" AUTHORIZED ");
  digitalWrite(relay,HIGH);
  
    for(int x=0; x<4; x++) 
  {

    delay(700);
  }
    ouverture();
  } 

   
 
 else if (verification != 4)                                                 // Card detected
                                      
   
    Serial.println("  DENIED ");
    for (int ab=0; ab<9;ab++)
    { 
  leds[0] = CRGB::Red;
  FastLED.show();
  delay(300);

                    
      attente();
    }





    
    Serial.println("PLEASE SCAN CARD.");
    leds[0] = CRGB::White;
    FastLED.show();
   }}
   
 

  




  
