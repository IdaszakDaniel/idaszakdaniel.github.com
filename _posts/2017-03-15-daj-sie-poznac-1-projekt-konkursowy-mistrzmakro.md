---
layout: post
title: "Daj Sie Poznac #1 Projekt konkursowy MistrzMakro"
titlePL: "Daj Się Poznać #1 Projekt konkursowy MistrzMakro"
description: ""
category: Article
ima: "/assets/img/dsppost1.png"
tags: ["DajSiePoznac","Ionic","AngularJS"]
linkTitle: [ 
		{a: "Kim jestem?", b: "a1"},
		{a: "MistrzMakro czyli projekt konkursowy", b: "a2"},
		{a: "MistrzMakro technologie z których skorzystam", b: "a3"},
		{a: "Ionic szybki start", b: "a4"} ]
excerpt_separator: <!--more-->
---
{% include JB/setup %}
<center>	
	<img src="{{ site.baseurl }}/assets/img/DSP.png" alt="" style="display: inline-block; padding-right: 20px;">
	<img src="{{ site.baseurl }}/assets/img/ionic.jpg" alt="" style="display: inline-block;">
	<img src="{{ site.baseurl }}/assets/img/angular.png" alt="" style="display: inline-block; padding-left: 20px;">
</center><br>
<p>Witaj na moim blogu. Został on stworzony na potrzeby konkursu Daj Się Poznać, którego uczestnicy są zobligowani do rozwijania dowolnego projektu programistycznego i dokumentacji przebiegu tego procesu na blogu. Przeglądając prace innych konkurentów zauważyłem, że bardzo popularne jest tworzenie pewnych założeń, dotyczących bloga, czy też samej aplikacji. W moim przypadku założeniami jest tylko dobra zabawa i ukończenie regulaminowej ilości postów na blogu. Projekt, który postanowiłem wykonać, będzie tworzony w technologii, w której czuje się w miarę pewnie. Natomiast wyzwaniem, które pozwoli mi się rozwinąć będzie przerobienie wykonanej aplikacji na aplikację mobilną. Zapewniam, że przeglądając moje posty, z pewnością nauczysz się wielu rzeczy.</p> <!--more-->
<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Kim jestem?</h3>
<p>Nazywam się Daniel Idaszak i w niedługim czasie mam nadzieję zostać inżynierem informatyki. Z programowaniem mam styczność od wielu lat, jednak z technologiami webowymi od ponad roku. Doświadczenie, które posiadam zdobywane było głównie na własnych błędach, ale także dzięki ciekawym ludziom, których spotkałem na mojej programistycznej drodze. Od bloga oczekuję, że przekazywanie wiedzy przełoży się na głębsze rozumienie opisywanych tematów.</p>
<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> MistrzMakro czyli projekt konkursowy</h3>
<p>Projekt, który wymyśliłem, to quiz polegający na uzupełnianiu tabeli makroskładników i kalorii dla podanego zdjęcia. Zadanie, które stawia przed nami aplikacja skierowane jest do osób, które korzystają z diety IIFYM, której zresztą jestem wielkim zwolennikiem. Na początek warto byłoby pokrótce wyjaśnić, na czym owa dieta polega. Rozwinięcie skrótu to "If It Fits Your Macros", czyli dieta prowadzona jest poprawnie, o ile wszystkie makroskładniki mają poprawne wartości. Aby utrzymywać taką dietę, która jest dobra zarówno w okresie budowania masy mięśniowej jak i redukcji tkanki tłuszczowej, wystarczy posiadać policzoną ilość kalorii i procentowy rozkład makroskładników z tychże kalorii. Dieta początkowo wydaje się być kontrowersyjna, ponieważ nie uwzględnia podaży białka na każdy posiłek, korzystania tylko ze zdrowej żywności czy nawet podziału na posiłki. IIFYM jest bardzo elastyczna i każdy da radę dopasować ją do swojego stylu życia, a przyzwolenie na włączenie niezdrowych produktów, czy nawet słodyczy, pozwala nam utrzymać się w dobrych relacjach z naszą dietą. Poznając praktyki utrzymywania dobrej diety, sami możemy wdrożyć zdrowe nawyki, które zmaksymalizują cel, jednak nie są one konieczne. W utrzymywaniu diety pomagają aplikacje takie jak MyFitnessPal czy Fitatu, które wyliczają wszystko za nas, a naszym zadaniem, jest tylko wpisywanie kolejnych posiłków wraz z odpowiadającymi im wagami. Aplikacja podlicza makroskładniki sama, dzięki ogromnej bazie danych produktów wraz z tabelami kalorii. Bardzo pomocny jest wbudowany skaner kodów paskowych, pozwalających na szybkie dodawanie produktów. Chciałbyś dowiedzieć się więcej jak dopasować ilość kalorii i rozkład makro? Odezwij się do mnie, podpowiem, albo poratuje odpowiednimi materiałami. Konkursowa aplikacja to żartobliwa odpowiedź, na wyliczanie makroskładników "na oko", kiedy nie mamy przy sobie wagi, lub nie mamy możliwości skorzystania z niej. Wiele osób twierdzi, że jedząc poza domem świetnie radzą sobie z takim liczeniem. Moja aplikacja zweryfikuje czy rzeczywiście jesteśmy Mistrzami Makro, czy jednak oszukujemy sami siebie.</p>
<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> MistrzMakro technologie z których skorzystam</h3>
<p>Projekt napiszę w języku JavaScript, ponieważ to właśnie jemu poświęcam teraz najwięcej czasu i to właśnie z tym językiem wiążę przyszłość. Framework z którego skorzystam, może nie jest najświeższym wynalazkiem, zważając, że na dniach pojawi się jego wersja nazwana czwartą, ale nadal jest najbardziej popularny, przynajmniej w Polsce. Mowa o AngularJs czyli pierwszej wersji tego frameworka stworzonego przez firmę Google. Miałem okazję pracować z nim w małych prywatnych projektach, więc mam nadzieję że moje spostrzeżenia przydadzą się początkującym. Nie jestem pewien czy metoda budowania aplikacji, jaką obieram jest poprawna lub najprostsza, jednak planuję początkowo pisać projekt tylko w Angularze. Kolejnym krokiem będzie przerobienie zbudowanej aplikacji na aplikacje mobilną i dodanie nowych funkcjonalności poprzez framework Ionic, także w pierwszej wersji.</p>
<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Ionic szybki start</h3>
<p>Na sam koniec posta, pokażę Ci jak zainstalować oraz jak wygląda Ionic.</p>
<ul style="padding-left: 30px;">
	<li>Jeśli jeszcze tego nie zrobiłeś, to pobierz i zainstaluj node.js <a href="https://nodejs.org">https://nodejs.org</a></li>
	<li>otwórz terminal i zainstaluj globalnie Ionic podaną komendą: </li>
	{% highlight Config file %}C:\> npm install --g cordova ionic{% endhighlight %}
	<li>podana poniżej komenda utworzy folder i pobierze do niego pliki projektu: </li>
	{% highlight Config file %}C:\> ionic start chapter2{% endhighlight %}
	<li>Otwórz folder projektu, powinny znajdować się tam takie pliki:</li>
	<img src="{{ site.baseurl }}/assets/img/DSP1_2.png" alt="menu kontekstowe"><br>
	<li>przejdź do folderu projektu w terminalu. Możesz to zrobić w prosty sposób klikając shift + ppm i wybierając "Otwórz okno polecenia tutaj"</li>
	<img src="{{ site.baseurl }}/assets/img/DSP1_1.png" alt="menu kontekstowe">
	lub komendą:
	{% highlight Config file %}C:\> cd chapter2{% endhighlight %}
	<li>Podana komenda automatycznie otworzy nasz projekt w przeglądarce:</li>
	{% highlight Config file %}C:\> ionic serve{% endhighlight %}
	<li>Gotowa aplikacja Ionic:</li><br>
	<img src="{{ site.baseurl }}/assets/img/DSP1_3.png" alt="menu kontekstowe">
</ul>
