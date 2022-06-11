# How PhoenixTales colors ASCII arts
Just like with making a mod for zEngine, it's tedious and manual process involving multiple tools. Could be made way easier if we were ok with cheating and making bitmap picture where characters are represented as pixels. But no, we want a real old-school ascii art that is both retro and lightweight.

We use third party tool that can convert grayscale image into ascii art. 
It does not matter what too it is, the approach works with any.

We came up with this exact procedure when preparing colored ascii art of Phoenix in "Stalker" variant. We tried out few different approaches, and below sequence of steps gave the best results.

## Step 1 - prepare color mask
1. Duplicate the input image into new layer "1"
2. Create new layer "2" filled with 50% saturated red (HSV)
3. Put "1" above "2" and set color mixing mode to HSV saturation
4. Merge layers "1" and "2 " into "2"
5. Desaturate and invert layer "2"
6. Duplicate the input image into new layer "3"
7. Desaturate layer "3"
8. Increase contrast of layer "3" to maximum and set brightness to point when parts too dark to be colored are all black
9. Put layer "3" above layer "2" and set color mixing mode to "Darken only"
8. Increase contrast of layer "2" to maximum and set brightness to point when only parts with visible color are white
9. Merge layers "3" and "2" into "2"
10. Scale layer "2" to the size of target ASCII art in characters (e.g. 98px x 95px for Phoenix mascot) (proportions will appear stretched horizontally, because each character in rendered ascii art is naturally taller than wide)
11. Replace white with transparency
12. The layer "2" is now our "color mask" that we will use in next step

## Step 2 - prepare color map
1. Slightly increase saturation of input image to compensate for coming partial color loss
2. Scale input image to the size of target ASCII art in characters
3. Put "color mask" prepared in step 1 above input image and merge it into it
4. Make sure that there is only one layer left in the image and no transparency
5. Change image mode to indexed, around 10 colors, with Floyd-Steinberg dithering
6. Replace black with transparency
7. The color map is ready - we will use it for distributing available colors between "ascii pixels" (it will only affect hue/saturation, not brightness as expressed by choice of different ascii characters)

## Step 3 - separate layers on input image
1. Put color map prepared in step 2 on top of input image
2. Use tool "select by color" (`shift-o`) all the time, with 0% tolerance setting and no antialiasing
3. Scale color map to image size, with no interpolation (it will look "pixelated")
4. Select black color on color map and remove it
5. Select any color on color map, delete that selection on color map, then cut that selection from input image and paste it as new layer
6. Repeat step 5 until color map is completely empty
7. Save each layer created in repeats of step 5 as separate file
8. Generate separate ascii art from each of these files, using 3rd party tool of your choice
9. Generate last ASCII art from parts of input image that were not covered by color map

## Step 4 - assemble with CSS
1. Display each ASCII art in separate divs
2. Position the divs absolutely on the same position, so that they perfectly overlap
3. Pick different color for each ASCII art, except of the last one, which contains "grayscale" part
4. Carefully tweak the colors in CSS so that overall the art has best reassemblance to the input image