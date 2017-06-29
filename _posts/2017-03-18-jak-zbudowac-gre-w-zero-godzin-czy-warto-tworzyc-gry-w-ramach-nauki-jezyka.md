---
layout: post
title: "Jak zbudowac gre w zero godzin Czy warto tworzyc gry w ramach nauki jezyka"
titlePL: "Jak zbudować grę w zero godzin? Czy warto tworzyć grę w ramach nauki języka?"
description: ""
category: Article
ima: "/assets/img/gk.png"
tags: ["DajSiePoznac", "PhaserJS"]
linkTitle: [ 
		{a: "Jak zbudowałem grę w zero godzin?", b: "a1"},
		{a: "Jakie korzyści przynosi tworzenie gry w procesie nauki języka programowania?", b:  "a2"},
		{a: "Jak zacząć przygodę z frameworkiem PhaserJs?", b:  "a3"},
		{a: "PhaserJs co dalej?", b: "a4"} ]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
![Image alt]({{ site.baseurl }}/assets/img/phaser.png "phaser logo")
<p>Jestem wielkim zwolennikiem poznawania technologii metodą rzucania się od razu na głęboką wodę. Zaczynając moją przygodę z programowaniem popełniałem podstawowy błąd - czytałem książki dotyczące teorii języka. Taki sposób jest bardzo nieefektywny, dlatego że nie potrafimy czytając wyobrazić sobie, jak dany kod mógłby zostać zastosowany w praktyce.
Dopiero po przetestowaniu języka, w czym bardzo pomocne były najróżniejsze tutoriale, otwierają się oczy. Nowe spojrzenie pozwala nam dużo lepiej i efektywniej przebrnąć przez labirynt językowych niuansów.</p><!--more-->
<br>
<i>“Computer science education cannot make anybody an expert programmer any more than studying brushes and pigment can make somebody an expert painter.” — Eric S. Raymond</i>
<p>Teoretyczne wykształcenie informatyczne nie czyni nikogo programistą ekspertem, nie bardziej jak studiowanie pędzli i farb może uczynić kogoś malarzem ekspertem. Dlatego postanowiłem zacząć od części praktycznej, bo w końcu na tym polega programowanie. Jedną z pierwszych rzeczy, które zrobiłem podczas poznawania JavaScriptu było stworzenie prostej gry.
Skoro posiadam już większość potrzebnych narzędzi, to dlaczego nie spróbować stworzyć gry w HTML5?
Wybór padł na Framework PhaserJs, który jest świetny do tworzenia gier platformowych i pozwala na odpalenie takiej gry na stronie internetowej lub w dowolnym urządzeniu mobilnym.</p>
<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Jak zbudowałem grę w zero godzin?</h3>
<p>Motywacją do budowy gry był dla mnie 0 godzinny Game Jam, wymyślony przez Sosa Sosowskiego, który polega na tworzeniu gry podczas zmiany czasu z zimowego na letni. Po godzinie budowania naszej gry czas zmienia się na letni, a zegar cofa się o godzinę. Gry zbudowane na tym Game Jamie można zobaczyć na stronie <a href="0hgame.eu">0hgame.eu</a> . Przyznam że wcześniej przerobiłem oficjalny tutorial Phasera i przygotowałem grafiki pixelart, które były bardzo pracochłonne. W trakcie Game Jamu sprawiłem że stworzona przeze mnie postać przysposobiła tajniki chodzenia. Nie ukrywam też, że trochę oszukałem, ponieważ godzina jest troszkę wyśrubowanym limitem, jak na połączenie wszystkich klatek animacji i zastosowanie fizyki.</p>
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Jakie korzyści przynosi tworzenie gry w procesie nauki języka programowania?</h3>
<p>Gamedev, to nie tylko klepanie kodu, ale cały kreatywny proces, który może otworzyć nas na wiele nowych rzeczy. Przy tworzeniu gry zahaczysz pewnie o takie tematy jak grafika, animacja, muzyka, user interface, fabuła, dialogi czy nawet sztuczna inteligencja. A wszystko to połączysz i uformujesz w gotowy produkt za pomocą kodu. Do tego dochodzi kolejna kwestia - urozmaicenie nauki. Kolejna aplikacja To Do, albo walidacja formularza? Gra z pewnością zapewni dużo więcej funu, a w tym właśnie o to chodzi, żebyś nie musiał się zmuszać do nauki.</p>  
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Jak zacząć przygodę z frameworkiem PhaserJs?</h3>
<p>Najlepszym sposobem i zarazem najszybszym jest przerobienie oficjalnego tutoriala, znajdującego sie na stronie:<br>
<a href="https://phaser.io/tutorials/making-your-first-phaser-game">https://phaser.io/tutorials/making-your-first-phaser-game</a><br>
Tutorial zapewnia wszystkie potrzebne grafiki i informacje, żeby bezboleśnie wytworzyć pierwszy ekran gry i sprawić żeby phaserowa maskotka poruszała się tak, jak podyktujemy to naszą klawiaturą. Jedynym problemem, który możemy napotkać na początku, jest wybór lokalnego serwera, w którym uruchomimy naszą grę. Tutaj przychodzę z pomocą ja i pokażę Ci, w jaki sposób zrobić to jak na webdevelopera przystało, czyli z wykorzystaniem gulpa.</p>
<ul style="padding-left: 30px;">
	<li>Zainstaluj node.js <a href="https://nodejs.org">https://nodejs.org</a></li>
	<li>W terminalu (CMD dla Windows) przejdź do folderu z projektem i wpisz podaną komendę:</li>
		{% highlight Config file %}C:\projekt\> npm init{% endhighlight %}
	<li>zainstaluj gulpa podaną komendą: </li>
		{% highlight Config file %}C:\projekt\> npm install --save-dev gulp{% endhighlight %}
	<li>Potrzebny będzie też browser-sync, którego zainstalujesz podaną komedą:</li>
		{% highlight Config file %}C:\projekt\> npm install --save-dev browser-sync{% endhighlight %}
	<li>Utwórz plik gulpfile.js z zawartością:</li>

	{% highlight JavaScript %}
	var gulp = require('gulp');
	var browserSync = require('browser-sync');

	gulp.task('reload', function() {
		browserSync.reload();
	});
	
	//jako "projekt" wpisz nazwę folderu z projektem
	gulp.task('serve', function() {
		browserSync({
			server:'projekt'
		});
		gulp.watch('projekt/*.html', ['reload']);
	});
	{% endhighlight %}

	<li>W końcowym kroku wystarczy tylko wpisać w terminalu:</li>
	{% highlight Config file %}C:\projekt\> gulp serve{% endhighlight %}
