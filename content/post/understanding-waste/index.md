---
title: Understanding AMS Waste
description: How does color switching waste time and filament?
slug: ams-waste
date: 2025-02-24
image: cover.jpg
tags:
    - ams
draft: false
---

# Setup
Download the [latest release](https://github.com/bambulab/BambuStudio/releases) of Bambu Studio if you want to follow along with the experiments using these default settings:

![Default settings](defaults.png)

1. Using the A1 w/ AMS Lite because this is what I own, you can repeat with the P1P profile to see if the regular AMS is faster / slower.
2. Use Bambu PLA Basic profile since the material has a high max flow setting.
3. Enable Advanced settings for more control.
4. Start with the Standard quality, but experiment here.

# Absolute Fastest Print Time
Use the Right Click menu to add a simple single color Cube, then slice it to understand the maximum speed at which a simple cube can be printed (18m26s).

Now's a good time to briefly switch the Printer to `Bambu Lab P1P 0.4 nozzle` to see if there's any major speed difference.  There doesn't seem to be much difference (19m23s) since this is a very small model without tiny features where the CoreXY accelerations may allow faster speed. 

![Add Cube](add-cube.png) ![Max Speed A1](max-speed-cube-a1.png) ![Max Speed P1](max-speed-cube-p1.png)


# Minimal Color Change Time
Switch the printer back to `Bambu Lab A1 0.4 nozzle` for now.

1. select the Color Painting tool
2. with a different filament color
3. using the Height Range brush
4. with a large Height Range of 8mm
5. to paint the top half of the cube blue

![Horizontal Paint](horizontal-paint.png)

then slice to see the time increase (23m32s)

![Initial Filament](horizontal-paint-initial-filament.png)![Initial Line Type](horizontal-paint-initial-line.png)

The Prime Tower (1) uses an extra 3g of filament and adds ~6m to the print time so let's disable it for now to get a feel for the pure AMS swap (cut, unload, load, purge).  The new print time is 19m18s so we can estimate swap time at 58s.

![No Prime](horizontal-paint-no-prime.png) 

Surprisingly if you add 2 more filament changes the time actually increases by 3m12, so average swap time has increased to 1m4s.  This is likely due to different before/after color combinations requiring more flushing (2-4)

![More Colors](horizontal-paint-no-prime-more-colors.png)

# Absolute Slowest Print Time
Let's do something horrible and rotate the model to cause color swaps on every layer.  This adds 604m to our intial 18m print time and averages out to 1m33s per AMS swap.  If you reslice using the P1P profile it'll be in the same ballpark.

![Vertical Paint](vertical-paint-rotate.png) ![Vertical Initial](vertical-paint-initial.png)

Color swaps are evil, avoid them when possible!

# Avoiding Color Swaps
The two previous experiments show the easiest trick for speeding up multi color prints is to orient them to minimize color swaps and maximize the amount of time the printer uses a filament.

But what if you want to print multiple objects that are different colors without having to manually swap filaments / plates like a poor Ender 3 owner?
![4 Cubes By Layer](four-cubes-by-layer.png)

Use `Print sequence: By object` so a single-color cube is fully printed before printing the next cube.  The buffer area shown around each object is necessary so that when the toolhead goes back down to a lower layer it doesn't collide with existing objects.  Note that on a P1P it's possible to use the Arrange tool to place all four cubes on one plate, but it can only place 3 cubes when using the A1 profile.  The Bambu wiki has a [detailed explanation of these limitations](https://wiki.bambulab.com/en/software/bambu-studio/sequent-print).

![4 Cubes By Object](four-cubes-by-object.png) ![Arranged](four-cubes-by-object-arranged.png)

