---
layout: post
title: "Podstawy Programowania Funkcyjnego #2 - domkniecia i higher order functions"
titlePL: "Podstawy Programowania Funkcyjnego #2 - domknięcia i higher order functions"
description: ""
category: Article
date: 2017-05-25 11:30:00 +0100
ima: "/assets/img/fp2.png"
tags: ["DajSiePoznac","Javascript","Programowanie Funkcyjne"]
linkTitle: [ 
		{a: "Czym jest domknięcie - krótka definicja", b: "a1"},
		{a: "Czym na prawdę jest domknięcie?", b: "a2"},
		{a: "Funkcje zwracające inne funkcje", b: "a3"},
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
<p>W tej serii uczymy się Podstaw Programowania Funkcyjnego. Wykorzystamy przy okazji wiedzę zdobytą w <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">serii dotyczącej ES6</a>. Tym razem zmierzymy się z postrachem początkujących programistów Javascript, czyli domknięciami. Udowodnię że na pewno już się z nimi spotkałeś i wcale nie są trudne do zrozumienia. Zaczynajmy!</p><!--more-->


<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Czym jest domknięcie - krótka definicja</h3>
<p>Domknięcie (Closure), to zagadnienie, którym doświadczeni programiści wręcz straszą początkujących. Wszędzie mówi się że to pytanie pojawia się zawsze na rozmowie kwalifikacyjnej. Jednak nie należy się tego bać, ponieważ domknięcie jest bardzo proste do zrozumienia, co więcej - jeśli już pisałeś jakiś kod w Javascript, to z pewnością zetknąłeś się już z Closures nieświadomie.</p>
<p>Jakiś czas temu na twitterze pojawił się challenge, który polegał na wytłumaczeniu czym jest domknięcie za pomocą limitu znaków jednego tweeta. Teraz jest to skarbnica definicji, pozwalająca zrozumieć to zagadnienie w bardzo łatwy sposób, dlatego przytoczę jedno z nich.</p>

<blockquote class="twitter-tweet" data-lang="pl"><p lang="en" dir="ltr"><a href="https://twitter.com/JS_Cheerleader">@JS_Cheerleader</a> a stateful function.</p>&mdash; David K. 🎹 (@DavidKPiano) <a href="https://twitter.com/DavidKPiano/status/683479456019779585">3 stycznia 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script> 
<p><cite>"Statefull function"</cite> , czyli funkcja ze stanem. Co to oznacza?</p>
<p>Czyste funkcje, które wspominaliśmy w <a href="https://www.idaszak.com/article/2017/05/19/podstawy-programowania-funkcyjnego-1-fundamenty#a4">poprzedniej części podstaw programowania funkcyjnego</a>, to funkcje bez stanu, czyli funkcje, które nie modyfikują zmiennych spoza swojego bloku. Domknięcie, możemy zaobserwować wtedy, kiedy funkcja ma dostęp do zmiennej spoza swojego bloku, czyli zmiennej globalnej, bądź zmiennej w funkcji nadrzędnej.</p>
{% highlight javascript %} 
function outer() {
	var x = 7;

	function inner() {
		console.log( x ); // 7
	}

	inner();
}

outer();
{% endhighlight %}
<p>W powyższym fragmencie kodu widzimy domknięcie, ponieważ wewnętrzna funkcja <code>inner</code> ma dostęp do zmiennej <code>x</code>.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Czym na prawdę jest domknięcie?</h3>

<p>Możemy teraz bardziej rozwinąć wcześniejszą definicję i pokazać czym na prawdę jest domknięcie.</p>
<p>Domknięcie możemy zaobserwować wtedy, gdy funkcja pamięta i ma dostęp do swojego zasięgu zmiennych, nawet jeśli jest wywołana pozna nim</p>
{% highlight javascript %} 
function outer() {
	var x = 7;

	function inner() {
		console.log( x );
	}

	return inner;
}

var closure = outer();
closure(); // 7
{% endhighlight %}
<p>Funkcja <code>outer()</code> zwraca funkcję <code>inner()</code>, która ma swój zasięg w całej funkcji <code>outer()</code>. Przypisujemy ją do zmiennej <code>closure</code>, więc wywołujemy funkcję <code>inner()</code> poza jej zasięgiem, a ona nadal ma dostęp do zmiennej "x".</p>

<p>W rzeczywistości funkcje, które nie zostaną już wykorzystane zostają usunięte z pamięci poprzez Garbage Collector, jednak w tym przypadku sytuacja jest inna. Funkcja <code>outer()</code> nie zostanie usunięta, ponieważ funkcja <code>closure()</code> cały czas będzie potrzebowała do niej dostęp.</p>
<center>
<iframe src="https://giphy.com/embed/jz0oM9Els8bHa" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/friends-jennifer-aniston-rachel-green-jz0oM9Els8bHa">via GIPHY</a></p></center>

<p>Do czego przydatne są domknięcia? Pozwalają na wyodrębnienie funkcji i zmiennych spoza zasięgu globalnego, co daje nam zmienne prywatne.</p>

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Funkcje zwracające inne funkcje</h3>
<p>Kolejny przykład pozwalający pokazać domknięcie, to Higher Order Functions, czyli funkcje zwracające inne funkcje:</p>
{% highlight javascript %} 
function outer() {
	return function inner(x){
		console.log(x);
	};
}

var higherOrder = outer();
higherOrder(5); // 5
{% endhighlight %}
<p>Dlaczego jest to możliwe? Ponieważ w języku Javascript funkcje mogą być traktowane jako wartości.</p>
<p>Jednak Higher Order Functions mogą także przyjmować funkcje jako argumenty:</p>
{% highlight javascript %} 
function hof(x,fn) {
	fn(x);
}

function add2(val){
	console.log( val + 2 );
}

hof(2, add2); //4
{% endhighlight %}
<p>Domknięcie sprawia, że funkcja pamięta swój zasięg i zmienne, ale również może pamiętać wartość przypisanej zmiennej.</p>
{% highlight javascript %} 
function add(x) {
	return function sum(y){
		return x + y;
	}
}

var add10 = add(10);
var add20 = add(20);

add10(2); //12
add10(5); //15
add20(3); //23
{% endhighlight %}
<p>Przypisując zmienną <code>add10</code> do funkcji z argumentem <code>add(10);</code> otrzymujemy funkcję <code>sum()</code>, która pamięta swój zasięg i zmienną <code>x</code>, w której mamy wpisaną wartość 10. Przy kolejnych wywołaniach <code>add10</code> podajemy tylko argument funkcji <code>sum</code>.</p>

<p>W serii o ES6, pisząc na temat <a href="https://www.idaszak.com/article/2017/05/07/es6-4-arrow-functions-funkcje-strzalkowe#a2">arrow functions</a>, skorzystaliśmy z przykładu użycia funkcji <code>filter</code>, który również możemy nazwać Higher Order Function, ponieważ przyjmuje inną funkcję jako argument.</p>
{% highlight javascript %} 
var HitMan = characters.filter(ev => ev.role == 'Hitman');
{% endhighlight %}
<p>W powyższym przykładzie funkcja <code>filter</code> jako argument przyjmuje funkcje anonimową z argumentem <code>ev</code>.</p>
