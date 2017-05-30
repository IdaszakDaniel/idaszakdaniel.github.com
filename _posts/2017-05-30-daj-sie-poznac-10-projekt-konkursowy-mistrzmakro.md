---
layout: post
title: "Daj Sie Poznac #10 Walidacja formularzy, losowanie produktow"
titlePL: "Daj Się Poznać #10 Walidacja formularzy, losowanie produktów"
description: ""
date: 2017-05-30 22:40:00 +0100
category: Article
ima: "/assets/img/dsppost10.png"
tags: ["DajSiePoznac","AngularJS"]
linkTitle: [ 
		{a: "Walidacja formularzy", b: "a1"},
    {a: "Losowanie produktów", b: "a2"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<!-- {% highlight js %} 
{% endhighlight %} -->
<center>	
<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
</center><br>
<p>Konkurs Daj Się Poznać zbliża się ku końcowi i jest to 10 post tej serii, co stanowi minimum wymagane przez regulamin. Stworzyliśmy aplikację Mistrz Makro w AngularJS, służącą do sprawdzenia rozbieżności w liczeniu kalorii "na oko". Pozostało nam tylko dopracować detale aplikacji, takie jak walidacja formularzy, czy losowa sekwencja pytań w quizie. Zaczynajmy!</p><!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout d4cf597e4ca7177a2a4b5fa6a5d230993c7be14b{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Walidacja formularzy</h3>
<p>Jednym z detali, który został nam na koniec, jest walidacja formularzy. Jednym z założeń jest pozostawienie użytkownikowi dowolności, co do wypełnionych pól. Jednak żeby aplikacja miała sens, musimy ograniczyć tą dowolność, do przynajmniej jednego pola.</p>
<p>Powinniśmy upewnić się, że użytkownik poda nam wartość, która jest typu numeric. Trzeba będzie poinformować Angulara, że chodzi nam tylko o wartości całkowite.</p>
<p>Kolejnym ograniczeniem powinna być liczba większa niż zero, ponieważ zakładamy że każde pole będzie miało jakąś wartość.</p>
<p>Na sam początek, zacznijmy od określenia typu inputa na taki, który pozwoli nam wprowadzać tam tylko liczby</p>
{% highlight html %} 
<p>Kcal:</p><input type="number" ng-model="c.macro1">
{% endhighlight %}
<p>Następnie ustalmy najmniejszą wartość jaką użytkownik może wprowadzić, poprzez atrybut <code>min</code>:</p>
{% highlight html %} 
<p>Kcal:</p><input type="number" ng-model="c.macro1" min="0">
{% endhighlight %}
<img src="{{ site.baseurl }}/assets/img/DSP10_1.png" alt="">
<p>Sprawdzając później w Angularze czy nasz formularz jest poprawny, otrzymamy błędny wynik, ponieważ angular zignoruje ograniczenie do liczb całkowitych. W praktyce HTML5 wyświetlił by komunikat o błędzie, jednak angular określiłby formularz jako <code>valid</code>. Musimy więc zastosować <code>RegExp</code>, który określi że w inpucie mogą pojawić się tylko liczby całkowite:</p>
{% highlight html %} 
<p>Kcal:</p><input type="number" ng-model="c.macro1" min="0" ng-pattern="/^(0|[1-9][0-9]*)$/">
{% endhighlight %}
<p>Krótko objaśniając jak działa ten <code>RegExp</code> - <code>/ /</code> dwa slashe oznaczają początek i koniec wyrażenia regularnego <code>RegExp</code>. <code>^</code> oznacza początek stringa. W nawiasie mamy naszą wartość, czyli 0 lub (oznaczone pionową kreską <code>|</code>) wartość zaczynającą się od jedynki, a następnie dowolnej ilości cyfr od 0-9, co oznaczone jest gwiazdką <code>*</code>. Dolar <code>$</code> oznacza zakończenie stringa.</p>
<img src="{{ site.baseurl }}/assets/img/DSP10_3.png" alt="">
<p>Musimy pamiętać jednak o jeszcze jednej rzeczy, a mianowicie o wypełnionym przynajmniej jednym polu.</p>
{% highlight html %} 
<p>Kcal:</p><input type="number" ng-model="c.macro1" min="0" ng-pattern="/^(0|[1-9][0-9]*)$/" ng-required='!(c.macro2 ||  c.macro3 || c.macro4)'>
{% endhighlight %}
<p>Dodajemy atrybut AngularJS <code>ng-required</code>, który dodaje atrybut <code>required</code>, zależnie od spełnionego warunku. W naszym przypadku required dodaje się jeśli nie jest wypełniony żaden inny input. W poprzednich przypadkach, każdy input będzie miał te same atrybuty, poza <code>ng-model</code>. Jednak <code>ng-required</code> nadamy tylko jednemu z nich, w moim przypadku - pierwszemu.</p>
<img src="{{ site.baseurl }}/assets/img/DSP10_2.png" alt="">
<p>Teraz wystarczy tylko zablokować użytkownikowi wduszenie przycisku wysyłania, jeśli formularz jest <code>$invalid</code>.</p>
{% highlight html %} 
<button class="nextBtn"  ng-click="c.MacroForm.$valid ? isFlipped=!isFlipped : ''; c.MacroForm.$valid ? c.check() : ''" ng-show="c.answerMode">Submit</button>
{% endhighlight %}
<p>W atrybucie <code>ng-click</code> mamy dwa warunki, które spełniają się wtedy, kiedy formularz spełnia wszystkie nasze założenia. Jako że nie da się inaczej ustawić dwóch zadań do wykonania dla jednego <code>ng-click</code>, użyłem dwóch skróconych warunków <code>if</code>.</p>
<p>Tak prezentuje się cały kod formularza po zmianach:</p>
{% highlight html %} 
<form name="c.MacroForm">
  <h2>{{c.name}}</h2>
  <img ng-src="/assets/{{c.img}}">
  <p>Kcal:</p><input type="number" ng-model="c.macro1" min="0" ng-required='!(c.macro2 ||  c.macro3 || c.macro4)' ng-pattern="/^(0|[1-9][0-9]*)$/">
  <p>Białko:</p><input type="number" ng-model="c.macro2" min="0" name="macro1" ng-pattern="/^(0|[1-9][0-9]*)$/">
  <p>Węglowodany:</p><input type="number" ng-model="c.macro3" min="0" ng-pattern="/^(0|[1-9][0-9]*)$/">
  <p>Tłuszcz:</p><input type="number" ng-model="c.macro4" min="0" ng-pattern="/^(0|[1-9][0-9]*)$/">
  {{c.MacroForm.$invalid}}
  <button class="nextBtn"  ng-click="c.MacroForm.$valid ? isFlipped=!isFlipped : ''; c.MacroForm.$valid ? c.check() : ''" ng-show="c.answerMode">Submit</button>
</form>
{% endhighlight %}

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Losowanie produktów</h3>
<p>Cóż by to był za monotonny quiz, jeśli cały czas pojawiałaby się nam ta sama sekwencja produktów do określania ich kaloryczności. Musimy więc stworzyć funkcje, która poda nam tablicę z losowymi, nie powtarzającymi się wartościami id. Zdecydowałem się na takie rozwiązanie, żeby nie modyfikować tak bardzo kodu, który już napisaliśmy. Zostawiłem też furtkę do określenia ilości produktów na jedną grę.</p>

{% highlight js %} 
shuffle(y){
  let tab = [...Array(++y).keys()].splice(1);
  let arr = [];
  const x = --tab.length;
  let z = x - 1;
  while(arr.length < x){
    let i = Math.floor((Math.random() * z--) + 0);
    arr.push(tab[i]);
    tab.splice(i, 1);
  }
  return arr;
}
{% endhighlight %}
<p>Nasza funkcja nazywa się <code>shuffle</code> i przyjmuje jeden argument - ilość elementów w tablicy do wygenerowania.</p>
<p>Na początku tworzymy lokalną tablicę za pomocą <code>let</code>, o którym więcej można poczytać <a href="https://www.idaszak.com/article/2017/04/05/es6-2-var-let-const">tutaj</a>. Argumenty do tablicy dodajemy za pomocą <a href="https://www.idaszak.com/article/2017/04/14/es6-3-destrukturyzacja-nowe-podejscie-do-tablic-i-obiektow#a3">operatora rest</a> i funkcji <code>keys()</code>, która tworzy tablice z kluczami tablicy stworzonej na jej potrzeby za pomocą <code>Array(++y)</code>. Jednak musimy rozwiązać pewien problem. <code>keys()</code> zwróci nam wartości od <code>0 do y-1</code>, a nasze id w modelu, zaczynają się od 1. Dlatego tworzymy tablicę o jeden argument większą niż <code>y</code>, a na samym końcu ucinamy pierwszy argument tablicy za pomocą <code>splice(1)</code>.</p>
<p>Tworzymy pusta tablicę <code>arr</code>, którą będziemy powoli wypełniać, a na końcu zwrócimy za pomocą <code>return</code>. Wartość <code>x</code> to ilość argumentów tablicy, która pozostanie niezmienna. Wartość <code>z</code>, to ilość argumentów, tylko już bez słowa kluczowego <code>const</code>.</p>
<p>Pętla dodająca kolejne argumenty będzie wykonywać się tak długo, dopóki tablica będzie krótsza od zadanej ilości argumentów w stałej <code>x</code>. Zmienna <code>i</code> przyjmuje losową liczbę od zera do zmiennej <code>z</code>, a dopiero potem następuje zmniejszenie tej zmiennej o jeden. Dodajemy do naszej tablicy element, wylosowany za pomocą id i zarazem kasujemy go z pierwotnej tablicy.</p>
{% highlight js %} 
export default class ProductsList {
  constructor() {
    this.list = [];
  }

  add(id, name, kcal, protein, carb, fat, img){
    const macros = new Macros(id, name, kcal, protein, carb, fat, img);
    this.list.push(macros);
  }

  getList(id){
    return id < this.list.length ? this.list[id] : false; 
  }

  listLength(){
    return this.list.length;
  }
}
{% endhighlight %}
<p>W klasie ProductsList dodajemy metodę zwracającą długość tablicy <code>listLength()</code>.</p>
{% highlight js %} 
start() {
  this.id = 0;
  this.quizEnd = false;
  this.inProgress = true;
  this.ProductArray = this.shuffle(this.productsList.listLength());
  this.getQuestion();
}
{% endhighlight %}
<p>Wywołujemy naszą funkcję <code>shuffle()</code> wewnątrz funkcji <code>start()</code>. Jako argument przekazujemy długość tablicy, jednak możemy określić żeby quiz korzystał z mniejszej ilości produktów w jednej grze.</p>
{% highlight js %} 
getQuestion() {
  var q = this.productsList.getList(this.ProductArray[this.id]);
  if(q) {
    this.name = q.name;
    this.kcal = q.kcal;
    this.protein = q.protein;
    this.carb = q.carb;
    this.fat = q.fat;
    this.img = q.img;
    this.answerMode = true;
  } else {
    this.quizEnd = true;
  }
}
{% endhighlight %}
<p>Teraz zamieniamy wczytywanie aktualnego produktu w funkcji <code>getQuestion()</code>. Zamiast zwykłego id, korzystamy z id w naszej nowej tablicy z dobraną losową sekwencją produktów.</p>