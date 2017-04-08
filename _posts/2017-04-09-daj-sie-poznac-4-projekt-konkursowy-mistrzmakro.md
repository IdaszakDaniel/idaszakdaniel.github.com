---
layout: post
title: "Daj Sie Poznac #4 OOP. Model w AngularJS"
titlePL: "Daj Się Poznać #4 OOP. Model w AngularJS"
description: ""
category: Article
tags: ["DajSiePoznac","AngularJS","OOP"]
linkTitle: [ 
		{a: "Tworzymy model dla makroskładników", b: "a1"},
    {a: "Tworzymy model dla produktów", b: "a2"},
    {a: "Kontroler czyli łączymy modele oraz serwis", b: "a3"}
		]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
</center><br>
<p>W poprzednich częściach projektu Mistrz Makro, pisanego w AngularJS stworzyliśmy wstępny szablon projektu, wyjaśniliśmy jak korzystać z <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">Gita</a>, zrozumieliśmy czym jest i do czego służy <a href="http://www.idaszak.com/article/2017/04/02/daj-sie-poznac-3-projekt-konkursowy-mistrzmakro">JSON</a>, dowiedzieliśmy się co to <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">MVC</a> i zaimplementowaliśmy kontroler oraz serwis, które pozwoliły wyświetlić nam odebrane z JSON dane w konsoli. W dzisiejszej, czwartej już części, skupimy się na tym żeby umieścić nasze dane w modelu, żeby w kolejnej części dyrektywa tworząca quiz mogła z nich swobodnie korzystać. Zaczynajmy!</p> <!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout 1b3cb2d8eee6994d93ad28fd3f6ce037b0b6a771{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Tworzymy model dla makroskładników</h3>
<p>AngularJS to framework MVC, czyli Model, Widok oraz Kontroler. Zajmiemy się tym razem Modelem, czyli przechowywaniem danych aplikacji. Na początek, musimy stworzyć serwis, w którym będzie zapisany pojedyńczy produkt:</p>
{% highlight javascript %} 
(function() {
  'use strict';

  angular
    .module('quizProject')
    .service('Macro', MacroModel);

  function MacroModel() {

    var Macro = function(id, name, kcal, protein, carb, fat, img) {
      this.id = id || 0;
      this.name = "";
      this.kcal = kcal || 0;
      this.protein = protein || 0;
      this.carb = carb || 0;
      this.fat = fat || 0;
      this.img = img || "";
    }

    Macro.prototype = {
      setId: function(id) {
        this.id = id;
      },

      setName: function(name) {
        this.name = name;
      },

      setKcal: function(kcal) {
        this.kcal = kcal;
      },

      setProtein: function(protein) {
        this.protein = protein;
      },

      setCarb: function(carb) {
        this.carb = carb;
      },

      setFat: function(fat) {
        this.fat = fat;
      },

      setImg: function(img) {
        this.img = img;
      },
  
    };
    return Macro;
  }
})();
{% endhighlight %}
<p>Na wzór <a href="http://www.idaszak.com/article/2017/04/02/czy-javascript-jest-obiektowy">klasy</a> tworzymy funkcję prototypową, która posiada takie parametry jak <code>id</code>, <code>name</code>, <code>kcal</code>, <code>protein</code>, <code>carb</code>, <code>fat</code>, <code>img</code> i nadaje im wartości domyślne:</p>
{% highlight javascript %} 
var Macro = function(id, name, kcal, protein, carb, fat, img) {
  this.id = id || 0;
  this.name = "";
  this.kcal = kcal || 0;
  this.protein = protein || 0;
  this.carb = carb || 0;
  this.fat = fat || 0;
  this.img = img || "";
}
{% endhighlight %}
<p>Wszystkie te parametry odpowiadają tym z naszego pliku <code>answers.json</code>:</p>
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
<p>Do prototypu tej funkcji, dodajemy funkcje, które będą dostępne w każdym utworzonym obiekcie. Dzięki nim, będziemy mogli przypisać nasze dane pobrane z serwisu do odpowiednich parametrów. Rozbiliśmy to na kilka pomniejszych funkcji, które pobrany parametr przypisują do zmiennej aktualnego obiektu, co wskazuje <code>this.</code>:</p>
{% highlight javascript %} 
Macro.prototype = {
  setId: function(id) {
    this.id = id;
  },

  setName: function(name) {
    this.name = name;
  },

  setKcal: function(kcal) {
    this.kcal = kcal;
  },

  setProtein: function(protein) {
    this.protein = protein;
  },

  setCarb: function(carb) {
    this.carb = carb;
  },

  setFat: function(fat) {
    this.fat = fat;
  },

  setImg: function(img) {
    this.img = img;
  },

};
{% endhighlight %}
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Tworzymy model dla produktów</h3>
<p>Kolejnym krokiem będzie stworzenie drugiego modelu, który będzie pełnił rolę listy obiektów z modelu makroskładników.</p>
{% highlight javascript %} 
(function() {
  'use strict';

  angular
    .module('quizProject')
    .service('ProductModel', ProductModel);

  function ProductModel() {
    var _list = [];

    return {
      addItem: function(model) {
        return _list.push(model);
      },

      listOfItems: function(){
        return _list;
      },

    }
  }
})();
{% endhighlight %}
<p>Serwis, który widzimy posiada prywatną tablicę, na której manipulacje możemy wykonywać za pomocą utworzonych metod. W tym przypadku jest to <code>addItem</code> oraz <code>listOfItems</code>. W pierwszym przypadku, <code>addItem</code> pozwala dodać obiekt stworzony przez <code>MacroModel</code> do tablicy. <code>listOfItems</code> zwraca nam całą listę aktualnych obiektów.</p>
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Kontroler czyli łączymy modele oraz serwis</h3>
<p>Nasz kontroler z poprzedniego posta po drobnych modyfikacjach, wygląda w taki sposób:</p>
{% highlight javascript %} 
(function() {
  'use strict';

  angular
    .module('quizProject')
    .controller('MainController', MainController);

  function MainController($scope, GetJson, $http, Macro, ProductModel) {

    var promiseAnswers = GetJson.getData();

    var Model = new Macro();
    $scope.Model = Model;
        
    promiseAnswers.then(function(data){
      data.questions.forEach(function(answer) {
        $scope.Model.setId(answer.id);
        $scope.Model.setName(answer.name);
        $scope.Model.setKcal(answer.kcal);
        $scope.Model.setProtein(answer.protein);
        $scope.Model.setCarb(answer.carb);
        $scope.Model.setFat(answer.fat);
        $scope.Model.setImg(answer.img);

        ProductModel.addItem($scope.Model);
        console.log(ProductModel.listOfItems());
        $scope.Model = new Macro();
      });
    });

  };
})();

