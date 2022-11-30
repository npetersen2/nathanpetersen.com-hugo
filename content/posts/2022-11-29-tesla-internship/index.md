---
title: 'Tesla Internship Experience, PhD Student Edition, Summer 2022'
categories: ["Experiences"]
tags: ["internship", "job", "summer", "palo-alto-ca", "motor", "motor-control", "simulation", "firmware"]
description: "Thoughts on being an intern on the Tesla motor controls team."
cover: /images/cover.jpg
date: 2022-11-29 00:00:00
math:
    enable: true
---

{{< image
    src="images/img-01.jpg"
    caption="Me in front of the Tesla HQ in sunny California, USA"
>}}

**I was an electric motor control summer intern at Tesla's engineering headquarters in Palo Alto, California.**
It was an awesome adventure! In this article, I describe the key parts of the Tesla internship experience and my thoughts on the whole ordeal.

{{< admonition type=info title="Disclaimer" open=true >}}
These are my *opinions* only. I am not endorsed by Tesla and not speaking on behalf of Tesla.
Please tweet [@Elon Musk](https://twitter.com/elonmusk) for Tesla inside scoops.
{{< /admonition >}}

## Introduction

I am currently a PhD student at the University of Wisconsin–Madison in Electrical & Computer Engineering where I research control techniques for levitated motor systems. In December 2021, about 6 months prior to my Tesla internship, I completed my MS degree in Electrical Engineering. Before diving full-time into my PhD research and having it consume my life, I really wanted to take a small break and do something exciting and fun for the summer—get out of Wisconsin and see the world a bit.

The last time I had a significant break from school was after undergrad, before grad school, when I worked full time for Beta Technologies on motor controls for electric airplanes. I never wrote about that experience on this blog, but boy was it special. Maybe I'll post a retroactive article about it.

{{< image
    src="images/img-02.jpg"
    caption="Tesla sign in front of the engineering HQ offices"
>}}

Tesla, Inc. ([tesla.com](https://www.tesla.com/)) is a company focused mainly on designing, manufacturing, and selling high-end electric vehicles. To do this profitably, they have invested huge amounts of resources into battery technology and fabrication plants. Most notable is the [Tesla Gigafactory](https://www.tesla.com/gigafactory) which is the largest volume battery plant in the world, producing about 20 GWh of batteries each year. Tesla has been rapidly building a global presence with large factories on nearly every continent. As of late 2022, there is substantially more consumer demand for Tesla cars compared to what Tesla can produce, meaning that scaling to higher volume production is the company's short/medium goal.

> Tesla is accelerating the world's transition to sustainable energy with electric cars, solar and integrated renewable energy solutions for homes and businesses.
>
> ---Tesla Website

The interview process to work at Tesla is notoriously difficult. They only hire what they say is the "best talent", and their interview process reflects that. The acceptance rate at Tesla is similar to that of top universities—somewhere around 5%. For my internship, I was contacted by my group's manager through my university involvement at [WEMPEC](https://wempec.wisc.edu/). I had two phone interviews for about an hour each, and a full 24 hour written exam. It was hard. Finally, the good news came: I got an offer letter!

**Internship offer:** :white_check_mark:

## Short-Term Housing

Before I flew out, I needed to arrange a place to stay. Finding short-term housing in the Bay Area is not for the faint of heart. Typical rents are around $1.5k to $2k a month for a room in a shared apartment. It took many hours of looking online, calling people, and nearly getting scammed on Craigslist until I finally found a reasonably priced room ($960/month) from which I could commute to the office. My best luck came from browsing Stanford's private "Craigslist" website, [supost.com](https://supost.com/search/cat/3). Only people with a `@stanford.edu` e-mail can create posts which makes the whole website come off as less prone to scams.

I did not plan on bringing my car, so I was going to be dependent on the public transit system for my commute. Luckily, there was a public bus going straight from the office to the place I found, which made for an easy 30-minute commute. I Venmoed a security deposit to the lady from the room posting, and she agreed to hold the room for me.

**Housing:** :white_check_mark:

I should note that Tesla has an internal company shuttle service which can be very helpful for commuting to work. Unfortunately, it didn't go to my selected housing, so I did not use it much over the internship. If I did it again, I would definitely recommend getting the shuttle routes from HR before selecting the neighborhood of housing. This way, the commute is not an afterthought.

## Arriving in CA

When I arrived in California at the [San Jose airport](https://www.flysanjose.com/), it was a beautiful sunny Saturday afternoon. The Lyft ride from the airport to my apartment was going to cost an arm and a leg, so I figured I would try my luck with the public transportation—I needed to figure it out eventually! The [Clipper card/app](https://www.clippercard.com/) is your friend in the Bay Area. One card to rule all public transit: buses, trams, light rail, trains, subway, everything! Simply scan your phone when you board and never worry about it again. Brilliant!

I ended up spending the whole afternoon taking a bus, train, and Lyft to my new house from the airport. What an adventure! The new sights amazed me: electric cars everywhere, gas costing almost twice what it did in Madison, beautiful plants and greenery abounded. The fresh breeze and sunshine filled my soul with the good CA vibes. I felt happy.

Unfortunately, due to some paperwork issues on both my side and Tesla's side, my first day of work had to be shifted back a week. This meant that I was in sunny CA with no responsibilities and nothing to do for a whole week. Let me tell you, this was a gift from above. The grad school grind leading up to coming to CA was intense, and I was ready for a break. A whole week of forced vacation was just what the doctor ordered!

I spent each day of my first week in CA wandering around by walking and taking the bus/train. I went to Palo Alto, walked around Stanford's campus, and went up to San Francisco. It was awesome to explore and enjoy the new area before starting the internship. I discovered that every day has the same weather: sunny and nice. Only took a few days before I found some sunscreen to protect my poor Wisconsin-conditioned skin from the constant rays.

**Exploring via public transit:** :white_check_mark:

## Starting the Internship

<!--
> TODO: talk about first food in CA, favorite restaurants, pictures.

> TODO: add picture of bus route, or route from airport to my place

> TODO: Add note about summer of worst gas prices, 6.50 vs 4.50 in Wisconsin 

> TODO: add comment on no AC in my room/house but that's ok since it was good weather all summer. Only rained twice!

> TODO: Add info about Tesla incentives for taking public transit to work / bike.
-->

The day finally came to start the internship. The first day involved online orientation (due to COVID-19) at home in the morning. Each Monday, Tesla holds an orientation session for North America which all new hires attend—there were hundreds of people on the call!

{{< image
    src="images/img-04.jpg"
    caption="My favorite spot to sit on the bus to work"
>}}

Finally, in the afternoon, I took the [DB1](https://dumbartonexpress.com/line-db1-schedule/) bus and went to the office. Upon arriving, I met my mentor, picked up my laptop, and got briefed on the summer projects. I was stationed at the [main engineering HQ](https://www.google.com/maps/place/Tesla+HQ/@37.3940974,-122.1523805,17z) in Palo Alto, CA, referred to as Deer Creek due to the building being on said road. The office space used to belong to HP before Tesla moved in.

The engineering HQ at Deer Creek feels like a giant warehouse. The top floor is full of hundreds of sitting/standing desks where engineers work. There is no privacy, no individual cubicles, and only a handful of conference rooms. You get assigned a desk and that becomes home base. The motor controls team had a little area near the side wall, so we could get natural light and enjoy the views of the hills outside.

{{< image
    src="images/img-07.jpg"
    caption="A sea of office desks where the magic happens"
>}}

{{< image
    src="images/img-05.jpg"
    caption="Beautiful view of the sunny CA landscape while getting some office coffee"
>}}

The other two floors of the building are mostly giant lab spaces. For the first couple weeks, I simply could not get over how big the labs were. I managed to get lost every day, until finally I built a mental map of the space. Crazy amounts of equipment, test benches for all the systems in the car, large garage space for technicians to work on cars, room after room of cool things.

I guess it's not surprising that the largest EV maker in the world has huge amounts of resources for R&D, but it took seeing it with my own eyes to believe it. I quickly realized that the company is so large that not one single employee can understand the whole operation. Team collaboration means that you only need to be an expert in your area and know who to pass the ball to for other areas.

{{< image
    src="images/img-08.jpg"
    caption="Outdoor seating can be enjoyed for lunch"
>}}

Over the summer, there were several lunch meetings held online for interns to learn more about the company and fulfill various training sessions. Unfortunately, due to COVID, there were no in-person intern events, so it was challenging to meet people outside my team. That said, I still got to interact with people from many areas of the company: reliability, design, QA, validation, firmware, hardware, software, integration. 

One thing I found very interesting is, when asked about something, how quick Tesla engineers are to say, "I do not know, but I know XYZ team/person would help answer your question." This is the ultimate goal: hundreds of smart people working towards a shared goal, each doing their own specialty. At Tesla, the team is truly more than the sum of the individuals.

## My Projects

For obvious reasons, I cannot say exactly what I did at Tesla. However, I can give a flavor.

As Tesla matures and creates new products, more and more of the products' internal components are designed/created in-house. For many years, the key components of Tesla cars have been in-house designs: motor, inverter, battery, charging, UI interface, etc. This allows for tight integration, designing components to meet very specific specifications, and keeping costs as low as possible. Check out [teardown videos](https://www.youtube.com/watch?v=oVge8I6kxPY) online for interesting views into Tesla's subsystems.

However, not everything is an in-house design.  For example, auxiliary systems like the motor which spins the blower for the HVAC system. A quick search on eBay for "[Tesla HVAC blower](https://www.ebay.com/sch/i.html?_nkw=Tesla+HVAC+blower)" reveals people selling spare parts, and a bit more investigation shows that the motor indeed is from supplier Delta Electronics. There is a similar story for finding suppliers and manufacturers of other low voltage pumps, fans, and compressors in the cars.

Historically, the motor drivers and controls which spin the
[HVAC blower](https://www.ebay.com/sch/i.html?_nkw=tesla+model+3+hvac+blower),
[oil pump](https://www.ebay.com/sch/i.html?_nkw=tesla+model+3+oil+pump),
[water pump](https://www.ebay.com/sch/i.html?_nkw=tesla+model+3+water+pump), and 
[radiator fan](https://www.ebay.com/sch/i.html?_nkw=tesla+model+3+radiator)
have not been designed in-house. However, for upcoming vehicle platforms, these are going to an in-house design.

My internship projects all revolved around sensorless motor control algorithms for these low voltage motors.

{{< image
    src="images/img-11.jpg"
    caption="Getting ready to test motor control code in the wild"
>}}

### Sensorless Field-Oriented Control

To keep costs down, the processor which implements the motor control algorithms for the low voltage pumps and fans is very resource constrained—minimal compute and memory. This poses significant challenges for high-performance motor controls. Specifically, sensorless field-oriented control (FOC) tends to be very compute intensive due to the fast control intervals, e.g., 50-100us.

To overcome this, the industry standard FOC dq current control approach is modified to effectively work the same, but consume significantly less compute time. This was probably the coolest technical part of the internship: understanding this unique way of viewing field-oriented control and its pros and cons.

<!-- These are the Melexis patents that seem to be describing what Tesla was using: -->
<!-- https://patents.google.com/patent/US8461789B2/en -->
<!-- https://patents.google.com/patent/US9906175B2/en -->

### Software-in-Loop Testing

The severely resource-constrained processor meant it was nearly impossible to view real-time signals for debugging control issues. Luckily, extensive software-in-loop (SIL) testing infrastructure was leveraged to gain a deeper understanding of the control behavior. This allowed for debugging stability issues, fixed-point math intricacies, and running motor control regression tests.

### Lab Testing

Software simulations can only go so far when it comes to controls validation, even with a near identical digital twin. Lab testing is required to prove the solution is valid.

Often, motor control is validated using a dynamometer, or dyno for short. A dyno involves two electric machines back-to-back. One machine is the "device under test" (DUT) while the other is the load machine (absorber). For motors, electrical power goes into the DUT and is transformed to mechanical torque and speed output to the absorber/load machine. The load machine acts as a generator and converts the mechanical power back to electrical power output. This power is often recirculated from the absorber back to the DUT, so only the system losses need to be supplied externally.

The low-voltage pumps and fans are challenging to validate using a conventional motor dyno because the rotary part is often not exposed. Instead, pumps and fans are often validated as they will eventually be used: in loops where fluid pressure and flow are measured. Motor performance usually translates to operating points in the torque/speed domain, while pump performance involves points in the flow/pressure domain.

Tesla uses a variety of "fluid test benches" which are designed to exercise pumps like they will be used in their end application. This means adjusting flow restriction to create load on the pump and measuring metrics like overall electric-to-hydraulic efficiency. Once the component-level testing is complete, the device is moved into the vehicle for system-level integration testing.

{{< image
    src="images/img-14.jpg"
    caption="Enjoying the thrills of wide-open throttle in the Tesla Model S Plaid"
>}}

### Pump Performance for the Electrical Engineer

Gaining intuition for how the pump's flow/pressure domain translates into the motor's speed/torque domain took some time. In the end, it is rather simple to think in terms of electrical circuit analogies. Think of flow (LPM) as current (A) and pressure (kPa) as voltage (V). Pumps are akin to voltage or current sources: they produce pressure or flow which interacts in a fluid loop to produce flow or pressure drops. There is no such thing as an ideal "wire" to carry flow. Pipes are used, but all realistic pipes have non-negligible pressure drops. Therefore, pipes act like resistors. Valves are akin to variable resistors which block current flow and create voltage potential. 

The output power from a pump is simply the product of the flow through it and the pressure drop across it. This exactly matches electrical power: voltage times current.

What becomes more interesting is how the pump's internal motor speed (RPM) and torque (Nm) translates to the pump's output flow and pressure. How do you regulate a pump's motor RPM to achieve a desired flow? Is it a direct linear relationship? How does pressure, fluid viscosity, and temperature couple into the RPM to flow mapping? Are all pumps the same, or each unique?

By running full flow/pressure performance maps and measuring flow and differential pressure, these relationships can be experimentally found. Part of my internship projects involved data processing to extract these types of mappings.

### Noise, Vibration, and Harshness (NVH)

Controlling a low-cost motor with a low-cost processor using minimal sensors and compute power is challenging. But, the solution is only acceptable as long as the overall NVH is acceptable. If the motor screeches when it spins, the end consumer is not going to put up with it. Often, silence is desired with no vibrations.

{{< image
    src="images/img-12.jpg"
    caption="Large anechoic chamber for testing acoustics---just hope you do not go crazy!"
>}}

Another part of my internship involved prototyping a method for NVH reduction for the low voltage pumps and fans. Some brief online searching will reveal that motor torque ripple is often a key offender when it comes to bad NVH. The torque ripple creates unwanted vibrations and sounds based on the speed of the motor. Usually, torque ripple appears at the 6th electrical harmonic due to how motors are designed and built. For example, in a 4-pole motor (2 pole-pairs) spinning at 600 RPM, the 6th electrical harmonic appears at 120 Hz. Usually, the cheaper the motor, the more severe the torque ripple. 

The standard way of reducing torque ripple is to use controls to inject current which creates an equal and opposite torque ripple.  This is called harmonic injection: for example, the 6th electrical harmonic is injected to counteract the torque from the  bad motor design.

At Tesla, I prototyped a harmonic injection technique built on top of the unique sensorless FOC controller. In simulation, I showed it could create tunable 6th order torque ripple. Using an anechoic chamber with NVH measurement equipment, I experimentally demonstrated a significant reduction in NVH on a prototype HVAC blower. Even with my naked ear, it was obvious my injection was working—the 6th harmonic tone simply disappeared when the harmonic injection was enabled. Very cool!

## Tesla Culture

Tesla has cultivated a very exciting environment in which to work. There are always new challenges which need solving, and it seems the timeline for solving them is always shorter than desired. It definitely takes the right person to thrive in such an environment, but the feeling of accomplishment is hard to come by elsewhere.

{{< image
    src="images/img-13.jpg"
    caption="Ran into a fellow intern who was a PhD student at UW-Madison with me!"
>}}

Although the company is quite large, it still maintains a start-up feel. I chatted with a few folks who have been at Tesla for many years, and they told me the cultural feel has remained the same throughout the years, despite the size growing rapidly. Even though you are a small cog in the big wheel, I felt that individuals could actually contribute meaningfully. The size and feel of working at Tesla ended up being much better than I initially thought it would be.

Throughout the summer, I presented a few times to my team about the work I was doing. This required me to consolidate my work into a few simple and succinct slides which the entire team could understand and ask questions about. By the end of the summer, it was quite nice to look back over my presentation slides and remember all my contributions. The motor controls team (and other teams) values sharing knowledge and ensuring everyone in the group has a sense of the work each person is doing.

## Fun Events

Tesla has fun work events to give breaks to employees and allow discussions between groups. A few times throughout the summer, the broader Controls group which the Motor Controls team is a part of got together for beers and discussion. This was quite fun to meet others, as well as getting my head out of motor control for a while. Turns out thermal controls are also very interesting!

For a few weeks of the summer, an office-wide table tennis (ping pong) tournament occurred. It was played outside on the shared ping pong table under the warm sunny sky. While I did not personally join in, it was fun to watch and cheer on others. For the final championship matches, Tesla bought pizza and beer for everyone to come watch and enjoy.

{{< image
    src="images/img-09.jpg"
    caption="A pizza and beer party one sunny afternoon"
>}}

{{< image
    src="images/img-10.jpg"
    caption="Ping pong tournament final championship match stops work for a few hours"
>}}

## Thoughts on Experience

Overall, my internship experience was great---my naive expectations going into the internship proved wrong, in good ways.

One thing that was very apparent to me is the large differences between product development in industry and PhD research in academia. The goal in PhD-land is to create knowledge/IP and write papers to share it with the world. Contrast this to a company like Tesla where the goal is to design, produce, and sell cars. These are very different, but it was a refreshing break from PhD-land. 

Outside of work, my housing situation for the summer could have been better. After chatting with many people, it has become clear to me that housing in the Bay Area, CA is challenging in general... I am lucky to have found such a "cheap" place at only $960 per month, but it came at the price of a run-down house, constant struggles with bugs in my room, fairly weird random housemates, and a very diverse neighborhood. That said, there were no real issues during my stay.

I am very grateful for the opportunity to intern at Tesla this summer. The team was a lot of fun to get to know, and it was great to work with such smart engineers to solve real motor control issues.

Overall, I feel that my prospective has been widened the most. For this, I am forever grateful.

<!--
{{< image
    src="images/img-03.jpg"
    caption="View from the bus stop at Tesla HQ offices"
>}}

{{< image
    src="images/img-06.jpg"
    caption="Breakfast cereal station provides unlimited snacks throughout the day"
>}}
-->

{{< image
    src="images/img-15.jpg"
    caption="Great summer internship experience!"
>}}

---

#### New Post Notifications

If you read this far, you might be interested in subscribing for email notifications about future new posts I write. I will never spam you! Every few months, I write a new article for this website, and will send you email about it. You can unsubscribe at any time. Thank you.

{{< mailchimpform >}}