---
layout: post
title: "Daj Sie Poznac #8 stylowanie aplikacji"
titlePL: "Daj Się Poznać #8 stylowanie aplikacji"
description: ""
date: 2017-05-21 19:40:00 +0100
category: Article
ima: "/assets/img/dsppost7.png"
tags: ["DajSiePoznac","AngularJS","ES6"]
linkTitle: [ 
		{a: "Plik HTML komponentu", b: "a1"},
    {a: " Style LESS", b: "a2"},
    {a: " Animacja infinite scrolling background", b: "a3"}
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
<p>W poprzednich częściach tworzyliśmy quiz Mistrz Makro, pozwalający na sprawdzenie rozbieżności w diecie IIFYM. Zbudowaliśmy całą działającą aplikację w AngularJS, a w poprzednim artykule zrefaktoryzowaliśmy kod przy użyciu standardu Javascript ES6. Tym razem nadamy wygląd naszej animacji korzystając z preprocesora CSS, czyli LESS. Stworzymy kilka animacji, które upiększą i poprawią interfejs aplikacji. Zaczynajmy!</p><!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout bbfcb167b0b5841b52e703e6c3e7484079452e4a{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Plik HTML komponentu</h3>
<p>Podjąłem decyzję, że jednak nie będę korzystał z bootstrapa, ponieważ nie będzie w ogóle przydatny w tym co planuję zrobić. Założeniem jest responsywny kontener, wycentrowany na ekranie, przypominający kartę z animacją obracania się. Po jednej strony karty znajdą się inputy do wprowadzenia wartości, a po drugiej wynik po sprawdzeniu danych.</p>
<p><code>mainFile.html</code>, czyli kod naszego komponentu:</p>
{% highlight html %} 
  <div class="container" ng-show="c.inProgress">
  <div class="flip" ng-show="!c.quizEnd"> 
    <div class="card" ng-class="{'flipped':isFlipped}">
      <div class="face front"> 
        <h2>{{c.name}}</h2>
        <img ng-src="/assets/{{c.img}}">
        <p>Kcal:</p><input type="number" ng-model="c.macro1">
        <p>Białko:</p><input type="number" ng-model="c.macro2">
        <p>Węglowodany:</p><input type="number" ng-model="c.macro3">
        <p>Tłuszcz:</p><input type="number" ng-model="c.macro4">
        <button class="nextBtn" ng-click="c.check(); isFlipped=!isFlipped" ng-show="c.answerMode">Submit</button>
      </div> 
      <div class="face back"> 
        <h2>{{c.name}}</h2>
        <img ng-src="/assets/{{c.img}}">
        <p>Kcal: {{c.macro1}}kcal to {{c.Res[0]}}</p>
        <p>Białko: {{c.macro2}}g białka to {{c.Res[1]}}</p>
        <p>Węglowodany: {{c.macro3}}g węglowodanów to {{c.Res[2]}}</p>
        <p>Tłuszcz: {{c.macro4}}g tłuszczu to {{c.Res[3]}}</p>
        <button class="text-center nextBtn" ng-click="isFlipped=!isFlipped; c.nextQuestion()">Next</button>
      </div> 
    </div> 
  </div>
  <div class="container" ng-class="{ 'start__outer' : c.quizEnd }">
    <div ng-show="c.quizEnd" class="midButton">
      <h1>Mistrz Makro</h1>
      <button ng-click="c.reset()">Play again</button>
    </div>
  </div>
</div>

<div class="container" ng-class="{ 'start__outer' : !c.inProgress }">
  <div class="midButton" ng-show="!c.inProgress">
    <h1>Mistrz Makro</h1>
    <button ng-click="c.start()">Start</button>
  </div>
</div>
{% endhighlight %}

<p>Pojawił się div <code>flip</code>, który będzie naszą kartą. Klasy <code>front</code> i <code>back</code> odpowiadają za awers i rewers karty, stąd w jednym znajdują się inputy, a w drugim wartości wynikowe.</p>
<p>Na przyciskach <code>Submit</code> i <code>Next</code> mamy w <code>ng-click</code> <code>isFlipped=!isFlipped</code>, zmieniające zmienną odpowiadającą za obrót karty w divie <code>card</code>, który posiada fragment kodu <code>ng-class="{'flipped':isFlipped}"</code>. Oznacza to, że div otrzyma klasę <code>flipped</code>, jeśli zmienna będzie miała wartość <code>true</code>.</p>
<p>Na ekranie "start" i "play again" dodajemy klasę <code>start__outer</code> kodem <code>ng-class="{ 'start__outer' : c.quizEnd }"</code>, ponieważ stworzone cssy mogą mieć problem z <code>ng-show</code>.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Style LESS</h3>
{% highlight scss %} 
.flip {
  -webkit-perspective: 800;
  height: 580px;
  width: 357px;
  position: relative;
  margin: 20px auto;
  h2{
    text-align: center;
  }
  img, p{
    padding-left: 30px;
  }
  input{
    margin-left: 30px;
  }
  button{
    display: block;
    margin: 15px auto;
  }
  .nextBtn{
    position: absolute;
    top:505px;
    left:30px;
  }
}
{% endhighlight %}
<p>Na początek stworzymy sobie kartę określając jej wymiary. Fragment <code>margin: 20px auto;</code> odpowiada za centrowanie karty na samym środku ekranu z zachowaniem responsywności. Nadajemy pozycję relatywną, ponieważ będziemy pozycjonować przyciski <code>.nextBtn</code> absolutnie. Odsuwamy od lewej krawędzi paddingami i marginesami nasze napisy i inputy. Nadajemy przyciskowi wartość <code>display:block</code>, zeby przesunąć go do nowej lini i także centrujemy go wewnątrz karty.</p>
{% highlight scss %} 
.flip .card.flipped {
  -webkit-transform: rotatex(-180deg);
}

.flip .card {
  width: 100%;
  height: 100%;
  -webkit-transform-style: preserve-3d;
  -webkit-transition: 0.5s;
}
{% endhighlight %}
<p>Tworzymy klasy potrzebne do animacji obrotu.</p>
{% highlight scss %} 
.flip .card .face {
  width: 100%;
  height: 100%;
  position: absolute;
  -webkit-backface-visibility: hidden;
  z-index: 2;
  border: 0px;
  border-radius: 25px;
}
{% endhighlight %}
<p>Określamy za pomocą <code>border-radius</code>, żeby nasza karta miała zaokrąglone rogi. Za pomocą <code>backface-visibity</code> ustawiamy, żeby druga strona karty nie była widoczna po odwróceniu.</p>
{% highlight scss %} 
.flip .card .front {
  position: absolute;
  z-index: 1;
  background: #708090;
  color: white;
  p{
    margin-top: 10px;
    margin-bottom: 10px;
  }
  input{
    color: black;
  }
}

.flip .card .back {
  -webkit-transform: rotatex(-180deg);
  background: #36454F;
  color: white;
}  
{% endhighlight %}
<p>W klasie <code>front</code> ustalamy kolor awersu karty, odstępy między paddingami oraz kolor tekstu wewnątrz inputu jako czarny, ponieważ cały tekst w aplikacji jest koloru białego. W klasie <code>back</code> również ustalamy kolor i sposób obrotu.</p>
{% highlight scss %} 
button{
  background-color: #536878;
  border-radius: 10px;
  color: white;
  border: 0;
  padding: 10px;
  width: 297px;
} 
{% endhighlight %}
<p>Określamy rozmiar, kształt, kolor tła i czcionki wszystkich przycisków</p>
<p>Tak wygląda awers naszej karty:</p>
<img src="{{ site.baseurl }}/assets/img/DSP8_3.png" alt="">
<p>A Tak będzie wyglądal rewers:</p>
<img src="{{ site.baseurl }}/assets/img/DSP8_4.png" alt="">
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Animacja infinite scrolling background</h3>
<p>Na ekranie tytułowym, oraz "play again" wstawimy tło, które będzie się powtarzać i przewijać w nieskończoność z dołu go góry.</p>
<p>Grafikę stworzyłem w Inkscape i wyeksportowałem jako PNG:</p>
<img src="{{ site.baseurl }}/assets/img/DSP8_1.png" alt="">
{% highlight scss %} 
.start__outer{
  width: 297px;
  min-height: 620px;
  height: 100vh;
  margin: 0 auto;
  background-image: url('http://localhost:3000/assets/b.png');  
  -webkit-animation:100s scroll infinite linear;
  -moz-animation:100s scroll infinite linear;
  -o-animation:100s scroll infinite linear;
  -ms-animation:100s scroll infinite linear;
  animation:100s scroll infinite linear;
}
{% endhighlight %}
<p>Klasa <code>start__outer</code> jest dodawana zależnie od aktuanych stanów, co można zaobserwować w kodzie HTML powyżej. Ustawiliśmy tam wysokość <code>100vh</code>, czyli 100% wysokości danego urządzenia, z zastrzeżeniem, że najmniej może to być 620px. Ustawiliśmy zdjęcie na tło, oraz nie wyłączyliśmy <code>background-repeat</code>. Zastosowaliśmy animację <code>scroll</code>, którą określiliśmy poniżej.</p>
{% highlight scss %} 
@-webkit-keyframes scroll{
  100%{
    background-position:0px -3000px;
  }
}

@-moz-keyframes scroll{
  100%{
    background-position:0px -3000px;
  }
}

@-o-keyframes scroll{
  100%{
    background-position:0px -3000px;
  }
}

@-ms-keyframes scroll{
  100%{
    background-position:0px -3000px;
  }
}

@keyframes scroll{
  100%{
    background-position:0px -3000px;
  }
}
{% endhighlight %}
<p>Tak będzie wyglądać ekran startowy, tylko z tłem poruszającym się od dołu do góry:</p>
<img src="{{ site.baseurl }}/assets/img/DSP8_2.png" alt="">