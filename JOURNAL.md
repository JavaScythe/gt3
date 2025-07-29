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

### 7-13-25: Plate shape and pcb placement
After working hard from reference images and looking at my very own two hands, I made a rough layout of the holes for my fingers and for the QR connection. This gave me an approximate spacing to test the pcb outline on. As it turns out, I should have done this first. While the board avoids the QR completely, the two side pieces extend too far down and would leave little to no room to place the paddle shifters ideally. After some grumbling and retrospective regret, I set out again to move the longest side to flatten it. Addtionally, at this point I realized how bad the USB data lines were but meh I'll fix that tommorow.

Here is the flatter PCB

<img width="2220" height="665" alt="image" src="https://github.com/user-attachments/assets/652b95a0-e95f-44a4-b76b-4c77eadf9509" />

3.5 hours

### 7-14-25: Plate shape and handle considerations
Honing my freecad skills through painful trial and error using the curve tool, I took my initial measurements from the day before and the outline of the GT Neo, I made half of the wheel (to be mirrored). Some considerations:

- Wide(r) outer grip surface: Thing that plastic grip will mount on made thicker. I can reasonably make the 3dprinted grip thin in some places so a wider plate area is ok. This is good for three reasons: Can put more mounting holes with less cost on wheel structure integrity (FEM analysis incoming) and more surface area for potential glue/wrap if needed, and reduced ability for the grip to turn on the mounting axis.
- Higher top area: Best mounting for LEDS and PCB. The shaft part doesnt matter at this point it's still the paddle shifters that get in the way.
- Multiple grip mount bolt holes. Enabled by the first contention in this list, I really hope to eliminate that dreaded creak.
- 2x Rotary placement. While the rotaries have threads, I will mount them in smooth hole with a nut on top. Creating those threads in CNC is impossible and I don't know if they'll tap the hole without addtional cost.
- 1x Rotary switch. This one's position is still in works. I'd like to be able to put some labels or at least some colors next to it to the positions
- 10x buttons. Again, not threaded hole. nut mounted. I am worried about the two being in the way of the paddle shifter but I might move them once I see it in final assembly
- Width: I decided early on to make it the same radius as my existing wheel. 300mm. Just what I'm used to in terms of feel and force.

6 hours

### 7-15-25: Grip
After messing the placement of rotaries for a while I just left them. Now I began work on the grip. Not terrible complex, just take parallel of plate edges, expand a bit, then round off the top to make it smooth. Now i just placed some mounting screw in (2 part, one for head and one for actual thread, to make it smooth, then on the other side of course the hex bolt). That's all there is to it for now in terms of choices, techincal implementation was very different.

So I tried to "round off" said shape in freecad and this proved to be a technical challenge that consumed far more time that it should have. However, the end result is there and I certainly learned something. (Fusion next time)

So far from what I've heard and felt raw/sanded PLA should be fine for grip long term, we'll see. I can always go back and unbolt and wrap

<img width="718" height="958" alt="image" src="https://github.com/user-attachments/assets/f22efd01-9f40-4ad5-9431-413321f8d58d" />
<img width="602" height="976" alt="image" src="https://github.com/user-attachments/assets/d4b69606-300e-476d-a724-d91251a07971" />
<img width="1101" height="593" alt="image" src="https://github.com/user-attachments/assets/e0573dbf-cacd-4ede-8ef8-d5fea9c413de" />
<img width="1131" height="581" alt="image" src="https://github.com/user-attachments/assets/8f7d54b5-ce95-4708-8c6e-68bf0ca230ff" />

4 hours

### 7-16-25
Now it's on to one of the final pieces of the design: the shifters. I started with some research on existing paddle shifter modules as well as some DIY ones.

All of them were magnetic, but what interested me most was the adjustment in some. If the piece is already made why not make a slot and have a screw tighten down where it should be? I want to include this in my design. 

The DIY ones I saw were plastic but I know this will not cut it. High force magnets with many many cycles, and without bearings is positively a disaster. Simply put, they are entry level, which this wheel is not. A CNCd set would perform much better at minimal extra cost. 

Regardless of this though, the design remains more or less the same. The adjustment doesn't add extra pieces just a differnt mounting onto the body. Speaking of which, let me outline the 3 pieces that make this thing:

