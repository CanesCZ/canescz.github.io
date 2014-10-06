---
layout: post
title: CakePHP - Automatické nastavení pro vývoj a pro produkci
---
	
<p>Článek je psán na verzi CakePHP 2.4.1 a snad mu porozumí začátečník i pokročilý. Pokud najdete chybku, nebo je něco nejasné, křičte na mě v komentářích!</p>

<p>Pro menší projekty je to možná zbytečné, ale občas je prostě potřeba měnit nastavení cache, session, případně nějaké jiné nastavení v core.php nebo bootstrap.php. Přepisovat nastavení debugu, případně databází, stojí za starou belu, takže by určitě bylo fajn pomoct si nějakou podmínkou.</p>

<p>Proto vytvoříme soubor <code>production_variable.php</code> v adresáři app/Config a umístíme do něj definici testovacího / produkčního prostředí.</p>

<pre>
&lt;?php
define('PRODUCTION', false); // pro ostrý provoz dáme true
</pre>

<p>Na localhostu necháme false, do produkce nahrajeme true, a odteď už na něj sahat nebudeme. A co s takovým souborem? Cake si nejdříve načte core.php a potom pokračuje na bootstrap.php, případně database.php, takže si tuto konstantu odchytíme hned na začátku core.php:</p>

<pre>
&lt;?php
require "production_variable.php";
...
</pre>

<p>Tím pádem celá aplikace ví, jestli si hrajeme, nebo jedeme na ostro, takže stačí už jen psát věci jako: <code>Configure::write('debug', PRODUCTION ? 0 : 2);</code> a podobně.</p>

<h2>Zapeklitější nastavení databáze</h2>

<p>S databází je to kapku těžší, ale jen kapku. :) V souboru database.php v třídě DATABASE_CONFIG budeme mít zaráz nastavení pro produkci: <code>$default = array(...)</code> i pro vývoj: <code>$dev = array(...)</code>. Teď už jen použít to správné, k tomu si upravíme konstruktor této třídy, který vložíme za definice databází:</p>

<pre>
function __construct (){
	if(!PRODUCTION){
		$this->default = $this->dev;
	}
}
</pre>

<p>Takže vždy, když bude Cake inicializovat databázi, použije nastavení podle konstanty PRODUCTION. No, není to skvělé? Je to skvělé!</p>

<a href="#wholeDatabasePHP">Zobrazit celý obsah database.php</a>

<pre id="wholeDatabasePHP" class="hidden">
&lt;?php
class DATABASE_CONFIG {

	public $default = array(
		'datasource' => 'Database/Mysql',
		'persistent' => false,
		'host' => 'mysql.canes.cz',
		'login' => 'canes_is_awesome',
		'password' => 'canes_sucks',
		'database' => 'canes_cz_database',
		'prefix' => '',
		'encoding' => 'utf8',
	);

	public $dev = array(
		'datasource' => 'Database/Mysql',
		'persistent' => false,
		'host' => 'localhost',
		'login' => 'root',
		'password' => 'ubertajneheslo',
		'database' => 'canes_cz_database',
		'prefix' => '',
		'encoding' => 'utf8',
	);
		
	function __construct (){
		if(!PRODUCTION){
			$this->default = $this->dev;
		}
	}

}
</pre>	
