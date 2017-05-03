---
layout: post
title: "Daj Sie Poznac #5 Dyrektywa w AngularJS - czesc 2"
titlePL: "Daj Się Poznać #5 Dyrektywa w AngularJS - część 2"
description: ""
date: 2017-05-03 23:00:00 +0100
category: Article
ima: "/assets/img/dsppost6.png"
tags: ["DajSiePoznac","AngularJS"]
linkTitle: [ 
		{a: "Zmiany w pliku HTML dyrektywy", b: "a1"},
    {a: "Tworzymy plik JS Dyrektywy", b: "a2"},
    {a: "Cały kod dyrektywy", b: "a3"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
</center><br>
<p>W poprzednich częściach tworzyliśmy aplikację AngularJS Mistrz Makro, która jest quizem, pozwalającym na zgadywanie makroskładników produktu przedstawionego na zdjęciu. Udało się nam stworzyć modele, pozwalające na zarządzanie danymi aplikacji. Odebraliśmy dane z pliku JSON i wyświetliliśmy je w konsoli. W poprzedniej części zaczęliśmy budować główny mechanizm napędzający aplikacje, czyli dyrektywę. Stworzyliśmy jej część składającą się z pliku HTML. Tym razem znów zajmiemy się dyrektywą i utworzymy plik Javascript, który sprawi, że nasz quiz zacznie działać tak jak powinien. Zaczynajmy!</p><!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout 28c29bb2082ce088dc554c5c73cd01c02163d8c9{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Zmiany w pliku HTML dyrektywy</h3>
<p> W stosunku do poprzedniego postu, w pliku HTML dyrektywy dodaliśmy tylko paragrafy, które wyświetlają wyniki, oraz dodatkowy stan <code>inProgress</code>. Tak prezentuje się cały kod:</p>
{% highlight html %} 
<div class="container" ng-show="inProgress">
  <div class="row">
    <div class="col-md-12">

      <div ng-show="!quizEnd">
        <h2>{{name}}</h2>
        <img ng-src="/app/assets/{{img}}">
        <p>Kcal:</p><input type="number" ng-model="macro1">
        <p ng-show="!answerMode">{{Res[0]}}</p>
        <p>Białko:</p><input type="number" ng-model="macro2">
        <p ng-show="!answerMode">{{Res[1]}}</p>
        <p>Węglowodany:</p><input type="number" ng-model="macro3">
        <p ng-show="!answerMode">{{Res[2]}}</p>
        <p>Tłuszcz:</p><input type="number" ng-model="macro4">
        <p ng-show="!answerMode">{{Res[3]}}</p>
        <button ng-click="check()" ng-show="answerMode">Submit</button>
      
        <div ng-show="!answerMode">
          <button ng-click="nextQuestion()">Next</button>
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
<p>Po każdym inpucie dodaliśmy paragraf, który wyświetla się jeśli nie jesteśmy w stanie <code>answerMode</code>, czyli w momencie kiedy wpisaliśmy wartość do inputów i wdusiliśmy przycisk sprawdzający. Wyświetlimy tam wyniki danego pytania.</p>
{% highlight html %}
<p ng-show="!answerMode">{{Res[0]}}</p>
{% endhighlight %}
<p>Stan <code>inProgress</code> znajduje się w dwóch głównych containerach i pozwala na wyświetlenie quizu dopiero po wduszeniu przycisku <code>start</code>.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Tworzymy plik JS Dyrektywy</h3>
<p>Na początku, dyrektywę tworzymy tak samo jak każdy plik AngularJS, czyli wstawiamy IIFE, podłączamy nasz główny moduł i tworzymy dyrektywę, którą nazwałem "quiz".</p>

{% highlight javascript %}
(function() {
  'use strict';

  angular
    .module('quizProject')
    .directive('quiz', quiz);
{% endhighlight %}

<p>Następnie tworzymy funkcje zgodnie z dokumentacją dyrektywy:</p>

{% highlight javascript %}
function quiz(ProductModel) {
  return {
    restrict: 'AE',
    scope: {},
    templateUrl: 'app/main/directive/html/macro.directive.html',
    link: function(scope, elem, attrs) {
{% endhighlight %}

<p>Restrict pozwala na ograniczenie w jaki sposób będziemy mogli wywoływać naszą dyrektywę. Do wyboru mamy cztery sposoby - <code>A</code>, <code>C</code>, <code>E</code>, <code>M</code>, jednak domyślnie jest to <code>AE</code> i właśnie tak będzie w naszej aplikacji. Jednak czym się różnią i w jaki sposób ją wywołują?</p>
{% highlight html %}
A = <div quiz></div>          <!-- jako Atrybut-->
C = <div class="quiz"></div>  <!-- jako Klasa -->
E = <quiz></quiz>             <!-- jako Element-->
M = <!--directive:quiz -->    <!--  jako Komentarz-->
{% endhighlight %}
<p>Scope pozwala na stworzenie wyizolowanego zasięgu zmiennych i pobranie w razie potrzeby wartości przy wywołaniu dyrektywy. Nam jednak nie będzie to potrzebne. Przykład takiego użycia:</p> 
{% highlight html %}
<div quiz name="ABC"></div>
{% endhighlight %}
{% highlight javascript %}
scope: {
  MyQuestionName: '@name'
},
{% endhighlight %}
<p>TemplateUrl przechowuje link do pliku HTML dyrektywy. Link jest funkcją, która wykonuje się przy wywołaniu dyrektywy.</p> 
<p>Teraz możemy wewnątrz funkcji link tworzyć funkcje.</p>

<p>Pierwszą funkcja, którą stworzymy będzie <code>start</code>, która wywołuje się po wduszeniu przycisku "start", po włączeniu aplikacji. Ustawia wszystkie stany, tak, aby mogły się pojawić kolejne elementy w widoku.</p>
{% highlight javascript %}
scope.start = function() {
  scope.id = 0;
  scope.quizEnd = false;
  scope.inProgress = true;
  scope.getQuestion();
};
{% endhighlight %}
<p>Po wywołaniu funkcji id wyświetlanego elementu ustawia się na zero. Dzięki zmiennej <code>quizEnd</code> wiemy że quiz się rozpoczął i nie zakończył. Zmienna <code>inProgress</code> wyświetla na ekranie quiz. Następnie wywołujemy zmienną <code>getQuestion()</code>.</p>

{% highlight javascript %}
scope.getQuestion = function() {
  var q = ProductModel.getQuestion(scope.id);
  if(q) {
    scope.name = q.name;
    scope.kcal = q.kcal;
    scope.protein = q.protein;
    scope.carb = q.carb;
    scope.fat = q.fat;
    scope.img = q.img;
    scope.answerMode = true;
  } else {
    scope.quizEnd = true;
  }
};
{% endhighlight %}
<p>Funkcja <code>getQuestion</code> odbiera z <code>ProductModel</code> dane, wybrane przez aktualną wartość zmiennej <code>scope.id</code>, poprzez funkcję:</p>
{% highlight javascript %}
var q = ProductModel.getQuestion(scope.id);
{% endhighlight %}
<p>A następnie przypisuje pobrane dane do odpowiadających im zmiennych. Zmieniamy stan <code>answerMode</code>, dzięki czemu pojawiają się produkt i inputy, które będziemy mogli wypełnić. Jeśli nasza funkcja nie znajdzie odpowiednich danych, to zakończy quiz zmienną <code>quizEnd</code>. Tak prezentuje się aplikacja po wduszeniu przycisku start:</p>
<img src="{{ site.baseurl }}/assets/img/DSP6_1.png" alt="aplikacja po wduszeniu start">
<p>Możemy teraz wpisać wartości numeryczne w pola, jednak nie są one walidowane w żaden sposób i prawdopodobnie pozostawię użytkownikowi wybór jakie pola wypełnić.</p>
<p>Po wduszeniu przycisku Submit, wywoła się funkcja <code>check()</code>, która wygląda w taki sposób:</p>

{% highlight javascript %}
scope.check = function(){
  scope.Res = [];
  scope.answerMode = false;
  scope.Res[0] = test(scope.kcal, scope.macro1);
  scope.Res[1] = test(scope.protein, scope.macro2);
  scope.Res[2] = test(scope.carb, scope.macro3);
  scope.Res[3] = test(scope.fat, scope.macro4);
};
{% endhighlight %}
<p>Funkcja <code>check()</code> zmienia stan <code>answerMode</code> na false, aby określić w aplikacji, że user już zatwierdził swoją odpowiedź. następnie tworzymy tablicę, w której przechowamy wyniki porównujące wartość podaną przez użytkownika z wartością w naszej aplikacji.</p>
<p>Funkcja test:</p>
{% highlight javascript %}
var test = function(modelData, viewData){
  if(modelData > viewData) return modelData - viewData + " za mało";
  if(modelData < viewData) return viewData - modelData + " za dużo";
  if(modelData == viewData) return "idealnie!";
};
{% endhighlight %}
<p>Funkcja <code>test</code>, jest funkcją nie przypisaną do <code>scope</code>, ponieważ nie będziemy wywoływać jej w widoku aplikacji, tylko bezpośrednio w dyrektywie. Funkcja ta przyjmuje dwa argumenty - wartość z inputa, oraz odpowiadającą wartość z aplikacji. Po porównaniu tych dwóch wartości, zwracana jest różnica wraz z opisem czy wartość była za mała czy za duża. W przypadku trafienia poprawnej wartości, funkcja zwraca string "idealnie!". Po przypisaniu tych wartości do tablicy <code>scope.Res</code>, możemy wyświetlić ją w widoku aplikacji. Stanie się tak, ponieważ paragrafy, które wyświetlają tą tablice, wyświetlą się w momencie, kiedy zmienimy stan <code>answerMode</code> na fałsz, co przed chwilą się wydarzyło. Tak wygląda nasza aplikacja, w momencie kiedy nie wpisaliśmy żadnej wartości i wdusiliśmy przycisk <code>submit</code>:</p>
<img src="{{ site.baseurl }}/assets/img/DSP6_2.png" alt="aplikacja po wduszeniu submit">
<p>Następna funkcja wywołuje się po wduszeniu klawisza <code>next</code>, który wyświetla się po wykonaniu funkcji <code>check()</code>. Nazywa się <code>nextQuestion()</code></p>
{% highlight javascript %}
scope.nextQuestion = function() {
  scope.id++;
  scope.getQuestion();
}
{% endhighlight %}
<p>Pozwala tylko na zwiększenie id, aby wyszukać kolejny produkt i znowu wywołuje funkcję <code>qetQuestion()</code></p>
{% highlight javascript %}
scope.reset = function() {
  scope.inProgress = false;
}
{% endhighlight %}
<p>Ostatnią już funkcją tej dyrektywy jest <code>scope.reset()</code>, które wywoływany jest przez przycisk <code>play again</code>, pojawiający się w momencie kiedy w bazie zabraknie pytań. Funkcja zmienia stan <code>inProgress</code>, dzięki czemu wracamy do przycisku <code>start</code>.</p>

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Cały kod dyrektywy</h3>
<p>Tak prezentuje się cały kod Javascript dyrektywy, którą zapisałem jako <code>macro.directive.js</code>:</p>
{% highlight javascript %}
(function() {
  'use strict';

  angular
    .module('quizProject')
    .directive('quiz', quiz);

  function quiz(ProductModel) {
      return {
        restrict: 'AE',
        scope: {},
        templateUrl: 'app/main/directive/html/macro.directive.html',
        link: function(scope, elem, attrs) {

          scope.start = function() {
            scope.id = 0;
            scope.quizEnd = false;
            scope.inProgress = true;
            scope.getQuestion();
          };

          scope.check = function(){
            scope.Res = [];
            scope.answerMode = false;
            scope.Res[0] = test(scope.kcal, scope.macro1);
            scope.Res[1] = test(scope.protein, scope.macro2);
            scope.Res[2] = test(scope.carb, scope.macro3);
            scope.Res[3] = test(scope.fat, scope.macro4);
          };

          var test = function(modelData, viewData){
            if(modelData > viewData) return modelData - viewData + " za mało";
            if(modelData < viewData) return viewData - modelData + " za dużo";
            if(modelData == viewData) return "idealnie!";
          };

          scope.getQuestion = function() {
            var q = ProductModel.getQuestion(scope.id);
            if(q) {
              scope.name = q.name;
              scope.kcal = q.kcal;
              scope.protein = q.protein;
              scope.carb = q.carb;
              scope.fat = q.fat;
              scope.img = q.img;
              scope.answerMode = true;
            } else {
              scope.quizEnd = true;
            }
          };

          scope.nextQuestion = function() {
            scope.id++;
            scope.getQuestion();
          }

        }
      }
    };
})();

{% endhighlight %}
<p>Proszę się nie martwić surowym wyglądem aplikacji, na stylowanie przyjdzie jeszcze czas - w najbliższych częściach postów Daj się Poznać.</p>