- 1: Body. This screws into the wheel (bolt whatever). Large U-shape. Has holes for axle, and a groove? for adjustable mounting of the 2nd part
<img width="1380" height="1111" alt="image" src="https://github.com/user-attachments/assets/d0c236b4-4db9-4e00-925c-40ba1a784b4f" />
<img width="1138" height="932" alt="image" src="https://github.com/user-attachments/assets/f80bc19d-8d36-4d7f-865b-61fa4a9e9777" />
- 2: Arm base: This holds the magnet and limits the travel of the actual arm. angle adjustable.
<img width="1065" height="919" alt="image" src="https://github.com/user-attachments/assets/e78315cb-c33d-4e5a-a902-86f2cc9e2b27" />
<img width="1236" height="668" alt="image" src="https://github.com/user-attachments/assets/48eaa1b8-7640-48fc-9a13-6c582abe3497" />
- 3: Arm: This is the actual piece that moves. Magnet, hits spst switch on low, has holes on the end for mounting the actual paddle.
<img width="1408" height="679" alt="image" src="https://github.com/user-attachments/assets/da6ed2ad-438d-4240-85de-9122c196a037" />
<img width="1364" height="1084" alt="image" src="https://github.com/user-attachments/assets/4cfb636b-e1b3-403f-ac66-0e9f1f0570cf" />

Here is the total thing assembled, with the switch I plan on using (disclaimer, i obviously did not make the model)

<img width="1468" height="957" alt="image" src="https://github.com/user-attachments/assets/1bc31ec0-a9ba-4361-8031-5e231b04e368" />
<img width="1017" height="850" alt="image" src="https://github.com/user-attachments/assets/6fa8e457-b136-4d6f-84cf-57c0568a13ca" />
<img width="1474" height="881" alt="image" src="https://github.com/user-attachments/assets/5aa761ac-0d10-4b79-9f51-6a406b4247c8" />
<img width="993" height="1043" alt="image" src="https://github.com/user-attachments/assets/d0bd4b32-1126-4fdc-bda2-6b7616d54498" />

PS I hate freecad 😄

8 hours

### 7-17-25: Magnets
I selected the magnet. Specifically, I spent a lot of time trying to calculate the force that a potential magnet combo would produce and comparing that to what I measured my exisitng ones taking. 

Measuring the existing ones proved difficult but I get about 10N at tip. The chosen magnet pair is about the same at 13 or so, which is fine considering the application. Like always, there are always tweaks to bring down this value in case it is truly too much.

I won't post a photo for this one. It's a circular metallic disk bro.

Alos, in case you wondered why magnets if you aren't using hall sensor: it's to provide that click feedback.

I imported all the cad files into an assembly but have yet to actually put them together

2 hours

### 7-18-25: Holes
Finalized rotary encoder holes, holes for shifter mounting, holes for PCB mounting.

Positioned the things in the assembly, the shifter was completely impossible to move for some reason. I only did one ok. I picked my battles with freecad and this was not one of them.

<img width="1552" height="873" alt="image" src="https://github.com/user-attachments/assets/0f4f29e3-0203-4e36-90ab-281c8041a1c4" />
<img width="1564" height="906" alt="image" src="https://github.com/user-attachments/assets/1a044558-7000-4030-a385-d04d9c10a199" />

Here are some completed assembly photos.

<img width="1108" height="1024" alt="image" src="https://github.com/user-attachments/assets/d27f2dcf-6455-4e97-82c6-abc20a692051" />
<img width="1292" height="1091" alt="image" src="https://github.com/user-attachments/assets/7b1e9318-486d-47cc-82b9-05e9d19cfc39" />
<img width="1142" height="1073" alt="image" src="https://github.com/user-attachments/assets/b0d74d3a-9063-4b0d-a35d-2a8935366a4b" />
<img width="1069" height="1029" alt="image" src="https://github.com/user-attachments/assets/aecf399d-bbba-4645-9009-d27d231d1f30" />

3 Hours

### 7-19-25: Submission preparation
Looking at the doubtlessly substaintial CNC cost, paired with the expensive (but unfortunately required) adapter, the budget for PCB/A is looking grim.

Since there are limited small packages, I am going to go without PCBA and opt instead to hotplate this stuff on. (with equipment purchased by hackclub... thanks guys 😅)


### 7-29-25: Return

I came back

Prepare BOM for CNC, PCB, parts, buttons, and adapater.


