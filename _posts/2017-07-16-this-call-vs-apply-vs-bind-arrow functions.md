---
layout: post
title: "This. Call vs apply vs bind. Arrow Functions i this"
titlePL: "This. Call vs apply vs bind. Arrow Functions i this"
description: ""
category: Article
date: 2017-07-16 12:45:00 +0100
ima: "/assets/img/gk.png"
tags: ["JavaScript"]
linkTitle: [ 
		{a: "Czym jest this?", b: "a1"},
		{a: "Apply i Call", b: "a2"},
		{a: "Arrow Functions i this", b: "a3"}
		]
excerpt_separator: <!--more-->
---
<!-- {% highlight javascript %} 
{% endhighlight %} -->
{% include JB/setup %}

<p>W tym artykule opiszę czym jest <code>this</code>, jak się ma do tego i czym się różni <code>call</code>, <code>apply</code> oraz <code>bind</code>. Wrócimy także do postów o arrow functions, gdzie należałoby bardziej rozwinąć jak zachowuje się w ich przypadku <code>this</code>.</p>
<p>Artykuł o arrow function można znaleźć: <a href="https://www.idaszak.com/article/2017/05/07/es6-4-arrow-functions-funkcje-strzalkowe">tutaj</a>.</p><!--more-->


<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Czym jest this?</h3>
<p>
<code>this</code> to specjalny keyword, który znajduje się w każdej funkcji, przechowujący wartość zależną od kontekstu w jakim została wywołana.</p>
<p>Zobaczmy jak to wygląda w praktyce:</p>

{% highlight javascript %} 
function foo() {
	console.log( this.a );
}
 
var a = 2; //globalne a
 
foo(); // 2 
 
var obj = {
	a: 42, //lokalne a
	foo: foo
};
 
obj.foo() //42
{% endhighlight %}
<p>funkcja <code>foo()</code> wywołana w zasięgu globalnym przyjmuje wartość this tego zasięgu, więc zmienna <code>a</code>, którą znajduje to globalna zmienna <code>a = 2</code>. W przypadku, którym stworzymy obiekt ze swoja wartością <code>a</code> oraz dodamy funkcję foo wartość this będzie inna. Funkcja wywołana w kontekście obiektu <code>obj</code> z przykladu ustawia wartość <code>this</code> na zasięg lokalny obiektu, dlatego zwraca lokalną wartość <code>a</code>, czyli 42.</p>
<p>Zgodnie z sugestią czytelnika, dodam jeszcze ciekawostkę dotyczącą trybu strict, który tłumaczyłem w jednym z wcześniejszych postów: <a href="https://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro#a4">Co to jest strict mode?</a></p>
{% highlight javascript %} 
"use strict";
function foo() {
	console.log( this.a );
}
 
var a = 2; //globalne a
 
foo(); //TypeError: Cannot read property 'a' of undefined
{% endhighlight %}
<p>W tym przypadku funkcja foo ma ustawioną wartość <code>this</code> na zasięg globalny, jednak tryb strict nie pozwala na odniesienie się do this jako obiektu globalnego i dlatego otrzymujemy błąd.</p>
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Apply i Call</h3>
<p>Każda z tych funkcji wywołuje metodę z ustawionym this. Różnią się tym, że Apply przyjmuje argumenty jako tablicę, a Call przyjmuje argumenty wypisane po przecinku. Dodatkowo Call jest szybsze w wykonaniu.</p>
<p>Stwórzmy sobie obiekt, w kontekście którego wywoływać będziemy funkcję z keywordem <code>this</code>:</p>
{% highlight javascript %} 
var band = {name: "ACDC"};
 
var tour = function(target, target2){
	console.log(`${this.name} tour countries: ${target} and ${target2}.`);
}
 
tour("Poland", "France"); //undefined tour countries: Poland and France.
tour.call(band, "Poland", "France"); //ACDC tour countries: Poland and France.
tour.apply(band, ["Poland", "France"]) //ACDC tour countries: Poland and France.
{% endhighlight %}
<p>We funkcji <code>tour</code> skorzystaliśmy z uproszczonego zapisu dzięki ES6 <code>`${this.name} tour countries: ${target} and ${target2}.`</code>. Wyświetlamy w nim zmienną zależną od this oraz dwa podane argumenty.</p>
<p>Na początek wywołujemy funkcję <code>tour</code>, która nie znajduje w zasięgu globalnym zmiennej <code>name</code>. Następnie ustawiamy kontekst funkcji <code>tour</code> na obiekt <code>band</code> za pomocą <code>call</code> i podajemy argumenty po przecinku. Podobnie jak poprzednio używamy funkcji <code>apply</code>, z tą różnicą, że argumenty przekazujemy jako tablica.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Bind</h3>
<p><code>bind</code> działa bardzo podobnie jak <code>call</code> i <code>apply</code>, z tą różnicą, że zwraca nową funkcję z ustawionym this.</p>
<p>Wróćmy do poprzedniego przykładu, który może być troszkę zaskakujący:</p>
{% highlight javascript %} 
function foo() {
	console.log( this.a );
}
 
