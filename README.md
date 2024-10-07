# Manual-AI-module
AI module: Gebruik de AI vision module en toon de demo in chrome

# Instructiehandleiding: Lokale AI Vision zonder Cloud met de SEEED AI Module en Arduino Uno

## Doel:
De SEEED AI Vision Module aansluiten op een **Arduino Uno** voor lokale beeldherkenning zonder gebruik te maken van de cloud. Deze module is ontworpen voor het Grove-systeem, dus het vereist wat aanpassingen om correct te werken.

---

## Benodigdheden:
- **Arduino Uno**
- **SEEED Grove AI Vision Module**
- **Grove-kabels**
- **USB-kabel voor Arduino**
- **Arduino IDE** (laatste versie)
- **Seeed_Arduino_GroveAI Library** geïnstalleerd in Arduino IDE

---

## Stappenplan:

### 1. Begin met de NodeMCU ESP32
In eerste instantie ben ik begonnen met het gebruik van een **NodeMCU ESP32** om de SEEED Grove AI Vision Module aan te sturen. Dit leek een geschikte keuze vanwege de veelzijdigheid van de ESP32. Echter, tijdens het proces kwam ik erachter dat de **bibliotheek die voor de Grove AI Vision Module werd gebruikt geen ondersteuning bood voor de analoge pinnen van de NodeMCU ESP32**. Hierdoor konden sommige noodzakelijke functies, zoals de initialisatie van de SDA- en SCL-pinnen (voor I2C-communicatie), niet correct worden uitgevoerd.

### 2. Reden voor Overstap naar Arduino Uno
Vanwege dit probleem heb ik besloten over te stappen naar een **Arduino Uno**. De Arduino Uno heeft:
- **Voldoende digitale pinnen** die goed ondersteund worden door de gebruikte bibliotheek.
- **Directe compatibiliteit met de Grove AI Vision Module** via de standaard I2C-pinnen zonder dat handmatige aanpassingen aan de bibliotheek nodig zijn.

### 3. Voordelen van de Arduino Uno
Na de overstap naar de **Arduino Uno** kon ik de SEEED Grove AI Vision Module zonder verdere problemen aan de praat krijgen. De Uno bood de stabiliteit en ondersteuning die ik nodig had, met voldoende I/O-mogelijkheden om de AI-visionfuncties lokaal uit te voeren zonder afhankelijk te zijn van de cloud.

---

## Installatie en Configuratie:

### 4. Voordelen van een lokaal vision-model overdenken
Voordat je begint, overweeg waarom je een lokaal AI-vision-model wilt gebruiken. Enkele voordelen zijn:
   - **Privacy**: Geen gegevens worden naar de cloud verzonden.
   - **Snelheid**: Directe verwerking zonder vertraging door internet.
   - **Geen internet nodig**: Werkt zelfs zonder een actieve internetverbinding.

