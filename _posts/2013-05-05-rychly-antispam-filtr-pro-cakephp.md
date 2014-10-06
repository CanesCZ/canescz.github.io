---
layout: post
title: Rychlý antispam filtr pro CakePHP
---
	
<p>Tento antispamový filtr je založen na dvou předpokladech. Zaprvé, že roboti si s javascriptem stále neporadí. Zadruhé, že jsou nenažraní a chtějí se pochlubit svými všetečnými <abbr title="www.increaseyourpenistentimes.com">odkazy</abbr>, kde to jen jde. Tento <em>zlepšovák</em> obyčejné uživatele nijak nepoškodí a funguje i bez javascriptu a CSS.</p>

<h2>Tvorba formuláře</h2>
<p>Nejdůležitější jsou inputy <code>antispam</code> a <code>blank</code>.</p>

<pre class="language-php"><code>
	&lt;?php
		echo $this-&gt;Form-&gt;create('Contact');
		echo $this-&gt;Form-&gt;input('name');
		echo $this-&gt;Form-&gt;input('email');
		echo $this-&gt;Form-&gt;input('text', array('escape' =&gt; false));
		?&gt; &lt;div class=&quot;antispamInput&quot;&gt; &lt;?php
			echo $this-&gt;Form-&gt;input('antispam', array('label' =&gt; __('Please confirm your humanity by writing &quot;pizza&quot;.'), 'div' =&gt; false));
		?&gt; &lt;/div&gt;&lt;div class=&quot;blankInput&quot;&gt; &lt;?php
			echo $this-&gt;Form-&gt;input('blank', array('label' =&gt; __('Leave this field blank.'), 'div' =&gt; false));
		?&gt; &lt;/div&gt; &lt;?php
		echo $this-&gt;Form-&gt;end(__('Send'));
	?&gt;

</code></pre>

<p>Do políčka <code>antispam</code> by měl uživatel, jak již jeho popisek naznačuje, vepsat slovo <em>pizza</em>, zatímco <code>blank</code> by měl uživatel nechat prázdné.</p>

<h2>Validace na straně serveru</h2>

<p>Předpokládá se, že pro daný formulář existuje také model. Pokud formulář data neukládá, není špatné mu přesto model vytvořit (a jednoduše tak ověřovat tvar emailu a obdobná data) a nastavit mu <code>public $useTable = false;</code>.</p>

<pre class="language-php"><code>
public $validate = array(
	'antispam' =&gt; array(
		'required' =&gt; true,
		'rule' =&gt; '/^pizza$/i',
		'message' =&gt; 'You are robot, aren\'t you! Try writing &quot;pizza&quot; here or just contact me directly via mail.',
	),
	'blank' =&gt; array(
		'required' =&gt; true,
		'rule' =&gt; '/^$/i',
		'message' =&gt; 'You are robot, aren\'t you! Try not filling this field or just contact me directly via mail.',
	),
);
</code></pre>

<h2>Neobtěžujme uživatele</h2>

<p>Chceme přeci odlákat spammery a jiné prodejce erotických pomůcek, ne potenciální zákazníky! Proto přichází ke slovu javascript, který stačí přihodit do HTML hned za formulář:</p>

<pre class=" language-javascript"><code>
document.querySelector('.antispamInput').style.display = 'none';
document.querySelector('#ContactAntispam').value = 'pizza';
</code></pre>

<p>Ten skryje políčko, které se má zaplnit pizzou a zároveň ho za uživatele ochotně vyplní</p>
<p>Druhé políčko, které má zůstat prázdné, není potřeba nějak složitě řešit. Prostě ho jen schováme pomocí CSS. :) <code>.blankInput{display:none}</code>
