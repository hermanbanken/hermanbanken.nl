---
title: Overerving in JavaScript
author: hbanken
type: post
date: 2010-11-02T14:35:14+00:00
url: /2010/11/02/overerving-in-javascript/
categories:
  - JavaScript

---
Sinds de ontwikkeling van JavaScript wordt het al onderschat. Eerst was de naam LiveScript, maar dat was niet verwarrend genoeg, dus is LiveScript hernoemd naar JavaScript. En dat terwijl het niet van Java afstamt, het niet op Java is gebaseerd en niet op Java lijkt, althans niet meer dan andere programmeertalen.

In JavaScript ontbreken classes en voor programmeurs die Java of C++ gewend zijn is dat wennen, maar zaken als inheritance zijn toch mogelijk in JavaScript door middel van prototypes.

<!--more-->

## Voorbeelden

Voor de volgende voorbeelden worden een paar [snippets][1] gebruikt. Deze zijn afkomstig van [Douglas Crockford][2].

### Normale overerving

<pre>function Animal(name)
{
	this.setName(name);
}

Animal.method('setName', function(value){
	this.name = value;
});

Animal.method('getName', function(){
	return this.name;
});

Animal.method('toString', function(){
	return 'Animal(' + this.getName() + ')';
});</pre>

Eerst maken we een klasse Animal, de parent classe. Daarna voegen we daar methoden aan toe. De functie &#8216;method&#8217; komt uit de snippets en zorgt ervoor dat de methoden aan het prototype worden toegevoegd.

We kunnen nu dus dit doen:

<pre>animal = new Animal("Klaartje");
console.log(animal.toString());</pre>

Er staat nu zoals verwacht &#8220;Animal(Klaartje);&#8221; in de console.

Nu maken we een nieuwe classe Koe en vervangen de methode toString:

<pre>function Koe(name)
{
	this.setName(name);
}

Koe.inherits(Animal);

Koe.method('toString', function(){
	return 'Koe(' + this.getName() + ')';
});</pre>

We kunnen nu dus dit doen:

<pre>koe = new Koe("Klaartje");
console.log(koe.toString());</pre>

Er staat nu &#8220;Koe(Klaartje);&#8221; in de console. De functies getName en setName zijn namelijk over geerfd.

Het is dus mogelijk met javascript zaken als overerving te regelen, ook al zijn er eigenlijk geen classes.

### Uitgebreide overerving

Met de &#8220;swiss&#8221;-methode is het mogelijk om een classe op te bouwen uit methodes van meerdere verschillende classes.

<pre>function Viervoeter(name, length)
{
	this.setName(name);
	this.setLength(length);
}

Viervoeter('setLength', function(value){
	this.length = value;
});

Koe.swiss(Viervoeter, "setLength");</pre>

We kunnen nu dit schrijven:

<pre>dier = new Koe("Henk");
dier.setLength("2 meter");
dier.setName("Mike");</pre>

Het is nu dus mogelijk methoden uit Koe, uit Viervoeter en uit Animal te gebruiken.

## Super

Zoals Java super heeft, zit er in de snippets voor JavaScript de functie &#8220;uber&#8221; ingebouwd. Met behulp van deze functie kunnen we de constructor van Viervoeter herschrijven:

<pre>function Viervoeter(name, length)
{
	uber(name);
	this.setLength(length);
}

Viervoeter.inherits(Animal);</pre>

## Conclusie

Door enkele kleine functies te schrijven (method, inherits, swiss) is er in JavaScript binnen no-time een goede vervanger voor classes. Het mooie van JavaScript is dan ook dat het zo flexibel is.

## Snippets {#snippets}

De methode uit de eerste snippet zorgt ervoor dat er een methode wordt toegevoegd aan het prototype van de class. Omdat de methode niets hoeft terug te geven returnen we this zodat het mogelijk is de functies aan een te rijgen.

<pre>Function.prototype.method = function (name, func)
{
    this.prototype[name] = func;
    return this;
};</pre>

De tweede methode is iets spannender. Deze krijgt één parameter mee, namelijk de classe die inherited/extended moet worden. De methode stelt het prototype van de functie in op een instancie van de parent en voegt ook nog één methode &#8220;uber&#8221; toe. Deze &#8220;uber&#8221;-methode is in feite de &#8220;super&#8221; uit de Java wereld.

<pre>Function.method('inherits', function (parent) {
    var d = {}, p = (this.prototype = new parent());
    this.method('uber', function uber(name) {
        if (!(name in d)) {
            d[name] = 0;
        }
        var f, r, t = d[name], v = parent.prototype;
        if (t) {
            while (t) {
                v = v.constructor.prototype;
                t -= 1;
            }
            f = v[name];
        } else {
            f = p[name];
            if (f == this[name]) {
                f = v[name];
            }
        }
        d[name] += 1;
        r = f.apply(this, Array.prototype.slice.apply(arguments, [1]));
        d[name] -= 1;
        return r;
    });
    return this;
});</pre>

Wanneer je enkele maar niet alle methode van een classe wilt overerven kan dat ook met de methode &#8220;swiss&#8221;. Deze heeft één vaste parameter parent waarvan moet worden overgeerfd en alle overige gegeven parameters worden opgevat als functienamen. Deze worden vervolgens toegevoegd aan het prototype van de nieuwe classe.

<pre>Function.method('swiss', function (parent) {
	var a = arguments;
    for (var i = a.length; --i;) {
        this.prototype[a[i]] = parent.prototype[a[i]];
    }
    return this;
});</pre>

Bron: Douglas Crockford&#8217;s [javascript.crockford.com][2]

 [1]: #snippets
 [2]: http://javascript.crockford.com