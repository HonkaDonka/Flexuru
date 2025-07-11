## July 6
When I first heard about flexible PCBs, I knew I wanted to make something related to origami. 

I wanted to make something similar to a badge; a pcb that includes an e-ink display, some buttons, and other peripherals. 

Later, I decided to make it a flex PCB that's able to fold into a crane. Why a crane? Well, it's one of the first things that come to mind when talking about origami!

Originally, I went with a hybrid-flex approach, where it would be a PCB that had a couple of flex sections. However, it was way too pricey and instead I decided on a pure flex PCB with stiffener. 

The size would be 150mm x 150mm (like a regular sheet of kami).

First, I folded an actual origami crane and created a crease pattern from it through [Oriedita](https://oriedita.github.io/). 

<img width="853" alt="image" src="https://github.com/user-attachments/assets/6fa8b1cc-8427-43aa-92e1-10586c6fe79b" />

This will later help me map out where to place components. 

Doing some research, it seems that flex PCBs should minimize the amount of copper going through folds, so I decided to take inspiration from [e-textiles](https://www.connectortips.com/what-connectors-are-there-for-woven-electronics-e-textiles-faq/?utm_source=chatgpt.com), specifically the connectors. 

I think my current plan is to utilize pogo-pin-like connectors to route signals and power through the PCB. This way, there would be no copper running through the PCB creases AND it also does the favor of keeping creases together. 

**Time spent this session: 2 hours**

## July 7-8
I have chosen the parts I want to include:
- An E-ink display
- Some LEDs
- A coin cell battery
- ESP32

First, let's put the crane crease pattern into KiCad. I opened the PCB editor, made a 150mm x 150mm square and imported the crease pattern as a .svg file. However, since Oriedita has no specification of dimensions, the imported graphic was a little too small. 

<img width="1168" height="366" alt="image" src="https://github.com/user-attachments/assets/2196827d-996f-4aea-b9a4-4a51b39744a5" />

I was able to scale it to the correct dimension using KiCad's scale feature though.

### E-ink Display
Originally, I wanted to have a 2.9" e-ink display on one of the crane's wings. However, upon looking at the footprint, it seems like 2.9" is way too big for the wings. After a bit of searching, I settled on [this](https://www.aliexpress.us/item/3256806104827483.html?spm=a2g0o.productlist.main.7.4450HqopHqopXa&algo_pvid=243ea8bd-84ba-48a1-95c3-a989ef9466ef&algo_exp_id=243ea8bd-84ba-48a1-95c3-a989ef9466ef-6&pdp_ext_f=%7B%22order%22%3A%221086%22%2C%22eval%22%3A%221%22%7D&pdp_npi=4%40dis%21USD%219.14%219.14%21%21%2165.16%2165.16%21%402101c5b217519047034655876e30c3%2112000037370742806%21sea%21US%212723986122%21X&curPageLogUid=bEMpul0BwcTT&utparam-url=scene%3Asearch%7Cquery_from%3A#nav-description). It already includes the driver module on the back of the board, saving some extra space on the crane wing. 

<img width="628" height="412" alt="image" src="https://github.com/user-attachments/assets/bebd4a31-c1e8-487a-8427-114ee8164d1a" />

I only need 8 pins to run the e-ink display. It still is a bit too big for the crane wing, so I scaled up the PCB from 150mm x 150mm to 200mm x 200mm. 

### LEDs
Pretty self explanatory, just some SK6812s that are driven by the ESP32

### Battery Charging & Voltage Regulating
Now this is where I spent the most of my time. After reading a couple of guides on battery charging circuits, I settled on using the MCP73871 as a charging IC for an LIR2450. I'd have a linear voltage regulator as well (RT9080). 

<img width="1252" height="952" alt="image" src="https://github.com/user-attachments/assets/43503759-7145-44d4-844e-916f1e0377eb" />

For the MCP73871, we'll be referencing [it's datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/MCP73871-Data-Sheet-20002090E.pdf) 

We want to charge using USB power, so we pull SEL to GND. This is for battery charging, so CE is pulled to 5V. Pulling PROG2 to high allows us to charge at 500mA, as per the datasheet. We dont actually want an NTC, so we're using a fixed resistor so that charging never gets cut off. 

TBA
