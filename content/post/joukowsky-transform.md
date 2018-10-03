+++
title = "The Joukowsky Transform"
author = ["Alasdair McAndrew"]
date = 2018-08-24
tags = ["mathematics", "geometry", "jsxgraph"]
draft = false
+++

The [Joukowksy Transform](<https://en.wikipedia.org/wiki/Joukowsky_transform>) is
an elegant and simple way to create an airfoil shape.

Let \\(C\\) be a circle in the complex plane that passes through the point \\(z=1\\) and
encompasses the point \\(z=-1\\).  The transform is defined as

\\[
\zeta=z+\frac{1}{z}.
\\]

We can explore the transform by looking at the circles centred at \\((-r,0)\\) with
\\(r<0\\) and with radius \\(1+r\\):

\\[
\\|z-r\\|=1+r
\\]

or in cartesian coordinates with parameter \\(t\\):

\begin{align\*}
x &= -r+(1+r)\cos(t)\\\\\\
y &= (1+r)\sin(t)
\end{align\*}

so that
\\[
(x,y)\rightarrow \left(x+\frac{x}{x^2+y^2},y-\frac{y}{x^2+y^2}\right).
\\]

To see this in action, move the point \\(P\\) in this diagram about, ensuring that
the point \\((-1,0)\\) always remains within the circle:

<div id="box" class="jxgbox" style="width:750px; height:600px;">
<script type="text/javascript">
 var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-3, 2.4, 1.6, -2],
                                            axis:true,
                                            keepAspectRatio:true});
 var p1 = board.create('point', [-1, 0], {size: 4,name: 'P'});
 var midpt = board.create('point', [function(){ return (p1.X()+1)/2.0; },
                                    function(){ return p1.Y()/2.0; }], {name:'',size: 0});
 var c = board.create('circle',[midpt, p1]);
 function fx(s) {
  return midpt.X()+c.Radius()*Math.cos(s);
 }
 function fy(s) {
  return midpt.Y()+c.Radius()*Math.sin(s);
 }
 function gx(t) {
  return fx(t)+fx(t)/(fx(t)*fx(t)+fy(t)*fy(t));
 }
 function gy(t) {
       return fy(t)-fy(t)/(fx(t)*fx(t)+fy(t)*fy(t));
      }
 var foil = board.create('curve',[function(phi){return gx(phi);},
                                  function(phi){return gy(phi);},
                                  0, 2*Math.PI],
                                  {strokeColor: 'green',strokeWidth: 4,name:''});
</script>
</div>

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
