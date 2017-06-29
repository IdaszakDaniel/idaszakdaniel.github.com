---
layout: post
title: "ES6#3 Destrukturyzacja czyli nowe podejscie do tablic i obiektow"
titlePL: "ES6#3 Destrukturyzacja, czyli nowe podejście do tablic i obiektów"
description: ""
category: Article
ima: "/assets/img/js.png"
tags: ["DajSiePoznac","JavaScript","ES6"]
linkTitle: [ 
		{a: "Destrukturyzacja tablic", b: "a1"},
		{a: "Destrukturyzacja obiektów", b: "a2"},
		{a: "Spread a Rest?", b: "a3"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}

<img src="{{ site.baseurl }}/assets/img/js.png" >

<p>W serii poświęconej standardowi ES6 języka JavaScript opisałem już pojawiające się tam słowo kluczowe <code>class</code>, pokazałem przykłady zastosowania <code>let</code> oraz <code>const</code>. Tym razem zajmiemy się destrukturyzacją i różnicami między operatorem <code>spread</code> i <code>rest</code>. Umożliwiają one użycie nowego sposobu obsługiwania tablic oraz obiektów, który jest szybszy, łatwiejszy oraz precyzyjniejszy. Możemy dzięki nim wydobywać najróżniejsze, nawet zagnieżdżone wartości, przy użyciu skróconego zapisu. Zaczynajmy!</p> <!--more-->

<p>Jeśli nie czytałeś jeszcze wcześniejszych części, to rzuć okiem na <a href="http://www.idaszak.com/article/2017/04/05/es6-2-var-let-const">http://www.idaszak.com/article/2017/04/05/es6-2-var-let-const</a>,<br> ponieważ w poniższym kodzie będziemy korzystać z <code>let</code> oraz <code>const</code>. Zapraszam także do zapoznania się z klasami w ES6: <br><a href="http://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">http://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy</a></p>

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Destrukturyzacja tablic</h3>
<p>Standardowym podejsciem do obsługi tablic, jest odwoływanie się do poszczególnych jej elementów za pomocą operatora <code>[]</code>:</p>
{% highlight javascript %} 
var array = ["raz","dwa","trzy"];
var first = array[0]; 
var second = array[1]; 
var third = array[2]; 
console.log(first, second, third); //"raz" "dwa" "trzy"
{% endhighlight %}
<p>Dzięki destrukturyzacji, możemy zapisać to w dużo prostszy i bardziej zwięzły sposób:</p>
{% highlight javascript %} 
const [first, second, third] = array;
console.log(first, second, third); //"raz", "dwa", "trzy"
{% endhighlight %}
<p>Nie musimy przypisywać do zmiennej każdego z elementów, możemy je pomijać:</p>
{% highlight javascript %} 
const [x, ,z] = array;
console.log(x, z); //"raz" "trzy"
const [,y] = array;
console.log(y); //"dwa"
{% endhighlight %}
<p>Jak wygląda prosty przykład zastosowania destrukturyzacji tablicy? Możemy bez wykorzystywania zmiennej tymczasowej zamienić wartości dwóch zmiennych:</p>
{% highlight javascript %} 
var a = 1, b = 2;
[b, a] = [a, b];
console.log(a, b);//2 1
{% endhighlight %}
<p>Kolejną funkcjonalnością ES6 w odwoływaniu się do elementów tablicy jest operator spread <code>...</code>, dzięki któremu możemy dotrzeć do pozostałych elementów, poza wybranymi:</p>
{% highlight javascript %} 
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const [a, b, c, d, ...others] = arr;
console.log(a); //1
console.log(others) //[5, 6, 7, 8, 9, 10]
{% endhighlight %}
<p>Możemy wykonać także kolejną dekonstrukcję wybranego elementu:</p>
{% highlight javascript %} 
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const [x, y, ...[z, , ...rest] = arr;
console.log(rest); //[5, 6, 7, 8, 9, 10]
console.log(z); //3
{% endhighlight %}
<p>Zastosowanie operatora <code>spread</code>? ES5 umożliwił korzystanie z funkcji <code>.concat</code> dla tablicy, co wygląda w taki sposób:</p>
{% highlight javascript %} 
var boys = ["Kyle", "Douglas", "Matt"];
var girls = ["Rebecca","Kate"];
var people = boys.concat(girls);
console.log(people); //["Kyle", "Douglas", "Matt", "Rebecca", "Kate"]
{% endhighlight %}
<p>W ES6 możemy skorzystać z operatora <code>spread</code>, co wygląda znacznie czytelniej:</p>
{% highlight javascript %} 
let people = [...boys, ...girls];
console.log(people); //["Kyle", "Douglas", "Matt", "Rebecca", "Kate"]
{% endhighlight %}

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Destrukturyzacja obiektów</h3>
<p>Destrukturyzacja obiektów jest bardzo podobna do destrukturyzacji tablic, z tą różnicą, że korzystamy z nazwy kluczy obiektu:</p>
{% highlight javascript %} 
const object = {a: "1", b: "2", c: "3"};
const {a, c} = object;
console.log(a, c);//1 3
{% endhighlight %}
<p>Możemy też stworzyć klucz, dzięki któremu dodamy kolejną wartość do tablicy:</p>
{% highlight javascript %} 
const key = "d";
object[key] = "4";
console.log(object);// {a: "1", b: "2", c: "3", d: "4"}
{% endhighlight %}
<p>Analogicznie do destrukturyzacji tablic, w obiektach możemy odwoływać się do zagnieżdżonych wartości:</p>
{% highlight javascript %} 
const obj = {x: {y: "5", z: "6"}};
const {x: {z: value}} = obj;
console.log(value);//6
{% endhighlight %}
<p>Możemy także korzystać z operatora spread <code>...</code>:</p>
{% highlight javascript %} 
const object = {a: "1", b: "2", c: "3"};
const {a, ...others} = object;
console.log(others); //{b: "2", c: "3"}
{% endhighlight %}

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Spread a Rest?</h3>
<p>Poznaliśmy już operator <code>spread</code>, który umożliwia nam rozszerzenie zmiennej na wiele elementów, która działa zarówno na tablicach jak i funkcjach. Jednak w ES6 jest jeszcze jeden operator składający się z trzech kropek.</p>
<p><code>rest</code> to operator o jakby przeciwstawnym działaniu, dlatego że pozwala na zamknięcie dowolnej ilości argumentów funkcji w jedną tablicę.</p>
<p>Żeby dowiedzieć się, jak działa operator rest <code>...</code> i jakie ma zastosowanie, stwórzmy sobie funkcję, która pozwoli na dodanie wszystkich argumentów przez nią przekazanych.</p>
{% highlight javascript %} 
function add() {
  return arguments.reduce(function(previousValue, currentValue) {
      return previousValue + currentValue;
    });
}
add(1, 2, 3); // TypeError: arguments.reduce is not a function
{% endhighlight %}
<p><code>arguments</code> to tablica, zawierająca wszystkie przekazane argumenty. Wykonujemy na niej funkcję <code>reduce</code>, która skraca nam naszą tablicę zgodnie z wzorem w podanej jako argument funkcji, czyli w tym przypadku dodaje każdy kolejny element.</p>
<p>Niestety, tak napisany kod nie działa, jednak można naprawić go funkcją rest <code>...</code></p>
{% highlight javascript %} 
function add(...numbers) {
  return numbers.reduce(function(previousValue, currentValue) {
      return previousValue + currentValue;
    });
}
add(1, 2, 3); // 6
{% endhighlight %}
<p>W tym przypadku dowolna ilość argumentów przekazanych do funkcji może zostać użyta do stworzenia ich sumy.</p>