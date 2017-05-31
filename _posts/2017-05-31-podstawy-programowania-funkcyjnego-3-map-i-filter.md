---
layout: post
title: "Podstawy Programowania Funkcyjnego #3 map i filter"
titlePL: "Podstawy Programowania Funkcyjnego #3 map i filter"
description: ""
category: Article
date: 2017-05-31 20:06:00 +0100
ima: "/assets/img/fp3.png"
tags: ["DajSiePoznac","Javascript","Programowanie Funkcyjne"]
linkTitle: [ 
		{a: "Czym jest Map, a czym jest Filter?", b: "a1"}
		]
excerpt_separator: <!--more-->
---
<!-- {% highlight javascript %} 
{% endhighlight %} -->
{% include JB/setup %}
<center>
<img src="{{ site.baseurl }}/assets/img/js.png" style="display: inline-block;">
<img src="{{ site.baseurl }}/assets/img/fp.png" style="display: inline-block;">
</center>
<p>W tej serii uczymy się Podstaw Programowania Funkcyjnego. Wykorzystamy przy okazji wiedzę zdobytą w <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">serii dotyczącej ES6</a>. Poznaliśmy już funkcję <code>Filter</code>, tym razem zajmiemy się transformowaniem jednej tablicy w drugą. Pomoże nam przy tym funkcja <code>map</code>. Zaczynajmy!</p><!--more-->


<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Czym jest Map, a czym jest Filter?</h3>
<p>Pamiętacie przykład zastosowania funkcji <code>filter</code>, zawierający odnośniki do filmu Pulp Fiction? Znajduje się w <a href="https://www.idaszak.com/article/2017/05/07/es6-4-arrow-functions-funkcje-strzalkowe">cześci dotyczącej arrow functions</a>. Przywołam go tutaj, ponieważ będzie nam potrzebny.</p>
{% highlight javascript %} 
const characters = [
	{name: 'Butch' , role: 'Boxer'},
	{name: 'Vincent', role: 'Hitman'},
	{name: 'Winston', role: 'Problem solver'},
	{name: 'Jules', role: 'Hitman'},
];

var HitMan = characters.filter(ev => ev.role == 'Hitman');

console.log(HitMan); //[{name: "Vincent", role: "Hitman"}, {name: "Jules", role: "Hitman"}]
{% endhighlight %}
<p>Powyższy kod zwraca nową tablicę obiektów stworzoną z tablicy <code>characters</code>. Jak możemy zauważyć, funkcja <code>filter</code> to <code>higher order function</code>, które poznaliśmy w poprzedniej części <a href="https://www.idaszak.com/article/2017/05/25/podstawy-programowania-funkcyjnego-2-closures-domkniecia#a3">podstaw programowania funkcyjnego</a>, ponieważ jako argument przyjmuje inną funkcję.</p>
<p>Skorzystaliśmy już z listy płatnych zabójców, jednak nie wszystko poszło po naszej myśli. W ramach zemsty chcemy stworzyć czytelną listę osób wraz z ich profesjami, po to by umieścić ją w policyjnej bazie danych. Znamy już pierwszą operację na tablicy <code>filter</code> i potrafimy wyszukać potrzebne nam elementy. Jednak tym razem będziemy potrzebowali funkcji, która pomoże nam przetransformować odpowiednie obiekty. Zaimplementujmy taką funkcję.</p>
{% highlight javascript %} 
function map(callback, array) {
    var newArray = [];
    for (var i = 0; i < array.length; i++) {
        newArray[i] = callback(array[i], i);
    }
    return newArray;
}
{% endhighlight %}
<p>Funkcja przyjmuje dwa argumenty, kolejną funkcję <code>callback</code> i pierwotną tablicę <code>array</code>. Funkcja tworzy tablicę, a następnie iteruje po tablicy przekazanej jako argument i wykonuje funkcję którą podaliśmy przy wywołaniu. Elementy dodają się do nowej tablicy, a na końcu cała tablica zostaje zwrócona.</p>
{% highlight javascript %} 
var PoliceDB = map(a => a.name + " is " + a.role, characters);
console.log(PoliceDB); //["Butch is Boxer", "Vincent is Hitman", "Winston is Problem solver", "Jules is Hitman"]
{% endhighlight %}
<p>Wykonując naszą funkcję map, jako pierwszy argument podaliśmy prostą funkcję, która komponuje <code>name</code> i <code>role</code> w string, a jako drugi tablicę, na której kod ma być wykonany.</p>
<p>Na szczęście nie musimy implementować funkcji map, za każdym razem kiedy będziemy tworzyć projekt. Javascript posiada funkcję map wbudowaną do każdej tablicy. Jest zbudowana w taki sposób, że bardzo łatwo użyć jej z innnymi funkcjami tablicowymi naraz, ponieważ nie przyjmuje tablicy jako argument, tak jak stworzyliśmy to powyżej.
Dlatego możemy stworzyć łańcuchy operacji na tablicy - <code>array.filter(...).map(...);</code></p>
{% highlight javascript %} 
var PoliceDB = characters.map(a => a.name + " is " + a.role);
console.log(PoliceDB); //["Butch is Boxer", "Vincent is Hitman", "Winston is Problem solver", "Jules is Hitman"]
{% endhighlight %}