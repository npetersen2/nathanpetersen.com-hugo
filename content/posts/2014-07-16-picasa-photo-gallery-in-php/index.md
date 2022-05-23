---
title: "Picasa Photo Gallery in PHP"
categories: ["Projects"]
tags: ["picasa", "php", "web", "design", "photography", "html", "4-h"]
desciption: "Custom front-end gallery to Google's Picasa photo service."
date: 2014-07-16 00:00:00
cover: "images/cover.jpg"
---

## Background

My earliest memories involve taking and cherishing family photos. Most of the photos I have and remember are stored in physical picture albums around my house. Since I was young, I can remember the process of my parents taking lots of pictures, developing the film, and organizing them into a multitude of albums and scrapbooks to save for memories. This system worked great for them and their parents, but I immediately saw flaws.

_What if I wanted to view the pictures when I wasn't home? What if the albums were stolen? What if the house caught fire and the albums were destroyed?_

There are a lot of alternatives to physical albums, with the most prominent being digital albums. Google offers a service called [Picasa Web Albums](https://picasaweb.google.com) which is essentially a digital version of the physical photo album. Users can upload and store images on the Picasa website, sort images into albums, and organize each album into a custom order. Picasa offers all of the functionality to maintain photo albums, but it lacks a good user interface to view them.

**For my 2013-2014 4-H computer programming project, I wrote a custom front-end viewer for Picasa using PHP.**

## What is Picasa

Picasa is a cloud-based image storage service. The "[cloud](https://en.wikipedia.org/wiki/Cloud_computing)" is the idea of a centralized system that is accessible from anywhere with internet on a multitude of devices. It facilitates the technical part of a system (i.e., uses hard drives to store files, serves files to clients when they are requested, etc).

After creating a Google account, users can log into Picasa Web Albums, create albums, and upload images. Once uploaded, the images can be edited, sorted, and transferred between albums. Every album can be set to either private or public, with the latter being available to the internet at large. Picasa also offers a viewer to browse through public albums, but the interface is not good; it doesn't work on tablets and phones, it's very cluttered, and it doesn't emphasize the actual photos.

Perhaps the more interesting part of Picasa is it's API services. Besides the web manager, Picasa offers developers the opportunity to programmatically upload and manage photos through its rich API available via a RESTful interface. Simply perform a `GET` request to a specific Picasa URL and Picasa will return an XML file full of information relating to your query. To modify the information you get back, simply change your query URL. For example, developers can use a simple `GET` request to the API to get a list of public albums published by a specific user. After getting the data into a PHP script, it is trivial to iterate over the list and generate the HTML to view the images. Picasa's APIs are well documented on their [website](https://developers.google.com/picasa-web).

## User Experience

I designed the Picasa Photo Gallery to be very neat and simple to use. There are only two main pages available to the user: the index page and the category viewer page. The website highlights its generic handling of any Picasa account by boldly displaying the user's name and profile picture in the upper right of every page, as well as in the footer. This creates a very personalized experience. A custom "about blurb" can also be configured from within the web interface of Picasa Web Albums that displays on the index page, explaining more about the account owner.

{{< image 
    caption="Home page of a site that uses the 21st Century Photo Album, nppictures.net, on a Nexus 9 tablet."
    src="images/nppictures-nexus-9_framed.png"
>}}

On the main page, a grid of "categories" is the main focus. These are the public albums stored in the user's account. Overlayed on the cover photo of each category is the category name, along with the number of images in the respective category. By overlaying the details, the interface is kept cleaner because of the simple square layout.

Upon clicking a category, the category viewer page is brought up. At the top of each viewer page is the category name. Below that are arrows that take you to the previous or next album in the list. Next, the main images appear. These take up 100% of the screen (at all screen widths) to provide a cleaner experience; no side whitespace means no distractions. After clicking on an image, a popup is generated with a larger version of the image for viewing.

## Technical Details

All source code can be found on GitHub. [Go to GitHub >](https://github.com/nathanpetersenn/nppictures.net)

### Caching System

When a browser requests the `index.php` page, a long chain of events commence. At a high level, the server basically tries to send the client a pre-rendered version of `index.php`. If the server can send this file, it sends it and the process stops. If it can't send the pre-rendered file, it is forced to generate it. After generating the file, it sends it and stores the contents of the file for the next request. Then, on subsequent requests for `index.php`, the server sends the pre-rendered file.

This is a technique known as caching. The pre-rendered file is known as a cache file. The reason for using a caching system is simple: it eliminates the need to run any code to generate a file, thus making the website quicker for the user. Since the 21st Century Photo Album stores all of it's photos on Picasa, it has to communicate with Picasa's APIs to know what content to display. Since the communication happens over the internet, it can be very slow. For example, the time it takes for the server to send _index.php_ with caching off is about 10 seconds. With caching on, it is instantaneous! All the caching code can be seen in [`util/_cache_start.php`](https://github.com/nathanpetersenn/nppictures.net/blob/master/util/_cache_start.php) and [`util/_cache_end.php`](https://github.com/nathanpetersenn/nppictures.net/blob/master/util/_cache_end.php).

### index.php

The main page for the site is `index.php`. This page uses the caching system to speed up load time by requiring the `_cache_start.php` file. The `_functions.php` file contains functions related to obtaining information from Picasa's APIs. These are used throughout the page. The `_header.php` file is included on every page. It renders the title bar, user profile name, profile picture, and the about text. Since not all pages want everything shown on them, there are configurable settings available by setting variables before requiring the file. Used on `index.php` are `$title` and `$show_about`.

In `_header.php`, the first part of page is the normal HTML used in webpages. Metadata is set and external JavaScript and CSS files are included. Inside the `<header>` tag is where the Picasa data are first used. The URL to the profile picture is the first thing constructed. The function `get_profile_pic()` uses Picasa's API to return the URL of the profile picture for the account configured. Since the user may want a different picture displayed on their website than displayed in Picasa, the function first tries to return the cover image of the 'Profile Data' album. If it can't find that custom album, it returns the default profile picture. The function `get_name()` returns an array of the user's name from Picasa. The `get_about_blurb()` function returns the description of the 'Profile Data' album. Finally, to close the header, a timestamp is generated using PHP. Because of caching, the web pages can become out-of-sync from Picasa. The date allows the user to know how old the page is.

Back in `index.php`, the categories grid of images is generated. The data for this come from the  `get_categories_array()` function. This returns an array of arrays, with the latter containing the name, number of photos, timestamp, thumbnail URL, and ID for each album. This information is used in the `foreach` loop to output the HTML. Lastly, the page is closed by requiring `_footer.php` and `_cache_end.php`.

### Other Pages

The same setup is used in `recent.php` and `category.php`, except instead of using `get_categories_array()`, these pages use `get_photos_from_album($id)`. That function requires the ID of the specific album to be used.

## Summary

In making the 21st Century Photo Album, I learned a lot about web developing. The most prominent skill would be the ability to interface with APIs to get 3rd-party information. This enables me to make more front-ends for different services, like Facebook or Twitter. Since those companies offer APIs to access their data, it would be trivial for me to make a Twitter viewer, for example.

**I love simple, easy-to-use user interfaces, so it always thrills me when I can make one.**

I enjoyed designing the look and feel of the site the most. Currently, I am using the 21st Century Photo Album to host my own personal photos on [nppictures.net](http://nppictures.net). When I went to France during spring break, I uploaded images straight to Picasa from an app on my phone so that people at home could view my experiences "live"!

### Future

I am a believer that software and code is never "done", so naturally there are more improvements that can be made on my project. On the index page, I would like to add the functionality to show live previews of the categories when the user hovers over the category cover image. The page would start a little slideshow inside the category square that fades between some of the images inside the category. Also, I would like to transition away from using stand-alone functions and global variables, and work toward a more object-oriented approach. If this method is used, then multiple users' images could be rendered at the same time on the same page. I think this would make for cleaner code.

{{< image 
    caption="NPPictures on a Nexus 6P."
    src="images/nppictures-nexus-6p_framed.png"
>}}

{{< image 
    caption="NPPictures also works on Android Wear!"
    src="images/nppictures-wear_framed.png"
>}}