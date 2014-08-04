---
layout: post
title: Podmíněné styly  a základní podpora HTML5 pro Internet Explorer
---

<p>Strašně rád bych už staré Explorery pohřbil, ale stále jich tu pár straší. Nejen podle <a href="http://caniuse.com/usage_table.php" target="_blank">globálních statistik</a>, ale i Google Analytics říkají, že místní publikum stále nemá to nejnovější.</p>

<p>Občas je potřeba ošetřit nějaké drobnosti, přidat tuhle to, támhle ono... Ošklivým hackům typu <code>_margin-top</code>, které fungují jen v některých verzích některých prohlížečů, bych se pro jistotu obloukem vyhnul. Nejenže validátory brečí a různé zvýrazňovače kódu si taky zavzlykají, hlavně je kód nepřehledný a údržba není žádná sláva.</p>

<h2>Podmínky FTW! Nebo radši ne?</h2>

<p>Na mnoha webech se doporučuje použít jiný soubor se styly:
<code><!--[if IE 7]><link rel="stylesheet" type="text/css" href="css/ie7.css">< ![endif]--></code>

Jenže to práci ani trochu neulehčí a spravovat více stylů pro více verzí prohlížečů je po chvíli na zbláznění.</p>

<p>Asi nejpříjemnějším (a dle mého názoru nejlepším) způsobem je použít podmínky trochu jinak. Přidat tagu html vlastní třídu podle prohlížeče. Naposledy mi stačilo pouze rozlišit starší a novější než IE8.<p>

<pre>&lt;!--[if lte IE 8 ]&gt;    &lt;html class="ie"&gt; &lt;![endif]--&gt;
&lt;!--[if (gte IE 9)|!(IE)]>&lt;!--&gt; &lt;html> &lt;!--&lt;![endif]--&gt;</pre>

<h2>Znásilnění starých IE pro HTML5</h2>

<p>A protože starší IE nepozřou HTML5 tagy jako jsou <code>&lt;article&gt;</code> nebo <code>&lt;nav&gt;</code>, musí se jim pomoct a není to vůbec těžké. (Od IE7 to funguje určitě, IE6 a níž už netestuji. Tam už mám strach o své duševní zdraví.)
</p>

<p>Nejdřív je potřeba si vytvořit pomocí javascriptu prázdné prvky. Ty se ani nemusí umisťovat do DOMu, takže to je rychlovka. IE tak pozná, že něco takového může existovat a zparsuje stránku včetně nových párových tagů správně.</p>

<pre>function cx(y){document.createElement(y)};
var z=['nav','article','section','aside','header','footer'];
var l=z.length;for(var i=0;i&lt;l;i++){cx(z[i])};</pre>

<p>Již zkomprimovaný kód je opravdu lehoučký a zkušenější oko uvidí deklaraci funkce <code>cx</code>, která vytváří prvky podle předaného parametru. Potom následuje pole prvků, které chceme použít. Je možné přidat ještě například figure, figcaption, video, a plno dalších kravin. A nakonec cyklus projede celé pole a pomocí výše zmíněné funkce všechny nové prvky vykouzlí do éteru.</p>

<p>Jenže neznámé elementy si IEčko vykresluje jako <code>display: inline</code>, takže zbývá ještě poslední fix.</p>

<pre>nav, article, section, aside, header, footer{
    display: block;
}</pre>

<p>To by mělo snad stačit pro všechny menší projekty. Pokud pracujete na nějakém monstru, je možná lepší využít komplexnějších knihoven, jako jsou <a href="http://modernizr.com/">Modernizr</a>, <a href="https://github.com/aFarkas/html5shiv">HTML5 Shiv</a> nebo jistě mnoho dalších. Já ale miluji malá, rychlá a srozumitelná řešení, pokud je to alespoň trochu možné.</p>


<h3>Zdroje a inspirace:</h3>
<ul>
<li><a href="http://www.paulirish.com/2008/conditional-stylesheets-vs-css-hacks-answer-neither/">Paul Irish: Conditional Stylesheets vs CSS Hacks? Answer: Neither!</a></li>
<li><a href="http://caniuse.com/usage_table.php">Can I use...: Browser usage</a></li>
<li><a href="https://github.com/aFarkas/html5shiv">HTML5 shiv (pro téměř plnou podporu všech HTML5 tagů ve starých prohlížečích)</a></li>
</ul>	
