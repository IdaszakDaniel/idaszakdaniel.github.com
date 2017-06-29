---
layout: post
title: "Daj Sie Poznac #3 JSON. Service i Controller w AngularJS"
titlePL: "Daj Się Poznać #3 JSON. Service i Controller w AngularJS"
description: ""
category: Article
ima: "/assets/img/dsppost3.png"
tags: ["DajSiePoznac","AngularJS","JSON"]
linkTitle: [ 
		{a: "Co to jest JSON?", b: "a1"},
		{a: "AngularJS Service", b: "a2"},
		{a: "AngularJS Controller", b: "a3"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
<img src="{{ site.baseurl }}/assets/img/json.png" alt="" style="display: inline-block; padding-left: 20px;">
</center><br>

<p>W trzeciej części projektu Mistrz Makro, który w tym momencie piszę w AngularJS zajmiemy się pierwszą częścią obsługi danych. Stworzymy strukturę danych w pliku JSON oraz serwis, który zwróci te dane do kontrolera. Kontroler natomiast wyświetli je nam w konsoli. W kolejnej części będziemy dalej przesyłać dane, tak by mogły się w aplikacji zapisać, a potem wyświetlić dzięki dyrektywie. Zaczynajmy! </p> <!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout 64b463ab817cdfb379375a6abb2ca0c616987fcf{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Co to jest JSON?</h3>
<p>JSON to skrót od JavaScript Object Notation. Jest to składnia do przechowywania i przesyłania danych. Między serwerem a przeglądarką, możemy przesyłać tylko tekst, a JSON upraszcza nam to, poprzez możliwość łatwej konwersji do obiektu języka JavaScript. Nazwa może być trochę myląca, jednak JSON jest kompatybilny nie tylko z JavaScriptem, ale także wieloma innymi językami.</p>

<p>W naszej aplikacji, musimy pobrać dane, zawierające ścieżki zdjęć produktów do quizu, zarówno jak i wszystkie dane, które użytkownik będzie zgadywał. Stwórzmy więc plik <code>answers.json</code>:</p>
{% highlight json %} 
{
"questions":[
		{
		  "id": "1",
		  "name": "bułka",
		  "kcal": 600,
		  "protein": 30,
		  "carb": 60,
		  "fat": 6,
		  "img": "img1.jpg"
		},
		{
		  "id": "2",
		  "name": "pizza",
		  "kcal": 900,
		  "protein": 20,
		  "carb": 80,
		  "fat": 20,
		  "img": "img2.jpg"
		}
	]
}
{% endhighlight %}
<p>W powyższym kodzie JSON będziemy mieli dwa obiekty, z kluczami i odpowiadającymi im wartościami, które potrzebne będą nam do quizu.</p>
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> AngularJS Service</h3>

<p>Musimy teraz stworzyć serwis, który odbierze nam ten plik i pozwoli na odebranie go w aplikacji. Plik nazwiemy <code>GetJson.service.js</code>, a wyglądał będzie tak:</p>
{% highlight javascript %} 
(function() {
  'use strict';

  angular
    .module('quizProject')
    .service('GetJson', GetJson);

    function GetJson($http) {
      return {
        getData:  function() {
          return $http.get('answers.json').then(function(response) {
        return response.data;
      });
        }
      };
    }
})();
{% endhighlight %}
<p>W powyższym kodzie pozwalamy serwisowi zwrócić funkcje <code>getData</code>, która pobiera plik i zwraca jako <code>promise</code>, który odbieramy funkcją <code>.then</code>, która jako parametry przyjmuje najpierw zachowanie w przypadku pozytywnego zakończenia <code>promise</code> a jako drugi argument, zachowanie w przypadku niepowodzenia. W naszym przypadku zwracamy dane, które odbierzemy w kontrolerze.</p>
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> AngularJS Controller</h3>
<p>Musimy tylko pamiętać, żeby dodać nasz kontroler w pliku <code>index.html</code>, podobnie jak wcześniej robiliśmy to z naszym modułem. Używamy do tego <code>ng-controller="MainController as main"</code> w <code>divie</code> o klasie <code>container</code>:</p>
{% highlight html %} 
<body>
  <div class="container" ng-controller="MainController as main">
    <h1 class="title">QuizApp</h1>
    <quiz/>
  </div>
</body>
{% endhighlight %}
<p>Nasz kontroler będzie wyglądał tak:</p>
{% highlight javascript %} 
(function() {
  'use strict';

  angular
    .module('quizProject')
    .controller('MainController', MainController);

  function MainController($scope, GetJson, $http) {

    var promiseAnswers = GetJson.getData();

    promiseAnswers.then(function(data){
      data.questions.forEach(function(answer) {
        console.log(answer.id);
        console.log(answer.name);
        console.log(answer.kcal);
        console.log(answer.protein);
        console.log(answer.carb);
        console.log(answer.fat);
      });
    });
  };
})();
{% endhighlight %}
<p>Wstrzykujemy zależność do <code>function MainController($scope, GetJson, $http)</code>, dzięki czemu możemy odwoływać się teraz do funkcji z <code>GetJson</code>.</p>
<p>poprzez <code>GetJson.getData();</code> pobieramy dane z serwisu do zmiennej i następnie odbieramy znów jako <code>promise</code> i odwołujemy się do parametru <code>data</code>. Korzystamy z funkcji <code>forEach</code>, który pozwala nam przeskoczyć po wszystkich argumentach tablicy obiektów <code>questions</code>. Następnie każdą wartość wyświetlamy z osobna w konsoli. Nasz rezultat powinien wyglądać w taki sposób po odpaleniu aplikacji:</p>
<center><img src="{{ site.baseurl }}/assets/img/DSP3_1.png" alt="" style="display: inline-block;"></center><br>
<p>Teraz pozostało stworzyć tylko modele, służące do przechowywania danych, oraz umożliwić dyrektywie swobodne korzystanie z nich, jednak to opiszę już w kolejnym poście.</p>