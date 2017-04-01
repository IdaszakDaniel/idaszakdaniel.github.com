---
layout: post
title: "Czy Javascript jest jezykiem zorientowanym obiektowo"
titlePL: "Czy Javascript jest językiem zorientowanym obiektowo?"
description: ""
category: Article
tags: ["DajSiePoznac","Javascript","ES6","OOP"]
linkTitle: [ 
		{a: "Czy Javascript jest językiem zorientowanym obiektowo?", b: "a1"},
		{a: "Jak wyglądają obiekty w Javascript i czym są klasy?", b: "a2"},
		{a: "Co to jest prototyp i jak wygląda dziedziczenie?", b: "a3"},
		{a: "Klasy i dziedziczenie w ES6", b: "a4"},
		{a: "Na czym polega polimorfizm?", b: "a5"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}

<img src="{{ site.baseurl }}/assets/img/js.png" >


<p>AngularJS, w którym piszę swój projekt, jest frameworkiem który promuje obiektowe podejście do programowania, jednak na początek warto byłoby zadać sobie pytanie, czy Javascript jest językiem zorientowanym obiektowo? Postaram się wyjaśnić czy jest to prawdą, oraz wytłumaczyć jak Javascript różni się od innych języków. Zahaczymy także o najnowszy standard tego języka i jego podejście do programowania obiektowego. Zaczynajmy!</p> <!--more-->

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Czy Javascript jest językiem zorientowanym obiektowo?</h3>
<p>Javascript jako jeden z niewielu języków programowania jest zorientowany obiektowo. Zaraz, zaraz, zapytacie - przecież inne języki też mają obiektowość! Jednak takie popularne języki jak C++ czy Python powinny nazwane być precyzyjniej "języki klasowo zorientowane". Javascript różni się od nich tym, że w ogóle nie posiada klas (nie jest to do końca prawdą, ale wytłumaczę to w dalszej części tego posta). Czy taka różnica jest zaletą czy wadą? Wielu programistów uważa że dziedziczenie metod i zmiennych z klas to złe podejście, także możemy potraktować to jako zaletę. Klasy jednak, upraszczają wiele rzeczy o których musielibyśmy pamiętać w Javascripcie, dlatego sporo osób dąży do stworzenia klas w tym języku.</p>

<p>Wiele osób twierdzi, że w języku Javascript wszystko jest obiektem, jednak nie jest to do końca prawdą. Argumentują to faktem, że np. <code>string</code> posiada swoje metody, takie jak <code>length</code>, jednak w rzeczywistości wygląda to inaczej. Sam <code>string</code> jest typem prymitywnym, jednak możemy stworzyć z niego obiekt:</p>
{% highlight javascript %} 
var testString = "who am I";
typeof testString; //"string"
var testObject = new String("who am I");
typeof testObject; // "object"
{% endhighlight %}
<p>W praktyce, jeśli zastosujemy na stringu funkcję taką jak <code>length</code> czy <code>charAt</code>, to język niejawnie tworzy z niego obiekt, a potem wywołuje daną funkcję.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Jak wyglądają obiekty w Javascript i czym są klasy?</h3>
<p>Na początek stwórzmy obiekt z trzema własnościami:</p>
{% highlight javascript %} 
var myObject = {
	firstVariable: 1,
	secondVariabe: 2,
	foo: function(){
		console.log("foo");
	}	
};
{% endhighlight %}
<p>Możemy teraz odwoływać się do poszczególnych własności obiektu, niezależnie czy są funkcjami czy zmiennymi, za pomocą operatora <code>.</code> lub <code>[ ]</code>:</p>
{% highlight javascript %} 
myObject.firstVariable; //1
myObject.secondVariable; //2
myObject["firstVariable"]; //1
myObject.foo; //"foo"
{% endhighlight %}


<p>Klasa sama w sobie nie jest obiektem, tylko schematem budowy obiektów. Żeby otrzymać obiekt, na którym będzie można przykładowo wykonać jakąś metodę, musimy najpierw stworzyć obiekt, nazywany instancją tej klasy.
Czym różni się dziedziczenie z klasy od dziedziczenia z obiektu? Jeśli tworzmy nowy obiekt z klasy, to wszystkie metody będą kopiowane do tego obiektu. W Javascript dziedziczenie jest utrudnione, ponieważ nowy obiekt zamiast skopiować metodę obiektu nadrzędnego, tylko się do niej odwołuje.</p> 

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Co to jest prototyp i jak wygląda dziedziczenie?</h3>

<p>Każdy obiekt w Javascript ma wbudowaną funkcję, która nazywa się <code>prototype</code> i łączy go z innym obiektem. Obiekt, który nie dziedziczy nic z innego obiektu, ma przypisany prototyp do <code>object.prototype</code>. W momencie, w którym wywołujemy własność obiektu i program nie znajdzie jej w danym obiekcie, wtedy przesuwa się po nadrzędnych obiektach tak długo dopóki jej nie znajdzie, lub nie dotrze do końca łańcucha prototypów. W praktyce możemy przez to wywołać nieistniejącą w interesującym nas obiekcie metodę, a otrzymać odpowiedź od obiektu położonego wyżej w łańcuchu prototypów.</p>

<p>Funkcje prototypowe są dostępne dla każdej instancji. Nową instancję obiektu możemy stworzyć używając słowa kluczowego <code>new</code>:</p>
{% highlight javascript %} 
function Droid(type) {
	this.type = type;
}

Droid.prototype.speak = function() {
	console.log("I am " + this.type );
};

var c3po = new Droid('protocol droid');

c3po.speak(); // I am protocol Droid
{% endhighlight %}
<p>Wybacz jeśli nie jesteś fanem Star Wars, ale mam nadzieję że takie przykłady bardziej zapadną w pamięć :)</p>

