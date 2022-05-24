---
title: "My Experience Interning at Silicon Labs - Summer 2017"
categories: ["Experiences"]
tags: ["internship", "silabs", "summer", "firmware", "digradio", "radio", "austin-tx", "programming", "job"]
description: "Summary of time spent in ATX interning."
cover: images/cover.jpg
date: 2017-09-02 00:00:00
aliases:
    - /2017/09/02/interning-at-silicon-labs/
---

Well, thus concludes my summer at Silicon Labs, and boy was it great. From learning about digital radio technology, to learning about bootloaders and embedded system design, to working with a dynamic group of developers, to going to the CEO's house for dinner, to multiple company dinners at fabulous restaurants in downtown Austin, I really enjoyed the experience and learned a lot. I am writing this post to document my memories, plus to shed some light on interning at Silicon Labs for future college students that might be applying there.

{{< image 
    caption="Austin interns giving back to the community"
    src="images/silabs-gives-back-04.jpg"
>}}

_All the words and their interpreted meaning in this post are my personal ideas, thoughts and experiences, so don't think that I speak for Silicon Labs as a company. These are just MY opinions._

## But first, Silicon WHO?
**Getting to know Silicon Labs...**

{{< image 
    caption="Silicon Labs Logo"
    src="images/silicon-labs-logo.png"
>}}

For those of you that don't know (don't worry, I didn't know either before applying), Silicon Labs is a company of about 1,300 employees based in Austin, TX, but with offices all around the globe. In their own words, they are "_a leading provider of silicon, software and solutions for a smarter, more connected world."_ Basically, that means they are an electrical engineering company that makes little chips that go into devices all around the world. Chances are you own a device that has a Silicon Labs chip in it (more on that later). They also offer software to go along with these chips to help developers shorten their time to market.

Great! So what kind of chips are we talkin'? Salt and vinegar? ...No no no, silly, these chips are highly integrated circuits made out of silicon. To understand Silicon Labs' spot in the market, it helps to understand the multiple divisions within the company and the different types of products in each division:

