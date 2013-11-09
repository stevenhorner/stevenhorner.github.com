---
layout: post
title: "The Most Visible Mountain in Scotland"
date: 2013-08-30 18:31
comments: true
categories: maps, gis
---

![Schiehallion Viewshed](https://dl.dropboxusercontent.com/u/2657852/website/images/Schiehallion_viewshed.jpg)

On a recent trip to the West Highlands I pointed out what I believed to be Schiehallion in the distance as we crossed Rannoch Moor in the car. I recalled how I had been told it was the mountain you could see from the most places in Scotland.
I decided that I would test if this was really true. I had a feeling it wasn't.
### How do you do that?
Obviously I am not going to stand in every spot in Scotland and work out what mountains I can see, I would need several life times. Instead I will use [Quantum GIS (QGIS)][2] on a computer along with elevation data and a handy plugin or 2. I had recently read [Michael Spencers][12] [blog post][11] regarding [Caleb's List][11] which gave some helpful information, have a look at what it [viewable from Arthurs Seat][11].
### This could take a long time...
I said I would need several life times to do this without a computer, but even using a standard computer it takes a long time. I worked out how far to the horizon and I would work out my calcuation based on seeing the top of the mountain, or more accurately someone stood on the top.
My first run with the full data took 2.5 days just to work out where in Scotland you could view someone stood on the top of Schiehallion. When I looked at the data annoyingly it hadn't done it correctly and half of the data was missing. Here's what was produced.
Remember I wanted to find out which mountain was the most visible in Scotland, within my criteria which you can read in the Additional Technical Details further down this post. I decided to limit the mountains to only Munros initially, this would mean that I have to run the same process that I tried on Schiehallion for each Munro.

    2.5 days * 282 Munros = A long time (705 days)

I am not the most patient person, 2015 for an answer is more than I am willing to wait.
### Additional Technical Details
#### Software used:

* [Quantum GIS][2]
* [Visibility Analysis plugin][3]
* [r.los GRASS module][4]


#### Data used:

* [OS Terrain 50][5]
* [OS Boundry-line][6]
 
#### Extra data needed for calculation:

* Calculated horizon: 90 miles (from Ben Nevis)
* Limit to only the 282 Munros

The software used was [Quantum GIS (QGIS)][2], I tried both the [QGIS Visibility Analysis plugin][3] and the [GRASS Line of Sight (r.los) module][4] within the Sextante plugin.

The Terrain data is [OS Terrain 50][5] Grid which I merged together to produce one large terrain TIF for Scotland, I then clipped this using the Scottish boundary (probably didn't need to do this).

I used [Boatsafe website][7] to calculate the horizon. This is different depending on your height, if you are stood on the top of Ben Nevis obviously you can see further than if you were at sea level. I decided to use 2 metres above the height of Ben Nevis, 1346m for all mountains. This calculated that the horizon would be just under 90 miles away, which I rounded up to 90. In reality the chances of actually being able to see that far are almost zero.

I also decided that I would use the following scenario, that if someone stood on the top of a mountain could they be seen within 90 miles without the direct line of sight being blocked by other hills. I have no way to calcuate trees or man made structures blocking the view and this would only increase the time taken to calculate. 

The QGIS Visibility Analysis took 2.5 days to complete on my Core i5 3.2Ghz with 8GB RAM machine. Annoying [QGIS][2] is only a 32 bit program and only uses a single core rather than the 4 available. For some reason it didn't complete correctly, but even with half of the results it was still interesting to see how far away Schiehallion can potentially be seen.

I decided to try the [GRASS line of sight (r.los) module][4] in the [Sextante plugin][8]. I have found GRASS to be faster in the past than some of the built-in functions. This time I ran a test with a 25 mile view and this completed in 10 minutes.
Running the full 90 mile range in GRASS took 20 hours, the image at the top of this post is the returned data overlaid on Google Terrain.

To complete this using [r.los][4] is still going to take 235 days, unless I can build a multi-threaded version of QGIS ([64 bit test version][1], not mulithreaded just released). This isn't planned until at least version 2.1, 2.0 is the next release. In the interim I will test with different viewable distances to find an acceptable processing time. The full 90 miles is further than anyone realistically could ever see anyway.

Once each mountains visibility is calculated, I convert the raster output to vector, crop using the Scottish Boundary-line data (removes visility at sea) then calculate the total area. This gives an easy figure to compare between each mountain. This figure could then be given as a percentage of the total area of Scotland eg.

    Schiehallion is visible from 0.0105% of Scotland.

I have also ran the above for Ben Nevis which can be seen below

![Ben Nevis Viewshed](https://dl.dropboxusercontent.com/u/2657852/website/images/BenNevis_Viewable.jpg)

    Ben Nevis is visible from 0.0151% of Scotland

You can already see if the above is correct Ben Nevis is viewable from a larger percentage of Scotland. I think other mountains maybe more visible than Ben Nevis.

### The Most Visible Mountains (So far)..

1. Ben Nevis (0.0151%)
2. Schiehallion (0.0105%)
3. TBC...

### Fixes
After generating the Ben Nevis and Schiehallion data I realised a couple of potential errors. I had calculated the Horizon from the top of Ben Nevis and used this for Schiehallion too, 261m higher than it actually is although this wouldn't actually alter the end results (Schiehallions height is used for the calcuation). Originally I used the [QGIS visibility plugin][3] which doesn't allow for the Earth's curvature (I don't think), [r.los][4] does allow me to choose this. I will re-run both Schiehallion and Ben Nevis without limiting the viewable range and enable the Earths curvature.

Ben Dolphin ([@CountrysideBen][10]) pointed out an excellent site [Panoramas][9] where you can view and download computer generated Panoramas of many mountains around the world, including Scotland. They are fantastic and happily the Ben Nevis ones look very close to my findings.

### Help and suggestions
I am not a mathematician, geographer, cartographer or scientist just someone who loves mountains, maps and computers. I would be very surprised if I haven't made many mistakes in my calculations. If you are aware of any, please let me know or if you can provide any help. I find it fascinating and hope others do.

[1]: http://faunaliagis.wordpress.com/2013/08/26/qgis-64bit-for-windows-is-ready-to-test/ "64bit QGIS"
[2]: http://www.qgis.org/ "Quantum GIS"
[3]: http://pyqgis.org/repo/contributed "QGIS Visibility Analysis"
[4]: http://grass.osgeo.org/grass64/manuals/r.los.html "r.los GRASS module"
[5]: http://www.ordnancesurvey.co.uk/business-and-government/products/terrain-50.html "OS Terrain 50"
[6]: http://www.ordnancesurvey.co.uk/business-and-government/products/boundary-line.html "OS Boundary-Line"
[7]: http://www.boatsafe.com/tools/horizon.htm "Boatsafe Horizon Calculator"
[8]: http://sextantegis.com/ "Sextante"
[9]: http://www.viewfinderpanoramas.org/panoramas.html "Panoramas"
[10]: https://twitter.com/CountrysideBen "Ben Dolphin"
[11]: http://scottishsnow.wordpress.com/2013/06/21/caleb-arthurs/ "Caleb's List"
[12]: https://twitter.com/MikeRSpencer "Michael Spencer on Twitter"
[13]: http://en.wikipedia.org/wiki/Schiehallion "Schiehallion"