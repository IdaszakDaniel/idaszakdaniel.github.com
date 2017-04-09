---
layout: post
title: "Daj Sie Poznac #2 Github w 2 minuty. Dobre praktyki AngularJS"
titlePL: "Daj Się Poznać #2 Github w 2 minuty. Dobre praktyki AngularJS"
description: ""
category: Article
ima: "/assets/img/dsppost2.png"
tags: ["DajSiePoznac","AngularJS","Github"]
linkTitle: [ 
		{a: "Co to jest GitHub? Instalacja Githuba", b: "a1"},
		{a: "Podstawy Githuba w 2 minuty", b: "a2"},
		{a: "Gulp i Bower. AngularJS Model, Widok oraz Kontroler", b: "a3"},
		{a: "Dobre praktyki AngularJS", b: "a4"} ]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
	<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
	<img src="{{ site.baseurl }}/assets/img/Git.png" alt="" style="display: inline-block; padding-right: 20px;">
	<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block;">
	<img src="{{ site.baseurl }}/assets/img/bower.png" alt="" style="display: inline-block; padding-left: 20px;">
	<img src="{{ site.baseurl }}/assets/img/gulp.png" alt="" style="display: inline-block; padding-left: 20px;">
</center><br>
<p>W drugiej już części projektu Daj Się poznać, której celem jest zbudowanie aplikacji Ionic MistrzMakro, skupimy się na obsłudze Githuba oraz wyjaśnimy parę podstaw AngularJS. Pokażę Ci też kilka dobrych praktyk, które warto wprowadzić już od początku nauki. Nie martw się, jeśli nigdy nie korzystałeś z Githuba, ani Angulara, mój post jest skierowany głównie do początkujących, aczkolwiek osoby z pewnym już doświadczeniem też będą miały okazję wyciągnąć z tego postu parę ciekawych informacji. Zaczynajmy!</p> <!--more-->
<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Co to jest GitHub? Instalacja Githuba</h3>
<p>Github to system kontroli wersji ułatwiający pracę zespołom. Przechowuje na serwerze kod źródłowy aplikacji, który jest również rozdzielony na etapy w których był tworzony, czyli w praktyce, możesz obejrzeć cały cykl jego powstawania. Uważam, że początkujący programista, powinien juz od samego początku korzystać z Githuba, jako że przedstawione tam prace, oraz sama umiejętność korzystania z tego narzędzia jest świetną wizytówką podczas poszukiwania pracy. Jednak najważniejszą zaletą jest to, że możemy "cofnąć się w czasie" do wcześniejszej wersji naszego programu. Jeśli nie posiadamy konta studenckiego, lub nie opłacimy prywatnych repozytoriów, to nasz kod będzie dostępny publicznie. Przejdźmy do instalacji, którą wykonamy w systemie Windows: 
<ul style="padding-left: 30px;">
	<li>Załóż konto na githubie i zainstaluj GitHub Desktop: <a href="https://desktop.github.com/">https://desktop.github.com/</a></li>
	<li>zainstaluj także git for windows: <a href="https://git-for-windows.github.io/">https://git-for-windows.github.io/</a></li>
