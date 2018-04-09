---
layout: post
title: "SmooshGate - kontrowersje wokol flatten i flatMap"
titlePL: "SmooshGate - kontrowersje wokół flatten i flatMap"
description: ""
category: Article
date: 2018-04-09 17:30:00 +0100
ima: "/assets/img/gk.png"
tags: ["JavaScript"]
linkTitle: [
		{a: "Joeyify - automatyczny słownik synonimów", b: "a1"}
		]
excerpt_separator: <!--more-->
---
<!-- {% highlight javascript %}
{% endhighlight %} -->
{% include JB/setup %}

<p>W dzisiejszym poście przybliżę pewną ciekawostkę z JavaScriptowego świata. Opowiem o aferze, okrzykniętej humorystycznie w internecie jako <a href="https://twitter.com/search?q=%23smooshgate">#SmooshGate</a>. Przybliżę jak powstają nowe standardy języka JavaScript i jakie towarzyszą temu problemy. Zaczynajmy!</p><!--more-->

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Flatten i FlatMap</h3>
<p>Wielkie afery, skandale i sytuacje kontrowersyjne na świecie nazywane były z przyrostkiem "gate". <code>Smoosh</code> to nieoficjalna propozycja nazwy dla nowej funkcji w języku JavaScript. Stąd nazwa - SmooshGate. Ale zacznijmy od początku:</p>
<p>TC39 18 stycznia zaproponowało dodanie do języka JavaScript funkcji <code>flatMap</code> oraz <code>flatten</code>. Samą propozycję można zobaczyć tutaj:
<a href="https://tc39.github.io/proposal-flatMap/">https://tc39.github.io/proposal-flatMap/</a></p>
<p>Czym jest TC39? To grupa będąca częścią instytucji standaryzującej język ECMAscript, znany nam jako JavaScript.</p>
<p>Obydwie funkcje rozszerzają typ tablicowy i mają podobną funkcjonalność. W skrócie:</p>
<p> <code>flatten</code> służy do "rozwijania" tablic zagnieżdżonych aż do podanej głębokości, czyli usuwania tablic zawierających się w innej tablicy i dodawaniu jej elementów do głównej tablicy. Spróbujmy może wytłumaczyć to jednak na przykładzie :)</p>
<script src="https://gist.github.com/IdaszakDaniel/191d02d2126d4f7697be1d697e730520.js"></script>
<p><code>flatMap</code> działa jak zwykły map, który opisałem już wcześniej w <a href="https://www.idaszak.com/article/2017/05/31/podstawy-programowania-funkcyjnego-3-map-i-filter">"podstawy programowania funkcyjnego 3 map i filter"</a>, z tą różnicą, że "rozwija" zagnieżdżone tablice z głębokością 1.</p>

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> SmooshGate</h3>
<p>A więc w czym problem, skoro funkcje znane z wielu bibliotek będą wprowadzane do oficjalnego standardu JavaScript? Kłopoty zaczęły się od zgłoszenia na portalu GitHub: <a href="https://github.com/tc39/proposal-flatMap/pull/56">https://github.com/tc39/proposal-flatMap/pull/56</a>, <br>kiedy to zorientowano się, że ponad ośmioletnia biblioteka <a href="https://mootools.net/">MooTools</a> wykorzystuje nazwę flatten dodaną do <code>array.prototype</code>. Dodanie takiej funkcji w nowym standardzie JS spowodowałoby, że starsze strony internetowe mogłyby przestać działać. Polityka ECMA zakłada, że nie wolno dokonywać w języku zmian psujących kompatybilność wsteczną. Stąd zaproponowane przez użytkowników nazwy - <code>smoosh oraz smooshMap</code>.</p>

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> To smoosh or not to smoosh?</h3>
<p>Szybko pojawiła się opozycja z ogromną ilością argumentów. Jeden z developerów postanowił nawet stworzyć bibliotekę, która "zarezerwuje" funkcję <code>smoosh</code> w prototypie typu tablicowego. Co więcej - zachęcając wszystkich do dodania jej do swojego projektu. Twórcy Webpacka i one drive wyrazili zainteresowanie dodania tej biblioteki, co można znaleźć na twitterze.</p>

