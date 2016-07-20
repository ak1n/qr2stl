# qr2stl
convert qr code to stl file

### overview
- Purpose: convert a QR code into a 3d printable STL
- Process:
	- generate the QR code in SVG format
	- convert SVG to an STL file using Blender
	- modify the design in Sketchup (e.g. add a base or frame)
	- fix any geometry errors with Netfabb
	- generate gcode the 3d printer uses with Slic3r
	- use the Prusa website to add a color-change point to the gcode
	- 3d print!
- Note: others may have already automated this, but despite thingi-tags on thingiverse, I couldn't figure this out.

### tested equipment/environment:
- josef prusa i3 plus with single extruder
- filament used: PLA; white for QR-code & gray for backing square
- operating system osx 10.9.5

### step 1: get SVG of QR code
- Start with the URL or text you want to encode in the QR code.
- Go to [https://qrd.by/qr-code-generator-svg](https://qrd.by/qr-code-generator-svg) and paste in the URL to your directive (note that ideally a free and open source utility would be used for this).
- SVG = scalable vector graphic - this format is easier for 3d printing software to use vs. normal net graphics formats.

### step 2: use Blender to convert SVG to STL file
- download and install [Blender] (https://www.blender.org)
- import the svg from the first step into Blender
- select the first curve object (scene window on right) and delete it (it's the backing square; move cursor into main window and hit delete on keyboard then hit the confirm ok)
- select other curves (bottom menu → select all by type → curves)
- enlarge with Scale so you can see (hit S and move mouse to enlarge)
- select the new first curve (right window curve 001)
- now select all curves again (bottom window select → select all by type)
- make curves a mesh by bottom window → object → convert to → mesh
- join the objects
- don't extrude the object (do that in other program)
- export as stl (File -> export -> STL)

### step 3: use Sketchup to prepare design for 3d print
- download and install Sketchup: [http://www.sketchup.com/] (http://www.sketchup.com/)
- import the STL file into sketchup (merge coplanar faces on import, and try mm vs. meters for import)
- the imported STL should not have triangular vertices if joined correctly in Blender - for some reason this fails sometimes and step 2 needs to be repeated to generate QR with more contiguous faces
- scale to appropriate units for printing (e.g. 4cm x 4cm; helps to set tape guides then use scale tool while holding shift to align with 2-tape-line intersectino point)
- extrude the shape using frodo joint push/pull plugin. making too thin will reduce color contrast between the QR and background as well as increase chance that any print errors will critically distort code. making too thick will increase shadows. QR thickeness between 0.6mm & 1mm have been tested and work well for us.
- make the extruded qr code a component
- add backing using rectangle tool and extrude (easier to make qr code component first, then hit QR to more easily make backing rectangle/square)
	export stl
	
### step 4: use netfabb to fix any geometry problems
- add part and select stl file
- click repair and do default automatic repair
-export stl file and click optimize in the dialog

### step 5: open stl file in slic3r with your print settings
- export the gcode file

### step 6: generate filament/color switch point in g-code using prusa webpage:
- this step only applies if your printer handles color-stops. if your printer does not you may have to manually stop the printer at a specific z-level to change filaments.
- go to [http://prusaprinters.org/color-print/] (http://prusaprinters.org/color-print/)
- upload gcode file
- using slic3r as guide set point at which color should switch (backing to qrcode part)

### refs / misc
- this instructable has a lot of useful information but the methods described did not work for me (hence the guide above): [http://www.instructables.com/id/Create-a-3D-Printed-QR-Code-in-Blender/?ALLSTEPS] (http://www.instructables.com/id/Create-a-3D-Printed-QR-Code-in-Blender/?ALLSTEPS)