</ul></p>
Jeśli udało Ci się wszystko zainstalować, to będziesz miał teraz możliwość korzystania z git basha.
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Podstawy Githuba w 2 minuty</h3>
<p>Na swoim koncie w repozytorium z projektem, jako pierwszy commit umieściłem skonfigurowany szablon zawierający już Angualara i inne potrzebne rzeczy. W kilku prostych krokach poprowadzę Cię do pierwszego commitu, który przyda się do tworzenia aplikacji Mistrz Makro. Oczywiście, wymienione poniżej kroki, można pominąć i po prostu forkować moje repozytorium, jednak dla celów edukacyjnych wybrałem taki sposób. Zaczynajmy!</p>
<ul style="padding-left: 30px;">
	<li>Na początek pobierzemy szablon aplikacji na dysk</li><br>
	<ul style="padding-left: 30px; list-style-type: circle">
		<li>Jeśli zainstalowałeś Git for Windows, to po kliknięciu prawym przyciskiem w folderze pojawi się opcja "Git bash here", dzięki któremu przejdziemy do Githubowego terminala w aktualnej lokalizacji.</li>
		<img src="{{ site.baseurl }}/assets/img/DSP2_1.png" alt="menu kontekstowe"><br>
		<li>Żeby pobrać szablon sklonujemy <a href="https://github.com/IdaszakDaniel/MistrzMakro">repozytorium:</a></li>
		{% highlight plain text %}$ git clone https://github.com/IdaszakDaniel/MistrzMakro.git {% endhighlight %}
		<li>Następnie musimy przejść do pierwszej wersji aplikacji, gdzie znajduje się tylko szablon. Commit z tą zawartością ma ID 35fb50ce7f90f1e6488b0cb715296481ab813099</li>
		{% highlight plain text %}$ git checkout 35fb50ce7f90f1e6488b0cb715296481ab813099{% endhighlight %}
	</ul>
	<li>Jeśli posiadasz juz szablon na dysku, to teraz warto byłoby stworzyć repozytorium i dodać go na swojego Gita:</li><br>
	<ul style="padding-left: 30px; list-style-type: circle">
		<li>Załóż konto na githubie i stwórz repozytorium o dowolnej nazwie. Zrobisz to poprzez stronę <a href="https://github.com/new">https://github.com/new</a></li>
		<li>W folderze z projektem utwórz lokalne repozytorium komendą:</li>
		{% highlight plain text %}$ git init{% endhighlight %}
		<li>W pliku .gitignore znajdują się foldery, które Github zignoruje. Powinny tam być foldery gulpa i bowera, zawierające biblioteki i frameworki, których nie chcemy w repozytorium</li>
		<li>Dodaj do utworzonego lokalnego repozytorium wszystkie pliki komendą:</li>
		{% highlight plain text %}$ git add . {% endhighlight %}
		<li>Teraz musimy podłączyć twoje lokalne repozytorium, z repozytorium na serwerze Githuba:</li>
		{% highlight plain text %}$ git remote add origin https://github.com/TWOJA_NAZWA_UŻYTKOWNIKA/TWOJA_NAZWA_PROJEKTU.git{% endhighlight %}
		<li>Utwórz commit, dzięki któremu znajdziesz obecną wersję aplikacji, pamiętaj żeby nazwa pozwalała na łatwą identyfikację commita</li>
		{% highlight plain text %}$ git commit -m "NAZWA COMMITA"{% endhighlight %}
		<li>Teraz wystarczy tylko wysłać lokalne repozytorium na serwer Githuba:</li>
		{% highlight plain text %}$ git push origin master {% endhighlight %}
	</ul>
</ul>
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Gulp i Bower. AngularJS Model, Widok oraz Kontroler.</h3>
<p>Teraz skupimy się na samym szablonie. Będziemy korzystać z NPM i Node.js. Pisałem o tym w  <a href="http://www.idaszak.com/article/2017/03/15/daj-sie-poznac-1-projekt-konkursowy-mistrzmakro#a4">pierwszej części Daj Się Poznać</a></p>
<p>Szablon skonfigurowany jest do pracy z AngularJS, Bootstrap, Sass oraz Gulp. Aby pobrać wszystkie potrzebne biblioteki należy wpisać komendę w terminalu z lokalizacją w folderze z szablonem</p>
{% highlight plain text %} npm install && bower install {% endhighlight %}
<p>Gdy już wszystkie biblioteki zostaną zainstalowane, możemy uruchomić gulpa, który automatycznie uruchomi nasz projekt w przeglądarce i będzie nasłuchiwał zmian</p>
{% highlight plain text %} gulp serve {% endhighlight %}
<p>Szablon pozwala na stylowanie aplikacji preprocesorem języka CSS - Sass. Nasz szablon przygotowany jest także do unit testów oraz testów e2e, ale więcej na ten temat napiszę w późniejszym poście.</p>
<p>Na początek powinienem pokrótce wyjaśnić czym jest framework AngularJS. A więc jest to framework MVC do budowania aplikacji SPA. A co oznaczają te akronimy? SPA to po prostu single page application, czyli aplikacja internetowa, która działa bez odświeżania strony internetowej. MVC to skrót od Model, Widok, Kontroler. Widok naszej aplikacji to wszystkie pliki HTML począwszy od index.html znajdującego się w folderze /src naszego szablonu. Jest to warstwa wizualna naszej aplikacji. Model to warstwa zawierająca dane i operująca na nich. Nasz model znajdziesz w folderze src/app/main/models. Kontroler to warstwa zapewniająca płynną komunikację między widokiem a modelem.</p>
<p>W pliku index.module.js w katalogu src/app mamy zadeklarowany moduł aplikacji o nazwie quizProject:</p>
	{% highlight javascript %} 
	(function() {
	  'use strict';

	  angular
	    .module('quizProject', []);

	})();
	{% endhighlight %}
