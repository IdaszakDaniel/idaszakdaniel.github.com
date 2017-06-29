---
layout: post
title: "ES6#4 Arrow functions czyli funkcje strzalkowe"
titlePL: "ES6#4 Arrow functions czyli funkcje strzałkowe"
description: ""
category: Article
date: 2017-05-07 13:40:00 +0100
ima: "/assets/img/js.png"
tags: ["DajSiePoznac","JavaScript","ES6","Programowanie Funkcyjne"]
linkTitle: [ 
		{a: "Czym są arrow functions?", b: "a1"},
		{a: "Zastosowanie arrow functions - funkcja filter", b: "a2"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}

<img src="{{ site.baseurl }}/assets/img/js.png" >

<p>W poprzednich częściach serii dotyczącej standardu ECMASCRIPT6 języka JavaScript poznaliśmy słowo kluczowe <code>class</code>, dowiedzieliśmy się czym jest <code>let</code> i <code>const</code> oraz <code>spread</code> i <code>rest</code> przy okazji omawiania destrukturyzacji. Tym razem zajmiemy się chyba najbardziej rozpoznawalną funkcjonalnością ES6, czyli funkcjami strzałkowymi. Arrow Functions pozwalają na skrócenie i zwiększenie czytelności naszego kodu, co jest bardzo pożądanym zjawiskiem przez wszystkich programistów. Zachaczymy też trochę o podstawy programowania funkcyjnego, ponieważ to właśnie przy okazji pisania takiego kodu najbardziej są przydatne te funkcje. Zaczynajmy!</p><!--more-->

<p>Jeśli nie czytałeś jeszcze wcześniejszych części, to rzuć okiem na <a href="http://www.idaszak.com/article/2017/04/05/es6-2-var-let-const">http://www.idaszak.com/article/2017/04/05/es6-2-var-let-const</a>,<br> ponieważ w poniższym kodzie będziemy korzystać z <code>let</code> oraz <code>const</code>. Zapraszam także do zapoznania się z klasami w ES6: <br><a href="http://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">http://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy</a></p>

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Czym są arrow functions?</h3>
<p>Funkcje strzałkowe, czyli arrow functions zastępują nam funkcje anonimowe, oferując znacznie krótszą składnie.</p>
<p>Aby je zastosować używamy znaku <code>=></code>, czyli grubej strzałki, której składnia wzięła się z CoffeScripta, czyli języka kompilowanego do JavaScriptu. Jeśli znasz taki język jak Haskell, to pewnie już spotkałeś się z takimi funkcjami.</p>
<p>Zacznijmy od prostego przykładu, który zobrazuje różnice między funkcjami.</p>

{% highlight javascript %} 
const a = 2;

const multiply = function (number) {
  return number * a;
}.bind(this);

console.log(multiply(2)); //4
{% endhighlight %}

<p>Tak wygląda zwykła funkcja, przyjmująca jeden argument, która posiada przypisaną aktualną wartość <code>this</code>, czyli <code>this</code> z momentu deklaracji, zamiast tego z momentu wywołania. Użycie <code>bind</code> w tym przykładzie jest bardzo istotne, ponieważ właśnie w taki sposób działają funkcje strzałkowe. Zobaczmy teraz jak można taką funkcję skrócić:</p>

{% highlight javascript %} 
const a = 2;

const multiply = number => number * a;

console.log(multiply(2)); //4
{% endhighlight %}

<p>W tym przypasku nie musieliśmy już pisać słowa kluczowego <code>function</code> ani <code>return</code>. Po lewej stronie strzałki znajdują się argumenty, które przyjmuje funkcja, a po prawej wartość, bądź wyrażenie, które zostanie zwrócone.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Zastosowanie arrow functions - funkcja filter</h3>

{% highlight javascript %} 
const characters = [
	{name: Butch , role: Boxer},
	{name: Vincent, role: Hitman},
	{name: Winston, role: Problem solver},
	{name: Jules, role: Hitman},
]
{% endhighlight %}
<p>Stworzyliśmy właśnie listę obiektów, składających się z wyszkolonych osób do wynajęcia, jednak w naszym przypadku lista nie jest satysfakcjonująca, ponieważ do naszego zadania potrzebujemy tylko płatnych zabójców! W takim przypadku możemy użyć funkcji <code>filter</code>, aby stworzyć listę osób, które mają rolę Hitman.</p>
{% highlight javascript %} 
const HitMan = characters.filter(function(ev){
   return ev.role == 'Hitman';
});

console.log(HitMan); //[{name: "Vincent", role: "Hitman"}, {name: "Jules", role: "Hitman"}]
{% endhighlight %}
<p>Jednak użyjmy teraz arrow function <code>=></code> i spójrzmy, jak bardzo skróci nasz kod.</p>
{% highlight javascript %} 
var HitMan = characters.filter(ev => ev.role == 'Hitman');
console.log(HitMan); //[{name: "Vincent", role: "Hitman"}, {name: "Jules", role: "Hitman"}]
{% endhighlight %}

<p>A co w przypadku jeśli funkcja przyjmuje więcej niż jeden argument lub nie przyjmuje żadnych argumentów?</p>
<p>W przypadku wielu argumentów, musimy pamiętać, żeby zamknąć nasze argumenty w nawiasie okrągłym:</p>
{% highlight javascript %} 
let max = (a, b) => a > b ? a : b;
{% endhighlight %}
<p>W powyższym przykładzie użyliśmy skróconej formy warunku <code>if</code>, gdzie naszym warunkiem jest <code>a > b</code>, wartością zwróconą w przypadku prawdy jest pierwszy argument po znaku zapytania, czyli <code>a</code>, a w przypadku fałszu jest to <code>b</code> znajdujące się po dwukropku.</p>
<p>W przypadku kiedy nasza funkcja strzałkowa nie przyjmuje żadnych argumentów, wstawiamy znak <code>()</code>.</p>
{% highlight javascript %} 
var arg = 72;
var foo = () => arg;

foo(); //72
{% endhighlight %}


