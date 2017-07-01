---
layout: post
title: "Podstawy Programowania Funkcyjnego #4 reduce"
titlePL: "Podstawy Programowania Funkcyjnego #4 reduce"
description: ""
category: Article
date: 2017-07-01 10:06:00 +0100
ima: "/assets/img/fp4.png"
tags: ["JavaScript","Programowanie Funkcyjne"]
linkTitle: [ 
		{a: "Czym jest Reduce?", b: "a1"},
		{a: "Wartość początkowa i inne argumenty", b: "a2"},
		{a: "Tablica jako wartość zwracana", b: "a3"},
		{a: "Obiekt jako wartość zwracana", b: "a4"}
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
<p>W tej serii uczymy się Podstaw Programowania Funkcyjnego. Wykorzystamy przy okazji wiedzę zdobytą w <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">serii dotyczącej ES6</a>. Poznaliśmy już funkcje <code>filter</code> i <code>map</code>, tym razem zajmiemy się tworzeniem jednej wartości z całej tablicy. Zaczynajmy!</p><!--more-->


<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Czym jest Reduce?</h3>
<p>W poprzednich postach poznaliśmy <code>map</code> i <code>filter</code>, czyli sposoby na przeszukiwanie i transformowanie tablic, tym razem jednak skorzystamy z bardziej rozbudowanego narzędzia, jakim jest <code>reduce</code>. Zwrócimy dzięki tej funkcji pojedynczą wartość na podstawie elementów tablicy. W prosty sposób będziemy mogli dodać wszystkie elementy tablicy, albo połączyć w string.</p>

<p>Reduce powinno przyjmować trzy argumenty: funkcję do wykonania na każdym elemencie, wartość początkową i tablicę na której będziemy pracować. Na początek zaimplementujemy sobie taką funkcję, gdzie <code>callback</code> to funkcja, <code>start</code> - wartość początkowa, a <code>array</code> to nasza tablica:</p>
{% highlight javascript %} 
var reduce = function(callback, start, array) {
    var value = start;
    for (var i = 0; i < array.length; i = i + 1) {
        value = callback(value, array[i]);
    }
    return value;
};
{% endhighlight %}
<p>Nasza funkcja przypisuje wartość początkową do zmiennej <code>value</code>, a następnie iteruje po kolejnych elementach tablicy wykonując dla każdego z nich podaną funkcję. Na końcu jako wynik zostaje zwrócona wartość <code>value</code>.</p>
{% highlight javascript %} 
const add = (value, element) => value+element;

const concat = (value, element) => `${value} ${element}`;
{% endhighlight %}
<p>Stworzyliśmy sobie dwie funkcje, z czego pierwsza z nich - <code>add</code> służy do dodawania dwóch elementów, a druga - <code>concat</code> słuzy do konkatenacji, czyli połączenia dwóch stringów w jeden. zapis w ES6 <code>`${value} ${element}`</code> jest równoważny <code>value+' '+element</code>.</p>
<p>Możemy teraz skorzystać z naszych funkcji, połączyć je i wykonać na tablicach:</p>
{% highlight javascript %}
const num = [1, 2, 3, 4, 5];
const WhoAmI = ["I", "am", "the", "one", "who", "knocks"]; 

reduce(add, 0, num); //15
reduce(concat,"",WhoAmI); //"I am the one who knocks"
{% endhighlight %}
<p>Jednak w rzeczywistości nie musimy implementować funkcji reduce, ponieważ w JavaScript jest z nami już od ES5:</p>
{% highlight javascript %}
num.reduce(add); //15
WhoAmI.reduce(concat); //"I am the one who knocks"
{% endhighlight %}
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Wartość początkowa i inne argumenty</h3>
<p>callback w Reduce może przyjmować cztery argumenty:</p>
<ul style="padding-left: 30px;">
	<li><code>previousValue</code>, czyli wartość zwróconą w ostatnim wywołaniu funkcji callback, lub wartość początkową jeśli taka została podana,</li>
	<li><code>currentValue</code>, czyli obecnie przetwarzany element tablicy,</li>
	<li><code>index</code>, index przetwarzanego elementu,</li>
	<li><code>array</code>, tablicę na której wykonujemy reduce.</li>
</ul>
<p>Użyjmy poprzedniego przykładu z dodawaniem, jednak ustawmy wartość początkową na <code>100</code>:</p>
{% highlight javascript %} 
num.reduce((value, element) => value+element, 100); //115
{% endhighlight %}
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Tablica jako wartość zwracana</h3>
<p>Poza stringiem czy liczbą, możemy zwrócić także tablice, wystarczy jako wartość początkową podać pustą tablicę, a następnie dodawać kolejne elementy funkcją <code>push</code>.</p>
{% highlight javascript %} 
num.reduce((value, element) => {
  value.push(element*2);
  return value;
}, []); //[ 2, 4, 6, 8, 10 ]
{% endhighlight %}
<p>Musimy pamiętać, że nasza funkcja wewnętrzna musi zwracać wartość, inaczej funkcja się nie wykona. W powyższym przykładzie korzystamy z tablicy liczb <code>num</code>, do pustej tablicy dodajemy kolejne elementy pomnożone przez 2. Na końcu otrzymujemy gotową tablicę. Oczywiście moglibyśmy wykonać to również funkcją <code>map</code>.</p>
<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Obiekt jako wartość zwracana</h3>
<p>Poza pojedynczymi wartościami i tablicami, możemy także zbudować obiekt za pomocą reduce. W przykładzie pokaże wam, jak policzyć ilość wystąpień elementów w tablicy.</p>
<p>Tablica która znajduje się poniżej, to wystąpienia słów kluczowych w tekście opisującym fabułę serialu. Znajdziemy teraz najczęściej występujące słowa:</p>
{% highlight javascript %} 
BreakingBad = ["chemistry", "crime", "teacher", "cancer", "student", "cancer", "DEA", "wife", "crystal", "wife", "crime"];
BreakingBad.reduce((obj, tag) => {
	obj[tag] = (obj[tag] || 0) + 1;
	return obj;
}, {}); /* { chemistry: 1,
  crime: 2,
  teacher: 1,
  cancer: 2,
  student: 1,
  DEA: 1,
  wife: 2,
  crystal: 1 } */
{% endhighlight %}
<p>Callback powyższego kodu przyjmuje dwa argumenty - zmienną <code>obj</code>, której wartością początkową jest pusty obiekt, oraz aktualny element tablicy <code>tag</code>. Element, który aktualnie przetwarzamy staje się kluczem w tablicy i jeśli już taki istnieje to przypisana mu zostaje jego wartość powiększona o jeden. W przypadku jeśli dla danego elementu nie ma jeszcze miejsca w tablicy to zostaje stworzony z ustawioną wartością 0, powiększoną o jeden. Na końcu jak zwykle musimy pamiętać o zwróceniu zmiennej <code>obj</code>.</p>
<p>Jeśli nadal masz problem ze zrozumieniem kodu, spójrz na poniższą, bardziej rozpisaną wersję:</p>
{% highlight javascript %} 
BreakingBad.reduce((obj, tag) => {
	if (!obj[tag]) {
		obj[tag] = 1;
	} else {
		obj[tag] = obj[tag] + 1;
	}
	return obj;
}, {});
{% endhighlight %}