<p>Moduł podłączony jest do aplikacji poprzez ng-app:</p>
	{% highlight html %} 
	<!DOCTYPE html>
	<html ng-app="quizProject">
	<head>
	...
	{% endhighlight %}
<p>Podłączeniem wszystkich plików zajmuje się za nas bower, który generuje je w sekcji head pliku index.html.</p>
<p>Model naszej aplikacji nazywa się "quizFactory" i w bloku funkcji zawarte będą wszystkie dane i operacje na nich. Podłączony jest do głównego modułu naszej aplikacji. Przechowywane będą tam zdjęcia i wszystkie makroskładniki, a odpowiednia metoda pozwali nam pobrać te wszystkie dane.</p>
	{% highlight javascript %} 
	(function() {
	  'use strict';

	  angular
	    .module('quizProject')
	    .factory('quizFactory', quizFactory);

	  function quizFactory() {
	  
	  };
	})();
	{% endhighlight %}
<p>Nasza aplikacja będzie korzystać z dyrektywy, która w prosty sposób pozwala na dołączanie kodu do widoku aplikacji umożliwiając reusability, czyli wielokrotny użytek kodu bez powtarzania się. Dyrektywa składa się z dwóch części - pliku gap.directive.js oraz gap.directive.html. Po napisaniu kodu, będzie działać to w taki sposób, że plik z logiką dobierze odpowiednie dane z modelu i utworzy kod html oraz umiejscowi go w miejsce, w którym pojawi się tag o wybranej przez nas nazwie. W tym przypadku będzie to tag "quiz" </p>
	{% highlight html %} 
	<body>
	  <div class="container">
	    <h1 class="title">QuizApp</h1>
	    <quiz/>
	  </div>
	</body>
	{% endhighlight %}
<p>W kodzie dyrektywy "quiz", który jest bardzo podobny do wcześniej omówionego modelu, dodatkowo można zauważyć Dependency Injection czyli wstrzykiwanie zależności. Widzimy to zjawisko w miejscu gdzie znajduje się parametr funkcji. Dyrektywa ma dzięki temu dostęp do wszystkich metod z modelu quizFactory. Wstrzykiwanie zależności ułatwia tworzenie niezależnych obiektów i sprawia że są one łatwiejsze do testowania.</p>
	{% highlight Javascript %} 
	(function() {
	  'use strict';

	  angular
	    .module('quizProject')
	    .directive('quiz', quiz);

	  function quiz(quizFactory) {

	    };
	})();
	{% endhighlight %}
<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Dobre praktyki AngularJS</h3>
<p>Dlaczego kod nie może znajdować się w jednym pliku? Po co każdy plik owinięty jest w IIFE? Co to jest strict mode? Dlaczego w innych tutorialach deklaracja modułu wygląda inaczej?</p>
	{% highlight Javascript %} 
	(function() {
	  'use strict';

	})();
	{% endhighlight %}
<p><b>IIFE czyli Immediately Invoked Function Expression</b> to anonimowa funkcja, wywołująca się automatycznie w miejscu, w którym została stworzona. Pozwala na użycie trybu strict tylko wewnątrz niej, co znacznie podnosi jakość naszego kodu. Jakie jeszcze ma zalety? Wszystkie zmienne zadeklarowane w jej wnętrzu są lokalne, dzięki czemu nie mają długości życia większej niż to potrzebne, oraz zapobiega to kolizji zmiennych.</p>
<p><b>Co to jest strict mode?</b> Pracując w strict mode, Javascript nie pomija niektórych mniejszych błędów, oraz poprawia błędy i złe praktyki pisania kodu, które przeszkadzają w optymalizacji kodu przez przeglądarkę. Przykładowo - przypadkowe stworzenie zmiennej globalnej zostanie pokazane jako błąd. Strict mode nie pozwala także na używanie słów kluczowych, które będą miały zastosowanie w nowszych wersjach ECMAScript. </p>
<p><b>Jeden komponent w jednym pliku</b>, najlepiej żeby plik nie przekraczał 400 linii kodu. Jakie przynosi to zalety? Zwiększona czytelność, oraz spore ułatwienie, jeśli pracujesz nad projektem z całym zespołem. Pojedyncze pliki łatwiej debuggować oraz przeprowadzać na nich testy jednostkowe.</p>
<p>Dlaczego w innych tutorialach wszyscy przypisują moduł do zmiennej? Jest to niepoprawne, a w przypadku gdzie mamy jeden moduł na plik wręcz zbędne. Brak przypisanej zmiennej poprawia czytelność kodu, oraz zapobiega przypadkowemu użyciu tej zmiennej ponownie.</p>