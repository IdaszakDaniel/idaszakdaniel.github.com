---
layout: post
title: "ES6#2 Var, Let, Const - zasieg zmiennych w JS"
titlePL: "ES6#2 Var, Let, Const - zasięg zmiennych w JS"
description: ""
category: Article
ima: "/assets/img/js.png"
tags: ["DajSiePoznac","JavaScript","ES6"]
linkTitle: [ 
		{a: "Zasięg zmiennych i funkcji w JavaScript", b: "a1"},
		{a: "IIFE, strict mode i hoisting", b: "a2"},
		{a: "ES6 let i const", b: "a3"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}

<img src="{{ site.baseurl }}/assets/img/js.png" >


<p>W poprzednim poście opisałem jak wygląda obiektowość, jakie są jej problemy oraz przedstawiłem jak jest to rozwiązane w ES6. Tym razem pokażę jakie utrudnienia możemy napotkać w JavaScript zajmując się zasięgiem zmiennych. Wrócimy krótko do zagadnienia jakim jest <code>strict mode</code> oraz <code>IIFE</code>, zainteresujemy się zmiennymi globalnymi oraz dowiemy się jak <code>let</code> oraz <code>const</code> zastępuje <code>var</code>. Zaczynajmy!</p> <!--more-->

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Zasięg zmiennych i funkcji w JavaScript</h3>
<p>Na początku powinniśmy wyjaśnić sobie jak działa zasięg funkcji oraz zmiennych. Czy zmienna zadeklarowana poprzez <code>var</code> jest dostępna poza tą funkcją?</p>
{% highlight javascript %} 
function drive(a){
  var b = 2;
  
  function start(){
    //...
  }
  var c = 3;
}
start(); // błąd
console.log(a); //błąd
console.log(b); //błąd
console.log(c); //błąd
{% endhighlight %}
<p>W powyższym przykładzie stworzyliśmy funkcję <code>drive</code> i zagnieżdżoną funkcję <code>start</code>. W funkcjach zasięg zmiennych i funkcji zagnieżdżonych zamyka się na najbliższej zewnętrznej funkcji. To właśnie dlatego wywołując wewnętrzną funkcję poza funkcją <code>drive</code> otrzymaliśmy błąd, który mówi że taka funkcja nie istnieje. To samo tyczy się zmiennych, które są zadeklarowane wewnątrz tej funkcji.</p>
<p>Spróbujmy teraz troszkę zmodyfikować nasz kod. Sprawdzimy teraz jak to jest ze zmiennymi globalnymi.</p>
{% highlight javascript %} 
var z = 100;
function drive(){
  
  function start(){
    
    function engineStart(){
      console.log(z);
    }
    engineStart();
  }
  start();
}
drive();//100
{% endhighlight %}
<p>W powyższym kodzie mamy podwójnie zagnieżdżoną funkcję. Wewnątrz <code>drive</code>, funkcja <code>engineStart</code> poszukuje zmiennej <code>z</code>, której nie znajduje w swoim zasięgu zmiennych, dlatego szuka jej o jeden stopień wyżej, czyli w funkcji <code>start</code>. Nie ma tam tej zmiennej, więc przechodzi później do funkcji <code>drive</code>. Tam też jej nie znajduje, więc udaje się do zasięgu globalnego i wypisuje tą zmienną. W przypadku, którym mielibyśmy tą zmienną zadeklarowaną jako globalną i drugą, o takiej samej nazwie wewnątrz funkcji, <code>engineStart</code> wypisałaby pierwszą na którą natrafi.</p>
<p>Czy jest to pożądane zachowanie, czy raczej niebezpieczny efekt uboczny? Bez takiego mechanizmu programowanie w JavaScript byłoby niesamowicie utrudnione, jednak musimy bardzo uważać, żeby nie sprawiło to nieoczekiwanych kłopotów. Spójrzmy na przykład:</p>
{% highlight javascript %} 
for (var i = 0; i < 5; i++){
    console.log(i);
}
console.log('na koniec pętli:', i);
{% endhighlight %}
<p>Deklarujemy zmienną <code>i</code> w bloku pętli for, więc zakładamy, że na zewnątrz tego bloku nie będzie dostępna. Niestety wynik tego kodu jest dosyć nieoczekiwany:</p>
{% highlight plain text %} 
0
1
2
3
4
"na koniec pętli:5"
{% endhighlight %}
<p>Dlaczego jest to złe? Dobrą praktyką pisania kodu jest ukrywanie kodu w funkcjach i tworzenie minimalnej ilości zmiennych. W takim przypadku musielibyśmy tworzyć inną zmienną do każdej pętli. Co spowodowało taki wynik? JavaScript ma tylko jeden mechanizm zasięgu i są to funkcje. A więc jak możemy naprawić nasz kod?</p>
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> IIFE, strict mode i hoisting</h3>
<p>Na temat IIFE oraz strict mode pisałem już w innym poście, przy okazji opisywania dobrych praktyk w AngularJS<br> <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro</a> dlatego pominę wstępne tłumaczenie.</p>
<p>Użyjmy IIFE do stworzenia zasięgu funkcji, który nie pozwoli naszej zmiennej <code>i</code> stać się zmienną globalną:</p>
{% highlight javascript %} 
(function(){
  for (var i = 0; i < 5; i++){
      console.log(i);
  }
})();
console.log('na koniec pętli:', i); //błąd
{% endhighlight %}
<p>Dzięki temu, <code>console.log(i);</code> wyrzuca błąd, że nie ma takiej zmiennej.</p>
<p>A co jeśli w przypadku IIFE zapomnimy zadeklarować zmiennej słowem kluczowym <code>var</code>?</p>
{% highlight javascript %} 
(function(){
  for (i = 0; i < 5; i++){
      console.log(i);
  }
})();
console.log('na koniec pętli:', i); //"na koniec pętli:5"
{% endhighlight %}
<p>Zmienna została wyciągnięta do zasięgu globalnego, przez co jest znowu dostępna poza blokiem funkcji!</p>
<p>Dlaczego tak się wydarzyło? Odpowiedzialny jest za to hoisting, czyli wyszukiwanie i wyciąganie deklaracji zmiennych do góry przed kompilacją kodu. Żeby to zobrazować, spójrzmy na fragment kodu:</p>
{% highlight javascript %} 
function drive(){
  x=7;
  console.log(x);//7
  var x;
}
drive();
console.log(x);//błąd
{% endhighlight %}
<p>Zmienna została normalnie zadeklarowana i nie jest dostępna poza zasięgiem tej funkcji.</p>
<p>Jak uniknąć przypadkowego tworzenia zmiennych globalnych? Tutaj przychodzi z pomocą tryb <code>strict mode</code>:</p>
{% highlight javascript %} 
"use strict";
(function(){
  for (i = 0; i < 5; i++){
      console.log(i);
  }
})();
console.log('na koniec pętli:', i); //błąd
{% endhighlight %}
<p>Nie pozwoli nam on stworzyć przypadkowej zmiennej globalnej.</p>
<p>Jednak wadą tych rozwiązań jest komplikowanie kodu. Czy nie da się tego napisać prościej? Z pomocą nadchodzi ES6.</p>
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> ES6 let i const</h3>
<p>W ES6 poza słowem kluczowym <code>var</code> do deklarowania zmiennych służy także <code>let</code>, które pozwala na deklarowanie zmiennej dostępnej w obecnym zasięgu i zagnieżdżonych funkcjach. Nasz kod wygląda w taki sposób przy użyciu <code>let</code>:</p>
{% highlight javascript %} 
for (let i = 0; i < 5; i++){
    console.log(i);
}
console.log('na koniec pętli:', i);//błąd
{% endhighlight %}
<p>Kolejną rzeczą, która została dodana, jest znane już z innych języków programowania <code>const</code>. Zmienna zadeklarowana w taki sposób nie może zmienić swojej wartości później w programie.</p>
{% highlight javascript %} 
const a = 1;
a = 2; //błąd
{% endhighlight %}
<p>Używanie const zapobiega przypadkowej zmianie wartości.</p>
<p><code>Let</code> umożliwia stworzenie innego zasięgu zmiennych, niż tylko zasięgu funkcji, a const pozwala stworzyć stałą zamiast zmiennej. To bardzo przydatne mechanizmy, dlatego powinniśmy z nich korzystać na codzień.</p>