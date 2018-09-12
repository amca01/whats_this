+++
title = "Exploring JSXGraph"
author = ["Alasdair McAndrew"]
date = 2018-04-14
tags = ["jsxgraph"]
draft = false
mathjax = true
+++

[JSXGraph](<https://jsxgraph.org/wp/index.html>) is a graphics package deveoped in
Javascript, and which seems to be tailor-made for a static blog such as this.
It consists of only two files: the javascript file itself, and an accompanying
css file, which you can download.   Alternaively you can simply link to the
online files at the Javascript content delivery site
[cdnjs](<https://cdnjs.com/about>) managed by
[cloudflare](<https://www.cloudflare.com/>).  There are cloudflare servers all
over the world - even in my home town of Melbourne, Australia.

So I modified the `head.html` file of my theme to include a link to the
necessary files:

```html

```

So I downloaded the javascript and css files as described
[here](<https://jsxgraph.uni-bayreuth.de/wp/download/index.html>)
and also, for good measure, added the script line (from that page) to the
`layouts/partials/head.html` file of the theme.  Then copied the following
snippet from the JSXGraph site:

```html
<div id="box" class="jxgbox" style="width:500px; height:500px;"></div>
<script type="text/javascript">
 var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-10, 10, 10, -10], axis:true});
</script>
```

However, to make this work the entire script needs to be inside a `<div>`,
`</div>` pair, like this:

```html
<div id="box" class="jxgbox" style="width:500px; height:500px;">
<script type="text/javascript">
 var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-10, 10, 10, -10], axis:true});
</script>
</div>
```

Just to see how well this works, here's Archimedes' _neusis_ construction of an
angle trisection: given an angle \\(\theta\\) in a unit semicircle, its trisection is
obtained by laying against the circle a straight line with points spaced 1
apart (drag point A about the circle to see this in action):

<div id="box" class="jxgbox" style="width:750px; height:500px;">
<script type="text/javascript">
 JXG.Options.axis.ticks.insertTicks = false;
 JXG.Options.axis.ticks.drawLabels = false;
 var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-1.5, 1.5, 3, -1.5],axis:true,keepAspectRatio:true});
 var p = board.create('point',[0,0],{visible:false,fixed:true});
 var neg = board.create('point',[-0.67,0],{visible:false,fixed:true});
 var c = board.create('circle',[[0,0],1.0]);
 var a = board.create('glider',[-Math.sqrt(0.5),Math.sqrt(0.5),c],{name:'A'});
 var l1 = board.create('segment',[a,p]);
 var ang = board.create('angle',[a,p,neg],{radius:0.67,name:'θ',type:'sector'});
 var theta = JXG.Math.Geometry.rad(a,p,neg);
 var bb = board.create('point',[
          () => Math.cos(Math.atan2(a.Y(),-a.X())/3),
          () => Math.sin(Math.atan2(a.Y(),-a.X())/3)
          ],{name:'B'});
 var w = board.create('point',[() =>  2*Math.cos(Math.atan2(a.Y(),-a.X())/3),0]);
 var l2 = board.create('line',[a,w]);
 var l3 = board.create('segment',[p,bb]);
 var l4 = board.create('segment',[bb,w],{strokeWidth:6,strokeColor:'#FF0000'});
 var ang2 = board.create('angle',[bb,w,neg],{radius:0.67,name:'θ/3'});
</script>
</div>

For what it's worth, here is the splendid javascript code to produce the above
figure:

```html
<div id="box" class="jxgbox" style="width:500px; height:333.33px;">
<script type="text/javascript">
 JXG.Options.axis.ticks.insertTicks = false;
 JXG.Options.axis.ticks.drawLabels = false;
 var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-1.5, 1.5, 3, -1.5],axis:true});
 var p = board.create('point',[0,0],{visible:false,fixed:true});
 var neg = board.create('point',[-0.67,0],{visible:false,fixed:true});
 var c = board.create('circle',[[0,0],1.0]);
 var a = board.create('glider',[-Math.sqrt(0.5),Math.sqrt(0.5),c],{name:'A'});
 var l1 = board.create('segment',[a,p]);
 var ang = board.create('angle',[a,p,neg],{radius:0.67,name:'θ'});
 var theta = JXG.Math.Geometry.rad(a,p,neg);
 var bb = board.create('point',[function(){return Math.cos(Math.atan2(a.Y(),-a.X())/3);},function(){return Math.sin(Math.atan2(a.Y(),-a.X())/3);}],{name:'B'});
 var w = board.create('point',[function(){return Math.cos(Math.atan2(a.Y(),-a.X())/3)/0.5;},0]);
 var l2 = board.create('line',[a,w]);
 var l3 = board.create('segment',[p,bb]);
 var l4 = board.create('segment',[bb,w],{strokeWidth:6,strokeColor:'#FF0000'});
 var ang2 = board.create('angle',[bb,w,neg],{radius:0.67,name:'θ/3'});
</script>
</div>
```

Quite wonderful, it is.

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
