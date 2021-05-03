---
author: chris_bull
comments: true
date: 2013-07-14 02:11:02+00:00
layout: post
slug: scipy-interpolate-griddata
title: Scipy.interpolate.griddata regridding data.
wordpress_id: 108
categories:
- python
tags:
- interpolate
- meshgrid
- python
- regrid
- scipy
---

Climate scientists are always wanting data on different grids. [Python docs](http://docs.scipy.org/doc/scipy-0.11.0/reference/generated/scipy.interpolate.griddata.html) are typically excellent but I couldn't find a nice example using rectangular/mesh grids so here it is...


{% highlight python %}

import numpy as np
import scipy.interpolate

old_grid_data=np.random.rand(4,3)

#old grid dim
loni=np.array([109.94999695, 110.05000305, 110.15000153])
depi=np.array([3.04677272, 9.45404911, 16.36396599, 23.89871025])

#new grid dim
lon=np.arange(110.,110.3,.1) #NB: 110.2 outside of convex hull of old so will produce nan
depth=np.array([3.1,9,16,23])

#create mesh
X, Y = np.meshgrid(loni, depi)
XI, YI = np.meshgrid(lon,depth)

#interp
new_grid=scipy.interpolate.griddata((X.flatten(),Y.flatten()),old_grid_data.flatten() , (XI,YI),method='cubic')

print "this is original"
print old_grid_data.reshape(4,3)
print ""
print "this is interp' by cubic"
print new_grid

print
print "this is diff"
print new_grid-old_grid_data.reshape(4,3)

{% endhighlight %}

NB: the old_grid and data you pass to scipy.interpolate.griddata needs to be flattened or [ravel](http://docs.scipy.org/doc/numpy/reference/generated/numpy.ravel.html) (the error messages are not instructive). You can pass a masked array too!
