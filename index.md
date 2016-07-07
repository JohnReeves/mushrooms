#Mushroom Topping worksheet

##Task 1 - Making one big mushroom

Open up your Python Pizza from last time, and remember the steps in making a minimal  tk canvas:
             
```
        #import the libraries
        #create a top level object
        #attach a canvas
        #draw something
        #run the mainloop
```


We need to make a compound shape - a semicircle on top of a rectangle.

![Imgur](https://i.imgur.com/SJdGZAZ.png)

###Make the semi-circle

```
Create_arc((),(), style=”chord”, start=angle, extent=180)
```

*NB: Changing the start angle but keeping the extent as 180, has the effect of rotating the arc.*

###Making the rectangle

There are two ways of creating reactangles in Tk:

*Using create_rectangle(...)

*With a bit of trial and error, you discover that it is hard to rotate using create_rectangle

*Using create_polygon(...)

*Polygons can be rotated with a bit of maths, so use create_polygon() to make your rectangles

###Making a compound shapes

Our mushroom is made by putting the arc on top of the rectangle.

Linking the shapes together based on the centre of the circular arc is useful for keeping the shapes together when we move them.

``` 
    l=r/2
    box=[(p[0]-r,p[1]-r),(p[0]+r,p[1]+r)]
    canvas.create_arc(topbox,style="chord", fill=color, start=rot,extent=180)
    poly=[(p[0]-l,p[1]),(p[0]+l,p[1]),(p[0]+l,p[1]+r),(p[0]-l,p[1]+r)]
    canvas.create_polygon(poly, fill=color,outline="black")
```
    
## Task 2 - Python functions

Putting the shapes into a function, keeps the shape together, and allows it to draw anywhere on the screen.

Functions in Python are identified with the keyword `def`, this is what mine looks like:

```
    Def mushroom(p=(50,50),r=100, color=”red”, rotation=0)
        l=r/2
        box=[(p[0]-r,p[1]-r),(p[0]+r,p[1]+r)]
        canvas.create_arc(topbox,style="chord", fill=color, start=rotation, extent=180)
        poly=[(p[0]-l,p[1]),(p[0]+l,p[1]),(p[0]+l,p[1]+r),(p[0]-l,p[1]+r)]
        canvas.create_polygon(poly, fill=color,outline="black")
```

 
## Task 3 - Rotating the big mushroom

![Imgur](https://i.imgur.com/9c4YRLc.png)

The arc rotates easily by changing the start angle. Rectangles don’t rotate but we can rotate the points of a rectangle separately if we create it as a polygon instead!

So, an excellent use for transformations from the maths classes at school, where a rotation of a shape a point is described as:

1. translate to origin
2. rotate
3. translate to original position

Maths has a simple, and somewhat scary equation, to give you the points of a rotation about the origin.

![Imgur](https://i.imgur.com/tmQxg0G.png)

Each point in the polygon is changed to become:

![Imgur](https://i.imgur.com/cudWZTy.png)

In Python this is written as:

```
    	#rotate
    	rotx = transx*math.cos(-theta)-transy*math.sin(-theta)
    	roty = transx*math.sin(-theta)+transy*math.cos(-theta)
```

* Putting it together

```
    	#translate to origin - -50,0 50,0
        transx=point[0]-origin[0]
        transy=point[1]-origin[1]
        
    	#rotate
    	rotx = transx*math.cos(-theta)-transy*math.sin(-theta)
    	roty = transx*math.sin(-theta)+transy*math.cos(-theta)
    	
    	#translate back
    	transx = rotx + origin[0]
    	transy = roty + origin[1]
    	
    	#add to the list of points
        rotatedPoints.append( transx )
        rotatedPoints.append( transy )
```
 
Using what we learned about Python functions in putting the shapes together, we can make a function that takes in a list of points and returns a list of points that have been rotated by an angle about a point.

```
def rotate(points, angle, origin):
  	import math
  	theta = math.radians(angle)
  	rotatedPoints = []
  	for point in points:
    	
    	#translate to origin - -50,0 50,0
        transx=point[0]-origin[0]
        transy=point[1]-origin[1]
        
    	#rotate
    	rotx = transx*math.cos(-theta)-transy*math.sin(-theta)
    	roty = transx*math.sin(-theta)+transy*math.cos(-theta)
    	
    	#translate back
    	transx = rotx + origin[0]
    	transy = roty + origin[1]
    	
    	#add to the list of points
        rotatedPoints.append( transx )
        rotatedPoints.append( transy )
        
  	return rotatedPoints
```

Notice that we try to name our variables in an understandable way, so the code reads like a little essay reminding us what we are trying to do.

*NB: now test your function, giving it lots of numbers, so that you understand what it means*

## Task 4 - Making many mushrooms

![Imgur](https://i.imgur.com/jr1hNi1.png)

Mushrooms have slight differences in size, and get a bit muddled when I put them on the pizza. 

It would be pretty neat if we could do the same in Python.

Our mushroom function lets us set size, position, color, and angle to give plenty of variety to the pizza topping, and lets us put mushroom anywhere we like.

Let’s see what happens when we add a little loop, and have a slight variation for size and rotation, to make the toppings look ‘natural’:

```
from random import randint
for x in range(5):
    px,py=randint(20,280),randint(20,280)
	r=randint(20,30)
    rot=randint(-20,30)
    mushroom((px,py),r,rot,color)
```

## Task 5 - Make other compound shapes

You could try:
* Celery sticks
* Sausage
* Tomato slices


* Here’s my complete code listing, so you know what it might look like:

```
"""rotate mushrooms on a canvas
give it size, colour, angle of rotation and a point to rotate it about
return points to draw
"""
 
try:
	import Tkinter as tk
except ImportError:
	import tkinter as tk
 
def mushroom(p=(250,400),r=100,rot=30,color="red"):
 
	def rotate(points, angle, origin):
  	import math
  	theta = math.radians(angle)
  	rotatedPoints = []
  	for point in points:
    	
    	#translate to origin - -50,0 50,0
        transx=point[0]-origin[0]
        transy=point[1]-origin[1]
    	#rotate
    	rotx = transx*math.cos(-theta)-transy*math.sin(-theta)
    	roty = transx*math.sin(-theta)+transy*math.cos(-theta)
    	#translate back
    	transx = rotx + origin[0]
    	transy = roty + origin[1]
    	
    	#add to the list of points
        rotatedPoints.append( transx )
        rotatedPoints.append( transy )
  	return rotatedPoints
	
	l=r/2
 
    topbox=[(p[0]-r,p[1]-r),(p[0]+r,p[1]+r)]
    canvas.create_arc(topbox,style="chord", fill=color, start=rot,extent=180)
 
    poly0=[(p[0]-l,p[1]),(p[0]+l,p[1]),(p[0]+l,p[1]+r),(p[0]-l,p[1]+r)]
	poly=rotate(poly0, rot, p)
    canvas.create_polygon(poly, fill=color,outline="black")
 
# step 1: create the top level Tk object
window=tk.Tk()
window.title("a window")
 
# step 2: create the canvas
c_width,c_height=300,300
canvas=tk.Canvas(width=c_width, height=c_height)
 
# step 3: draw stuff on the canvas
p,r,rot=(250,400),100,5
color="grey"
 
from random import randint
 
for x in range(5):
    px,py=randint(20,280),randint(20,280)
	r=randint(20,30)
    rot=randint(-20,30)
    mushroom((px,py),r,rot,color)
 
#px,py=c_width/2,c_height/2
#r=100
#rot=10
#mushroom((px,py),r,rot)
#
canvas.pack()
window.mainloop()
```


