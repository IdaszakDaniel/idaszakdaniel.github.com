---
layout: post
title: "Podstawy Programowania Funkcyjnego #4 Partial Aplication, Currying"
titlePL: "Podstawy Programowania Funkcyjnego #4 Partial Aplication, Currying"
description: ""
category: Article
date: 2017-10-22 13:06:00 +0100
ima: "/assets/img/fp5.png"
tags: ["JavaScript","Programowanie Funkcyjne"]
linkTitle: [ 
		{a: "Partial Application", b: "a1"},
		{a: "Currying", b: "a2"},
		{a: "Currying - zastosowania", b: "a3"}
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
<p>W tej serii uczymy się Podstaw Programowania Funkcyjnego. Wykorzystamy przy okazji wiedzę zdobytą w <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">serii dotyczącej ES6</a>. Tym razem dowiemy się czym są oraz czym się różnią terminy Partial Application oraz Currying. Zaczynajmy!</p><!--more-->

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Partial Application</h3>
<p>Standardowo dla stworzonej wcześniej funkcji, przy wywołaniu "aplikujemy" jednocześnie wszystkie argumenty i oczekujemy na jej rezultat. 
Wyobraźmy sobie panel sterowania statku kosmicznego, w którym musimy podawać komunikaty o kolorze świecącej się diody:</p>
<script src="https://gist.github.com/IdaszakDaniel/8291b02eaaed83a7d351c10741ad758e.js"></script>
<!-- {% highlight javascript %} 
let checkLed = (a,b) => console.log(`${a} ma kolor ${b}`);

checkLed("dioda","czerwony"); //"dioda ma kolor czerwony"
checkLed("dioda","zielony"); //"dioda ma kolor zielony"
{% endhighlight %} -->
<p>Jeżeli świeci się czerwona dioda, wyświetlimy komunikat <code>checkLed("dioda","czerwony");</code>, jednak kod traci na czytelności i za każdym razem musimy używać słowa "dioda".</p>
<p>Żeby wyeliminować ciągłe powtarzanie tego elementu zastosujemy częściową aplikację argumentów (Partial Application), równocześnie zachowując możliwość zmiany słowa "dioda" w przyszłości.</p>
<script src="https://gist.github.com/IdaszakDaniel/02413c98e8eaf5952b86ca46e4a5299e.js"></script>
<!-- {% highlight javascript %} 
let checkLed = (a,b) => console.log(`${a} ma kolor ${b}`);

let checkDiode = y => checkLed("dioda", y);

checkDiode("czerwony"); //"dioda ma kolor czerwony"
checkDiode("niebieski"); //"dioda ma kolor niebieski"
checkDiode("biały"); //"dioda ma kolor biały"
{% endhighlight %} -->
<p>Jeśli nie korzystamy z <a href="https://www.idaszak.com/article/2017/05/07/es6-4-arrow-functions-funkcje-strzalkowe">funkcji strzałkowych</a>, to możemy skorzystać z funkcji bind, lub stworzyć <a href="https://www.idaszak.com/article/2017/05/25/podstawy-programowania-funkcyjnego-2-closures-domkniecia#a3">Higher Order Function</a>:</p>
<script src="https://gist.github.com/IdaszakDaniel/f9c1d3996edb129c840ace2952bfc360.js"></script>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Currying</h3>
<p>Na jednej z rozmów kwalifikacyjnych padło pytanie, czy <code>funkcja(a)(b)</code> jest poprawnym kodem w języku JavaScript. To dziwne wywołanie kodu to Currying. Currying jest terminem, którego nie da się przetłumaczyć, ponieważ pochodzi od nazwiska matematyka - Haskell Brooks Curry. Na jego cześć został nazwany język programowania "Haskell".</p>
<p>A więc czym jest currying? To przetwarzanie funkcji o wielu argumentach we funkcje jednoargumentowe. Dzięki temu otrzymujemy funkcję, która przyjmuje po jednym argumencie na każdym etapie wywołania.</p>
<p>Co to w praktyce oznacza? Funkcja po przyjęciu pierwszego argumentu zwraca kolejną funkcję przyjmującą kolejny, pojedynczy argument. Na początek zróbmy zwykłą funkcję przyjmującą trzy argumenty:</p>
<script src="https://gist.github.com/IdaszakDaniel/975f317f18105603b0380fdd46e308d7.js"></script>
<!-- {% highlight javascript %} 
let fun1 = function(color,item,light){
  return color + ' ' + item + ' ' + light;
}
console.log(fun1("zielona","kontrolka","pulsuje")); //zielona kontrolka pulsuje
{% endhighlight %}  -->

<p>A teraz dla porównania stwórzmy przy użyciu <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">ES6</a> funkcję przyjmującą po jednym argumencie:</p>
<script src="https://gist.github.com/IdaszakDaniel/5e6e2634af0e1d718894abfc43e3221d.js"></script>
<!-- {% highlight javascript %} 
let checkIndicator = 
    color =>
      item =>
        light =>
          `${color}, ${item}, ${light}`;

console.log(checkIndicator("zielona")("kontrolka")("pulsuje")); ////zielona kontrolka pulsuje
{% endhighlight %}  -->
<p>Funkcyjne języki programowania posiadają funkcje, które pozwalają na Currying zwykych funkcji. Niestety w JavaScript nie mamy takiej funkcji wbudowanej, dlatego musimy skorzystać z biblioteki <a href="https://lodash.com/">Lodash</a>:</p>
<script src="https://gist.github.com/IdaszakDaniel/29ba9596719149cff5a9a2d32d4b5f25.js"></script>
<!-- {% highlight javascript %} 
let checkIndicator = function(color,item,light){
  return color + ' ' + item + ' ' + light;
}

curriedCheckIndicator = _.curry(checkIndicator);

console.log(curriedCheckIndicator("niebieska")("kontrolka")("nie świeci")); ////niebieska kontrolka nie świeci
{% endhighlight %} 
 -->
<p>Używamy zwykłej funkcji <code>checkIndicator</code>, którą stworzyliśmy wcześniej i używamy funkcji Lodash <code>_.curry()</code>. Teraz możemy aplikować po jednym argumencie.</p>

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Currying - zastosowania</h3>

<p>Dlaczego powinno się stosować currying, skoro na pierwszy rzut oka nie ma to żadnych korzyści?</p>
<p>W miarę poznawania kolejnych zagadnień programowania funkcyjnego currying będzie coraz bardziej potrzebny. Ułatwi nam pracę z <a href="https://www.idaszak.com/article/2017/05/31/podstawy-programowania-funkcyjnego-3-map-i-filter">map</a> oraz kompozycją funkcji, którą poznamy wkrótce na blogu. Poza tym, currying daje nam dużą elastyczność, pozwala na reusability poprzednich funkcji, oraz pozwala stosować partial application bez modyfikacji pierwotnej funkcji:</p>
<script src="https://gist.github.com/IdaszakDaniel/af48655be2ea30a71900df68bde1d6e2.js"></script>
<!-- {% highlight javascript %} 
let checkIndicator = function(color,item,light){
  return color + ' ' + item + ' ' + light;
}

curriedCheckIndicator = _.curry(checkIndicator);

let redIndicator = curriedCheckIndicator("czerwona");

console.log(redIndicator("kontrolka")("świeci")); //czerwona kontrolka świeci
console.log(redIndicator("dioda")("pulsuje")); //czerwona dioda pulsuje
{% endhighlight %}  -->
<p>Po użyciu funkcji <code>_.curry()</code> podajemy pierwszy argument <code>"czerwona"</code> a dopiero później podajemy resztę argumentów. Nasza pierwotna funkcja <code>checkIndicator</code> nie musi zwracać kolejnej funkcji żeby zrobić <code>partial application</code>. Dzięki temu zabiegowi początkowa funkcja nie musi być w ogóle modyfikowana, a cel jakim była częściowa aplikacja argumentów został zrealizowany.</p>