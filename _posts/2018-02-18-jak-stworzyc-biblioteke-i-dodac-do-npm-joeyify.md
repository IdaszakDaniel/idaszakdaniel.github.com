---
layout: post
title: "Jak stworzyc biblioteke i dodac do NPM?"
titlePL: "Jak stworzyć bibliotekę i dodać do NPM?"
description: ""
category: Article
date: 2018-02-18 11:00:00 +0100
ima: "/assets/img/gk.png"
tags: ["JavaScript", "Node.JS", "Testowanie"]
linkTitle: [
		{a: "Joeyify - automatyczny słownik synonimów", b: "a1"},
		{a: "Konfiguracja biblioteki JavaScript", b: "a2"},
		{a: "Jak stworzyć bibliotekę JavaScript?", b: "a3"},
		{a: "Jak napisać testy jednostkowe?", b: "a4"},
		{a: "Dodajemy bibliotekę do NPM", b: "a5"},
		{a: "Podsumowanie", b: "a6"}
		]
excerpt_separator: <!--more-->
---
<!-- {% highlight javascript %}
{% endhighlight %} -->
{% include JB/setup %}
<center>
<img src="{{ site.baseurl }}/assets/img/npm.png" style="width:70px; display: inline-block;">
</center>
<p>Tym razem zrobimy coś innego, odbiegającego od reszty kursów. Na przykładzie mojej biblioteki pokażę wam jak stworzyć i dodać coś swojego do NPM. Chyba każda
	osoba pisząca w Javascript ma pojęcie czym jest środowisko Node.JS i zbudowany (początkowo tylko dla niego) manager bibliotek JS. NPM jest obecnie największym
	na świecie tego typu managerem i osobiście wychodzę z założenia, że jeśli nie ma czegoś w NPM, to nie da się tego zrobić :)
	 Zaczynajmy!</p><<!--more--></!--more-->

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Joeyify - automatyczny słownik synonimów</h3>
<p> Dzięki bibliotece, którą stworzyłem możemy zastosować w tekście automatyczny słownik synonimów, czyli tzw. Thesaurus. Moja biblioteka została stworzona
	dla żartu, zainspirowana sceną z serialu "Friends", dlatego też zamienia automatycznie każde napotkane słowo na synonim. Poza celami humorystycznymi,
	biblioteka nie ma zastosowań, ale zdecydowałem że jest świetnym przykładem do opisu - jak dodać bibliotekę do NPM. </p>
<p>Więcej informacji na temat biblioteki można znaleźć na jej stronie - <a href="https://www.npmjs.com/package/joeyify">Joeyify w NPM</a></p>

<h3 id="a2"><span style="color:gray; font-size: 30px;">#</span> Konfiguracja biblioteki JavaScript</h3>
<p>Pierwszym krokiem będzie utworzenie pliku package.json - wystarczy tylko w konkretnym folderze za pomocą CMD użyć komendy <code>npm init</code>.
W terminalu pojawi się wiele pytań, na które możemy udzielić informacji, bądź pominąć za pomocą przycisku enter.</p>
<p>Po inicjalizacji npm należałoby teraz zainstalować biblioteki, z których wasza biblioteka będzie korzystać. W moim przypadku jest to znaleziona w NPM
	biblioteka <a href="https://github.com/daizoru/node-thesaurus">"Thesaurus"</a>. Biblioteki dodajemy za pomocą komendy CMD - <code>npm install [package_name]
		--save</code>, czyli w moim przypadku <code>npm install thesaurus --save</code>.</p>
<p>Czy jest różnica między bibliotekami potrzebnymi do uruchomienia naszej biblioteki, a tymi wykorzystywanymi do jej budowy?</p>
<p>Biblioteki, które dodajemy tylko dla siebie i ludzi, którzy będą uczestniczyli w naszym projekcie dodajemy za pomocą <code>npm install [package_name] --save-dev
</code>. Dzięki temu, użytkownik, który będzie chciał użyć Joeyify w swoim projekcie, nie pobierze niepotrzebnych bibliotek.</p>
<p>W moim przypadku jako devDependencies dodałem biblioteki do testowania - Mocha i Chai. A tak wygląda finalny plik <code>package.json</code> mojej biblioteki:</p>
<script src="https://gist.github.com/IdaszakDaniel/e860b3304f68c190b9e010d85282ce7b.js"></script>
<p>Pamiętaj także, żeby dodać do folderu plik <code>.gitignore</code> zawierający w treści - <code>/node_modules</code>. Dzięki temu zamiast całych bibliotek
do NPM i do GitHub dodany będzie tylko plik package.json</p>