*   [IoT](https://www.silabs.com/solutions/iot) - [MCUs](https://www.silabs.com/products/mcu), [Software](https://www.silabs.com/products/development-tools/software/simplicity-studio) and [Sensors](https://www.silabs.com/products/sensors), and [Wireless Connectivity](https://www.silabs.com/products/wireless)
*   Broadcast - [Radio](https://www.silabs.com/products/audio-and-radio) / [TV tuners](https://www.silabs.com/products/tv-and-video) and [Automotive](https://www.silabs.com/solutions/automotive)
*   Infrastructure - [Timing](https://www.silabs.com/products/timing) and [Isolation](https://www.silabs.com/products/isolation)
*   Access - [Power of Ethernet (PoE)](https://www.silabs.com/products/power-over-ethernet), [SLIC](https://www.silabs.com/products/voice) and [Modem](https://www.silabs.com/products/modems-and-daas)

As you can see, Silicon Labs has a very diverse product portfolio that targets many different market sectors. The end customers of these products are usually not average consumers because Silicon Labs' products go _inside_ the things consumers buy. Silicon Labs' solutions are in products from the market leaders in home automation, electric vehicles, green technology, smart TVs and home voice control automation.

#### Applications of Silicon Labs' products

Inside the Austin headquarters, Silicon Labs has many different display cases that showcase end products using their products.

{{< image 
    caption="Products made from industrial solutions"
    src="images/silabs-customers-02.jpg"
>}}

For more information about the company, check out their recent [corporate presentation](https://www.silabs.com/documents/public/presentations/Silicon-Labs-Corporate-Presentation-April-2017.pdf)!

## What'd Ya Do?
**Looking into my summer projects...**

I worked under the Infrastructure and Automotive (I&A) group on the Digital Radio team, with my official title being "Firmware Engineer Intern". Basically, this group develops the firmware for the [Automotive Digital Radio product line](https://www.silabs.com/products/audio-and-radio/automotive-digital-radios). While radio has existed for quite some time, radio technology and standards are still progressing, enabling "smarter" and more complex radios. Silicon Labs has a comprehensive product line for many different radio applications with varying feature sets.

For those interested, all the embedded firmware I wrote this summer was in C or C++, scripting was done in [Tcl](https://www.tcl.tk/), and visualization was written in Java as an Eclipse plugin.

{{< image 
    caption="An evaluation board like the one I used to develop and test firmware modifications and additions"
    src="images/evb-board.jpg"
>}}

### Task One: Getting Familiar With The Project Code

My very first task was to implement a simple customer request: the ability to independently mute different audio paths within Si468x demodulator devices. This relatively trivial task involved understanding the basic architecture of the system (namely the integration of the DSP co-processor for audio processing), thus it forced me to explore the project code base. I implemented the feature quicker than expected, and moved onto my next task.

### Task Two: Monitoring System Behavior

Embedded devices can be challenging to debug, and the family of devices in the Automotive Digital Radio are no exception. The team told me that some firmware crashes take weeks to debug, consuming valuable time. They needed a better way to monitor the system.

I was tasked with creating a Monitoring Module, a conditionally-compiled module that could track system resources and events. They envisioned two major use cases for such a module: 
- Monitoring thread stack sizes to conserve precious memory by shrinking static buffers
- Recording system events (system tracing) for post-mortem analysis of firmware crashes as well as inspection of system performance

I created a monitoring framework that included thread monitoring and system [tracing](https://en.wikipedia.org/wiki/Tracing_(software) functionality. Thread monitoring is used for stack utilization analysis and other thread related data, while the tracing framework can enable tracing, filter event types, pull event logs off devices, format events into a nice CSV file, and view them with a custom viewer to graphically see how the system was behaving during the trace. Basically, the tracing module is just like [LTTng](https://lttng.org/), except designed for embedded digital radio firmware, not Linux.

{{< image 
    caption="Sample view from the GUI I made to visualize system events"
    src="images/tracecompass_ui.PNG"
>}}

Overall, the team was very happy with my Monitoring Module, since it worked well and enabled debugging capabilities previously not available.

### Task Three: Adding Error Detection to Internal Data Path

Silicon Labs offers a device they call the Digital Falcon:

>The Digital Falcon (Si469x) family of digital radio coprocessors provides channel demodulation and source decoding of HD Radio and DAB/DAB+ digital signals delivering audio and data.  Digital Falcon coprocessors simplify system design and minimize the bill of materials (BOM) by eliminating the need for an external RAM memory module for channel decoding typically required by third-party digital radio processors. The Digital Falcon family enables designs to scale from low- to high-end systems with its seamless blending capabilities for DAB/DAB+, as well as its support for Automatic Level and Time Alignment (ALTA) for HD systems.

Internally, this device reuses existing silicon from other less complex devices (like the Si4680) to create a [multi-chip module](https://en.wikipedia.org/wiki/Multi-chip_module). This packaged device is then treated as if it were any other chip, with normal communication and interfacing.

The multiple chips internally need to communicate with each other to share the processing workload, so interconnects exist that the firmware can utilize to send data internally. Since the interconnects go between dies, there is a possibility of transmission error. If the data being sent is critical (like a pointer to memory), an invalid bit flip in transmission could be detrimental to the system. Therefore, error detection is used to help detect if such a condition happens so that the receiving application can decide what further action is needed (throw away packet, process as usual, etc).

I implemented [CRC](https://en.wikipedia.org/wiki/Cyclic_redundancy_check) onto the transmitter and receiver modules to provide valid / invalid signals to the application.

### (Micro) Task Four: Implementing an API Feature

My fourth task was to complete an API feature to allow manual override of the gain applied to audio from a HD source on the AM/FM HD devices. This was trivial to implement, so it was a quick and easy check off the list.

### Task Five: Optimizing Seamless Linker's Delay Subsystem Memory Allocation ([Si4692](https://www.silabs.com/documents/public/data-shorts/Si4692-short.pdf))

First off, let me just say that this was definitely the most fun and challenging of the my summer tasks. Of the diverse products in the Automotive Digital Radio product line, this task involves the Si469x devices. Silicon Labs describes the Si4692 as a "High-Performance Digital Radio Baseband Processor with Seamless Blending."

To understand how my work fits into the system, I will first explain the point of the device. Modern radio standards enable radio stations to broadcast the same content on multiple different frequencies and channels to enable better reception. A car radio with an Si4692 chip is normally tuned to these multiple different frequencies and channels and is constantly correlating the different audio sources to align them in time and level (loudness). This enables the radio to seamlessly switch between audio sources when signal quality reduces. **Basically, this means that when you drive behind a building in a crowded downtown, the music playing on your car radio stays crisp and clear because the radio is seamlessly switching between sources, all without you even knowing!** Pretty cool, if you ask me!

{{< image 
    caption="Block diagram for the Si4692, from www.silabs.com"
    src="images/si4692-block-diagram.jpg"
>}}

Aligning audio signals requires the ability to buffer an audio stream until the desired time offset is achieved. This is fairly straight-forward to implement: a queue data structure can be used to hold audio data chunks until they are needed; simply append the new audio chunks to the end and pull delayed audio chunks off the front. The more interesting part is dynamically allocating memory when audio chunks come in, as the module doesn't know the desired delay (i.e. the total amount of memory needed). To complicate matters, audio data chunks vary in size between channels, and channels can dynamically change their audio block size. These facts impose a few constraints on allocator design:
- Needs to be deterministic, as this is running in a real-time system with hard timing constraints
- Cannot have fragmentation (due to deterministic nature above)
- Must support a max of ~10 different block sizes at one time
- Maximize memory utilization, thus maximizing total possible correlation offset between channels

Typical memory allocation schemes aren't well suited for these unique constraints, so I decided to implement a pool-based allocation scheme. This would handle the fragmentation issues, as well as maximize memory utilization since pools can be setup to minimize overhead per block size.

{{< image 
    caption="Diagram of improved memory layout for pool-based allocation"
    src="images/memory-diagram.png"
>}}

I coded up the allocator, subdividing the total memory region into "pages," with each page divided into unique block-sized chunks. Requests for memory blocks are then fulfilled from existing pages. If an appropriate page doesn't exist to satisfy the request, the block is allocated in the "scratch" memory area, and a thread is kicked off to go and divide up more memory to make a new page.

**Looking at performance improvements, my allocation scheme improved the total delay limit by over 2x, enabling the device to meet the promised specifications.**

### Task Six: Debugging Why Firmware Failed Testcase

Next, I was tasked to debug the bug de jour, an automated testcase failing on an HD audio device. The FM audio baseband layout is fairly complex, with different frequencies being used for analog mono audio, analog stereo audio, digital HD audio, and various data streams. To learn about Frequency Modulation (FM) and Silicon Labs' devices, read this [paper](https://www.silabs.com/Marcom%20Documents/Resources/FMTutorial.pdf).

{{< image 
    caption="FM baseband spectrum division"
    src="images/FM-spectrum.png"
>}}

In areas of poor reception, usually the radio switches to analog mono audio as the most power is dedicated to it. When reception improves and digital audio is available, the part seamlessly switches back to digital. For the transition to be as seamless as possible, the digital audio has to be massaged to sound like the (mono) analog audio. One aspect of this is stereo separation, which can be set to anything between full stereo and mono which helps make the digital audio sound similar to the analog audio.

The test case simply reported that setting the separation to full mono didn't work; it was still fairly stereo. I reviewed the relevant code and proved that the firmware was implementing the model correctly by looking at the internal variable values. To add to my detective work, when I listened to OTA radio at my desk, I couldn't replicate the issue. Seemed fishy.

In the end, I determined that the test simply wasn't waiting long enough after setting the stereo separation value for the audio outputs to settle to the desired separation. The bug was in the test code, not the firmware. I provided some solutions for the test code to implement and marked the bug as solved. Easy peasy. :)

### Task Seven: Fixing Design Flaw in Code

One aspect that is needed to enable Automatic Level and Time Alignment (ALTA) in HD radio devices is the ability to make a measurement of the "loudness" of various audio signals. This is implemented using a standard loudness measuring technique, [ITU-R BS.1770](https://www.itu.int/dms_pub/itu-r/opb/rep/R-REP-BS.2054-4-2014-PDF-E.pdf). For this to work, it needs to know the "power" of the audio sample. The system has a mean-squared measurement module to do just that, but there was an obscure design flaw in the code that resulted in the wrong output for certain corner-case inputs.

I implemented a simple solution to fix this problem that only increased cycle usage by 20% for this calculation.

## Why'd Ya Take the Internship?
**Diving into how I ended up at Silicon Labs**

First, it might help to know a little background on me. When I accepted this internship, it was the middle of the spring semester of my sophomore year of college, at the University of Wisconsin - Madison. I was studying computer science, but was in some EE classes that spring, just for fun. They really interested me, so I was considering pursuing a major in EE as well as CS, but I wasn't really sure.

I applied for a couple summer internships and got offers for two: one in Silicon Valley (option to work remote in Madison) at a stealth startup doing web security type things, and one in Austin, TX at Silicon Labs. I was working at the computer science department on campus at the time doing web development for them, so working for the startup would have basically furthered my knowledge of web development. Accepting the Silicon Labs internship would expose me to low-level embedded development, something that I didn't have as much experience in, but was very interested in (I had played with Arduinos for quite some time before this, see my [BCLD project](http://nathanpetersen.com/2015/07/05/bluetooth-controlled-led-display/)).

Basically, if I took the stealth startup position, it would be relatively same old same old, but if I took the Silicon Labs position, it would be off into the unknown to discover if furthering my education into EE type things was want I wanted to do. Obviously, I chose the latter.

Am I happy with my choice? Absolutely.


## What's It Like There?
**Describing Silicon Labs' culture and living in Austin, TX in general**

{{< image 
    caption="View from my office window"
    src="images/office-views-03.jpg"
>}}

First of all, Silicon Labs is an awesome company and working for them is amazing. Plastered on every conference room wall and practically required memorization for all new employees are Silicon Labs' values:

*   We hire, foster and empower great talent.  
*   We create customer value and commercial success through innovation and simplicity.  
*   We meet our commitments and hold ourselves accountable.  
*   We do the right thing.  

When I first saw these company values, I thought "bleh, corporate bullshit," and while that may be true, my internship experience didn't feel like that. I think there is something to be said for having values and advertising them like Silicon Labs does. It really does create a culture that follows them, simply because corporate makes such a big deal out of them and each and every employee knows them and can probably recite them by heart. Just from my perspective as an intern for a summer, I can attest that all of them are followed: their employees are the best of the best, their products are cutting edge and make building end-user products much simpler, people do what they say they'll do, and they don't take shortcuts. Everything they do is high quality and very customer friendly, from the parts themselves to the datasheets to the customer support. It's all top notch. I can't give them enough compliments.

{{< image 
    caption="Company social at the CEO's house"
    src="images/tyson-party-01.jpg"
>}}

{{< image 
    caption="Interns chatting"
    src="images/tyson-party-02.jpg"
>}}

The perks are also great:

*   Offices in the middle of downtown Austin
*   Great views right over Lady Bird Lake
*   Relaxed work environment
*   Free snacks and drinks (including free beer!)
*   Wonderful coffee/espresso machine for making fancy pick-me-ups
*   Employee training and talks about cutting edge research  

As far as living in Austin goes, I can't really complain. The weather is definitely hot during the summer, but 75 on Christmas or Thanksgiving?! How can you beat that? Traffic is a nuisance, but my commute was usually less than 30 minutes each way (max of 55 minutes). The food is absolutely outstanding, with places like Terry Black's and Rudy's for BBQ and Chuy's for Tex-Mex. Overall, I would recommend living in Austin. Great city.

{{< image 
    caption="Early morning sunrise view of downtown Austin from my commute to work"
    src="images/downtown-sunrise-01.jpg"
>}}

{{< image 
    caption="Sitting in traffic... 60 minute commutes are a little too much"
    src="images/atx-traffic-04.jpg"
>}}

{{< image 
    caption="Another early morning view of downtown Austin from my commute to work, on a less than sunny morning"
    src="images/downtown-cloudy.jpg"
>}}


## Fun Statistics
**For people like me who like numbers too much...**

I'm addicted to numbers and stats, so during the summer:

| Description | Amount |
|----------------------------------------------|-----------|
| Lines of code written | ~10k |
| Lines of code in project | ~800k |
| Miles driven | ~3050 miles |
| Miles driven per day | ~45 miles |
| Working days | 55 |
| Time spent commuting | ~55 hours |
| Workouts | 45 |



## Fun Office Perks
**Snacks and beer... mmm...**

Couldn't not put in some pictures of the great office perks. Great place to work! :)

{{< image 
    caption="Daily fresh fruit"
    src="images/office-perks-01.jpg"
>}}

{{< image 
    caption="Array of healthy snack bars"
    src="images/office-perks-02.jpg"
>}}

{{< image 
    caption="Nice cold drinks"
    src="images/office-perks-03.jpg"
>}}

{{< image 
    caption="Free beer all the time!"
    src="images/office-perks-05.jpg"
>}}

## Summary

Overall, I'm **extremely** happy I took this internship. It was definitely career directing and I can't wait for possibly returning next summer for a repeat internship, or after school for a job. Highly recommend the experience!!! :)

{{< image 
    caption="Me presenting at the intern symposium"
    src="images/nathan.jpg"
>}}

---

#### New Post Notifications

If you read this far into this article, you might be interested in subscribing for email notifications about future new posts I write. I will never spam you! Every few months, I write a new article for this website, and will send you email about it. You can unsubscribe at any time. Thank you.

{{< mailchimpform >}}

[office-perks-01]: images/office-perks-01.jpg
[office-perks-02]: images/office-perks-02.jpg
[office-perks-03]: images/office-perks-03.jpg
[office-perks-05]: images/office-perks-05.jpg
[tyson-party-01]: images/tyson-party-01.jpg
[tyson-party-02]: images/tyson-party-02.jpg
[atx-traffic-04]: images/atx-traffic-04.jpg
[office-views-03]: images/office-views-03.jpg
[downtown-sunrise-01]: images/downtown-sunrise-01.jpg
[downtown-cloudy]: images/downtown-cloudy.jpg
[silabs-gives-back-04]: images/silabs-gives-back-04.jpg
[silicon-labs-logo]: images/silicon-labs-logo.png
[silabs-customers-02]: images/silabs-customers-02.jpg
[evb-board]: images/evb-board.jpg
[tracecompass_ui]: images/tracecompass_ui.PNG
[si4692-block-diagram]: images/si4692-block-diagram.jpg
[memory-diagram]: images/memory-diagram.png
[FM-spectrum]: images/FM-spectrum.png
[nathan]: images/nathan.jpg