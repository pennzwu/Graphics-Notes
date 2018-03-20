Penn Wu  
Mr. Dyrland-Weaver, Period 5  
Computer Graphics  
Spring 2018

## Table of Contents

| Date  | Topic                                                                      |
| ----- | -------------------------------------------------------------------------- |
| 01/31 | [Peering into the Depths of Color](#0131-peering-into-the-depths-of-color) |
| 02/01 | [Peering into the Depths of Color](#0131-peering-into-the-depths-of-color) |
| 02/02 | [Useful Terminal Commands](#0202-useful-terminal-commands)                 |
| 02/05 | [Bresenham's Line Algorithm (1)](#0205-bresenhams-line-algorithm-1)        |
| 02/06 | [Bresenham's Line Algorithm (2)](#0206-bresenhams-line-algorithm-2)        |
| 02/07 | [Bresenham's Line Algorithm (3)](#0207-bresenhams-line-algorithm-3)        |
| 02/12 | [Representing Image Data (1)](#0212-representing-image-data-1)             |
| 02/13 | [Representing Image Data (2)](#0213-representing-image-data-2)             |
| 02/26 | [Transformations (1)](#0226-transformations-1)                             |
| 02/27 | [Transformations (2)](#0227-transformations-2)                             |
| 03/05 | [Parametric Equations](#0305-parametric-equations)                         |
| 03/06 | [Hermite Curves](#0306-hermite-curves)                                     |
| 03/07 | [Bezier Curves](#0307-bezier-curves)                                       |
| 03/13 | [3D Shapes (1)](#0313-3d-shapes-1)                                         |
| 03/14 | [3D Shapes (2)](#0314-3d-shapes-2)                                         |
| 03/20 | [Vectors](#0320-vectors)                                                   |

---

## 03/20: Vectors

* A vector is a unit with both direction and magnitude
* <x, y, z>

![](https://mathinsight.org/media/image/image/vector_2d_coordinates.png)

---

## 03/14: 3D Shapes (2)

#### Sphere
* Pseudocode:
```
for (ϕ: 0 -> 2π) {
	for (θ: 0 -> π) { // Semicircle
		x = rcosθ + cx
		y = rsinθcosϕ + cy
		z = rsinθsinϕ + cz
	}
}
```

#### Torus
* Defining points are on the surface
* Given center, radius of tube (r), distance from center to center of tube (R)
* Generate points by drawing and rotating a circle, but unlike a sphere, it's not centered at the origin
	* If we move over x, rotate about y and vice versa
```
[ cosϕ  0  sinϕ][rcosθ + R]
[    0  1     0][    rsinθ]
[-sinϕ  0  cosϕ][        0]
```
* x = cosϕ(rcosθ + R) + cx
* y = rsinθ + cy
* z = -sinϕ(rcosθ + R) + cz

---

## 03/13: 3D Shapes (1)

#### Box
* Defining points are vertices
* Given any single vertex (top-left-front), width (x), height (y), and depth (z)
```
add_box(pt, x, y, z) {
	add_edge(pt, pt + x)
	Do this another 11 times for the other 11 lines
	Amazing
}
```

#### Sphere
* Defining points are on the surface
* Given center and radius
* Generate points by drawing and rotating a circle about the x-axis (horizontal poles) or y-axis (vertical poles)
* X-axis rotation matrix:
```
[1     0     0][rcosθ]
[0  cosϕ -sinϕ][rsinθ]
[0  sinϕ  cosϕ][    0]
```
* x = rcosθ + cx
* y = rsinθcosϕ + cy
* z = rsinθsinϕ + cz
* θ = angle of circle creation, ϕ = angle of circle rotation
* A circle only needs to be rotated halfway before creating a sphere
* Can also do it the other way, completely rotating a semicircle
	* We're doing it this way (makes stuff easier to calculate later on)

---

## 03/07: Bezier Curves

The 'r' is silent!

* Can apply to a polynomial of any degree, though we're only doing cubics.
* A polynomial of degree n is defined by n + 1 points.

#### Line
* P<sub>t</sub> moves along the line P<sub>0</sub>P<sub>1</sub>
* P<sub>1</sub> "pulls" the curve towards it over time
* P<sub>t</sub> = (1 - t)P<sub>0</sub> + tP<sub>1</sub>

#### Quadratic
* P<sub>0</sub>, P<sub>1</sub>, P<sub>2</sub>
* Q<sub>0</sub> moves along the line P<sub>0</sub>P<sub>1</sub>
* Q<sub>1</sub> moves along the line P<sub>1</sub>P<sub>2</sub>
* Q<sub>t</sub> moves along the line Q<sub>0</sub>Q<sub>1</sub>
* Q<sub>t</sub> = (1 - t)Q<sub>0</sub> + tQ<sub>1</sub>
* Q<sub>t</sub> = (1 - t)\[(1 - t)P<sub>0</sub> + tP<sub>1</sub>] + t\[(1 - t)P<sub>1</sub> + tP<sub>2</sub>]
* Q<sub>t</sub> = (1 - t)<sup>2</sup>P<sub>0</sub> + t(1 - t)P<sub>1</sub> + t(1 - t)P<sub>1</sub> + t<sup>2</sup>P<sub>2</sub>
* Q<sub>t</sub> = (1 - t)<sup>2</sup>P<sub>0</sub> + 2t(1 - t)P<sub>1</sub> + t<sup>2</sup>P<sub>2</sub>

#### Cubic
* P<sub>0</sub>, P<sub>1</sub>, P<sub>2</sub>, P<sub>3</sub>
* Q<sub>0</sub> <-> P<sub>0</sub>P<sub>1</sub>, Q<sub>1</sub> <-> P<sub>1</sub>P<sub>2</sub>, Q<sub>2</sub> <-> P<sub>2</sub>P<sub>3</sub>
* R moves along quadratics defined by Q
* R<sub>0</sub> <-> Q<sub>0</sub>Q<sub>1</sub>, R<sub>1</sub> <-> Q<sub>1</sub>Q<sub>2</sub>
* R<sub>t</sub> moves along the line R<sub>0</sub>R<sub>1</sub>
* R<sub>t</sub> = (1 - t)R<sub>0</sub> + tR<sub>1</sub>
* R<sub>t</sub> = (1 - t)\[(1 - t)<sup>2</sup>P<sub>0</sub> + 2t(1 - t)P<sub>1</sub> + t<sup>2</sup>P<sub>2</sub>] + t\[(1 - t)<sup>2</sup>P<sub>1</sub> + 2t(1 - t)P<sub>2</sub> + t<sup>2</sup>P<sub>3</sub>]
* ...ALGEBRA
* R<sub>t</sub> = (1 - t)<sup>3</sup>P<sub>0</sub> + 3t(1 - t)<sup>2</sup>P<sub>1</sub> + 3t<sup>2</sup>(1 - t)P<sub>2</sub> + t<sup>3</sup>P<sub>3</sub>
* Rearrange into standard polynomial format
* R<sub>t</sub> = (-P<sub>0</sub> + 3P<sub>1</sub> - 3P<sub>2</sub> + P<sub>3</sub>)t<sup>3</sup> + (3P<sub>0</sub> - 6P<sub>1</sub> + 3P<sub>2</sub>)t<sup>2</sup> + (-3P<sub>0</sub> + 3P<sub>1</sub>)t + P<sub>0</sub> = at<sup>3</sup> + bt<sup>2</sup> + ct + d
	* a = -P<sub>0</sub> + 3P<sub>1</sub> - 3P<sub>2</sub> + P<sub>3</sub>
	* b = 3P<sub>0</sub> - 6P<sub>1</sub> + 3P<sub>2</sub>
	* c = -3P<sub>0</sub> + 3P<sub>1</sub>
	* d = P<sub>0</sub>

Matrix format:
```
[-1  3 -3  1][P0]   [a]
[ 3 -6  3  0][P1]   [b]
[-3  3  0  0][P2] = [c]
[ 1  0  0  0][P3]   [d]
```

---

## 03/06: Hermite Curves

Given information:
* P<sub>0</sub>, P<sub>1</sub> - Endpoints
* R<sub>0</sub>, R<sub>1</sub> - Rates of change at each endpoint
* f(t) = at<sup>3</sup> + bt<sup>2</sup> + ct + d
* f'(t) = 3at<sup>2</sup> + 2bt + c
* f(0) = d = P<sub>0</sub>
* f'(0) = c = R<sub>0</sub>
* f(1) = a + b + c + d = P<sub>1</sub>
* f'(1) = 3a + 2b + 1c = R<sub>1</sub>

This is a system of equations. Let's solve this using a matrix!
```
[0 0 0 1][a]   [P0]
[1 1 1 1][b]   [P1]
[0 0 1 0][c] = [R0]
[3 2 1 0][d]   [R1]
```
Because H • C (coefficients) = G (givens), H<sup>-1</sup> • G = C:
```
[ 2 -2  1  1][P0]   [a]
[-3  3 -2 -1][P1]   [b]
[ 0  0  1  0][R0] = [c]
[ 1  0  0  0][R1]   [d]
```

---

## 03/05: Parametric Equations
  
* Define a curve as a system of equations based on an independent variable (t).
* x = f(t)
* y = g(t)
* z = h(t)
* Restrict t between 0 and 1, sorta like a percentage.

#### General Parametric Framework (Pseudocode)

```
for (0 <= t <= 1, t += step) {
	x = f(t)
	y = g(t)
	add_edge(x, y)
}
```
* Line - (x<sub>0</sub>, y<sub>0</sub>) -> (x<sub>1</sub>, y<sub>1</sub>)
	* f(t) = x<sub>0</sub> + t(Δx)
	* g(t) = y<sub>0</sub> + t(Δy)
* Circle - (x<sub>c</sub>, y<sub>c</sub>) r
	* f(t) = rcos(2πt) + x<sub>c</sub>
	* g(t) = rsin(2πt) + y<sub>c</sub>
	* 0 <= θ <= 2π
* Consider making the loop an integer. For example, have t range from 0 to 100, and simply divide it by 100 for calculation purposes.
* We want to draw **splines**, or curves that can be combined smoothly. Only cubics will be drawn.

---

## 02/27: Transformations (2)

More rotations!

#### X-Axis
* Rotation about the x-axis is pretty much the same thing as with the z-axis. Just swap x with y and y with z.
* y = rcos(ϕ)
* y<sub>1</sub> = rcos(ϕ + θ) = rcos(ϕ)cos(θ) - rsin(ϕ)sin(θ) = ycos(θ) - zsin(θ)
* z = rsin(ϕ)
* z<sub>1</sub> = rsin(ϕ + θ) = rsin(ϕ)cos(θ) + rcos(ϕ)sin(θ) = zcos(θ) + ysin(θ)

#### Y-Axis
* And again for the y-axis, swapping x with z and y with x:
* z = rcos(ϕ)
* z<sub>1</sub> = rcos(ϕ + θ) = rcos(ϕ)cos(θ) - rsin(ϕ)sin(θ) = zcos(θ) - xsin(θ)
* x = rsin(ϕ)
* x<sub>1</sub> = rsin(ϕ + θ) = rsin(ϕ)cos(θ) + rcos(ϕ)sin(θ) = xcos(θ) + zsin(θ)

#### Combining Transformations
Given the following matrices:
* E<sub>0</sub> - Edges
* T - Translate
* R - Rotate
* S - Scale

T • E<sub>0</sub> = E<sub>1</sub>  
R • E<sub>1</sub> = E<sub>2</sub>  
S • E<sub>2</sub> = E<sub>3</sub>  
E<sub>3</sub> = (S • R • T) • E<sub>0</sub>  
**THIS IS RIGHT-TO-LEFT!**

---

## 02/26: Transformations (1)

**Remember to use the `clear_screen()` function when initializing a screen!**

#### Dilation
* Making an image bigger or smaller.
* (x, y, z) ---D<sub>(a, b, c)</sub>---> (ax, by, cz)
* Dilation matrix:
```
[a 0 0 0][x]   [ax]
[0 b 0 0][y]   [by]
[0 0 c 0][z] = [cz]
[0 0 0 1][1]   [ 1]
```
* If you want to make an image bigger/smaller without also moving one of its corners, translate it to the origin, dilate it, and translate it back again.

#### Rotation (Z-Axis)
* Set aside the z-axis for now.
* (x, y) ---R<sub>θ</sub>---> (?, ?)
* Use polar coordinates!
* x = rcos(ϕ)
* x<sub>1</sub> = rcos(ϕ + θ) = rcos(ϕ)cos(θ) - rsin(ϕ)sin(θ) = xcos(θ) - ysin(θ)
* y = rsin(ϕ)
* y<sub>1</sub> = rsin(ϕ + θ) = rsin(ϕ)cos(θ) + rcos(ϕ)sin(θ) = ycos(θ) + xsin(θ)
* (x, y) ---R<sub>θ</sub>---> (xcos(θ) - ysin(θ), ycos(θ) + xsin(θ))
* z is unchanged!
* Rotation matrix:
```
[cos(θ) -sin(θ) 0 0][x]   [xcos(θ) - ysin(θ)]
[sin(θ)  cos(θ) 0 0][y]   [ycos(θ) + xsin(θ)]
[0           0  1 0][z] = [                z]
[0           0  0 1][1]   [                1]
```

---

## 02/13: Representing Image Data (2)

Imagine the edge list as a 4xN matrix.

#### Matrix Math for Graphics
* \# of columns in matrix M = # of rows in matrix N.
	* A x B • B x C = A x C
* Non-commutative - M • N isn't _always_ N • M. Sometimes it's not even possible to reverse M and N, given the previous restriction.
* Examples:
```
          [a]
[1 2 3] • [b] = [1a + 2b + 3c]
          [c]
  1x3     3x1        1x1

[1 2 3]   [a b]   [1a + 2c + 3e  1b + 2d + 3f]
[4 5 6] • [c d] = [4a + 5c + 6e  4b + 5d + 6f]
[7 8 9]   [e f]   [7a + 8c + 9e  7b + 8d + 9f]
  3x3      3x2                3x2
```
**You will have to write a procedure to do this.**
* Multiplicative Identity Matrix - M • I = M
	* I is a square matrix with a top left/bottom right diagonal of 1's, and 0's everywhere else:
	```
	[1 0][a]   [a]
	[0 1][b] = [b]
	```

#### Transformations
* Focus on translations, dilations, and rotations.
* These are also known as **affine transformations**, which preserve vertices and order.
* Let E = edge matrix, T = transformation matrix. Should we do E • T or T • E?
* T • E = 4x4 • 4xN = 4xN

#### Translations
* Goal: Define the T matrix to be multiplied with a point matrix to perform a translation.
* (x, y, z) ---T<sub>(a, b, c)</sub>---> (x + a, y + b, z + c)
* Use 4x4 matrices with a diagonal of 1's like an identity matrix.
* The rightmost side will also have a, b, and c in descending order.
* Multiply this matrix with the column matrix of a point + a 4th row with a value of 1.
* Translation matrix:
```
[1 0 0 a][x]   [x + a]
[0 1 0 b][y]   [y + b]
[0 0 1 c][z] = [z + c]
[0 0 0 1][1]   [    1]
```

---

## 02/12: Representing Image Data (1)

The origin will always be on the bottom left, not the top left, in order to make it easier to visualize image transformations.

#### Triangle Shenanigans
* Triangles can be represented through a list of their 3 endpoints.
* **Edge list** - List of points in the image. Every 2 points determine a line.
	* \[P<sub>0</sub>, P<sub>1</sub>, P<sub>2</sub>, ..., P<sub>n</sub>] generates lines \[P<sub>0</sub>P<sub>1</sub>, P<sub>1</sub>P<sub>2</sub>, ..., P<sub>n-1</sub>P<sub>n</sub>].
		* P<sub>0</sub> = (x<sub>0</sub>, y<sub>0</sub>). The edge list is actually a 2D array!
		* For 3D images, points will include z-coordinates, which will also show up for rotations and other transformations.
	* This does have a bit of repetition, but space is relatively cheap.

---

## 02/07: Bresenham's Line Algorithm (3)

#### Algorithm for Octant II
* Midpoint = (x + 0.5, y + 1)
* d<sub>0</sub> = f(x<sub>0</sub> + 0.5, y<sub>0</sub> + 1) = A + 2B
```
x = x0, y = y0
d = A + 2B
while x <= x1
	plot(x, y)
	if d < 0
		x ++
		d += 2A
	y ++
	d += 2B
```

#### Algorithm for Octant VIII
* Midpoint = (x + 1, y - 0.5)
* d<sub>0</sub> = f(x<sub>0</sub> + 1, y<sub>0</sub> - 0.5) = 2A - B
```
x = x0, y = y0
d = 2A - B
while x <= x1
	plot(x, y)
	if d < 0
		y --
		d -= 2B
	x ++
	d += 2A
```

#### Algorithm for Octant VII
* Midpoint = (x + 0.5, y - 1)
* d<sub>0</sub> = f(x<sub>0</sub> + 0.5, y<sub>0</sub> - 1) = A - 2B
```
x = x0, y = y0
d = 2A - B
while x <= x1
	plot(x, y)
	if d < 0
		x ++
		d += 2A
	y --
	d -= 2B
```

For Octants III, IV, V, and VI, just reverse the x-y coordinates and use the algorithms from Octants VII, VIII, I, and II respectively.

---

## 02/06: Bresenham's Line Algorithm (2)

#### Testing the Midpoint
* Midpoint = (x + 1, y + 0.5)
* d<sub>0</sub> = f(x<sub>0</sub> + 1, y<sub>0</sub> + 0.5) = 2A + B
```
y = mx + b
0 = mx - y + b

m = Δy / Δx

0 = (Δy / Δx)x - y + b
  = xΔy -yΔx + bΔx
  = Ax + By + C
A = Δy
B = -Δx
C = bΔx

f(x,y) = Ax + By + C
d = f(x0 + 1, y0 + 0.5)
d = A + 0.5B
2d = 2A + B (we don't like decimals)
```
* Notice that B is negative; adding B reduces the value of B.
* When the midpoint is directly on the line, f(x, y) = 0 (this is not really that important).
* When the midpoint is above the line, f(x, y) < 0.
* When the midpoint is below the line, f(x, y) > 0.

#### Algorithm for Octant I
```
x = x0, y = y0
d = 2A + B
while x <= x1
	plot(x, y)
	if d > 0
		y ++
		d += 2B
	x ++
	d += 2A
```

---

## 02/05: Bresenham's Line Algorithm (1)

* In CS, we need to approximate values of y into integers.
* We don't care about the grid lines, we care about the boxes!
* Each box is represented by a single pixel.
* Instead of quadrants, we use octants.

#### Bresenham's Line Algorithm
* Goal: find the pixels that best approximate a target line.
* Assume that:
	1. (x<sub>0</sub>, y<sub>0</sub>) -> (x<sub>1</sub>, y<sub>1</sub>) ϵ Z
	2. For lines in octant I, 0 < m < 1
	3. x<sub>0</sub> < x<sub>1</sub> (can just swap their values, but make sure you swap the y-values too!)
* Because of these assumptions, we can take any colored pixel and move either right or right and up, never just up (slope is restricted).
* But which of those two is it?
* Find the value of the line at x + 1, and evaluate whether it is above or below (x + 1, y + 0.5).
	* If above, color the upper pixel.
	* If below, color the lower pixel.
	* If on, arbitrarily pick and stick with one.

---

## 02/02: Useful Terminal Commands

#### Commands for ImageMagick
* `$ display image.ppm`
* `$ convert image.ppm image.png`

---

## 01/31: Peering into the Depths of Color

**Color Depth** - Amount of data used to represent a single pixel.

| Size   | Color Options                           |
| ------ | --------------------------------------- |
| 1 bit  | 1 color, on \| off                      |
| 2 bit  | 1 color, on \| off, 1 bit for intensity |
| 3 bit  | RGB, on \| off                          |
| 4 bit  | RGB with intensity                      |
| 6 bit  | RGB, _each_ color has its own intensity |
| 3 byte | RGB, each with 256 possible intensities |

#### Other Color Spaces
* RGBA - Red, Green, Blue + Alpha (transparency)
* HSB - Hue, Saturation, Brightness

#### Image File Formats
* Raster vs. Vector
	* Vector formats represent images as a series of drawing instructions and are infinitely scalable.
		* SVG - Scalable Vector Graphics
	* Raster formats represent images as a grid of pixels.
* Uncompressed vs. Compressed (Raster)
	* Uncompressed formats contain data for each pixel.
		* Examples include BMP, TIFF, and RAW files.
	* Compressed formats use a compression algorithm to minimize file size.
	* Lossless vs. Lossy
		* Lossless compression algorithms contain enough information to exactly recreate the original image.
		* Lossless algorithms include PNG and GIF.
		* Lossy compression algorithms don't retain all the details of the original image.
		* The most commonplace lossy algorithm is JPEG.

#### PPM (Portable PixMap)
* Uncompressed raster format.
* Pixel data is represented by RGB triplets in either ASCII or binary.
* All whitespace is equivalent.
* Example:
```
P3 (file type, P3 is ASCII and P6 is binary)
4 3 (x-y dimensions)
255 (maximum value)
255 0 0  255 0 0  255 0 0  255 0 0
0 255 0  0 255 0  0 255 0  0 255 0
0 0 255  0 0 255  0 0 255  0 0 255
```
### Use the terminal command `convert file.ppm file.png` to compress your work!

---
