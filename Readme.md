# **🤖 4xiDraw H-Bot Pen Plotter**

The 4xiDraw Extensions for Inkscape - Software to drive the 4xiDraw drawing machine.

More about 4xiDraw:https://www.instructables.com/4xiDraw/

## **🌟 Project Overview**

The **4xiDraw** is an open-source, DIY pen plotter/drawing machine inspired by the commercial AxiDraw. It utilizes an **H-Bot** kinematic configuration with a single belt and two stepper motors to achieve smooth, precise movement across a large drawing area.

This machine is designed to be highly versatile, capable of adapting to almost any document size and paper type. It can draw with various writing instruments, including **felt-tip pens, ballpoint pens, and fountain pens**, thanks to its variable pen orientation and servo-controlled pen lift mechanism.

| Feature | Description |
| :---- | :---- |
| **Kinematics** | H-Bot (Single belt, dual stepper) |
| **Controller** | Arduino UNO \+ CNC Shield |
| **Firmware** | GRBL (Special Servo-enabled version) |
| **Output** | Adaptable to various pen types and paper sizes |

## **🛠️ Bill of Materials (BOM)**

The majority of the structural parts are designed to be **3D-printed** (STL files available from the original source).

### **⚙️ Electronics & Motion**

| Qty | Component | Notes | Symbol |
| :---- | :---- | :---- | :---- |
| 2 | Nema 17 Stepper Motors | 40mm or shorter recommended | 🔌 |
| 1 | Arduino UNO | The main controller board | 🧠 |
| 1 | CNC Shield V3 | Shield to interface drivers and motors | 🛡️ |
| 2 | Pololu Stepstick Drivers | Stepper motor drivers (e.g., A4988) | 🎛️ |
| 1 | Micro Servo SG90 | Used for the automatic pen lift | 🖊️ |
| 1 | GT2 Belt | Approx. 1.4 meters long | 🔗 |
| 2 | 20-tooth GT2 Pulleys | For the stepper motors | ⚙️ |
| 1 | Power Supply | 12V 2A is required | ⚡ |
| 1 | USB Cable | For communication and power (during setup) | 💻 |

### **🔩 Mechanical & Structure**

| Qty | Component | Notes | Symbol |
| :---- | :---- | :---- | :---- |
| 4 | 8mm Smooth Rods | Two 400mm, Two 320mm | 📏 |
| 8 | LM8UU Linear Bearings | For smooth rod movement | 🛹 |
| 10 | 6223ZZ Bearings | Flanged bearings for belt guidance | 🌀 |
| 2 | M10 Threaded Rods | 400mm long each, for frame support | 🏗️ |
| 8 | M10 Nuts | To fix the M10 threaded rods | 🌰 |
| 2 | Carbon Fiber Tubes | 4mm OD, 100mm long (for vertical carriage) | 🥢 |
| \- | Assorted M3 Screws/Nuts | M3 x 6mm, 15mm, 16mm, 30mm | 🔩 |

## **💻 Setup & Configuration Guide**

### **1\. 🏗️ Mechanical Assembly**

Follow the assembly sequence provided in the original Instructables guide. You will need to carefully integrate the linear bearings, motor mounts, the X/Y carriages, and route the single GT2 belt.

* **Key Step:** Ensure the smooth rods are perpendicular (right angle) when assembling the carriages.  
* **Servo:** The SG90 servo and pen holder attach to the vertical carriage.

### **2\. 🔌 Wiring & Electronics**

The CNC Shield is stacked onto the Arduino UNO, and the Pololu drivers are mounted on the shield.

* **JUMPERS:** Place three jumpers under the X and Y driver headers on the CNC Shield before mounting the drivers.  
* **POWER JUMPER (CRITICAL):** Connect the **Vin** header to the **\+** header on the CNC Shield. This is crucial for properly powering the servo.  
* **Stepper Motors:** Connect the two Nema 17 motors to the **X** and **Y** axis headers on the CNC Shield.  
* **Servo:** Use the 250mm extension cable and connect the servo to:  
  * **\+5V** (Red wire)  
  * **GND** (Black wire)  
  * **Digital Pin 11** (Signal wire)

### **3\. 💾 Firmware Installation**

The standard GRBL firmware **does not** support the servo. You must use the special servo-enabled version.

1. **Download:** Obtain the special GRBL flavor (e.g., grbl-servo from misan's GitHub, as noted in the source).  
2. **Installation:** Install the code as an Arduino library.  
3. **Verification:** Connect to the board using the Arduino Serial Monitor at **115200 bps**. You should see the welcome message: grbl 0.9i \['$' for help\]

### **4\. 📐 GRBL Calibration (Steps/mm)**

Use a G-Code sender program (see step 5\) to configure the correct scale.

1. Open the terminal tab in your G-Code sender.  
2. Type $$ and press enter to view the current GRBL settings.  
3. Adjust the X and Y steps per millimeter parameters:  
   * Type $100=80 and press enter (Sets X-axis).  
   * Type $101=80 and press enter (Sets Y-axis).

*These values (80 steps/mm) are based on a 200-step motor, a 20-tooth pulley, and a GT2 belt (2mm pitch).*

### **5\. 🎨 Computer Software**

You need two pieces of software to operate the plotter: one for G-Code generation and one for sending the G-Code.

| Purpose | Recommended Software | Notes |
| :---- | :---- | :---- |
| **G-Code Generation** | **Inkscape** (Vector Drawing Program) | Requires a specialized plugin for vector-to-Gcode conversion. |
| **G-Code Sending** | **Universal G-Code Sender (UGS)** | A Java program to load and stream the G-code file to the plotter. |
| **Alternative** | **4xiDraw Inkscape Plugin** | A single plugin that handles both G-Code generation and sending from within Inkscape. (Search for bullestock/4xidraw on GitHub). |

## **💡 Troubleshooting & Notes**

* **Motor Vibration:** If motors vibrate instead of rotating, it usually indicates incorrect coil wiring. Use an ohmmeter to confirm the coil pairs.  
* **Servo Control:** The servo functions via the special GRBL's implementation of the **M3** (pen down) and **M5** (pen up) G-Code commands.  
* **Diagonal Drawing:** If your plotter draws diagonally instead of straight lines, you likely need to enable the **COREXY** feature in the firmware's config.h file, though the special GRBL version should handle this automatically.

Enjoy building your 4xiDraw\!
