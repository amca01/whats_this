+++
title = "Python GIS, and election results"
author = ["Alasdair McAndrew"]
date = 2018-03-31
tags = ["python", "voting"]
draft = false
mathjax = true
+++

## Election mapping {#election-mapping}

A few weeks ago there was a by-election in my local electorate (known as an
electoral _division_) of Batman here in Australia.  I was interested in
comparing the results of this election with the previous election two years ago.
In this division it's become a two-horse race: the Greens against the Australian
Labor Party.  Although Batman had been a solid Labor seat for almost its entire
existence - it used to be considered one of the safest Labor seats in the
country - over the past decade or so the Greens have been making inroads into
this Labor heartland, to the extent that is no longer considered a safe seat.
And in fact for this particular election the Greens were the popular choice to
win.  In the end Labor won, but my interest is not so much tracing the votes,
but trying to map them.

Python has a vast suite of mapping tools, so much so that it may be that Python
has become the GIS tool of choice.  And there are lots of web pages devoted to
discussing these tools and their uses, such as [this
one](<http://matthewrocklin.com/blog/work/2017/09/21/accelerating-geopandas-1>).

My interest was producing maps such as are produced by
[pollbludger](<https://www.pollbludger.net/by-elections/fed-2018-03-batman.htm>)
This is the image from that page:

\![pollbludger](/pollbludger\_batman.png)

As you can see there are basically three elements:

-   the underlying streetmap
-   the border of the division
-   the numbers showing the percentage wins of each party at the various polling
    booths.

I wanted to do something similar, but replace the numbers with circles whose
sizes showed the strength of the percentage win at each place.


## Getting the information {#getting-the-information}

Because this election was in a federal division, the management of the polls and
of the results (including counting the votes) was managed by the Australian
Electoral Commission, whose [pages about this by-election](
<http://www.aec.gov.au/Elections/supplementary_by_elections/2018-batman/>) contain
pretty much all publicly available information.  You can copy and paste the
results from their pages, or download them as CSV files.

Then I needed to find the coordinates (Longitude and Latitude) of all the
polling places, of which there were 42 at fixed locations.  There didn't seem to
be a downloadable file for this, so for each booth address (given on the AEC
site), I entered it into Google Maps and copied down the coordinates as given.

The boundaries of all the divisions can again be downloaded from the [AEC GIS
page](<http://www.aec.gov.au/Electorates/gis/index.htm>).  These are given in
various standard GIS files.


## Putting it all together {#putting-it-all-together}

The tools I felt brave enough to use were:

-   [Pandas:](<https://pandas.pydata.org>) Python's data analysis library.  I
    really only needed to read information from CSV files that I could then use
    later.
-   [Geopandas:](<http://geopandas.org>) This is a GIS library with Pandas-like
    syntax, and is designed in part to be a GIS extension to Pandas.  I would
    use it to extract and manage the boundary data of the electoral division.
-   [Cartopy:](<http://scitools.org.uk/cartopy/>) which is a library of
    "cartographic tools".

And of course the standard [matplotlib](<http://matplotlib.org>) for plotting,
[numpy](<http://www.numpy.org>) for array handling.

My guides were the [London tube stations
example](<http://scitools.org.uk/cartopy/docs/latest/gallery/tube_stations.html>)
from Cartopy and a local (Australian) data analysis blog which discussed the
[use of Cartopy](<http://www.net-analysis.com/blog/cartopytiles.html>) including
adding graphics to an map image.

There are lots of other GIS tools for Python, some of which seem to be very good
indeed, and all of which I downloaded:

-   [Fiona](<https://github.com/Toblerity/Fiona>): which is a "nimble" API for
    handling maps
-   [Descartes](<https://bitbucket.org/sgillies/descartes/>): which provides a
    means by which matplotlib can be used to manage geographic objects
-   [geoplotlib](<https://github.com/andrea-cuttone/geoplotlib>): for "visualizing
    geographical data and making maps"
-   [Folium](<http://python-visualization.github.io/folium/>): for visualizing maps
    using the [leaflet.js](<http://leafletjs.com>) library.  It may be that the
    mapping I wanted to do with Python could have been done just as well in
    Javascript alone.  And probably other languages.  I stuck with Python simply
    because I knew it best.
-   [QGIS](<https://qgis.org/en/site/>): which is designed to be a complete free
    and open source GIS, and with APIs both for Python and C++
-   [GDAL](<http://www.gdal.org>): the "Geospatial Data Abstraction Library" which
    has a [Python package](<https://pypi.python.org/pypi/GDAL>) also called GDAL,
    for manipulating geospatial raster and vector data.

I suspect that if I was professionally working in the GIS area some or all of
these packages would be at least as - and maybe even more - suitable than the
ones I ended up using.  But then, I was starting from a position of absolute
zero with regards to GIS, and also I wanted to be able to make use of the tools
I already knew, such as Pandas, matplotlib, and numpy.

Here's the start, importing the libraries, or the bits of them I needed:

```python
import matplotlib.pyplot as plt
import numpy as np
import cartopy.crs as ccrs
from cartopy.io.img_tiles import GoogleTiles
import geopandas as gpd
import pandas as pd
```

I then had to read in the election data, which was a CSV files from the AEC
containing the Booth, and the final distributed percentage weighting to the ALP
and Greens candidates, and heir percentage scores.  As well, I read in the
boundary data:

```python
bb = pd.read_csv('Elections/batman_booths_coords.csv')  # contains all election info plus lat, long of booths
longs = np.array(bb['Long'])
lats = np.array(bb['Lat'])
v = gpd.read_file('VicMaps/VIC_ELB.MIF')  # all electoral divisions in MapInfo form
bg = v.loc[2].geometry                    # This is the Polygon representing Batman
b_longs = bg.exterior.xy[0]               # These next two lines are the longitudes and latitudes
b_lats = bg.exterior.xy[1]                #
```

Notice that `bb` uses Pandas to read in the CSV files which contains all the AEC
information, as well as the latitude and longitude of each Booth, which I'd
added myself.  Here `longs` and `lats` are the coordinates of the polling
booths, and `b_longs` and `b-lats` are all the vertices which form the boundary
of the division.

Now it's all pretty straigtforward, especially with the examples mentioned above:

```python
fig = plt.figure(figsize=(16,16))

tiler = GoogleTiles()
ax = plt.axes(projection=tiler.crs)

margin=0.01
ax.set_extent((bg.bounds[0]-margin, bg.bounds[2]+margin,bg.bounds[1]-margin, bg.bounds[3]+margin))

ax.add_image(tiler,12)
for i in range(44):
    plt.plot(longs[i],lats[i],ga2[i],markersize=abs(ga[i]),alpha=0.7,transform=ccrs.Geodetic())

plt.plot(b_longs,b_lats,'k-',linewidth=5,transform=ccrs.Geodetic())
plt.title('Booth results in the 2018 Batman by-election')
plt.show()
```

Here `GoogleTiles` provide the street map to be used as the "base" of our map.
Open Streep Map (as OSM) is available too, but I thin in this instance, Google
Maps is better.  Because the map is rendered as an image (with some unavoidable
blurring), I find that Google gave a better result than OSM.

Also, `ga2` is a little array which simply produces plotting of the style `ro`
(red circle) or `go` (green circle).  Again, I make the program do most of the
work.

And here is the result, saved as an image:

\![Batman 2018](/batman2018trim.png)

I'm quite pleased with this output.

And a quick check of some maths, first inline
$ (x+2y)^3=x^3+6x^2y+12xy^2+8y^3 $ and also displayed:
<div>
\\[
\int^\infty\_{-\infty}e^{-x^2}\,dx=\sqrt{\pi}.
\\]
</div>

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