### 5. Bestudeer de SEEED AI Vision Module
   Raadpleeg de documentatie over de **SEEED Grove AI Vision Module**:
   - [SEEED Grove AI Vision Module Wiki](https://wiki.seeedstudio.com/Grove-Vision-AI-Module/)

   Bestudeer vooral de pinconfiguratie en compatibiliteit met de Arduino Uno.

   ![Screenshot van de SEEED Grove AI Vision Module documentatie](images/grove_ai_vision_module_documentation.png "Bestudeer de pinconfiguratie en compatibiliteit met de Arduino Uno.")

### 6. Installeer de vereiste libraries
   Open de Arduino IDE en installeer de benodigde library:
   - Ga naar **Sketch > Bibliotheek beheren**, zoek naar **Seeed_Arduino_GroveAI** en installeer deze library.

   ![Screenshot van de Arduino IDE bibliotheekbeheer](images/arduino_library_manager.png "Open de bibliotheekbeheer in de Arduino IDE.")

### 7. Sluit de Grove AI Vision Module aan op de Arduino Uno
   Sluit de Grove AI Vision Module aan op de **I2C-pinnen** van de Arduino Uno:
   - **SDA (A4)** op de Arduino Uno naar **SDA** op de Grove Module.
   - **SCL (A5)** op de Arduino Uno naar **SCL** op de Grove Module.
   - **GND** naar **GND** op de Arduino Uno.
   - **VCC** naar **5V** op de Arduino Uno.

   **Let op**: Een veelvoorkomende fout is het verwisselen van de SDA- en SCL-verbindingen. Zorg ervoor dat de draden correct zijn aangesloten.

   ![Screenshot van de aansluitingen op de Arduino Uno](images/arduino_uno_connections.png "Sluit de Grove AI Vision Module aan op de juiste I2C-pinnen.")

### 8. Open de object_detection Voorbeeldcode
   - Ga in de Arduino IDE naar **Bestand > Voorbeelden > Seeed_Arduino_GroveAI-master > object_detection**.
   - Dit voorbeeldprogramma gebruik je om objectdetectie uit te voeren met de Grove AI Vision Module.

   ![Screenshot van het openen van voorbeeldcode in Arduino IDE](images/open_object_detection_example.png "Open de object_detection voorbeeldcode in de Arduino IDE.")

### 9. Pas de Voorbeeldcode aan voor de Arduino Uno
   Voeg de volgende regels toe om de I2C-pinnen correct te initialiseren voor de Arduino Uno:

```cpp
#include <Wire.h>
#include <Seeed_Arduino_GroveAI.h>

WEI AI;  // Object voor de AI-module

void setup() {
    Serial.begin(115200);  // Start seriële communicatie met 115200 baudrate

    // Start de I2C-communicatie op de juiste pinnen voor de Arduino Uno
    Wire.begin();  // SDA = A4, SCL = A5

    // Initialiseer de AI-module
    if (AI.begin()) {
        Serial.println("AI module successfully started!");
    } else {
        Serial.println("Algo begin failed.");
        while(1);  // Stop het programma als de AI-module niet kan worden geïnitialiseerd
    }
}

void loop() {
    // Hier kun je je code voor objectdetectie of gezichtsherkenning plaatsen
}
```

10. Upload de Voorbeeldcode naar de Arduino Uno
Verbind de Arduino Uno met je computer via de USB-kabel.
Selecteer in de Arduino IDE de juiste Board (Arduino Uno) en Poort.
Klik op de Upload-knop om de code naar de Arduino Uno te uploaden.

11. Controleer de Seriële Monitor
Open de Seriële Monitor in de Arduino IDE (Hulpmiddelen > Seriële Monitor) en stel de baudrate in op 115200.
Je zou een bericht moeten zien zoals "AI module successfully started!".
Als je de foutmelding "Algo begin failed" ziet, controleer dan of de module correct is aangesloten en of de juiste bibliotheken zijn geïnstalleerd.

12. Verbinding met de Laptop voor Beeldherkenning
Verbind de Grove AI Vision Module via een tweede USB-C-kabel direct met je laptop.
Open Google Chrome (andere browsers kunnen problemen geven) en probeer gezichtsherkenning of objectdetectie te activeren via de AI-module. Met het inpluggen van de usb-c kabel zou er een popup moeten komen voor de webaplicatie die je de results van de face detection zou moeten geven.

14. Problemen Oplossen: "Geen beeld?"
Als je geen beeld krijgt of als de gezichtsherkenning niet werkt:

Controleer of je SDA en SCL correct hebt aangesloten (A4 = SDA, A5 = SCL).
Probeer de draden voor SDA en SCL om te draaien (gele en witte kabels).
Controleer of de voeding correct is (de module moet 5V krijgen).
Raadpleeg online forums of de SEEED-handleiding voor mogelijke oplossingen.
Veelvoorkomende Fouten
Verkeerd aangesloten pinnen: SDA en SCL verkeerd aangesloten. Zorg ervoor dat SDA naar A4 en SCL naar A5 is aangesloten.
Foutieve bibliotheekversie: Zorg ervoor dat de nieuwste versie van de Seeed_Arduino_GroveAI library is geïnstalleerd.
Problemen met seriële communicatie: Controleer of de seriële monitor op de juiste baudrate is ingesteld (115200).
Algo begin failed: Dit kan duiden op een probleem met de I2C-verbinding. Controleer de verbindingen en zorg ervoor dat de module goed werkt.
## Einde
Je hebt nu de Grove AI Vision Module werkend met een Arduino Uno voor lokale beeldherkenning zonder cloud. Als alles correct is ingesteld, zou de AI Vision Module objecten moeten kunnen detecteren en gezichten herkennen via de Arduino Uno.