<p>Nazywanie obiektów z dużej litery, to praktyka z innych języków programowania, jednak nie ma to wpływu na wykonanie kodu, chociaż możliwe jest, że niektóre lintery sprawdzające jakość kodu mogą tego wymagać.</p>

<p>Możemy teraz stworzyć kolejny obiekt, który będzie dziedziczył z funkcji Droid i tworzył także swoje instancje:</p>
{% highlight javascript %} 
function Mech(type) {
	Droid.call(this, type);
}

Mech.prototype = new Droid();

var r2d2 = new Mech('astromech');
r2dr.speak(); // I am astromech
{% endhighlight %}
<p>Prototyp funkcji <code>Mech</code> przypisaliśmy do obiektu <code>Droid</code>, dzięki czemu <code>Mech</code> dziedziczy wszystkie własności obiektu <code>Droid</code>. Użyliśmy funkcji <code>call</code>, która wywołuje nadrzędny obiekt z wartością <code>this</code>, wskazująca na obecny obiekt, dzięki temu ustawiając wartość <code>this.type</code> zapisuje się ona w obecnym obiekcie, zamiast nadrzędnym.</p>

<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Klasy i dziedziczenie w ES6</h3>
<p>Jeśli chcesz pisać w ES6, musisz zainstalować kompilator <a href="https://babeljs.io/">babel</a> lub przetestować kod online np. na stronie <a href="http://jsbin.com">http://jsbin.com</a> </p>
<p>Skoro w standardzie ES6 języka Javascript pojawiło się słowo kluczowe <code>class</code>, czy oznacza to, że wprowadzono normalne klasy? Tak naprawdę zmieniła się tylko składnia kodu na krótszą i czytelniejszą, jednak za kulisami wszystko działa tak samo jak w przypadku funkcji z prototypem.</p>

<p>Spróbujmy teraz stworzyć klasę w ES6 i stworzyć jej instancję:</p>
{% highlight javascript %} 
class Ship {
  constructor(options) {
    this.speed = options.speed;
    this.hasHyperdrive = options.hasHyperdrive;
  }

  getSpeed() {
    return this.speed;
  }
}

var TieFighter = new Ship({
  speed: 1050,
  hasHyperdrive: false
});

console.log(TieFighter.getSpeed()); //1050
{% endhighlight %}
<p>W tym przypadku, nie widzimy już słowa kluczowego <code>function</code> w metodach. Wystarczy tylko nazwa, argumenty w okrągłym nawiasie i para nawiasów klamrowych. Nasz obiekt zwraca funkcją <code>getSpeed</code> jedną z wartości. Nowe instancje tworzymy za pomocą słowa kluczowego <code>new</code>, a funkcja <code>constructor</code> wywoływana jest automatycznie przy tworzeniu nowej instancji klasy.</p>

<p>Stwórzmy teraz kolejną klasę <code>StarShip</code>, która będzie dziedziczyła z klasy <code>Ship</code>:</p>

{% highlight javascript %} 
class StarShip extends Ship {
	getSpeed() {
		var speed = super.getSpeed();
		console.log(speed, ", but Hyperdrive allows entering hyperspace");
	}
}

var FalconMillenium = new StarShip({
  speed: 1050,
  hasHyperdrive: true
});

FalconMillenium.getSpeed(); //"1050, but Hyperdrive allows entering hyperspace"
{% endhighlight %}
<p><code>extends</code> oznacza, że klasa <code>StarShip</code> dziedziczy wszystko z co klasa <code>Ship</code> posiada. Jednak w naszym kodzie skorzystaliśmy z polimorfizmu, który w ES6 także ma uproszczoną składnię.</p>

<h3 id="a5"><span style="color:gray; font-size: 30px;">#</span> Na czym polega polimorfizm?</h3>

<p>Polimorfizm umożliwia różne zachowanie tych samych metod. Jeśli w obiekcie nadpiszemy funkcję o tej samej nazwie, to pojawi się w łańcuchu prototypów jako pierwsza i właśnie ona się wykona.</p>
<p>W powyższym kodzie użyliśmy słowa kluczowego <code>super</code>, który pozwala na wywołanie funkcji klasy, z której dany obiekt dziedziczy. Dzięki temu możemy przypisać wartość do zmiennej i wyświetlić ją w inny sposób.</p>