---
layout: post
title: Jednoduchý web nad Google Maps API v3
---
	
<p>Takže co budeme budovat? Celý web bude jedna velká mapa, nad kterou se bude "vznášet" obsah včetně navigačních prvků. V plánu je celý seriál, proto se <a href="http://pokus.canes.cz/redir/maplatest">finální verze</a> může čas od času měnit. Důležitější je ale <a href="http://pokus.canes.cz/pages/map">živá ukázka</a> tohoto článku.</p>

<h2>Tak, jdeme na to!</h2>

<p>První věc je samotná stránka, do které mapu načteme. Použijeme už nové HTML5 elementy, ať tvoříme pro budoucnost, ne pro minulost.</p>

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Skvělý web s Google Maps</title>
	<meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
	<link rel="stylesheet" type="text/css" href="/css/bb_css.css" />
	<script type="text/javascript" src="/js/bb_js.js"></script>
	<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?key=SEM_ZKOPÍROVAT_API_KLÍČ&sensor=true"></script>
</head>
<body onload="initialize()">

	<div id="map_canvas"></div>

	<header>
		<nav class="mainMenu">
			<a href="#"><h1>Google Maps web</h1></a>
			<a href="#">Domů</a>
			<a href="#">Druhý odkaz</a>
			<a href="#">Další</a>
			<a href="#">A ještě něco</a>
		</nav>
	</header>
		
	<footer>
	</footer>
</body>
</html>
{% endhighlight %}

<p>Taky budou potřeba nějaké styly, které uložíme do souboru <code>css/bb_css.css</code>. (Proč to bb_? Protože mám více projektů v jednom adresáři, toto je jediná podivnost, slibuju!)</p>

{% highlight css %}
html{
	height: 100%;
}
body{
	height: 100%;
	margin: 0;
	padding: 0;
}
#map_canvas{
	height: 100%;
	width: 100%;
}
{% endhighlight %}

<p>CSS nám roztáhne <code>html</code> element na celou stránku, stejně jako <code>body</code>. Zároveň zrušíme jakékoliv okraje, které prohlížeče přidávají a nakonec roztáhneme <code>#map_canvas</code>, do kterého budeme načítat mapové podklady, přes celou stránku.</p>

<p>Zbývá trocha javascriptu, který vložíme do souboru <code>js/bb_js.js</code>.</p>

{% highlight javascript %}
function initialize() {
var mapOptions = {
	center: new google.maps.LatLng(49.195102, 16.608528),
	zoom: 15,
	mapTypeId: google.maps.MapTypeId.HYBRID
};
var map = new google.maps.Map(document.querySelector("#map_canvas"), mapOptions);
}
{% endhighlight %}

<p>Vytvoříme si funkci <code>initialize()</code>, kterou zavoláme po načtení stránky. V ní definujeme objekt <code>mapOptions</code>, který nese nastavení pro mapu jako je výchozí poloha (center), výchozí přiblížení (zoom) a typ mapy, v tomto případě hybridní. Nakonec už jen vytvoříme novou instanci objektu <code>google.maps.Map</code> a předáme mu kam se má načíst (do map_canvasu) a s jakým nastavením.</p>

<p>Tak! To bychom měli. Jediný problém je, že to nefunguje... Je totiž potřeba si nechat vygenerovat API klíč od Googlu, aby nás neignoroval a začal posílat mapové podklady.</p>

<h2>Google Maps API key</h2>
<p>Přihlásíme se se svým Google účtem na stránku <a href="https://code.google.com/apis/console/">Google APIs</a> a asi ještě nemáme žádný projekt, tak si hned první vytvoříme tlačítkem <em>Create project...</em>. Poté si najdeme vedle popisku <em>Google Maps API v3</em> (asi třicáté shora) přepínač <em>OFF</em> a klikneme na něj. Pročteme :) a odsouhlasíme licenci a znovu si najdeme odkaz <em>Google Maps API v3</em>, vedle kterého je už na přepínači <em>ON</em>. Tento odkaz nás zavede na přehled využití našich map. Tam se toho moc nedočteme, mnohem zajímavější je v levém menu položka <em>API Access</em>. Na této stránce se nachází vytoužený čtyřicetimístný <em>API key</em>, který je denně omezený na "pouhých" 25 000 požadavků. :)</p>
<p>Nyní už jen stačí klíč zkopírovat do naší stránky, konkrétně do adresy v tagu <code>script</code>, ve kterém se dožadujeme Google map.</p>

{% highlight javascript %}
...
<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?key=SEM_ZKOPÍROVAT_API_KLÍČ&sensor=true"></script>
...
{% endhighlight %}


<h2>Hle, Brno!</h2>

<p>Ještě si můžeme pohrát s parametry v našem souboru <code>bb_js.js</code>, změnit výchozí souřadnice, počáteční zoom, nebo typ mapy. Tyto typy jsou <code>HYBRID</code>, <code>SATELLITE</code>, <code>ROADMAP</code> a <code>TERRAIN</code>. A protože se nám nelíbí ovládání od Googlu a všichni máme myš s kolečkem, tak ho skryjeme přidáním <code>disableDefaultUI: true</code> do <code>mapOptions</code>.</p>

<p>Cílem článku však není jen mít neovladatenou mapu. Chceme i nějaké menu. Zkopírujeme si tedy následující styly do <code>bb_css.css</code>, ať máme mapu o něco veselejší. Důležité je nastavení <code>position</code> a <code>z-index</code>ů u jednotlivých prvků.</p>

{% highlight css %}
html{
	height: 100%;
}
body{
	font-family: sans-serif;
	height: 100%;
	margin: 0;
	padding: 0;
}
#map_canvas{
	height: 100%;
	position: absolute;
	width: 100%;
}

.mainMenu{
	margin: 0 auto;
	padding-top: 1em;
	position: relative;
	width: 60%;
	z-index: 100;
}
.mainMenu h1{
	display: inline;
	font-size: 100%;
	font-weight: normal;
	padding: 0;
	margin: 0;
}
.mainMenu a{
	background: #fff;
	border-right: 1px solid #eee;
	color: #333;
	display: block;
	float: left;
	padding: 0.7em 0;
	text-decoration: none;
	text-align: center;
	text-shadow: 1px 1px 1px #d7d7d7;
	width: 19%;
}
.mainMenu a:first-child{
	border-top-left-radius: 3px;
	border-bottom-left-radius: 3px;
}
.mainMenu a:last-child{
	border-right: none;
	border-top-right-radius: 3px;
	border-bottom-right-radius: 3px;
}
.mainMenu a:hover{
	background: #333;
	border-color: #999;
	color: #fff;
}
{% endhighlight %}

<p>Nyní by měl výsledek vypadat stejně jako moje <a href="http://pokus.canes.cz/pages/map">pokusná Google mapa</a>. Pokud ne, tak napište do komentářů pod článkem, co se nepovedlo. Spolu to pak dáme dohromady!</p>
<p>Myslím, že jsme už něco vytvořili a můžeme být na to málo hrdí. Příště se budeme věnovat hlavně mapě a umisťování jednotlivých bodů na různé souřadnice.</p>	
