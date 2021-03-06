---
layout: page
title: Scaling & Geometry

order: 200
---

## Scaling
---

Fyne is built entirely using vector graphics, which means applications written with Fyne will scale to any size beautifully
(not just whole number increments).
This is a great benefit to the rising popularity of high density displays on mobile devices and high-end computers.
The default scale value is calculated to match your operating system - on some systems this is user configuration 
and on others from your screen's pixel density (DPI - dots per inch).
If a Fyne window is moved to another screen it will re-scale and adjust the window size accordingly!
We call this "auto scaling", and it is designed to keep an app user interface the same size as you change monitor.

You can adjust the size of applications using the `fyne_settings` app or by setting a specific scale using the `FYNE_SCALE` environment variable.
These values can make content larger or smaller than the system settings, using "1.5" will make things 50% larger
or setting 0.8 will make it 20% smaller.

<table style="text-align: center; margin: auto;"><tr>
<td><img src="/images/architecture/hello-normal.png" style="width: 207px;"  alt="Hello normal size" />
  <br />Standard size</td>
<td><img src="/images/architecture/hello-small.png" style="width: 160px;" alt="Hello small size" />
  <br />FYNE_SCALE=0.5</td>
<td><img src="/images/architecture/hello-large.png" style="width: 350px;"  alt="Hello large size" />
  <br />FYNE_SCALE=2.5</td>
</tr></table>

## Geometry
---

Fyne apps are based on 1 canvas per window.
Each canvas has a root CanvasObject which can be a single widget or a Container for many sub-objects whose size and position are controlled by a Layout.

Each canvas has its origin at the top left (0, 0) every element of the UI may be scaled depending on the output device and so the API does not describe pixels or exact measurements.
The position (10, 10) may be 10 pixels right and down from the origin on, for example, a 96DPI monitor but on a HDPI (or "Retina") display this will probably be 15 pixels or more.
As Fyne is based entirely on vector graphics this should never be an issue - but if you start loading bitmaps you should bear this in mind.
Fyne defines a 'canvas.Raster' primitive which will draw pixels exactly at the pixel density of the output device.
If for some reason you need "pixel perfect" positioning you need to multiply `CanvasObject.Size()` by `Canvas.Scale()`.

Every position referenced by a CanvasObject is relative to it's parent.
This is important for layout algorithms but also for developers in situations such as the `Tappable.Tapped(PointEvent)` handlers.
Here the the X/Y coordinates will be calculated from the top left of the button not the overall canvas.
This is designed to allow code to be as self-contained as possible.

