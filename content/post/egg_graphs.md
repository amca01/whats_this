+++
title = "Graphs of Eggs"
author = ["Alasdair McAndrew"]
date = 2018-04-20
tags = ["geometry", "jsxgraph"]
draft = false
mathjax = true
+++

I recently came across some nice material on [John Cook's
blog](<https://www.johndcook.com/blog/>) about equations that described eggs.

It turns out there are vast number of equations whose graphs are egg-shaped:
that is, basically ellipse shape, but with one end "rounder" than the other.

You can see lots at Jürgen Köller's [Mathematische
Basteleien](<http://www.mathematische-basteleien.de/eggcurves.htm>) page.
(Although this blog is mostly in German, there are enough English language pages
for monoglots such as me).  And plenty of egg equations can be found in the
[2dcurves](<http://www.2dcurves.com/>) pages.

Another excellent source of eggy equations is [TDCC
Laboratory](<http://www.geocities.jp/nyjp07/index_egg_E.html>) from Japan (the
link here is to their English language page).  For the purposes of experimenting
we will use equations from this TDCC, adjusted as necessary.  Many of their
equations are given in parametric form, which means they can be easily graphed
and explored using [JSXGraph](<https://jsxgraph.org/wp/index.html>).

The first set of parametric equations, whose author is given to be Nobuo
Yamamoto, is:

\begin{align\*}
x&=(a+b+b\cos\theta)\cos\theta\\\\\\
y&=(a+b\cos\theta)\sin\theta
\end{align\*}

If we divide these equations by \\(a\\), and use the parameter \\(c\\) for \\(b/a\\) we
obtain slightly simpler equations:

\begin{align\*}
x&=(1+c+c\cos\theta)\cos\theta\\\\\\
y&=(1+c\cos\theta)\sin\theta
\end{align\*}

Here you can explore values of \\(c\\) between 0 and 1:

<div id="box" class="jxgbox" style="width:750px; height:375px;">
<script type="text/javascript">
 var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-2, 2, 4, -1.5], axis:true,keepAspectRatio:true});
 var c = board.create('slider',[[1,1.5],[3,1.5],[0,0,1]],{name:'c'});
 var egg = board.create('curve',
                       [function(t){ return (1+c.Value()+c.Value()*Math.cos(t))*Math.cos(t);},
                        function(t){ return (1+c.Value()*Math.cos(t))*Math.sin(t);},
                        0, 2*Math.PI],{strokeWidth:4}
                        );

</script>
</div>

Another [set of equations](<http://www.geocities.jp/nyjp07/index_egg_by_Itou_E.html>) is said to be due to [Tadao
Ito](<http://web1.kcn.jp/hp28ah77/us_author.htm>) (whose surname is sometimes
transliterated as Itou):

\begin{align\*}
x&=\cos\theta\\\\\\
y&=c\cos\frac{\theta}{4}\sin\theta
\end{align\*}

<div id="box2" class="jxgbox" style="width:500px; height:375px;">
<script type="text/javascript">
 // var board2 = JXG.JSXGraph.freeBoard(board2);
 var board2 = JXG.JSXGraph.initBoard('box2', {boundingbox: [-1.5, 1.5, 2, -1.5], axis:true,keepAspectRatio:true});
 var c2 = board2.create('slider',[[0.25,1.25],[1.75,1.25],[0,0,1.5]],{name:'c'});
 var egg2 = board2.create('curve',
                       [function(t){ return Math.cos(t);},
                        function(t){ return c2.Value()*Math.cos(t/4)*Math.sin(t);},
                        -Math.PI, Math.PI],{strokeWidth:4}
                        );

</script>
</div>

Many more equations: parametric, implicit, can be found at the sites linked above.

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
