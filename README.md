# Arduino-3.2-TFT-LCD-Touch-Screen-and-an-SD-card
Project about Arduino 3.2" TFT LCD Touch Screen and an SD card run with arduino uno

# Background 
The background project for this code could be a photo viewer using an Arduino 3.2" TFT LCD Touch Screen and an SD card. The touch screen would display images stored on the SD card, allowing the user to navigate through the images and view them full-screen. The user could select images by pressing the touch screen or using buttons connected to the Arduino.

This project can be used as a simple photo viewer for personal or commercial use, or as an educational tool to teach the basics of programming and electronics. The code provided is just a starting point and can be modified and expanded upon to fit the specific requirements of the project. For example, the code could be expanded to include features such as image editing, image filtering, or the ability to play slide shows.

# Methodology 
- Gather materials: To build this project, you will need an Arduino Uno board, a 3.2" TFT LCD Touch Screen, an SD card, and a card reader. You may also need a breadboard, wires, and resistors depending on the specific screen and card reader you are using.

- Set up the hardware: Connect the TFT LCD Touch Screen and the SD card reader to the Arduino Uno board using the appropriate wiring diagram for your components.

- Load the libraries: Download and install the TFT_ILI9163C and SD libraries to be used in the project. These libraries will provide the necessary functions for communicating with the TFT screen and reading data from the SD card.

- Write the code: Write the code that will be used to display the images stored on the SD card on the TFT screen. The code should include functions for initializing the TFT screen and SD card, opening the image files, reading the header information, setting the address window for the TFT screen, and writing the pixel data to the screen.

- Test and debug: Upload the code to the Arduino board and test the project. If there are any errors or issues, debug the code and make any necessary changes.

- Add additional features: Once the basic project is working, you can expand the code to add additional features such as image editing, filtering, or slide shows.

- Finalize and present: Once the project is working as desired, finalize the project by adding any additional hardware or features and document the steps involved in building and using the photo viewer. Present the project to others, either in person or online.

# Example code
Here is an example of code that can be used to display a picture from an SD card on an Arduino 3.2" TFT LCD Touch Screen:
```
#include <SPI.h>
#include <TFT_ILI9163C.h>
#include <SD.h>

// Define the CS, DC, and RST pins for the TFT screen
#define CS 10
#define DC 9
#define RST 8

// Create a TFT object
TFT_ILI9163C tft = TFT_ILI9163C(CS, DC, RST);

void setup() {
  Serial.begin(9600);
  
  // Initialize the TFT screen
  tft.begin();
  tft.setRotation(3);
  
  // Initialize the SD card
  if (!SD.begin(4)) {
    Serial.println("SD card initialization failed");
    return;
  }
}

void loop() {
  // Open the file containing the picture
  File picture = SD.open("image.bmp");
  
  // Check if the file was successfully opened
  if (!picture) {
    Serial.println("File not found");
    return;
  }
  
  // Read the header information from the file
  uint32_t bmpWidth = picture.read() | (picture.read() << 8 );
  uint32_t bmpHeight = picture.read() | ( picture.read() << 8 );
  picture.read();
  
  // Set the address window for the TFT screen
  tft.setAddrWindow(0, 0, bmpWidth-1, bmpHeight-1);
  
  // Read the pixel data from the file and write it to the TFT screen
  for (int y = 0; y < bmpHeight; y++) {
    for (int x = 0; x < bmpWidth; x++) {
      uint16_t color = picture.read();
      color |= picture.read() << 8;
      tft.pushColor(color);
    }
  }
  
  // Close the file
  picture.close();
}
```

Note:
This code uses the TFT_ILI9163C and SD libraries to display a .bmp image from an SD card on the TFT LCD touch screen. In the setup() function, the TFT screen and SD card are initialized. In the loop() function, the code opens the file containing the picture, reads the header information, sets the address window for the TFT screen, and writes the pixel data to the screen. The file is then closed. Note that this code assumes the picture is a 16-bit .bmp file and may need to be adjusted for other file types or resolutions.
