---
title: "Razzler: POV LED Top"
categories: ["Projects"]
tags: ["hardware", "electronics", "design", "firmware", "leds", "display", "animations", "pcb", "mcu", "eagle", "stm32", "pov", "imu", "mpu-6050", "rtos", "cnc"]
description: "Custom designed persistence of vision toy top to display animations while spinning."
cover: "images/cover.jpg"
date: 2018-05-15 15:29:00
---

## Background

![Razzler: the Persistence of Vision LED Top][pic-09]

During the summer of 2017, I started working on my first real electronics project. This project marks the first time I have ever seen an electronics project all the way through, from start to finish: initial idea, schematic design, PCB layout, manufacturing, board assembly, testing, device driver development, and full firmware creation.

I love LEDs and interactive toys, so I decided that I wanted to build a top that people could spin to entertain themselves. The PCB would be circular and consist of a single row of LEDs, LED driver, accelerometer / gyroscope and microcontroller (MCU). Having these components would enable the MCU to detect angular velocity while spinning to display persistence of vision (POV) animations on the LED row. I decided to name the project Razzler because blinky LEDs always seem to razzle dazzle. ;)

Since I started this project in the summer of 2017, I figured I'd have working devices by Christmas to give to family and friends as gifts. Little did I know how tall the mountain was that I needed to climb to complete the project... Read on to learn about my journey!

### Timeline

**When I started, I remember thinking that there was an unknown number of unknowns.** I really had no idea the questions to ask, or even the problems that I needed to solve. Little by little, I progressed forward until completion! It only took me seven months...

- 2017 Jul: Idea conception
- 2017 Aug: Schematic design started
- 2017 Nov: PCB v1 ordered
- 2017 Dec: Device driver firmware working
- 2018 Jan: "Final" firmware finished

## Hardware

### Using Eagle EDA

To design and draw the device schematic, I needed an EDA. Eagle is a common hobbyist/student EDA that has a rich history with many examples and tutorials. The software had recently been acquired by Autodesk, so it was regularly receiving major updates (SPICE / Fusion 360 integration, etc). I chose Eagle for these compelling reasons.

![Autodesk Eagle EDA][eagle]

When first starting out, a big hurdle of electronics design is choosing parts and modeling them in your EDA. As a board designer, one should be able to use any part simply by looking at its datasheet. This means creating a symbol for schematic usage and associated footprint for PCB layout. This process only needs to be done once; future projects can use the same library.

