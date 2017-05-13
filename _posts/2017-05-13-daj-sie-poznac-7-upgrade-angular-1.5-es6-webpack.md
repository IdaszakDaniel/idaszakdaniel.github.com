---
layout: post
title: "Daj Sie Poznac #7 Upgrade do AngularJS 1.6 - Components, ES6, Webpack"
titlePL: "Daj Się Poznać #7 Upgrade do AngularJS 1.6 - Components, ES6, Webpack"
description: ""
date: 2017-05-13 19:30:00 +0100
category: Article
ima: "/assets/img/dsppost7.png"
tags: ["DajSiePoznac","AngularJS","ES6"]
linkTitle: [ 
		{a: "Co się zmieni w stosunku do poprzedniego kodu?", b: "a1"},
    {a: "GetJson service - zmiany", b: "a2"},
    {a: "Modele z danymi - zmiany", b: "a3"},
    {a: "Directive - zmiana na Component - controller", b: "a4"},
    {a: "Directive - zmiana na Component - pozostałe pliki", b: "a5"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
</center><br>
<p>W poprzednich częściach tworzyliśmy aplikację AngularJS Mistrz Makro, która jest quizem, pozwalającym na zgadywanie makroskładników produktu przedstawionego na zdjęciu. Udało nam się ukończyć wstępny zarys działającej aplikacji. Tym razem zajmiemy się zmianą kodu do wersji AngularJS 1.6. Nasza aplikacja będzie przerobiona na najnowszy standard, pozwalający na łatwiejszą zmianę do Angulara (2/4) w razie potrzeby. Skorzystamy także z Babela, który pozwoli nam pisać kod w ES6, dzięki czemu zastosujemy sporo rzeczy, które poznaliśmy przy <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">serii postów o ES6</a>. Zaczynajmy!</p><!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout 1a8c63fba51ef10ff143c719c30e8430c1759ab9{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Co się zmieni w stosunku do poprzedniego kodu?</h3>

<p>Przede wszystkim skorzystamy tym razem z innego startera. <a href="https://github.com/AngularClass/NG6-starter">NG6-starter</a> ze skonfigurowanym gulpem, webpackiem, babelem do ES6, przygotowany do korzystania z SASS oraz testowania będzie naszym wyborem.</p>
<ul style="padding-left: 30px;">
  <li>Dyrektywa zostanie zamieniona przez <code>component</code></li>
  <li><code>Class</code> - Użyjemy klas z ES6 do tworzenia serwisów i komponentów. Więcej na ten temat możesz dowiedzieć się <a href="https://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">tutaj</a>.</li>
  <li><code>Webpack</code> - zamiast bowera skorzystamy z webpacka.</li>
  <li><code>arrow functions</code> - w kodzie będziemy używali funkcji strzałkowych z ES6. Przeczytać na ten temat możesz <a href="https://www.idaszak.com/article/2017/05/07/es6-4-arrow-functions-funkcje-strzalkowe">tutaj</a>.</li>
  <li><code>let</code> skorzystamy także z nowego słowa kluczowego, które poznaliśmy w ES6. Więcej: <a href="https://www.idaszak.com/article/2017/04/05/es6-2-var-let-const">tutaj</a>.</li>
</ul>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> GetJson service - zmiany</h3>

<p>Serwisy tworzymy jako klasy z konstruktorem i metodami.</p>
{% highlight js %} 
export default class GetJson {
  constructor($http) {
    this._$http = $http;
  }

  getData() {
    let request = {
      url: 'answers.JSON',
      method: 'GET',
    };
    return this._$http(request).then((res) => res.data);
  }
}
{% endhighlight %}
<p>Tworzymy klasę <code>GetJson</code> gotową do eksportu i użycia w komponencie. W konstruktorze, który wywołuje się przy stworzeniu obiektu mamy wstrzykiwanie zależności, dzięki któremu możemy korzystać z funkcji <code>$http</code>. Metoda <code>getData</code> określa ścieżkę pliku i typ komunikacji. Jako wartość zwracaną mamy <code>this._$http</code>, z którego korzystamy tak jak przypisaliśmy to w konstruktorze. Funkcja zwraca dane z piku <code>answers.JSON</code>.</p>
{% highlight js %} 
import angular from 'angular';

import productsList from './productsList';
import GetJson from './GetJson';

export default angular
  .module('app.services', [])
  .service({ productsList })
  .service({ GetJson });
{% endhighlight %}
<p>W pliku <code>services.js</code> importujemy nasz serwis i rejestrujemy go do modułu <code>app.services</code>.</p>

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Modele z danymi - zmiany</h3>
<p>Wcześniej mieliśmy pliki <code>macro.model.js</code> oraz <code>product.model.js</code>, które były serwisami, do których zapisywaliśmy nasze dane. Tym razem stworzymy jeden serwis składający się z dwóch klas.</p>
<p>Tworzymy plik ProductsList.js zawierający dwie klasy.</p>
{% highlight js %} 
class Macros {
  constructor(id, name, kcal, protein, carb, fat, img){
    this.id = id;
    this.name = name;
    this.kcal = kcal;
    this.protein = protein;
    this.carb = carb;
    this.fat = fat;
    this.img = img;
  }
}
{% endhighlight %}
<p>Macros to pierwsza klasa odpowiadająca za <code>macro.model.js</code>. Nie będzie wykorzystywana poza tym plikiem, więc nie musimy jej eksportować. Nasza klasa tworzy nowe obiekty z podanych wartości.</p>
<p>Kolejną klasą w tym pliku będzie <code>ProductsList</code> czyli odpowiednik <code>product.model.js</code>.</p>
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
}
{% endhighlight %}
<p>Eksportujemy klasę, ponieważ będziemy korzystać z niej w naszym komponencie. W konstruktorze tworzymy listę, w której będziemy przechowywać obiekty. Metoda <code>add</code> dodaje nasze dane do obiektu wytworzonego przez klasę <code>Macros</code>, a następnie dołącza go do tablicy <code>this.list</code>. Metoda <code>getList</code> wyszukuje obiekt o podanym <code>id</code> i go zwraca. Serwis rejestrujemy tak jak to było w przypadku <code>GetJson</code>.</p>

<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Directive - zmiana na Component - controller</h3>

<p>Nasz plik <code>macro.directive.js</code> zostanie zastąpiony plikiem <code>controller.js</code></p>
{% highlight js %} 
export default class Quiz {
  /**
   * @param {ProductsList} productsList
   */
  constructor(productsList, GetJson) {
    "ngInject";
    this.productsList = productsList;
    this.GetJson = GetJson;

    this.id = 0;
    this.quizEnd = true;
    this.inProgress = false;
    this.Res = [];
    this.id = 0;
    this.name = "";
    this.kcal = "";
    this.protein;
    this.carb;
    this.fat;
    this.img = "";
    this.answerMode = false;

    this.init();
  }
{% endhighlight %}
<p>Stworzyliśmy klasę <code>Quiz</code>, w którą wstrzykujemy <code>productsList</code> i <code>GetJson</code>. Nasz konstruktor ustawia wartości początkowe i wywołuje metodę <code>this.init();</code>.</p>
{% highlight js %}
init(){
  let answers = this.GetJson.getData();
  answers.then(data => {
    data.questions.forEach(answer => {
      this.productsList.add(answer.id,
                            answer.name,
                            answer.kcal,
                            answer.protein,
                            answer.carb,
                            answer.fat,
                            answer.img);
    });
  });
}
{% endhighlight %}
<p>Funkcja <code>init()</code> zmieniła się nieznacznie w stosunku do naszego poprzedniego kodu. Odbieramy dane z <code>GetJson</code> i dodajemy je do serwisu <code>productsList</code> za pomocą metody <code>add()</code>.</p>
{% highlight js %}
start() {
  this.id = 0;
  this.quizEnd = false;
  this.inProgress = true;
  this.getQuestion();
}
 getQuestion() {
  var q = this.productsList.getList(this.id);
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

nextQuestion() {
  this.id++;
  this.getQuestion();
}

reset() {
  this.inProgress = false;
  this.score = 0;
}
{% endhighlight %}
<p>Funkcje <code>start()</code>, <code>getQuestion()</code>, <code>nextQuestion()</code>, <code>reset()</code> pozostają praktycznie bez zmian, poza różnicami w składni.</p>
{% highlight js %}
check(){
  let test = function(modelData, viewData){
    if(!viewData) return "";
    if(modelData == viewData) return "idealnie!";
    return modelData > viewData ? modelData - viewData + " za mało" : viewData - modelData + " za dużo";
  };
  this.Res = [];
  this.answerMode = false;
  this.Res[0] = test(this.kcal, this.macro1);
  this.Res[1] = test(this.protein, this.macro2);
  this.Res[2] = test(this.carb, this.macro3);
  this.Res[3] = test(this.fat, this.macro4);
}
{% endhighlight %}
<p>Funkcja <code>check()</code> została tylko troszkę skrócona, a funkcja <code>test</code> została dołączona wewnątrz metody.</p>

<h3 id="a5"><span style="color:gray; font-size: 30px;">#</span> Directive - zmiana na Component - pozostałe pliki</h3>
<p>Nasz komponent musimy określić w pliku <code>quiz.component.js</code>. <code>Restrict</code>, <code>bindings</code>, <code>template</code> znamy już z dyrektywy.</p>
{% highlight js %}
import template from './mainFile.html';
import controller from './controller';

let quizComponent = {
  restrict: 'E',
  bindings: {},
  template,
  controller,
  controllerAs: 'c'
};

export default quizComponent;
{% endhighlight %}
<p>Nowością jest <code>controllerAs</code>, który określa jak będziemy odwoływać się do zmiennych i metod, zamiast użycia <code>$scope</code>.</p>
<p>Używamy tym razem odwołania <code>c.zmienna</code> zamiast <code>$scope.zmienna</code>. Poza tymi zmianami, plik html naszej dyrektywy pozostaje taki sam:</p>
{% highlight html %}
<div class="container" ng-show="c.inProgress">
  <div class="row">
    <div class="col-md-12">

      <div ng-show="!c.quizEnd">
        <h2>{{name}}</h2>
        <img ng-src="/assets/{{c.img}}">
        <p>Kcal:</p><input type="number" ng-model="c.macro1">
        <p ng-show="!c.answerMode">{{c.Res[0]}}</p>
        <p>Białko:</p><input type="number" ng-model="c.macro2">
        <p ng-show="!c.answerMode">{{c.Res[1]}}</p>
        <p>Węglowodany:</p><input type="number" ng-model="c.macro3">
        <p ng-show="!c.answerMode">{{c.Res[2]}}</p>
        <p>Tłuszcz:</p><input type="number" ng-model="c.macro4">
        <p ng-show="!c.answerMode">{{c.Res[3]}}</p>
        <button ng-click="c.check()" ng-show="c.answerMode">Submit</button>
      
        <div ng-show="!c.answerMode">
          <button ng-click="c.nextQuestion()">Next</button>
        </div>
      </div>

      <div ng-show="c.quizEnd">
        <button ng-click="c.reset()">Play again</button>
      </div>

    </div>
  </div>
</div>

<div class="container" ng-show="!c.inProgress">
  <div class="row">
    <button ng-click="c.start()">Start</button>
  </div>
</div>
{% endhighlight %}
<p>Ostatnim plikiem jest <code>components.js</code>, który pozwala zarejestrować nasz komponent do kolejnego modułu <code>app.components</code>:</p>
{% highlight js %}
import angular from 'angular';

import quizComponent from './quiz.component';

export default angular
  .module('app.components', [
  ])
  .component('quiz', quizComponent);
{% endhighlight %}