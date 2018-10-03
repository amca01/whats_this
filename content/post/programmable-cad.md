+++
title = "Programmable CAD"
author = ["Alasdair McAndrew"]
date = 2017-11-24
draft = false
mathjax = true
+++

Every few years I decide to have a go at using a CAD package for the
creation of 3D diagrams and shapes, and every time I give it up. There's
simply too much to learn in terms of creating shapes, moving them about,
and so on, and every system seems to have its own ways of doing things.
My son (who is an expert in [Blender](https://www.blender.org))
recommended that I experiment with
[Tinkercad](https://www.tinkercad.com), and indeed this is probably a
pretty easy way of getting started with 3D CAD. But it didn't suit me: I
wanted to place things precisely in relation to each other, and fiddling
with dragging and dropping with the mouse was harder and more
inconvenient than it should have been. No doubt there are ways of
getting exact line ups, but it isn't obvious to the raw beginner.

I then discovered that there are lots of different CAD "programming
languages"; or more properly scripting languages, where the user
describes how the figure is to be built in the system's language. Then
the system builds it from the script. In this sense these systems are
descendants of the venerable
[VRML](https://en.wikipedia.org/wiki/VRML), of which you can see some
[examples here](http://cs.lmu.edu/~ray/notes/vrmlexamples/), and its
modern version [X3D](https://en.wikipedia.org/wiki/X3D).

Some of the systems that I looked at were:

-   [OpenSCAD](http://www.openscad.org), which uses its own scripting
    language
-   [OpenJSCAD](https://openjscad.org), based on JavaScript
-   [implicitCAD](https://github.com/colah/ImplicitCAD), based on
    Haskell,

No doubt there are others. All of these systems have primitive shapes
(spheres, cubes, cylinders etc), operations on shapes (shifting,
stretching, rotating, extruding etc) so a vast array of different forms
can be generated. Some systems allow for a great deal of flexibility, so
that a cylinder with a radius of zero at one end will be a cone, or of
different radii at each end a frustum.

I ended up choosing OpenJSCAD, which is being actively developed, is
based on a well known and robust language, and is also great fun to use.
Here is a simple example, to construct a tetrahedron whose vertices are
chosen from the vertices of a cube with vertices . The vertices whose
product is 1 will be the vertices of a tetrahedron. We can make a nice
tetrahedral shape by putting a small sphere at each vertex, and joining
each sphere by a cylinder of the same radius:

<div class="CSG" style="width:750px; height:500px;">
<script type="text/javascript">
  var rad = 0.1; // radius of sphere at vertex and cylinders

  var v0 = [1,1,1];
  var v1 = [1,-1,-1];
  var v2 = [-1,1,-1];
  var v3 = [-1,-1,1];
  var vertices = [v0,v1,v2,v3];

  // adjacency lists:
  var adj = [[1,2,3],[0,2,3],[0,1,3],[0,1,2]];

  function main() {
    var t = [];
    for(var i = 0; i &lt; 4; i++) {    // loop through the list of vertices
      var here = vertices[i];
      t.push(translate(here,sphere({r:rad})));
      for(var j = 0; j &lt; 3; j++) {  // for each vertex join it to the others in its adjacency list
        var there = vertices[adj[i][j]];
        t.push(cylinder({start:here,end:there,r:rad}));
      }
    }
    return union(t);
  }
  </script>
</div>

The code should be fairly self-explanatory. And here is the tetrahedron:

I won't put these models in this post, as one of them is slow to render:
but look at a
[coloured
tetrahedron](https://numbersandshapes.net/openjscad/tetrahedron.html), and an
[icosahedron](https://numbersandshapes.net/openjscad/icosahedron.html).

Note that CAD design of this sort is not so much for animated media so
much as precise designs for 3D printing. But I like it for exploring 3D
geometry.

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