{% endhighlight %}
<p>Na początku zaczynamy od wstrzykiwania zależności, które opisywałem w poprzedniej cześci. Dzięki temu zabiegowi, będziemy mogli odwoływać się do naszych modeli z poziomu kontrolera.</p>
{% highlight javascript %} 
function MainController($scope, GetJson, $http, Macro, ProductModel) {
{% endhighlight %}
<p>Dodaliśmy <code>Macro</code> oraz <code>ProductModel</code>, czyli nazwy serwisów, dzięki czemu możemy teraz wykonać operację <code>new Macro();</code>:</p>
{% highlight javascript %} 
  var Model = new Macro();
  $scope.Model = Model;
{% endhighlight %}
<p>W ten sposób tworzymy nowy obiekt dla makroskładników jednego z produktów i nazywamy go <code>Model</code>.</p>
{% highlight javascript %} 
promiseAnswers.then(function(data){
  data.questions.forEach(function(answer) {
    $scope.Model.setId(answer.id);
    $scope.Model.setName(answer.name);
    $scope.Model.setKcal(answer.kcal);
    $scope.Model.setProtein(answer.protein);
    $scope.Model.setCarb(answer.carb);
    $scope.Model.setFat(answer.fat);
    $scope.Model.setImg(answer.img);

    ProductModel.addItem($scope.Model);
    console.log(ProductModel.listOfItems());
    $scope.Model = new Macro();
  });
});
{% endhighlight %}
<p>Nasza funkcja z poprzedniego posta, która pozwoliła wyświetlić promise, wysłany z serwisu <code>GetJson</code> jest teraz zmieniona w taki sposób, żeby odebrane dane dodawać do obiektu model. Odwołujemy się do Modelu i jego metod takich jak <code>setId</code>.</p>
{% highlight javascript %} 
  ProductModel.addItem($scope.Model);
  console.log(ProductModel.listOfItems());
  $scope.Model = new Macro();
{% endhighlight %}
<p>Następnie dodajemy model do tablicy obiektów znajdującej się w <code>ProductModel</code> za pomocą metody <code>addItem</code>. Wyświetlamy całość listy obiektów i ponownie tworzymy nowy obiekt, który wypełnimy kolejnymi danymi.</p>
<img src="{{ site.baseurl }}/assets/img/DSP4_1.png" alt="widok konsoli">
<p>W taki sposób powinna wyglądać konsola po uruchomieniu aplikacji. Wyświetlona jest tablica zawierająca dwa obiekty, których każdy parametr odpowiada tym z <code>answers.json</code>.</p>
