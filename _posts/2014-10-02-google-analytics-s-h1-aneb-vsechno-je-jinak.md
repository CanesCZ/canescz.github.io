---
layout: post
title: Google Analytics s H1 aneb všechno je jinak!
excerpt: "Google Analytics ukazují čísla, ale že neznamenají to, co je u nich napsáno, to mě upřímně nenapadlo. A možná ani vás..."
---

[JIC](http://jic.cz) se přestěhoval až k technologickému parku, tak jsem jejich nové sídlo musel jít omrknout. A kdy jindy než na [první středě s H1](http://www.h1.cz/prvni-streda-v-brne), tentokrát o Google Analytics.

## Mind = blown!

Jakub Drahokoupil se ze začátku nevrhl přímo na nějaké profi fígle, ale jal se upřesnit, jak se různé metriky měří. A to bylo asi nejpřínosnější.

### Bounce Rate - míra opouštění

> To je kolik lidí na stránku přijde a hned uteče, ne? Stráví tam asi 2 vteřiny a fííí...

No, ne... Bounce Rate je číslo, kolik lidí ukončilo procházení webu na dané stránce. I kdyby si četli článek 10 minut, potom nascrollovali na kontakt v patičce a pak vám napsali nadšený email. Pokud je "zobrazení stránky" ta poslední akce, kterou Google Analytics zjistí, potom připočte k bounce rate jedničku.

> Jo tak proto...

### Čas strávený na stránce

> Tak přijdu na stránku a dokud tam jsem, tak se mi započítává čas...
>
> A jak to Google dělá?
>
> Hmmm, asi si neustále posílá data... Jenže to nějak smysluplně nejde... To přece nemůže věštit?!

A přesně tuto metriku podle mě Google nejvíc "věští". :) Čas na stránce __A__ je počítán od navštívení stránky __A__ až do navšívení stránky __B__ na stejném webu. To dává trochu smysl, ale...

* Co když si článek nebo produkt otevřu v tabu, jdu si dát oběd a po hodině se vrátím? No, Google to po 30 minutách neaktivity vzdá a řekne si, že ten člověk prostě z webu odešel. Prásk! Bounce rate +1

* Co když si otevřu dvě stránky stejného webu po 1 vteřině, tu druhou zavřu a tu první si čtu? První dostane čas strávený 1 vteřinu a druhé se čas načítá a načítá...

* Mám jednostránkový web? Čas na stránce se v podstatě počítat nedá. Tady pan Drahokoupil doporučil měřit scrollování pomocí GA eventů.

### Ostatní postřehy a poučení

* Je skvělé využít svá data o uživatelích (pohlaví, počet nákupů, klidně i ID) a sázet je do vlastních proměnných v GA. Takto člověk získá hlubší přehled o návštěvnících. Je však potřeba informovat uživatele v podmínkách použití.

* Když už se teda chci podívat do Analytics, nejdříve bych si měl položit otázku _"Co se chci z dat dozvědět?"_ Ze surových dat žádné otázky nevykoukám, natož odpovědi. Toto si musím vzít k srdci! :)

## Zase o kus chytřejší

Doteď jsem Googlu věřil a to až moc. Budu taky muset přemýšlet v mnohem širším kontextu a neizolovat data. Krátký čas na stránce se zdá jako negativní, ale s nízkým bounce rate může znamenat, že uživatel rychle našel, co chtěl. Nebo také ne, ale odešel až z jiné stránky, na kterou se v naději dostal...


