# .HSD Structure
An .HSD is a passworded .ZIP file. Every file I've encountered has had the same password, "huion2018". Within the archive is one subdirectory, "temp". This contains all of the project files, which I will go into individually.

## flayer_#
This is a ZIP file, with a single file called "zip". This file contains image data for a layer. The given number corresponds to the layer's filename-id in the project.json.
The image data is stored as an RGBA bitmap. The first four bytes, as far as I can tell, do not affect the image data. They're probably used to identify the file type.
I've given labels to the next 8 bytes, grouping them into four two-byte chunks. They are ints, stored little-endian. XXpos and YYpos determine the position of the layer on the canvas, relative to the bottom left pixel. XXpos is the distance from the left side of the canvas in pixels. YYpos is the distance from the bottom side of the canvas in pixels. XXwid is the width of the layer and YYtal is the height of the layer, both in pixels.

`56 45 52 01    97 00 9a 02    8e 03 c9 03` <br />
`Identifier     XXpos YYpos    XXwid YYtal`

Everything following this header is a series of 4 byte long chunks, each of which represents the RGBA value for a given pixel. Any empty (i.e. fully transparent) pixels at the end of the bitmap are omitted. Entirely empty layers do not have flayer files generated.

## hsd.json
This contains HiPaint specific information. This includes the kind of device that file was exported from, the version of HiPaint the file supports, and the display name of the project. There is also a version value, which I have not investigated.

## layer3d.json
A JSON object. Contains information relating to 3D model layers. As the actual model is not included in the HSD, only the data on positioning, lighting, etc., I have not investigated this file in any detail.

## preview
This is the project rendered as a flat PNG image at full resolution.

## previewthumb
This is the project rendered as a flat PNG image at thumbnail resolution.

## project.json
This contains all data related to the drawing that is not straight image data. It is a file containing a JSON object which contains the following:
### bounds
A JSON object. Contains information on canvas dimensions and cropping.
### layers
A JSON array. Contains a list of JSON objects that represent the project layers, in order from bottommost to topmost. Most have self-explanatory names, but there are some oddities. 
- "id" represents this same layer order from bottom to top in number form, starting from 0. This is *not* the value used by parent-id.
- "filename-id" refers to the flayer that this layer uses, even if it is an empty layer and such a file does not exist. 
- "parent-id" is the filename-id of the group the layer belongs to, if any; if it does not belong to any group, this may be set to strange values, like -3 or an arbitrary positive value that does not match any other layer. 
- "blend" represents the layer's blending mode as a number. A list of which blending modes correspond to which numbers is provided below.
<details>
<summary>Blend modes</summary>
0:"svg:src-over" <br />
1:"svg:overlay"<br />
2:"svg:darken"<br />
3:"svg:multiply"<br />
4:"svg:color-burn"<br />
5:"krita:linear_burn" <br />
6:"krita:darker color" <br />
7:"svg:lighten"<br />
8:"svg:screen"<br />
9:"svg:color-dodge"<br />
10:"svg:plus"<br />
11:"krita:lighter color" <br />
12:"svg:soft-light"<br />
13:""<br />
14:"svg:hard-light"<br />
15:"krita:vivid_light" <br />
16:"krita:linear light" <br />
17:"krita:pin_light" <br />
18:"krita:hard mix" <br />
19:"svg:difference"<br />
20:"krita:exclusion" <br />
21:""<br />
22:"krita:divide" <br />
23:""<br />
24:""<br />
25:"svg:hue"<br />
26:"svg:saturation"<br />
27:"svg:color"<br />
28:"svg:luminosity"<br />
29:"krita:subtract" <br />
30:"krita:dissolve" <br />
31:"" <br />
32:"" <br />
33:"group penetrate"
</details>
- "clip" determines if a layer is a clipping layer. 
- "bihuacount" determines stroke count on a given layer. 
- "bean-type" is 0 for a paint layer, and 1 for a folder layer. I do not know if other possible values exist.
- "is-3d" determines whether a layer was generated using HiPaint's 3D model tool. The layer is still a flat image in the HSD, and does not contain the actual 3D model.
- I have not investigated the use of "is-foreground" or "is-background", as they were false in all of my project files. 

## background
A JSON object. Contains colour information for the background layer, stored in a signed 32-bit integer format. (I was told this, but I'm unfamiliar with the format.) 

## camera
A JSON object. Contains information relating to viewport positioning within HiPaint.

## selected-layer
The filename-id of the selected layer.

## ref-layer
Contains information on reference images, including how they are positioned within HiPaint, zoom/rotation levels, and the path to the image on the user's device. As the reference images themselves are not stored in the HSD, I have not investigated this thoroughly.

## paintbrushid
The currently selected paint brush within HiPaint.

## eraserbrushid
As above, but for the eraser.

## smudgebrushid
As above, but for the smudge brush.

## blurbrushid
As above, but for the blur brush.

## mixbrushid
As above, but for the mix brush.

## gifframes
A JSON object. Contains information relating to animation projects, including onion skin settings, frame rate, etc.

## symmetry
A JSON object. Contains information relating to HiPaint's symmetry tool.

## auxiliary
A JSON object. Contains information relating to HiPaint's grid tool, which is called auxiliary line.

## referencepic-using#
A boolean that determines whether a given slot for reference images is in use.

## work-createtime
A Unix timestamp of when the project was made.

## work-durition
A measure of how long the project has been worked on.

## work-drawcnt
I have not investigated this.

## work-ppi
The PPI of the project.

## version
I have not investigated this.

## project.json.ok
Very similar to project.json. Probably a known good backup, considering the differences in work-durition.