</ul>
<p>Nasz projekt powinien otworzyć się w przeglądarce internetowej. Do gulpa dodaliśmy także task browser-sync, dzięki czemu każda zmiana, którą wprowadzimy w projekcie pojawi się od razu w oknie naszej przeglądarki pod adresem Localhost:3000.</p>
<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> PhaserJs co dalej?</h3>
<p>Jeśli już zbudujesz sobie w głowie zarys gry którą chciałbyś stworzyć, to warto zajrzeć do przykładów na stronie phasera znajdujących się w podanym linku <a href="https://phaser.io/examples">https://phaser.io/examples</a>. Jest tam wiele gotowych rozwiązań posegregowanych na najróżniejsze kategorie takie jak fizyka, obsługa klawiatury, animacje czy kamera. Jest to bardzo wygodne rozwiązanie, a jeśli zagłębisz się w bardziej skomplikowany problem, to z pewnością pomoże Ci dokumentacja ze strony phasera, która z resztą jest bardzo przejrzyście napisana.</p>
<p>Dbając o kod, warto zainteresować się, jak podzielić poszczególne stany na pliki. Możemy zrobić to przy użyciu najnowszego standardu języka JavaScript, czyli ES6, lub łatwiejszą, klasyczną metodą.
Wersję ES6 świetnie opisał Josh Morony:<br> <a href="https://www.joshmorony.com/level-up-your-phaser-games-with-es6/">https://www.joshmorony.com/level-up-your-phaser-games-with-es6/</a><br>
Ale rozwiązanie Chada także jest bardzo dobre:<br> <a href="http://perplexingtech.weebly.com/game-dev-blog/using-states-in-phaserjs-javascript-game-developement">http://perplexingtech.weebly.com/game-dev-blog/using-states-in-phaserjs-javascript-game-developement</a></p>
<p>Wiele pomocnych postów udało mi się znaleźć także na forum programistów gier w HTML5, gdzie phaserowi poświęcony jest cały dział. Możesz znaleźć tam też informacje na temat innych, chociaż mniej popularnych frameworków do gier pisanych w języku JavaScript.<br> <a href="http://www.html5gamedevs.com/forum/14-phaser/">http://www.html5gamedevs.com/forum/14-phaser/</a></p>