var a = 2; //globalne a
 
var obj = {
	a: 42, //lokalne a
	foo: foo
};
 
const butWhy = obj.foo;
butWhy(); //2
{% endhighlight %}
<p>Dlaczego tym razem funkcja <code>butWhy()</code> zwróciła globalne <code>a</code>, zamiast lokalnego, mimo że przypisujemy do niej lokalne <code>foo</code>? Przypisując do nowej zmiennej funkcję z obiektu <code>obj</code>, tak naprawdę przypisaliśmy tą funkcję, ale bez kontekstu. W momencie, kiedy wywołujemy nową zmienną <code>butWhy()</code> funkcja jest wywołana poza obiektem i ma zasięg globalny. Żeby to naprawić należy użyć <code>bind</code>:</p>
{% highlight javascript %} 
const letsBind = obj.foo.bind(obj);
letsBind(); //42
{% endhighlight %}
<p>Stworzyliśmy zmienną <code>letsBind</code> zawierającą funkcję <code>foo</code> oraz nadaliśmy jej kontekst obiektu <code>obj</code>, dzieki czemu nasze <code>this.a</code> wskazuje na lokalną zmienną <code>a</code>.</p>

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Arrow Functions i this</h3>
<p>Funkcje o których pisaliśmy już wcześniej, przy okazji serii dotyczącej ES6: <a href="https://www.idaszak.com/article/2017/05/07/es6-4-arrow-functions-funkcje-strzalkowe">tutaj</a>, oferują nie tylko skrócony syntax. W przypadku arrow functions wartość this zachowuje się trochę inaczej niż normalnie. Funkcja strzałkowa ignoruje wszystkie zasady this oraz apply, call i bind. Kontekst this jest zawsze otaczającym zasięgiem z momentu utworzenia tej funkcji zamiast wywołania.</p>

<p>Wróćmy do poprzedniego przykładu, z tą różnicą, że użyjemy funkcji strzałkowej:</p>
{% highlight javascript %} 
foo = () => console.log(this.a);
 
var a = 2; //globalne a
 
var obj = {
	a: 42, //lokalne a
	foo: foo
};
 
obj.foo();

const letsBind = obj.foo.bind(obj);
letsBind();
{% endhighlight %}
<p>Funkcja <code>foo</code> posiada wartość this ustawioną na tą z momentu utworzenia funkcji. Zarówno wywołanie na obiekcie jak i użycie <code>bind</code>, które działały w przypadku zwykłej funkcji, nie zmieniają kontekstu <code>this</code>. Za każdym razem funkcja wywołuje globalną zmienną <code>a = 2</code>.</p>
<p>Kolejny przykład który świetnie obrazuje <code>arrow functions</code> i <code>this</code>, to przykład z użyciem <code>map</code>, o którym więcej można dowiedzieć się w jednym z poprzednich postów dotyczących programowania funkcyjnego: <a href="https://www.idaszak.com/article/2017/05/31/podstawy-programowania-funkcyjnego-3-map-i-filter#a1">Czym jest Map, a czym jest Filter?</a></p>
{% highlight javascript %} 
const number = {
    num: 5
}
let arr = [1,2,3];

arr.map(n => this.num * n, number); //[NaN, NaN, NaN]

arr.map(function (n) {
    return this.num * n;
}, number); //[5,10,15]
{% endhighlight %}
<p><code>Map</code>, jako drugi argument może przyjmować wartość, którą ma użyć jako this podczas wywołania. W tym przypadku wksazujemy na obiekt <code>number</code> i odwołujemy się do <code>this.num</code>. Jednak przy użyciu funkcji strzałkowej, nie ma możliwości nadania wartości <code>this</code>, która zawsze przyjmuje zasięg z momentu stworzenia tej funkcji. Jak możemy zaobserwować, w przypadku użycia normalnej funkcji, <code>this</code> przyjmuje taką wartość, jaką podaliśmy w <code>map</code>.</p>