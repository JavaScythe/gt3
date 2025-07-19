### 6-9-25: Back
After a one day break from this YSWS, the work continues.
This time I'm making a GT3-esque sim racing wheel. I've already determined that it will aluminium plate as the main connecting piece.

It also has to use the required logitech QR (unfortunately 60$). There exists a reversed engineered version but of course that is also paid. Of course, the mechanical connection can always be made, but there is a chip and pins in the middle which the wheel will not operate without.

I took a DWG example off online, and ran some FEM simulations (my favorite) to determine the ideal material + thickness. While I did that, I also looked for some vendors who cut and mail aluminum sheets.

I came up with a bunch, but of the group, Xometry was cheapest. They (and a few others) have an instant quote feature which makes it easy to determine the price for DWG+thickness+material.

It looks to be about 50$, and shipped free. Amazing service, they have a bunch of (expensive) features like tapping, finishing (black brushed finish would have been nice).

They also appear to bill by size, not complexity.

<img width="507" alt="image" src="https://github.com/user-attachments/assets/90446fdb-324a-4719-bc12-484cff2d9dc3" />
Note: This shape is mine, and obviously not GT3 style

I did this because GMSH is geeked:
<img width="322" alt="image" src="https://github.com/user-attachments/assets/0b7792a7-03a7-4715-9905-d1b653ca593a" />

Regardless, 0.250in thick 6061 T6 alum. is very strong, and will survive the 11Nm without issue. I am worried about trueforce making the grip rattle.

5 Hours

### 6-10-25: Design
Now begins the fun part selecting compontents.

I love the RPM indicators on wheel, so I will likely do a row of LEDS on the top. For buttons, I'd like to have light up feedback for stuff like pit limiter. I don't usually use rotary encoders, so I won't use a lot of those.

Addtionally, the on-wheel screens are silly in my opinion. The wheel should be moving way too much to read.

To connect all this, since the built in pins are not usable, a external spring cable will connect USB to a PCB I will design to connect all these buttons. Found a twisted pair version on aliexpress, perfect length and cost.
<img width="237" alt="image" src="https://github.com/user-attachments/assets/89a26144-572d-494e-aa33-ca33ee8e5fe6" />

Going to design the PCB, I quickly realized that I'd have to use neopixel type controlled LEDs, since the typical RGB 5mm bulbs would take 3 pins each. Depending on cost/space I am also considering regular RGB diffisued and addding led driver ics. Main concern is still space.

I spent some time testing some firmware out and simhub would be able to provide the telemetry in question, and even in case it doesn't it's fairly easy to add button state for example that pit limiter if pressed once state is on and should flash, and turns off led once toggled again.

4 hours

### 7-7-25: Return
One national championship later I return to work.

Now for pin allocation and MCU selection. After reviewing other wheels and seeing button/rotary count (and placement, most are similar because that's the most effective for reaching it with hands-i will replicate), I have decided to at least try to have the following inputs:

- 10x button
- 2x rotary
- 1x rotary switch

The goal/reason for the rotary switch is maybe have layers for those buttons which will be selectable through simple turning, more reasonable than other methods like button combinations.

Additonally, while I know of wheels that have built in haptic feedback and would be fairly easy to implement, I opted against because the trueforce of my base already is a very powerful haptic (so powerful it rattles the plastic case of the wheel)

<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/c4e3195b-7752-4b68-89c6-9ef93fe2d178" />
<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/b6190f2b-f85e-4ac5-969f-5640a2da96fb" />
<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/e568bf5a-f7de-4b5c-9d9a-eee8a12c6f76" />
<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/73a5142e-1c06-41b0-ab0a-9521f9623f26" />

### 7-8-25: Schematic
(EDIT: I selected a 6 position rotary switch, forgot to put) 

Now I get to wire 10+3+3+6 pairs. Amazing

A person familiar to these arrays would quickly realize that the rotary switch would require that at least one of the dimensions of the button array is at least 6. This turned out to be ok, because a 6*4 matrix still provided enough pins, but a 5\*5 would have provided more. (EDIT: What I didn't know was that those last two buttons would be critical because I needed paddle shifters which I had neglected)

I selected the STM32F042G6U. I love my STM32s and the STM32Cube IDE is a work of art. Highly recommend. Anyway, it's a 28pin package of which 7 are power and usb. 10 are going to buttons, and one to neopixle data. In order to drive all the button's leds, I opted for the TLC5916 to make it fit on the mcu i selected.
<img width="1916" height="944" alt="image" src="https://github.com/user-attachments/assets/69ba7b95-4bdb-4157-8441-a6d5747d2400" />

4 hours

### 7-9-25: Schematic start
Selected reasonable LDO, finally a project with no ADC noise requirement.

I managed to actually find a diffused 5mm neopixel led, I had my doubts if that product even exists. But it does, they're bright, and cheap. Peak.

<img width="896" height="597" alt="image" src="https://github.com/user-attachments/assets/99c6f770-954e-412e-95ce-5770788b66e8" />

Wired power and tried some spacing in CAD (would put a picture but its literally two lines in ms paint and some math) to determine what kind of space I have to work with. I'm targeting a semi circle shape that avoids the center shaft of course.

3 hours

### 7-10-25: Schematic end
Really easy choice for the led drivers, added the messy button matrix.

<img width="1074" height="942" alt="image" src="https://github.com/user-attachments/assets/af223e53-83b4-4fe5-b009-e7d3ec65a80a" />


This is pretty much the final schemtic barring anything odd. Should be douable to fit all onto the target shape. Addtionally, I've selected a pitch for the leds

5 hours

### 7-11-25: Routing 1
Now I started routing. Beginning in the top left with the usb and associated stuff, I redid it serveral times in pursit of the optimal shape. The LED drivers proved somewhat difficult to integrate.

About the footprints: I love screw terminals. but they are large and there's no need for a temporary fixture like them so I opted for just hand wire to board solder pads. With enough flux everything is possible

I spent nearly the whole day on KiCAD while on call with my friends doing other pcb designs. Good fun.

Addtionally, I really need to start putting mounting holes before beginning. It would make it a lot easier.

<img width="1509" height="552" alt="image" src="https://github.com/user-attachments/assets/ef622b26-9100-477d-843e-6f1aa3d7935b" />


8 hours

### 7-12-25: Schematic 2
Most unfortunately, I have completely neglected paddle shifters from my design. Luckily I have the matrix space (just barely) but it means that I have to make four other pads fit on the board. I added them to the schematic, but then contemplated if I would rather use hall sensors.

After some internal reflection (looking at CubeMX and grimacing at the lack of avaliable pins), as well as footprint considerations, I am opting for regular spst switch paddle actuators. 

These were added, to the schematic without problem, but stuffing them on the board proved very difficult. The right side was, in css terms, reflowed.

<img width="1184" height="962" alt="image" src="https://github.com/user-attachments/assets/b97fb590-6010-4810-895b-fb6a26bd78d0" />

<img width="1558" height="640" alt="image" src="https://github.com/user-attachments/assets/f73157ed-458e-4aeb-8f03-87fd8c472151" />

4 hours
