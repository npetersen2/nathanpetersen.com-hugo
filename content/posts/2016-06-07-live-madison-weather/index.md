---
title: "Live Madison Weather"
categories: ["Projects"]
tags: ["web", "html", "design", "weather", "temperature", "data"]
description: "Custom interface to live weather data stream and webcams from UW-Madison."
cover: "/images/cover.jpg"
date: 2016-06-07 00:00:00
---

Check out the live weather in Madison! [Go to App >](http://livemadisonweather.com/)

Check out the source code behind the app! [Go to GitHub >](https://github.com/nathanpetersenn/LiveMadisonWeather)

----------

## Background

**I started college at the University of Wisconsin - Madison during the fall of 2015. After nearly completing a successful year of school, I started working in the Computer Sciences Department in the Computer Sciences Lab ([CSL](https://csl.cs.wisc.edu/)).**

The CSL is located in an internal room, thus there are no windows to see the outside. Without the natural sunlight, one can become very anxious. Multiple times, when I left I was disappointed to find it pouring rain outside when it was sunny when I came to work. There had to be a solution...

Enter the Atmospheric, Oceanic and Space Science ([AO&SS](http://www.aos.wisc.edu/)) department, conveniently [located](http://map.wisc.edu/s/j1lqmmbk) across the street from the CS building. By chance, I actually interviewed for a web development position in the AO&SS department, but chose the CSL instead. The AO&SS has many projects on their website for all kinds or weather data and space sciencey stuff, but the coolest (in my opinion) projects are the [live webcams](http://metobs.ssec.wisc.edu/aoss/cameras/) and [live weather data](http://metobs.ssec.wisc.edu/download/index.php?site=rig) from the top of their 15 story building.

**The only logical thing to do with all this data is to rig up a simple web interface for easily viewing it in real time.**

The live weather page I put together is very simple. I intended it to be used as a T.V. background or something of that sort. Every 10 seconds the weather information on the page updates (AO&SS reports every 5 seconds, but there's no need to refresh that much if this is running 24/7 on a T.V. Gotta play nice with their servers!). Also, every 60 seconds, the background image changes to cycle through different views of Madison as seen from the top of the AO&SS building (North, Northeast, East, South, and West).

## Screenshots

{{< image 
    caption="Screenshot 1"
    src="images/screenshot1.jpg"
>}}

{{< image 
    caption="Screenshot 2"
    src="images/screenshot2.png"
>}}

{{< image 
    caption="Screenshot 3"
    src="images/screenshot3.png"
>}}