<h3 id="a3"><span style="color:gray; font-size: 30px;">#</span> Jak stworzyć bibliotekę JavaScript?</h3>
<p>Gdy nasza biblioteka jest już skonfigurowana, należy zabrać się za stworzenie jej funkcjonalności. Przedstawię pliki biblioteki Joeyify, które dostępne są
w repozytorium na GitHubie - <a href="https://github.com/IdaszakDaniel/Joeyify">Joeyify GitHub</a></p>
<p>Jako że biblioteka jest niewielka i bardzo prosta - całe jej ciało znajduje się w zaledwie jednym pliku - <code>joeyify.js</code>. Na samym jego początku
musimy zaimportować biblioteki, które będą nam potrzebne. W moim przypadku dodaję Thesaurus - <code>const thesaurus = require("thesaurus");
</code>.<br> W tym momencie możemy używać zmiennej <code>thesaurus</code> do wywoływania biblioteki.</p>
<p>Następnym krokiem jest stworzenie funkcji, która będzie zawierała cały kod biblioteki. Funkcja ta musi być przypisana do <code>module.exports</code>, żeby można
było się do niej odwoływać z innego pliku.</p>
<script src="https://gist.github.com/IdaszakDaniel/47ef3110ebf036fce9c4ac8f506b1eb4.js"></script>
<p>Moja biblioteka składa się z funkcji pomocniczych i wartości zwracanej. Na początku biblioteka przyjmuje argument <code>text</code>, który zawiera przekazany
tekst i zwraca mapowany wynik. Jeśli nie znasz jeszcze funkcji <code>map</code>, to zapraszam do jednego z wcześniejszych postów na tym blogu -
<a href="https://www.idaszak.com/article/2017/05/31/podstawy-programowania-funkcyjnego-3-map-i-filter">
	https://www.idaszak.com/article/2017/05/31/podstawy-programowania-funkcyjnego-3-map-i-filter</a>.</p>
<p>Nie będę zagłębiał się w szczegóły implementacji biblioteki, jako że nie jest to programistyczne arcydzieło i zarazem nie to jest istotą tego postu. Jeśli
chcesz upewnić się, czy twoja biblioteka działa, spróbuj zwrócić dowolną wartość w <code>return</code></p>
<script src="https://gist.github.com/IdaszakDaniel/728f7c43cac98a4b8f6f5f9742a4fe24.js"></script>
<p>Plik JavaScript zwracający wartość string - "testowy moduł".</p>
<p>Teraz spróbujemy odebrać tą wartość w pliku, który skorzysta z biblioteki.</p>
<p> Zgodnie z przykładem ze strony NPM Joeyify, wystarczy stworzyć nowy plik, w którym użyjemy <code>const joeyify = require("./joeyify");</code>. W momencie
wywołania zmiennej <code>joeyify</code>, powinna pokazać się ta zwracana wartość, lub jeśli korzystasz z mojej biblioteki to wywołanie powinno wyglądać w taki
sposób:</p>
<script src="https://gist.github.com/IdaszakDaniel/1ecee9ba7c7d8f094e9b80ce4f20a21f.js"></script>
<p>Wystarczy teraz tylko użyć komendy <code>node [nazwa_pliku].js</code> z plikiem wykorzystującym bibliotekę. Jak widać, Joeyify wyszukuje słów, pomijając znaki
	 interpunkcyjne i zamienia je na przypadkowe synonimy.</p>

