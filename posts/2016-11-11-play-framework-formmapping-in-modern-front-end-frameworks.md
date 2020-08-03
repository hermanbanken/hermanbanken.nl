---
title: Play Framework FormMapping in modern front-end frameworks
author: hbanken
type: post
date: 2016-11-11T16:06:27+00:00
url: /2016/11/11/play-framework-formmapping-in-modern-front-end-frameworks/
categories:
  - No category

---
One of the celebrated features of Play (at least for me) are the built-in form validation and the JSON parsers and combinators. While the team behind Play wants everything to be reactive, they have not build in support to become reactive on the front-end too: Play supports classical HTTP requests and form posts, but the defined form validation is not easily available in say, JavaScript. Looking at all the work done to expose the Routes to JavaScript makes me wonder why no-one made the validation available too.

After having a conversation with another Play user I decided to give it a go and TADA: a converter from FormMapping to JSON. See my latest gist:

<https://gist.github.com/hermanbanken/d65f88167b391bfd9cd72c3f9bef592b>

TLDR: use something like this

<pre class="brush: scala; title: ; notranslate" title="">def jsConstaints = Json.toJson(userForm.mapping.asInstanceOf[Mapping[Any]])(mappingFormat)
</pre>

to generate JSON like this

<pre class="brush: jscript; title: ; notranslate" title="">{
  "key": "",
  "mappings": [
    { "key": "name" },
    { "key": "age", "format": ["format.numeric"] },
    {
      "key": "address",
      "mappings": [
        { "key": "address.street" },
        {
          "key": "address.city",
          "constraints": [ { 
            "name": "constraint.minLength", 
            "args": [ 10 ] 
          } ]
        }
      ]
    }
  ]
}
</pre>