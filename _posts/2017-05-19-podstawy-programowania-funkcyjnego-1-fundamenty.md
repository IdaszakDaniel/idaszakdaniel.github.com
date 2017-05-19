---
layout: post
title: "Podstawy Programowania Funkcyjnego #1 - fundamenty"
titlePL: "Podstawy Programowania Funkcyjnego #1 - fundamenty"
description: ""
category: Article
date: 2017-05-19 17:00:00 +0100
ima: "/assets/img/fp1.png"
tags: ["DajSiePoznac","Javascript","ES6","Programowanie Funkcyjne"]
linkTitle: [ 
		{a: "Argumenty funkcji", b: "a1"},
		{a: "Rest - destrukturyzacja", b: "a2"},
		{a: "Argumenty domyślne i tablica argumentów", b: "a3"},
		{a: "Purity - czyste funkcje", b: "a4"}
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
<p>W tej serii możecie nauczyć się razem ze mną Podstaw Programowania Funkcyjnego. Będzie to świetny sposób na poznanie zastosowań standardu Javascript - ES6. Nawet jeśli piszesz kod obiektowo, warto poznać kilka podstaw, które pozwolą pisać krótszy i czytelniejszy kod. Na sam początek zaczniemy od fundamentów i poznamy najważniejsze zasady. Zaczynajmy!</p><!--more-->



<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Argumenty funkcji</h3>
<p>W języku Javascript, funkcja może przyjmować różną ilość argumentów i zależnie od ich ilości możemy wykonać w niej inny fragment kodu. Wcześniej, czyli przed wprowadzeniem standardu ES6 mogliśmy korzystać tylko ze specjalnej zmiennej <code>arguments</code>, jednak mamy teraz lepsze możliwości, a korzystanie z tej zmiennej uznawane jest za złą praktykę.</p>
<p>Spójrzmy na poniższy kod, który korzysta z tej zmiennej:</p>
{% highlight javascript %} 
var show = function(a, b) {
    console.log(arguments);
}
show('pierwszy', 'drugi'); //{ '0': 'pierwszy', '1': 'drugi' }
{% endhighlight %}
<p>Zakładając że nie znamy ilości argumentów, które pojawią się w tej funkcji, możemy wywołać funkcję <code>length</code> do zmiennej <code>arguments</code>:</p>
{% highlight javascript %} 
var show = function() {
    console.log(arguments.length);
}
show('a','b','c','d'); //4
{% endhighlight %}
<p>Skoro zmienna <code>arguments</code> działa całkowicie w porządku, to można zadać pytanie, dlaczego nie powinno się jej stosować?</p>
<p><code>arguments.length</code> jest bardzo użytecznym zastosowaniem, jednak problem zaczyna się w momencie, gdy chcielibyśmy użyć argumentów z tej zmiennej, bo nie jest ona prawdziwą tablicą.</p>
<p>Jak więc zastąpić <code>arguments</code>? Na ratunek przychodzi zmienna <code>rest</code>, którą omówiliśmy już przy okazji <a href="https://www.idaszak.com/article/2017/04/14/es6-3-destrukturyzacja-nowe-podejscie-do-tablic-i-obiektow#a3">postu dotyczącego destrukturyzjacji.</a>  Pojawia się tam przykład, który tłumaczy różnicę pomiędzy arguments a <code>...</code> czyli operatorem reszty.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Rest - destrukturyzacja</h3>
<p>Jak działa rest?</p>
{% highlight javascript %} 
function show( x, y, ...args ) {
	console.log( x, y, args );
}
show(); //undefined undefined []
show( 1, 2, 3, 4, 5, 6); //1 2 [3, 4, 5, 6]
{% endhighlight %}
<p>jak widzimy, argumenty nie przypisane do odpowiadającym im zmiennych, wchodzą w skład tablicy <code>args</code>.</p>
<p>To teraz proste zadanie, z którym miałem okazję zetknąć się przy zgłębianiu tajników języka Prolog. Użytkownik może podać dowolną ilość argumentów, z zaznaczeniem że ich minimalna ilość to 2. Zadaniem funkcji jest zwrócić tablicę z argumentami, jednak argument podany jako pierwszy ma znajdować się na jej samym końcu.</p>
{% highlight javascript %} 
const shift = (head, ...tail) => [tail, head];
shift(1,2,3); //[2, 3, 1]
{% endhighlight %}

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Argumenty domyślne i tablica argumentów</h3>
<p>Dzięki tablicy argumentów, możemy odwoływać się do poszczególnych argumentów oraz wykonywać na niej inne funkcje takie jak np. <code>filter</code>. Nie musimy także określać ilości przekazywanych argumentów. Jest jeszcze jedna funkcjonalność dodana w ES6, o której jeszcze nie wspominaliśmy na tym blogu, a mianowicie - argument domyślny, czyli taki, który ustawiamy, jeśli argumentów będzie mniej niż oczekujemy.</p>

{% highlight javascript %} 
function show(y, x = 2){
  console.log(y * x);
}
show(2); //4
show(2, 3); //6
{% endhighlight %}
<p>Wcześniej musieliśmy sobie radzić z argumentem domyślnym w taki sposób:</p>
{% highlight javascript %} 
function show(y, x) {
  x = x || 2;
  console.log(y * x);
}
{% endhighlight %}

<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Purity - czyste funkcje</h3>
<p>W programowaniu funkcyjnym powinniśmy trzymać się w miarę możliwości zasady czystości funkcji.</p>
<p>Funkcja powinna być prosta i przyjmować argumenty, w innym wypadku byłaby bezużyteczna. Czyste funkcje, nie powinny modyfikować zmiennych spoza swojego zasięgu i zmiennych globalnych, dlatego muszą zwracać jakąś wartość. Dzięki prostocie tych funkcji możemy zwiększać reusability kodu zgodnie z zasadą KISS, czyli Keep It Simple, Stupid. Nasze programy staną się łatwiejsze do późniejszej modyfikacji. Funkcje, które modyfikują lub operują na zmiennych spoza swojego zasięgu mają efekt uboczny - nie zawsze zwracają przewidywalny wynik. Czysta funkcja musi zwrócić zawsze ten sam wynik dla takich samych wartości. Oczywiście nie da się wyeliminować całkowicie funkcji, które nie są czyste, ale w miarę możliwości powinniśmy starać się ich unikać.</p>