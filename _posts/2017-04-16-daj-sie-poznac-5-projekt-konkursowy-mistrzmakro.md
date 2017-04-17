---
layout: post
title: "Daj Sie Poznac #5 Dyrektywa w AngularJS - czesc 1"
titlePL: "Daj Się Poznać #5 Dyrektywa w AngularJS - część 1"
description: ""
date: 2017-04-16 17:20:00 +0100
category: Article
ima: "/assets/img/dsppost5.png"
tags: ["DajSiePoznac","AngularJS"]
date: site.time
linkTitle: [ 
		{a: "Nowa metoda modelu ProductModel", b: "a1"},
    {a: "Tworzymy widok dyrektywy", b: "a2"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
</center><br>
<p>W poprzednich częściach projektu Mistrz Makro udało nam sie stworzyć kontroler oraz serwis AngularJS, które pozwoliły naszej aplikacji zapisywać i wyświetlać dane pobrane z pliku JSON. W dzisiejszej części, bardzo krótko opiszę jak planuję stworzyć dyrektywę i jak miałby działać główny mechanizm aplikacji. Utworzymy dyrektywę, która wyświetli nam wszystkie potrzebne informacje z modeli. Zajmiemy się głównie plikiem widoku, który nasza dyrektywa utworzy. Dopiero w kolejnej części sprawimy żeby dyrektywa zadziałała. Zaczynajmy!</p><!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout b57974c18ad12830b4a67d2bad1c4ba58891f6e3{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Nowa metoda modelu ProductModel</h3>
<p>Musimy cofnąć się do poprzedniej części projektu Mistrz Makro, gdzie tworzyliśmy modele dla naszej aplikacji, ponieważ będziemy musieli utworzyć nową metodę. Pozwoli nam ona na wyświetlenie konkretnego produktu, czy dania za pomocą przekazanego id. Plik <code>product.model.js</code>:</p>
{% highlight javascript %} 
getQuestion: function(id) {
  if(id < _list.length) {
    return _list[id];
  } else {
    return false;
  }
}
{% endhighlight %}
<p>Jeśli podane przy wywołaniu funkcji id jest mniejsze niż ilość elementów tablicy obiektów <code>_list</code>, to zwraca konkretne makroskładniki. Możemy teraz wywołać tą funkcję przykładowo w kontrolerze:</p>
{% highlight javascript %} 
console.log(ProductModel.getQuestion(0));
{% endhighlight %}
<p>Podany kod wyświetli nam element o indeksie <code>0</code> pobrany z pliku JSON.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Tworzymy widok dyrektywy</h3>
<p>Stworzymy teraz strukturę pliku HTML, który zostanie wstrzyknięty do naszego widoku poprzez dyrektywę. Nie będzie jeszcze gotowa do użycia, ponieważ potrzebujemy jeszcze kodu Javascript, ale tym zajmiemy się w części kolejnej.</p>
{% highlight html %} 
<div class="container" ng-show="inProgress">
  <div class="row">
    <div class="col-md-12">
      <div ng-show="!quizEnd">
        <img ng-src="/app/images/{{macro.img}}" alt="">
        <h2>{{macro.name}}</h2>
        <p>Kcal:</p><input type="number" name="macro1">
        <p>Białko:</p><input type="number" name="macro2">
        <p>Węglowodany:</p><input type="number" name="macro3">
        <p>Tłuszcz:</p><input type="number" name="macro4">
        <button ng-click="check()" ng-show="">Submit</button>
      
        <div ng-show="">
          <button ng-click="next()">Next</button>
        </div>
      </div>

      <div ng-show="quizEnd">
        <button ng-click="reset()">Play again</button>
      </div>

    </div>
  </div>
</div>

<div class="container" ng-show="!inProgress">
  <div class="row">
    <button ng-click="start()">Start</button>
  </div>
</div>

{% endhighlight %}
<p>Powyższy kod zapisujemy jako <code>macro.directive.html</code>, najlepiej w podfolderze <code>html</code> folderu <code>directive</code>.</p>
<p>Korzystamy tutaj z bootstrapa, a będąc dokładniejszym - z jego grida, który pozwoli na szybkie pozycjonowanie elementów quizu. Jest to jak na razie wstępny zarys, więc jeszcze możemy coś zmieniać w tym kodzie.</p>

{% highlight html %} 
<div class="container">
  <div class="row">
    <div class="col-md-12">
    </div>
  </div>
</div>
{% endhighlight %}
<p>Ta część kodu pozwala na stworzenie Bootstrapowego kontenera, wydzielenia pierwszego rzędu elementów oraz określenia jego szerokości na 12 linii z 12 możliwych.</p>
{% highlight html %} 
<div class="container" ng-show="inProgress">
{% endhighlight %}
<p>Parametr <code>ng-show</code> określa czy dany kontener ma być wyświetlony czy ukryty. Jest to zależne od zmiennej <code>inProgress</code>, która określi czy quiz już się zaczął, czy jeszcze jesteśmy na ekranie startowym aplikacji.</p>

{% highlight html %} 
<div ng-show="!quizEnd">
  <img ng-src="/app/images/{{macro.img}}" alt="">
  <h2>{{macro.name}}</h2>
  <p>Kcal:</p><input type="number" name="macro1">
  <p>Białko:</p><input type="number" name="macro2">
  <p>Węglowodany:</p><input type="number" name="macro3">
  <p>Tłuszcz:</p><input type="number" name="macro4">
  <button ng-click="check()" ng-show="">Submit</button>

  <div ng-show="">
    <button ng-click="next()">Next</button>
  </div>
</div>
{% endhighlight %}
<p>W przypadku jeśli quiz nie jest zakończony, co specyfikuje zmienna <code>quizEnd</code>, aplikacja powinna wyświetlić odpowiedni produkt wyświetlając takie elementy jak: <code>img</code> i <code>name</code>. Dla poszczególnych makroskładników będą to pola <code>input</code>, które są typu <code>number</code>, ponieważ użytkownik będzie musiał wstawić tam odpowiednią liczbę. Pola <code>name</code> pozwolą nam zidentyfikować <code>inputy</code> podczas weryfikacji quizu.</p>
<p>Po naciśnięciu przycisku <code>Submit</code> zatwierdzimy nasze odpowiedzi, które sprawdzi funkcja <code>check()</code>. Po wysłaniu ukryjemy ten przycisk i sprawimy żeby pojawił się kolejny, który pozwoli na przejście do kolejnego zadania w naszym quizie. Będzie to przycisk <code>Next</code> i jego obecnością będziemy manipulować także za pomocą <code>ng-show</code>.</p>
{% highlight html %} 
<div class="container" ng-show="!inProgress">
  <div class="row">
    <button ng-click="start()">Start</button>
  </div>
</div>
{% endhighlight %}
<p>Na samym dole jest ostatni <code>container</code>, który podczas wykonania quizu pojawi się tylko raz, jako ekran początkowy. Użytkownik po wduszeniu przycisku <code>start</code> uruchomi funkcję <code>start()</code>, dzięki której zmienna <code>inProgress</code> typu <code>boolean</code> zmieni swoją wartość na przeciwną i zmieni wyświetlane kontenery inicjując start quizu.</p>