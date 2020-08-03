---
title: Floating point precision, in practice
author: hbanken
type: post
date: 2016-04-18T19:32:48+00:00
url: /2016/04/18/floating-point-precision-in-practice/
categories:
  - No category

---
Normally as a programmer Float&#8217;s are more than precise enough. During one of my projects however I had some serious headaches because of it. At start of the project I was rolling my own points in 3D-space, just a fancy wrapper around 3 Doubles. When I needed more vector powers (cross products, quaternations, rotation matrices) I switched to `com.github.jpbetz.subspace.Vector3` from https://github.com/jpbetz/subspace. This Vector3 uses 3 Floats internally, which was fine, cause it wouldn&#8217;t make that much of a difference, I thought.

The project I&#8217;m working on includes fitting some points to a predefined shape (a inline skate track to be precise). I was calculating the center of the GPS input like this:

<pre class="brush: scala; title: ; notranslate" title="">val vs: Seq[Vector3] = /* about 3600 points */ ???
val center = vs.reduce(_ + _) / vs.size</pre>

Using this center I plotted a blue &#8216;perfect&#8217; track and my input points in green. The result:  
[<img class="aligncenter wp-image-529 size-large" src="https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-wrong-1024x905.png" alt="GPX points wrongly centered" width="660" height="583" srcset="https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-wrong-1024x905.png 1024w, https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-wrong-300x265.png 300w" sizes="(max-width: 660px) 100vw, 660px" />][1]

As you can see there is some offset between the green and the blue tracks. The orientation is different too, but first I wanted to fix the positioning problem. After many hours of thinkering and continuing with other issues, I came across another issue with my Floats: the formatting back to GPS longitude and latitudes. For some reason I wrongly typed the formatting string, but due to this issue my attention shifted to floating point precision and it&#8217;s quirks. Then I wrote this little test:

<pre class="brush: scala; title: ; notranslate" title="">val sum_d = vs
  .map(v =&gt; (v.x.toDouble, v.y.toDouble, v.z.toDouble))
  .reduce((a,b) =&gt; (a._1+b._1, a._2+b._2, a._3+b._3))
val center_d = Vector3(
  (sum_d._1/vs.size).toFloat,
  (sum_d._2/vs.size).toFloat,
  (sum_d._3/vs.size)).toFloat
)
var center = vs.reduce(_ + _) / vs.size
println(s"Center: $center")
println(s"Center using doubles: $center_d")
println(s"Diff: ${center - center_d}")</pre>

which printed

<pre class="brush: scala; title: ; notranslate" title="">Center: (3912099.2, 301133.1, 5028436.0)
Center using doubles: (3912057.8, 301132.88, 5028494.5)
Diff = (41.5, 0.21875, -58.5)</pre>

Tada! Issue found. Adding all those large numbers (vectors relative to the earths center) caused the precision of the resulting float to be too little to accomodate for the large numbers plus their precision. Using doubles to store the accumulating numbers results in a better center, which produces the following image:

[<img class="aligncenter wp-image-528 size-large" src="https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-good-1024x908.png" alt="GPX points correctly centered" width="660" height="585" srcset="https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-good-1024x908.png 1024w, https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-good-300x266.png 300w" sizes="(max-width: 660px) 100vw, 660px" />][2]

Much better! So remember, floats are not always good enough.

 [1]: https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-wrong.png
 [2]: https://hermanbanken.nl/wp-content/uploads/2016/04/gpx-center-good.png