<p><center><blockquote class="twitter-tweet" data-lang="pl"><p lang="en" dir="ltr">Find a way to add this to the webpack runtime code and I&#39;ll do my best to consider landing. No promises. :)</p>&mdash; Sean Thomas Larkin (@TheLarkInn) <a href="https://twitter.com/TheLarkInn/status/971585313259704321?ref_src=twsrc%5Etfw">8 marca 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</center></p>

<p> Więcej informacji: <a href="https://github.com/staltz/prevent-smoosh">https://github.com/staltz/prevent-smoosh</a>.</p>
<p>Flatten to bardzo popularna nazwa, służąca do "rozwijania" zagnieżdżonych tablic. Możemy znaleźć je w różnych bibliotekach, takich jak:</p>
<ul style="padding-left: 30px;">
	<li><a href="http://underscorejs.org/#flatten">Underscore - flatten</a></li>
	<li><a href="https://lodash.com/docs/4.17.5#flatten">Lodash - flatten</a></li>
	<li><a href="http://ramdajs.com/docs/#flatten">Ramda - flatten</a></li>
</ul>
<p>Przy czym <code>flatMap</code> możemy znaleźć tylko w bibliotece <a href="https://lodash.com/docs/4.17.5#flatMap">lodash</a>.</p>

<p>Kolejnym istotnym miejscem zastosowania <code>flatMap</code> są monady znane z programowania funkcyjnego. Przeciwnicy nowej nazwy wysuwają argumenty, że używanie <code>smoosh</code> zamiast <code>flatMap</code> utrudni zrozumienie, skomplikowanych już bardzo monad, w których to funkcja ta jest wykorzystywana. Czasem pojawia się w ich implementacji nazwa <code>chain()</code>, czasem <code>bind()</code>, jednak to <code>flatMap()</code> jest najpopularniejsza. Prawdopodobnie nigdzie nie spotkamy się z nazwą <code>smoosh</code>.</p>

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Jak stworzyć funkcję prototypową?</h3>
<p>Z przyczyn, które opisałem powyżej wiemy, że dodawanie funkcji do prototypu typu tablicowego to bardzo zła praktyka, ale mimo wszystko - warto wiedzieć jak taka implementacja wygląda. Nie będziemy bawić się w implementację "rozwijania" tablic, mądrzejsi ludzie zrobili to już wcześniej.</p>
<script src="https://gist.github.com/IdaszakDaniel/a0cb4a6c9e8ab6438fc536d34026281d.js"></script>
<p>Do prototypu typu tablicowego dodaliśmy przekornie nazwaną funkcję <code>smoosh</code>, która po użyciu zwraca nową, całkowicie zamienioną tablicę.</p>

<h3 id="a1"><span style="color:gray; font-size: 30px;">#</span> Co dalej?</h3>
<p>Nie chcę zabierać jakiegoś szczególnego stanowiska w tym temacie, jednak uważam, że jako i kompatybilność wsteczna jest priorytetem, tak jednak sama nazwa niekoniecznie musi być zmieniona na <code>smoosh</code>. Istnieją inne propozycje i alternatywy, które wyglądają znacznie lepiej i są bardziej intuicyjne.</p>
<p><center>
<blockquote class="twitter-tweet" data-lang="pl"><p lang="en" dir="ltr">smooth &amp; smooshMap aren’t really on the table, they were intended to be examples of “ridiculous names if we don’t solve this problem” :)<br>I’m hoping that we can avoid any renaming, either by clever spec work, or by adjusting the algorithm to match the extant implementations, WDYT?</p>&mdash; Rick Waldron (@rwaldron) <a href="https://twitter.com/rwaldron/status/972117275020013569?ref_src=twsrc%5Etfw">9 marca 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script></center></p>
<p>Z wypowiedzi Ricka Waldrona z ECMA/TC39 wynika, że nazwy <code>smoosh</code> i <code>smooshMap</code> były użyte humorystycznie i nigdy nikt nie myślał o nich poważnie, a sama sprawa będzie w najbliższym czasie rozwiązywana przez TC39.</p>
