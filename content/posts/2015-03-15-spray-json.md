---
title: Spray JSON
author: hbanken
type: post
date: 2015-03-15T21:28:41+00:00
url: /2015/03/15/spray-json/
categories:
  - Coding
tags:
  - json
  - scala

---
So you&#8217;re making yet another Scala app and you need to parse or write some data to file/database/webservice.

What do you do? If you&#8217;re not already inside an (awesome) framework like Play Framework you will probably search &#8220;Scala JSON&#8221; and find something like [Spray JSON][1]. That&#8217;s what I did. So then you write:

<pre class="brush: scala; title: ; notranslate" title="">import scalax.io._
import spray.json._
import DefaultJsonProtocol._

val jsonString = largeMapObject.toJson.compactPrint
Resource.fromFile("someFile").write(jsonString)(Codec.UTF8)</pre>

&#8230; and you get an exception upon either writing, or parsing this string after it was only partially written. You have several megabytes of this so just writing to a String will not work.

You will need a JSON printer that can output streams or handle writers. After searching I found Scala Stuff&#8217;s [json-parser][2] that has support for several JSON AST libraries like Spray. With json-parser writing large JSON objects is just a matter of:

<pre class="brush: scala; title: ; notranslate" title="">import org.scalastuff.json.spray._
val writer = new PrintWriter(new File(file))
new SprayJsonPrinter(writer, 0)(largeMapObject.toJson)
writer.close()</pre>

and reading is just as easy:

<pre class="brush: scala; title: ; notranslate" title="">val reader = Source.fromFile(file).bufferedReader()
val jsValue = SprayJsonParser.parse(reader)</pre>

Although this basic usage of Spray and Scala Stuff&#8217;s json-parser allow you to parse and write large JSON object, your machines memory is still a bottleneck as they keep the resulting JSON AST in memory. When dealing with huge JSON streams I recommend that you take a look at json-parser&#8217;s [JsonHandler trait][3] or Play Frameworks [JSON Transformers][4].

 [1]: https://github.com/spray/spray-json#installation
 [2]: https://github.com/scalastuff/json-parser
 [3]: https://github.com/scalastuff/json-parser/blob/master/src/main/scala/org/scalastuff/json/JsonHandler.scala
 [4]: https://playframework.com/documentation/latest/ScalaJsonTransformers