Vendors like [DigiKey](https://www.digikey.com/) reduce part cost when purchased in high quantities (1000+). For small quantities, I quickly realized that buying parts from online Chinese stores (like [AliExpress](https://www.aliexpress.com/)) was *much* more economical since they buy in bulk, but had long ship times and limited component selection. I placed many orders for seemly random parts that might be useful for projects: batteries, battery holders, MCUs, LEDs, resistors, capacitors, common ICs, etc.

Weeks after placing these orders, packages started arriving. Upon delivery, I modeled the component in Eagle for later use. Since packages arrive on different dates, this made component entry very manageable. I now have a library of solely parts that I own, so I know I can use any of them in a future project without issue. 


### Schematic Design

Creating a circuit schematic can prove to be a challenging part of design because one must be aware of how parts interact. It can be very frustrating to get PCBs back just to discover excess power supply noise or misconnected device pins. Before a part is used and a support circuit is proven to work, one never really knows if a circuit will work; simulation and analysis only go so far.

As a beginner, I didn't know much. I struggled with determining correct decoupling capacitor requirements, what to do with no-connect (NC) pins, and reading datasheets. I also didn't know how many components could actually fit on my desired PCB size; this eventually determined between RGB LEDs and single-colored LEDs. I designed the original schematic using RGB, but after attempting to layout the PCB, I switched to single-color LEDs.

I think the biggest lesson learned at this step is to look online for examples of component use. Many device datasheets have application circuits that can be used. Also, make sure schematic design is finalized before PCB layout. Altering trivial things on the circuit schematic can undo hours of previous PCB layout work.

![Finalized schematic for Razzler project][sch-full]

View PDF version of schematic: [click here][razzler-pdf-REV20171104A].


#### Circuit Modules

##### Microcontroller

The STM32 line of MCUs was chosen for this project; specifically the [STM32F030K6](http://www.st.com/en/microcontrollers/stm32f030k6.html). This is basically the cheapest, lowest performance 32-bit MCU ST makes. Since the Razzler's firmware doesn't need to do much, this choice made sense.

![Microcontroller circuitry][sch-mcu]

To interface with the MCU, a simple four pin header is provided which provides access to the SWD debugging signals. An external module is then needed to do the programming.

![Simple SWD debugging interface][sch-debug]

##### LED Driver / LEDs

To control 16 LEDs with minimal GPIO pins, a 16-channel constant current LED driver was chosen for this project; specifically the [MBI5024](http://belchip.by/sitedocs/00006155.pdf). This is a Chinese clone (even pin compatible) of TI's LED driver that was found on AliExpress. To control it, the MCU sends a simple serial data stream that encodes which LED outputs are on / off.

![16-channel constant current LED driver][sch-mbi5024]
![16 LEDs][sch-leds]

##### Accelerometer / Gyroscope

Motion is detected via the [MPU-6050](https://www.invensense.com/products/motion-tracking/6-axis/mpu-6050/) accelerometer / gyroscope IC. Since this IC is fairly old, it is easy to find from distributors in China. It records linear and angular accelerations in the X, Y, and Z directions. These samples can be queued in its 1024 byte FIFO, and later drained by the host controller. The MPU-6050 communicates via the I2C protocol.

![Accelerometer / gyroscope motion sensor][sch-mpu6050]

##### Power

To keep the PCB balanced for appropriate spinning, two CR2032 batteries are used. A master power switch allows batteries to be permanently installed and a main power LED indicates when the device is switched on.

![Battery and switch for master power][sch-power]

---

### PCB Layout

After schematic design completion, I moved onto PCB layout. Since I included footprints when I modeled my components, I just needed to arrange the parts and route traces to implement the schematic.

I didn't know what to set for layer stack-up or design rules. Turns out, these are answerable questions after selecting a board manufacturer. Research pointed me to low-cost Chinese fabs; I chose Seeed Studio ([read my review of their service](/2018/02/14/pcbs-from-seeed-studio/)). To qualify for their cheapest service, I used a two layer stack-up with 6/6 mil clearances.

![10x PCBs from Seeed Studio arrive neatly packaged][seeed-package]

Since this board was going to be spun as a top, it needed to be balanced. This affected component placement. For example, two batteries were used to keep the center of gravity about the center point. The row of LEDs was offset slightly so that they meshed together every 180 degrees. Little concern was placed on accel/gyro placement in terms of radius from center (turned out to be a big mistake).

Laying out the PCB was a very time consuming task. It wasn't that hard, but with a practically unlimited number of choices, it was easy to spend countless nights perfecting everything. I wanted the routing to be pretty, so I aligned every trace to optimize space utilization.

To speed up layout, it's best to start with important interface component placement. For this project, that meant locking in the positions of LEDs, batteries, mounting holes, switches, buttons, and programming headers. Next, look at the rats nest of wires and place other parts to optimize routing traces. Plan the power signals (ground/power planes, etc). Route sensitive signals first, then route power signals. The whole process is very iterative; just keep chugging and eventually the layout will pass the design rule check (DRC) and be ready for production.

![PCB top view][pcb-top]
![PCB bottom view][pcb-bot]

### PCB Assembly

I uploaded the gerber files to Seeed Studio and submitted my order. Once a few weeks passed, a pack of 10 boards showed up and were ready to assemble.

![Shrink wrapped pack of boards delivered from China][seeed-shrinkwrap]

To assemble the boards, I manually apply solder paste from a tube and then place all the SMD parts using tweezers. 0603 packages were prevalent in the design but proved to be fairly easy to solder. Since I haven't built/bought a DIY reflow oven, I used a hot air reflow station to manually attach each component separately.

From start to finish, the board took about one hour to assemble.

![Workspace in which board was assembled][assem-workspace]

![Final assembled PCB right off the assembly line :)][assem-clamp]

#### Seeed Studio PCB Assembly

While I manually assembled my board for this project, Seeed Studio offers a great [PCB assembly service](https://www.seeedstudio.com/prototype-pcb-assembly.html). They also offer a nice free library of parts ([Open Parts Library](https://www.seeedstudio.com/opl.html)) which supports Eagle directly. If you design with this library and have them assemble your boards, many of the OPL parts are actually free! This can be a great option for batch board runs.

I currently have a project in the works to take advantage of Seeed Studio's PCBA service using their OPL. Check back later for an article about my experience!

### Wooden Frame Creation

The goal of the project was to create a spinning top based around a PCB. I wanted a simple, yet attractive frame around the PCB to add the structure of the top. My father has a CNC machine at home in the garage, so I worked with him to design a simple two-part assembly on which to affix the PCB.

![CNC machine setup and ready to cut out the bottom piece of the assembly][wood-cnc]

![Top (right) and bottom (left) pieces, after sanding][pic-08]



## Firmware

When programming simple hobby embedded devices, typically little thought is given to overall system design. Many times, Arduino-like semantics are employed (setup and loop routines). The problem with this architecture is lack of fine-grained control for real-time events. For example, if many distinct tasks need to run at different rates on the MCU, a loop approach can become cumbersome.

Implementing a very basic scheduler can make firmware much more modular and easier to extend with new functionality. Certain system designs guarantee properties that may be important for specific applications, like deterministic execution or exact update frequency.

### System Architecture

Razzler's firmware is **task based**, with a fixed time slice duration (10ms) that the duration of all module tasks together must fit within. Modules register their tasks with the scheduler during start-up and then are called at their requested frequency (up to 100Hz per task). This allows many tasks to run "concurrently," at independent intervals.

Modular use of the accelerometer / gyroscope data is accomplished using the Digital Motion Processor (DMP) module. This module registers itself with the main scheduler, but also allows other modules to register DMP tasks with it. A **DMP task** is sample rate dependent, as opposed to real time dependent. DMP tasks can be evaluated using bursts of samples from a buffer, given that the samples stored in the buffer were taken at the requested sample rate. Razzler supports a maximum sample rate of 200Hz from the accelerometer / gyroscope.

Since the constant current LED driver simply turns each LED on/off, the illusion of variable LED brightnesses requires pulse-width modulating (PWM) each LED's on-time. To prevent visible flickering, this modulating needs to happen at a high frequency (60Hz or more) and without interruptions. While this could be accomplished via the main scheduler, the Razzler uses a dedicated hardware timer interrupt to perform LED updates completely asynchronously. This naturally pauses other system activity, but the interrupt is very short so no noticeable delay is introduced.

### General Operation

Razzler's firmware is constantly running multiple independent modules to realize device functionality. Independent module operation is described below, as well as how modules interact with each other.

#### `MBI5024` Module

Completely asynchronous to all tasks, the `MBI5024` module sends virtual LED brightness levels (off, low, medium, high, on) to the MBI5024 IC via a serial data stream, which creates the variable brightness levels.

#### `Animator` Module

Registered to run at 50Hz, the `Animator` module calculates each LED's brightness level according to predefined animation patterns. It then updates the LED output buffer virtual brightnesses in the `MBI5024` module. The `Animator` module also queries the `Motion` module to determine if the device is spinning or not. If it's spinning, it plays spinning animations, otherwise it plays static animations (spinning animations don't use PWM, static animations do). The `Animator` module also exposes an API to other modules to disable/enable animation updates.

#### `Digital Motion Processing (DMP)` Module

Registered to run at 100Hz, the `DMP` module interacts with the MPU-6050 accelerometer / gyroscope. The MPU-6050 is configured to record samples at 200Hz and store them in its internal buffer. The `DMP` module then drains the buffer every 100Hz, which ends up pulling two sets of samples (a sample is six 16-bit numbers: linear / angular acceleration in X / Y / Z directions) per run. For each sample, the `DMP` module checks to see if any registered child DMP tasks needs to run. If so, it passes off the sample for processing.

![MPU-6050 internal block diagram][mpu-6050-diagram]

#### `Motion` Module

The `Motion` module is responsible for two things: knowing if the device is spinning, and recording how long the device is motionless. It uses one real-time dependent task and two sample rate dependent DMP tasks to realize this functionality.

The DMP task for spinning recognition is registered at 200 samples per second (SPS), while the stillness recognition DMP task is registered at 10 SPS. The high sample rate for the spinning DMP task is necessary because the `Animator` module must switch from static to spinning animations with very little latency when the top starts spinning.

During each DMP task execution, the supplied sample is simply copied to a local buffer for later reference. This means that the main DMP task registered with the main scheduler runs very quickly.

For counting time that the device is still, a real-time task is run at 10Hz which looks at the local sample buffer and calculates maximum deltas across acceleration samples. If these maximum deltas are less than some threshold, it knows the device has been still for the past 100ms.

When another module wants to know if the device is spinning, they run a `Motion` module API which compares the past few samples' centripetal accelerations to some threshold. If they are all over that threshold, the device is spinning. This means that the actual determination of spinning is done within the caller's context.

#### `Power` Module

Registered to run at 10Hz, this module checks if the total still time is over some threshold (120 seconds). If so, it disables animations and puts the device into a low power state. It then waits until the `Motion` module reports that the device is spinning, to which it responds by restoring normal operation.


## Mistakes

**There's always room for improvement**

Growing up, I remember my dad saying something along the lines of __"success can always be achieved if you redefine what it means to be successful."__ I feel that this describes my relationship with this project. I set out to do one thing, but ended up doing another very similar thing that I still classify as success. While my first attempt at a POV spinning top was mostly a success, a couple things didn't quite go to plan.

### Avoidable Errors

The first issue should have been easily avoidable: _I wired the I2C interface for the MPU-6050 to GPIO pins that didn't support hardware I2C_. The firmware could still control these pins as regular GPIO, but I needed to implement the I2C protocol in software. After much studying of the protocol and some sample code, I managed to communicate with the inertial sensor. Since the processor is directly doing the bit banging (as opposed to using the DMA engine), there is less time for other tasks in the system. Overall, this issue was fairly easy to overcome.

![Debugging serial interface problems; clk signal looks good][osc-clk]

![Adjusting slew rate of GPIO pins to meet requirements for serial interface][osc-clk-edge]

### Design Flaws

The other issue was a more fundamental design flaw: _I never did any physics to calculate if the inertial sensor was capable of sensing the high angular rate at which the top would spin._ I originally intended to use the data from the accelerometer / gyroscope to determine how fast the top was spinning. If the display algorithm knew the precise angular velocity, it could lock the POV animations to the spinning rate, creating a still image.

After I created the top, I took some short exposure pictures of it spinning to try to get an estimate of peak angular velocity. On the computer, I measured the angle that it rotated while the shutter was open, which I then extrapolated to estimate a peak angular velocity of slightly less than 1,500 rotations per minute (RPM). If the inertial sensor could measure 1,500 RPM, my initial plan would work.

#### Angular Velocity from Gyroscope

Gyroscopes are designed to directly measure angular velocity. A little unit conversion reveals that 1,500 RPM is 9,000 degrees per second (dps). The spec for the MPU-6050 states that the gyroscope can detect a maximum angular rate of 250 dps. 9,000 is much much bigger than 250...

**Conclusion: the gyroscope cannot determine how fast the top spins.**

After a little research, it seems that there is a _very_ limited selection of sensors that can detect +/- 9,000 dps. Analog Devices' [ADXRS649](http://www.analog.com/media/en/technical-documentation/data-sheets/ADXRS649.pdf) would fit the bill, but at a unit cost of ~$92, this sensor must be plated in gold. Not reasonable for my project.

#### Angular Velocity from Accelerometer

As a last resort, I turned to the accelerometer data. Turns out that the circular motion unit from physics class in high school actually is useful for something! The x-axis of the MPU-6050 is directly inline with the PCB center, so conveniently measures centripetal acceleration while spinning. Simply plug and chug into the standard circular motion equation:

{% math %}

a_c = R \cdot \omega^2

{% endmath %}

Letting {% math %} R = 22.86mm {% endmath %} and {% math %} \omega_{max} = 1500 rpm {% endmath %}, then {% math %} a_{c, max} = 564 m/s^2 {% endmath %} or {% math %} 57.5g {% endmath %}. The MPU-6050's accelerometer only supports +/- 16g...

**Conclusion: the accelerometer cannot determine how fast the top spins.**

Fortunately, when the acceleration is above the MPU-6050's limit, the sensor saturates and reports a full scale value (probably to indicate an overflow). The firmware then recognizes this to create a boolean spinning indicator.

![Longer exposure picture of spinning top to show light trails from LED line][pic-12]

#### Spin Direction from Accelerometer

Since using the gyroscope is out of the picture due to high RPMs, I was curious if there was a simple way to determine spin direction (CW / CCW) from the accelerometer data. When the top is initially spun, it must accelerate from rest to its peak angular velocity. The y-axis of the MPU-6050 should be able to measure this; if the acceleration is positive, the top was spun clockwise, if negative then counter-clockwise. If we assume that the spin-up acceleration is constant:

{% math %}

a_{tan} = \frac{d}{dt} (R \omega) = R \frac{\Delta \omega}{\Delta t}

{% endmath %}

Letting {% math %} R = 22.86mm {% endmath %}, {% math %} \Delta \omega_{max} = 1500rpm {% endmath %}, {% math %} \Delta t_{min} = 200 ms {% endmath %}, then {% math %} a_{tan, max} = 1.6g {% endmath %}. This is well within the limit of the MPU-6050, so this method can be used to determine spin direction.


## Razzler v2

For version two, I intend on changing a handful of things:

- Programming headers (through-hole to surface mount)
- I2C pin connections (enable hardware I2C support)
- MPU-6050 placement (reduce radius so max centripetal acceleration is in sensor range)

Check back later for a new article about my experience creating version two!

#### New Post Notifications

If you read this far into this article, you might be interested in subscribing for email notifications about future new posts I write. I will never spam you! Every few months, I write a new article for this website, and will send you email about it. You can unsubscribe at any time. Thank you.

{% mailchimpform %}

## Picture Gallery

![Top side of assembled board][pic-01]
![Bottom side of assembled board][pic-02]
![Board components follow symmetrical design][pic-03]
![Seeed Studio sends a minimum of 10 boards][pic-04]
![Assembled board with top and bottom blank boards][pic-05]
![Blank boards][pic-06]
![Close-up of top side][pic-07]
![Wood frame around board to create top structure][pic-10]
![Side view of top with wooden frame][pic-11]

[pic-01]: /assets/images/razzler/done/pic-01.jpg
[pic-02]: /assets/images/razzler/done/pic-02.jpg
[pic-03]: /assets/images/razzler/done/pic-03.jpg
[pic-04]: /assets/images/razzler/done/pic-04.jpg
[pic-05]: /assets/images/razzler/done/pic-05.jpg
[pic-06]: /assets/images/razzler/done/pic-06.jpg
[pic-07]: /assets/images/razzler/done/pic-07.jpg
[pic-08]: /assets/images/razzler/done/pic-08.jpg
[pic-09]: /assets/images/razzler/done/pic-09.jpg
[pic-10]: /assets/images/razzler/done/pic-10.jpg
[pic-11]: /assets/images/razzler/done/pic-11.jpg
[pic-12]: /assets/images/razzler/done/pic-12.jpg

[assem-clamp]: /assets/images/razzler/assem/clamp.jpg
[assem-workspace]: /assets/images/razzler/assem/workspace.jpg
[osc-clk]: /assets/images/razzler/osc/clk.jpg
[osc-clk-edge]: /assets/images/razzler/osc/clk-edge.jpg
[seeed-package]: /assets/images/razzler/seeed/package.jpg
[seeed-shrinkwrap]: /assets/images/razzler/seeed/shrinkwrap.jpg
[wood-cnc]: /assets/images/razzler/wood/cnc.jpg
[wood-cnc-close]: /assets/images/razzler/wood/cnc-close.jpg

[razzler-pdf-REV20171104A]: /assets/pdf/razzler/razzler-REV20171104A.pdf

[sch-full]: /assets/images/razzler/sch/full.png
[sch-debug]: /assets/images/razzler/sch/debug.png
[sch-leds]: /assets/images/razzler/sch/leds.png
[sch-mbi5024]: /assets/images/razzler/sch/mbi5024.png
[sch-mcu]: /assets/images/razzler/sch/mcu.png
[sch-mpu6050]: /assets/images/razzler/sch/mpu6050.png
[sch-power]: /assets/images/razzler/sch/power.png

[pcb-bot]: /assets/images/razzler/brd/pcb-bot.png
[pcb-top]: /assets/images/razzler/brd/pcb-top.png

[eagle]: /assets/images/razzler/eagle.png
[mpu-6050-diagram]: /assets/images/razzler/mpu-6050-diagram.png