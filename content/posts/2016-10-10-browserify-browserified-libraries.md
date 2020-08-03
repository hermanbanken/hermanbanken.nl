---
title: Browserify browserified libraries
author: hbanken
type: post
date: 2016-10-10T13:26:14+00:00
url: /2016/10/10/browserify-browserified-libraries/
categories:
  - No category

---
Browserify does not like to eat his own output. When creating a npm library that is browserified make sure to derequire/minify the output so it can be reused in other projects without mystical &#8216;Cannot find module X&#8217; errors where X is some file you know nothing about.

Possible fixes found in the wild:

  * Setting `noParse` for bundle A in the bundle B operation; or
  * Running the standalone bundle through `derequire`; or
  * Minifying the bundle

Summary,  
if you own a library: make sure to minify or `derequire` the package,  
if you use a library: make sure to `noParse` the library.

Reference: https://github.com/substack/node-browserify/pull/1151