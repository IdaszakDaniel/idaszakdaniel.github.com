---
layout: post
title: "Daj Sie Poznac #9 Ekran wynikowy, czyszczenie inputow"
titlePL: "Daj Się Poznać #9 Ekran wynikowy, czyszczenie inputów"
description: ""
date: 2017-05-29 00:20:00 +0100
category: Article
ima: "/assets/img/dsppost9.png"
tags: ["DajSiePoznac","AngularJS"]
linkTitle: [ 
		{a: "Wyświetlanie podsumowania gry", b: "a1"},
    {a: "Czyszczenie inputów", b: "a2"}
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
<p>W tej serii tworzymy aplikację AngularJS służącą do sprawdzania swojej umiejętności obliczania makroskładników "na oko". Jesteśmy już na półmetku, ponieważ aplikacja jest już prawie ukończona. Dopracowujemy teraz detale, których zabrakło w poprzednich częściach. Zajmiemy się ekranem wynikowym naszej aplikacji, a później zastanowimy się, jak sprawić by nasze formularze nie były wypełnione po zmianie produktu. Zaczynajmy!</p><!--more-->

<x>Jeśli pobrałeś już moje repozytorium z Githuba, co opisałem w <a href="http://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">poprzednim poście</a> to możesz teraz przeskoczyć do kolejnego commita, który pokaże Ci kod z tego posta:</x>
{% highlight plain text %}$ git checkout 18c28cdeaebca64473a1c0271544915bf66a0be1{% endhighlight %}

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Wyświetlanie podsumowania gry</h3>
<p>Jednym z zadań naszej gry, poza wyświetleniem wyniku po każdym pokazanym produkcie mogłoby być główne podsumowanie. Dlatego stworzyłem funkcję, która zbiera nasze dane przez cały quiz. Na końcu, w momencie kiedy widzimy ekran końcowy, ponad przyciskiem "play again" wyświetlimy tekst, który pokaże nasze łączne wyniki przez całą grę. Musimy się też zastanowić nad stylowaniem aplikacji w taki sposób, żeby tło ekranu końcowego nie utrudniało przeczytania tekstu.</p>

<p>Na początek, w pliku <code>controller.js</code> komponentu, w konstruktorze stworzymy sobie tablice wyników <code>this.result</code>, którą wypełnimy zerami, żeby bez problemu dodawać wyniki po kolejnych pytaniach. </p>
{% highlight js %} 
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
    this.result = [0,0,0,0];

    this.init();
  }
{% endhighlight %} 
<p>Musimy dodać kilka linijek do funkcji <code>c.check()</code>, która odpala się w momencie, kiedy wysyłamy nasz formularz:</p>
{% highlight html %} 
<h2>{{c.name}}</h2>
<img ng-src="/assets/{{c.img}}">
<p>Kcal:</p><input type="number" ng-model="c.macro1">
<p>Białko:</p><input type="number" ng-model="c.macro2">
<p>Węglowodany:</p><input type="number" ng-model="c.macro3">
<p>Tłuszcz:</p><input type="number" ng-model="c.macro4">
<button class="nextBtn" ng-click="isFlipped=!isFlipped; c.check();" ng-show="c.answerMode">Submit</button>
{% endhighlight %}
<p>Funkcja <code>c.check()</code>:</p>
{% highlight js %} 
check(){
  let i = 0;
  let test = (modelData, viewData) => {
    if(!viewData) return "";
    if(modelData == viewData) return "idealnie!";
    this.result[i++] += viewData - modelData;
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
<p>Ze zmian jakich dokonaliśmy w tym fragmencie kodu, najłatwiej można zauważyć linijkę kodu zaczynającą się <code>this.result[i++]</code>, jednak zanim do niej przejdziemy, musimy stworzyć sobie zmienną lokalną, stworzoną przez <code>let</code>, zainicjalizowaną zerem, czyli pierwszym elementem tablicy. Następnie, kiedy wykona się ta linia kodu, komórka tablicy o wartości <code>[i]</code> zostaje zwiększona o daną wartość a następnie zmienna <code>i</code> zostaje zwiększona o jeden. Wartość, którą dodajemy do komórki, to wartość podana przez użytkownika, pomniejszona o prawdziwą wartość produktu.</p>
<p>Jednak musimy pamiętać o jeszcze jednej rzeczy. W tym momencie po wduszeniu przycisku "play again", wartość tablicy pozostawałaby taka sama, a powinniśmy ją wyzerować. Dlatego dodamy zerowanie do funkcji <code>reset()</code>.</p>
{% highlight js %} 
reset() {
  this.inProgress = false;
  this.score = 0;
  this.result = [0,0,0,0];
  this.start();
}
{% endhighlight %} 
<p>Musimy teraz wyświetlić naszą tablice na ekranie "play again".</p>
{% highlight html %} 
<div class="container" ng-class="{ 'start__outer' : c.quizEnd }">
  <div ng-show="c.quizEnd" class="midButton midButton--fin">
    <div class="midButton--white">
      <h3>Twoje makro na koniec dnia:</h3>
      <p>Kalorie: { {c.result[0]} }</p>
      <p>Białko: { {c.result[1]} }</p>
      <p>Węglowodany: { {c.result[2]} }</p>
      <p>Tłuszcze: { {c.result[3]} }</p>
      <h1>Mistrz Makro</h1>
      <form ng-submit="c.reset()">
        <button>Play again</button>
      </form>
    </div>
  </div>
</div>
{% endhighlight %} 
<p>Do kodu wyświetlającego się po zakończeniu gry, dodaliśmy wyświetlanie łącznych wyników dla każdego makroskładnika i kalorii. Dodaliśmy także diva z klasą <code>class="midButton--white"</code>, dzięki niemu, będziemy mogli stworzyć tło pod naszymi wynikami. Troszkę wyżej dodana jest też klasa <code>class="midButton midButton--fin"</code>, dzięki której zmienimy margines, który domyślnie powinien być taki sam jak w ekranie tytułowym, jednak przeszkadzał by w wyświetlaniu wyników.</p>
<p>W pliku <code>style.less</code>, znajdującym się w folderze z komponentem nadajemy naszemu divowi style:</p>
{% highlight css %} 
.midButton--white{
  background-color: rgba(255, 255, 255, 0.9);
  padding-top: 10px;
}
.midButton--fin{
  padding-top: 120px;
}
{% endhighlight %} 
<p>dla klasy <code>.midButton--white</code> nadajemy biały kolor tła o przezroczystości 0.9. Dodajemy także <code>padding-top: 10px;</code>, żeby nasze tło nie przylegało równo z tekstem.</p>
<p>Tak prezentuje się ekran końcowy:</p>
<img src="{{ site.baseurl }}/assets/img/DSP9_1.png" alt="Ekran końcowy play again">
<p>Wyniki oczywiście opisują rozbieżności w stosunku do zaplanowanej pulii kalorii i rozkładu makroskładników.</p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Czyszczenie inputów</h3>

<p>Wcześniej pisząc aplikację, nie dodaliśmy tagu <code>form</code> tam, gdzie wduszaliśmy przycisk oraz wysyłaliśmy wypełnione, bądź nie inputy.</p>
{% highlight html %} 
<div class="face front"> 
  <form name="c.MacroForm">
    <h2>{{c.name}}</h2>
    <img ng-src="/assets/{{c.img}}">
    <p>Kcal:</p><input type="number" ng-model="c.macro1">
    <p>Białko:</p><input type="number" ng-model="c.macro2">
    <p>Węglowodany:</p><input type="number" ng-model="c.macro3">
    <p>Tłuszcz:</p><input type="number" ng-model="c.macro4">
    <button class="nextBtn" ng-click="isFlipped=!isFlipped; c.check();" ng-show="c.answerMode">Submit</button>
  </form>
</div> 
{% endhighlight %} 
<p>Nasz formularz nazwalismy <code>c.MacroForm</code>.</p>
<p>Niestety, w tym momencie, wpisane przez nas makroskładniki nie znikają z inputów, dlatego musimy przy każdym nowym pytaniu najpierw je wymazać. Żeby to zmienić, stworzymy funkcję, która będzie ustawiała puste wartości w naszym formularzu. Funkcję nazwiemy <code>formReset()</code>:</p>
{% highlight js %} 
formReset(){
  this.MacroForm.$setPristine();
  this.macro1 = "";
  this.macro2 = "";
  this.macro3 = "";
  this.macro4 = "";
}
{% endhighlight %} 
<p>Formularz nazwany w pliku HTML <code>c.MacroForm</code>, obsługiwany w pliku js jest jako <code>this.MacroForm</code>. Ustawiamy wartość <code>$pristine</code> funkcją <code>$setPristine</code>, dzięki czemu wiemy że nasz formularz jest wyczyszczony, jednak nie daje to takich rezultatów jak byśmy chcieli. Musimy jeszcze przypisać do naszych wartości <code>ng-model</code> inputów pusty ciąg znaków. Dopiero po takim zabiegu, nasz formularz będzie wyczyszczony, kiedy pojawi się kolejne pytanie o produkt.</p>
<p>Funkcje <code>formReset()</code> moglibyśmy wywołać w funkcji <code>check()</code>, jednak na ekranie wynikowym, bezpośrednio po pytaniu, pojawiłyby się puste wartości. Jako że aktualne wartości formularza są nam nadal wtedy potrzebne, nie możemy wywołać tam tej funkcji.</p>
<p>Rozwiązaniem jest funkcja <code>nextQuestion()</code> wywołująca się po każdym pytaniu.</p>
{% highlight js %} 
nextQuestion() {
  this.id++;
  this.formReset();
  this.getQuestion();
}
{% endhighlight %} 
<p>Zwiększamy id, żeby pobrać kolejny element z tablicy obiektów, czyścimy formularz i wywołujemy funkcję pobierającą nowy produkt.</p>