<h3 id="a4"><span style="color:gray; font-size: 30px;">#</span> Jak napisać testy jednostkowe?</h3>
<p>W przypadku tworzenia biblioteki, warto upewnić się czy poszczególne jej elementy działają zgodnie z założeniem. Odpowiemy sobie na pytanie - w jaki sposób
napisać testy w Node.js. Wykorzystałem jeden z najpopularniejszych zestawów narzędzi do testowania czyli
<code>Mocha + Chai</code>.</p>
<p>Utworzyłem w projekcie folder <code>test</code>, w którym umieściłem plik <code>joeyify.spec.js</code> z testami biblioteki. Cały test można znaleźć w
repozytorium <a href="https://github.com/IdaszakDaniel/Joeyify/blob/master/test/joeyify.spec.js">Joeyify</a>, a poniższy przykład ograniczyłem do jednego
testu:</p>
<script src="https://gist.github.com/IdaszakDaniel/8cbcc8cd9a4a77dace2ed38dc69de7db.js"></script>
<p>Biblioteka <code>Chai</code>, pozwala na trzy style pisania testów - Assert, BDD - <code>expect</code> lub <code>should</code>. W powyższym przykładzie
skorzystałem z <code>should</code> stylu Behaviour Driven Development. Dodałem je za pomocą <code>var should = require('chai').should();</code>.</p>
<p>BDD polega na tworzeniu "opowieści" w testach za pomocą tekstu. Przykładowy test może nazywać się - "#Synonyms - dot is unchanged", ale warto tworzyć jeszcze
bardziej opisowe nazwy, żeby można było łatwiej zidentyfikować testy.</p>
<p>Następnie dodaję moją bibliotekę, żeby testy mogły ją wykonywać - <code>var joeyify = require('../joeyify');</code>. Blok <code>describe()</code> Zaczyna
zestaw testów rozpoczynających się blokiem <code>it()</code>. <code>joeyify('.').should.equal('.');</code> - Przekazuje w nim jako parametr string "." i oczekuje
że biblioteka zwróci go bez zmian, jako że Joeyify powinien ignorować znaki interpunkcyjne.</p>
<p>Testy uruchamiamy komendą terminala - "npm test". Poniżej screen z wykonanymi wszystkimi testami biblioteki:</p>
<img src="{{ site.baseurl }}/assets/img/joeyifyTests.png" alt="wykonane testy jednostkowe" style="height:400px"><br>

<h3 id="a5"><span style="color:gray; font-size: 30px;">#</span> Dodajemy bibliotekę do NPM</h3>
<p>W tym momencie, biblioteka jest już skończona i gotowa do opublikowania w NPM. Na początek jednak, musimy wrzucić je do repozytorium GIT. Pisałem już
na ten temat w poście <a href="https://www.idaszak.com/article/2017/03/23/daj-sie-poznac-2-projekt-konkursowy-mistrzmakro">Github w 2 minuty</a>, ale wspomnę
o tym skrótowo:</p>
<p>Na naszym koncie GitHub musimy stworzyć repozytorium, a następnie użyć kolejno komend: </p>
<ul style="padding-left: 30px;">
	<li><code>git add .</code> - dodajemy wszystkie pliki biblioteki</li>
	<li><code>git commit -m "initial commit"</code> - nazywamy wersje projektu</li>
	<li><code>git remote add origin https://github.com/[nick github]/[nazwa repozytorium].git</code> - podpinamy projekt pod repozytorium</li>
	<li><code>git push -u origin master</code> - wypychamy kod na GitHub</li>
</ul>
<p>W momencie gdy nasz kod jest już publicznie dostępny w serwisie GitHub, możemy zająć się publikacją biblioteki w NPM.</p>
<p>Załóż konto na stronie <a href="https://www.npmjs.com/">https://www.npmjs.com/</a> i zaloguj się w terminalu za pomocą <code>npm login</code>. Nazwę
	biblioteki jak i wszystkie informacje muszą być dodane w pliku <code>package.json</code>. Musimy stworzyć także plik <code>readme.md</code> w którym opisane
jest zastosowanie biblioteki, przykłady użycia i informacje dla ludzi, którzy mieliby ochotę pomóc w jej rozwoju.</p>
<p>Finalny krok! w terminalu wpisz <code>npm publish</code>.</p>
<p>Twoja biblioteka powinna teraz być opublikowana na stronie <code>https://npmjs.com/package/[nazwa biblioteki]</code>.</p>
<p>Z poziomu terminala spróbuj zrobić <code>npm install [nazwa biblioteki]</code> i upewnij się że wszystko działa poprawnie.</p>

<h3 id="a6"><span style="color:gray; font-size: 30px;">#</span> Podsumowanie</h3>
<p>Gratulacje! masz już pierwszą bibliotekę w NPM. Jeżeli wystąpiły problemy, to spójrz do dokumentacji NPM <a href="https://docs.npmjs.com/">https://docs.npmjs.com/</a>.
Możesz teraz obserwować na stronie bliblioteki w NPM - liczniki pobrań, albo czekać na zgłoszenia ewentualnych bugów w serwisie GitHub :)</p>
