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

### 6-10-25: Design
Now begins the fun part selecting compontents.

I love the RPM indicators on wheel, so I will likely do a row of LEDS on the top. For buttons, I'd like to have light up feedback for stuff like pit limiter. I don't usually use rotary encoders, so I won't use a lot of those.

Addtionally, the on-wheel screens are silly in my opinion. The wheel should be moving way too much to read.

To connect all this, since the built in pins are not usable, a external spring cable will connect USB to a PCB I will design to connect all these buttons. Found a twisted pair version on aliexpress, perfect length and cost.
<img width="237" alt="image" src="https://github.com/user-attachments/assets/89a26144-572d-494e-aa33-ca33ee8e5fe6" />

Going to design the PCB, I quickly realized that I'd have to use neopixel type controlled LEDs, since the typical RGB 5mm bulbs would take 3 pins each


