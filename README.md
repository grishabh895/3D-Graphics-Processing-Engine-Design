# 3D Graphics Processing Engine Design with Diffuse Reflection and Shadow Computation
## Introduction
The objective of the project is to implement a 3D graphics engine using LPC1769 and color LCD display using Serial Peripheral Interface (SPI) communication. A solid 3D image of a cube is to be shown on the world coordinate system with an elevation from the xy plane. A light point source is taken and the top surface of the cube has diffused reflection with the best dynamic range of color. Computed the diffuse reflection at 4 corners on the top surface of the cube. A linear decoration algorithm has been implemented that places a tree on one of the 2 frontal surfaces of the cube, rotating squares at the other side and the initial of the name on the top side of cube.

## Objectives
1. To generate and plot a world coordinate system and solid cube;  
2. The size of the cube side length is taken as 100, and the virtual camera location E = (ex,ey,ez) = (200, 200, 200). The cube is placed floating in the world coordinate system with floating height = 10 from the x-y plane;    
3. To compute the diffuse reflection at 4 corners on the top surface of the cube, with K_r = 0.9, and to implement post processing technique to match the full dynamic range of the LPC1769 LCD. Use the linear equation to scale up the diffuse reflection color to cover the display range from 20 to 255
4. To Place a point light source P_s(xs, ys, zs) in the world coordinate system, and design its location.
5. To compute the ray equation and its intersection with the x_w-y_w plane.
6. To compute diffuse reflection on the top surface of the cube.
7. To use decoration algorithm to decorate the cube surface, place a letter of your initial with your customer designed font on the top surface, and decorate the other two visible cube surface with trees and rotating squares respectively.

## Algorithm used and project methodology
### 3D Cube
The first part of the project consists of displaying a 3D cube at the origin of x-y-z coordinate axes. The transformation matrix is used to convert World coordinate system in viewer coordinate system.

### Shadow
To draw the shadow:
1. Computed the ray equations from a light point source to each of the 4 vertices of the top side of the cube.
2. Found the point of intersection of each of these ray equation with the x-y plane.
3. Draw lines between all the line points calucluates and fill with colour to get the shadow.
 
### Diffused Reflection
Light source Is(x, y) consists of r,g,b 3 primitive colors as follows, but we need to simplify it as white color and have the highest value.
Color intensity contribution from r,g,b are:
Is(x, y, z) = (Ir (x, y, z), Ig(x, y, z), Ib(x ,y ,z))
Kd = (Kr, Kg, Kb)
||r||2 = xr2+xr2+xr2
Ir = Kdr * (n.r).1/ (||n||*||r|| * ||r||)2
Ig = Kdg * (n.r).1/ (||n||*||r|| * ||r||)2
Ib = Kdb * (n.r).1/ (||n||*||r|| * ||r||)2
1. Only the top surface of the cube is considered for diffused reflection.
2. The reflectivity of the primitive colors is given green = 0, blue =0, red = 0.9.
3. The diffused reflection is only computed in the world coordinate system.
4. A scaling factor is required since diffused reflection is very small and scaling it up is necessary for the dynamic range of display devices. An extra offset of 20 is taken to make it more visible.
5. The diffused reflection on boundary points needs to be computed. A diffused reflection model is used, and the color and intensity are computed. Boundary color to be computed by a simpler technique interpolation. The color change is taken as a function of both x and y:
a. Contribution of color changing from x
b. Contribution of color changing from y
Then the final color is the average of contribution x and contribution from y. Intensity of diffuse reflection contribution from y:
Idiff(y) = (Idiff(yi) - Idiff(yj)y /(yi - yj)) - (yi(Idiff(yi) - Idiff(yj))/(yi - yj)) + Idiff(yj)
Idiff(x) = (Idiff(xi) - Idiff(xj)x /(xi - xj)) - (xi(Idiff(xi) - Idiff(xj))/(xi - xj)) + Idiff(xj)
Idiff(x,y) = ½ (Idiff(x)+Idiff(y)).
6. Bilinear interpolation in conjunction with DDA algorithm could be used to compute reflection at the boundary line.
7. The following steps are taken to compute diffused reflection at interior points:
a) Scan the display conceptually from top left corner to right, top to bottom.
b) Scanning line L has 2 intersection points which are boundary points. We use interpolation techniques to find interpolation points.

### Initials
To display the initial letter of my name:
1. Compute the x and y coordinate end points to be used as per x-y plane.
2. Transpose those x and y coordinates as per y-x plane and put them at an elevation of 10 from x-y plane and at a distance of cube size (100)from x-y plane
3. Draw lines to display pixels as per the initial.

### Tree
1. Created patch of tree by modifying one parent tree;
2. Randomized angles for the branches;
3. Transpose the x and y coordinates as per y-z plane and put them at an elevation of 10 from x-y plane and at a distance of cube size (100)from y-z plane

### Rotating Squares
Start rotating square
1. Create two dimensional rotating patterns with data set of "parent" square;
2. Randomize location by using rand() function
3. Randomize reduction of the parent square;
4. Find all 4 coordniates of the square and transpose those x and y coordinates as per x-z plane and put them at an elevation of 10 from x-y plane and at a distance of cube size (100)from x-z plane.
4. Choose one color for each set of rotation patterns, and rotates at level of 10
5. Continue to display each set of patterns without erasing the patterns.

## How to use the code
The LPC1769 and the LCD display are soldered on a prototype board. A master-slave framework is used here. In this master-slave architecture, the LPC acts as a master and the LCD as the slave. In the connections, there are four important SPI logic signals – SCK (Serial Clock), MOSI (Master Output Slave Input), MISO (Master Input Slave Output) and CS (Chip Select, active low). A USB link aids the data transfer from the host computer to the CPU module, which then transfers the data to the LCD module.

Once the setup is ready, MCU expresso can be used to flash the code in the LPC Board.
