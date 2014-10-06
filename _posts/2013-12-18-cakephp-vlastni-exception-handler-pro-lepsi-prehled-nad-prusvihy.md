---
layout: post
title: CakePHP - Vlastní Exception Handler pro lepší přehled nad průšvihy
---

<p>Článek je psán na verzi CakePHP 2.4.1 a snad mu porozumí začátečník i pokročilý. Pokud najdete chybku, nebo je něco nejasné, křičte na mě v komentářích!</p>

<p>Žádný program není bez chyb (teda alespoň ty moje...), ale je fajn se o nich dozvědět a občas s nimi něco udělat. Cake používá <a href="http://book.cakephp.org/2.0/en/development/exceptions.html#built-in-exceptions-for-cakephp">výjimky</a> hlavně pro nenalezené stránky a špatné parametry, ale člověk je může vyvolávat kde chce a jak chce. Jen pozor na výkon, přece jen vytvoření objektu výjimky nějaké prostředky zabere, tak ať to stojí za to!</p>

<h2>K věci: Vlastní Exception Handler pro více informací</h2>

<p>V původním Exception Handleru toho Cake moc říct nechce. V podstatě jen loguje chybu se špetkou kontextu. Já chci ale vědět, o které URL se mluví a hlavně jak se tam uživatel dostal, takže jdeme vařit podle vlastní kuchařky!</p>

<p>V app/Lib si vytvoříme soubor <code>AppExceptionHandler.php</code> do kterého umístímě následující:</p>

<pre class="language-php">
&lt;?php
App::uses('ExceptionHandler', 'Error');

class AppExceptionHandler {

	public static function handle(Exception $exception) {
		CakeLog::write(LOG_ERR, self::_getMessage($exception));
		ErrorHandler::handleException($exception);
	}

	protected static function _getMessage($exception) {
		// chtělo by to vracet nějaké info
		return print_r($exception, true);
	}
}
</pre>

<p>Jednoduchá metoda <code>handle</code> vlastně jen přijme výjimku, pomocí <code>CakeLog::write</code> ji zaloguje a pak dá vědět návštěvníkovi, že něco rozbil. Nám je ale v tento moment návštěvník u samého konce trávícího traktu, chceme vytřískat co nejvíce informací z výjimky a proto doplníme metodu <code>_getMessage</code> o něco užitečnějšího.</p>
	
<pre class=" language-php">
	protected static function _getMessage($exception) {
		$message = sprintf("[%s] %s",
			get_class($exception),
			$exception->getMessage()
		);
		if (method_exists($exception, 'getAttributes')) {
			$attributes = $exception->getAttributes();
			if ($attributes) {
				$message .= "\nException Attributes: " . var_export($exception->getAttributes(), true);
			}
		}
		if (php_sapi_name() !== 'cli') {
			$request = Router::getRequest();
			if ($request) {
				// přidám do zprávy kýžené URL
				$message .= "\nRequest URL: " . $request->here();
				$message .= "\nReferer URL: " . $request->referer();
			}
		}
		// a přihodím seznam volaných funkcí, které mě k chybě dovedly
		$message .= "\nStack Trace:\n" . $exception->getTraceAsString();
		$message .= "\n---------------------------------------------";
		return $message;
	}
</pre>

<p>Abych byl upřímný, polovina kódu je pro mě černou magií zkopírovanou z původního exception handleru, ale důležité je, že si ukládám navíc Request URL a Referer URL, plus pro přehlednost přihodím hromadu pomlček, abych se v tom balastu lépe vyznal.</p>

<h2>Integrace do aplikace je už maličkost</h2>

<p>No, a nakonec je potřeba aplikaci o Exception handleru někde říct, takže v core.php změníme záznam s <code>Configure::write('Exception',...)</code> na toto:</p>

<pre class=" language-php">
Configure::write('Exception', array(
	'handler' => 'AppExceptionHandler::handle',
	'renderer' => 'ExceptionRenderer',
	'log' => false
));
</pre>

<p>...a v bootstrap.php si někde na začátku handler načteme pomocí <code>App::uses('AppExceptionHandler', 'Lib');</code>. A je to!</p>

<p>Samozřejmě je možné si zobrazit i jiné věci z <a href="http://book.cakephp.org/2.0/en/controllers/request-response.html#cakerequest-api">request objektu</a>, ale nic užitečného mě nenapadá. Článek je inspirován <a href="http://book.cakephp.org/2.0/en/development/exceptions.html#create-your-own-exception-handler-with-exception-handler">oficiálním návodem</a> a trochou zoufalství při odhalování bugů. Pokud máte nějaké dotazy nebo připomínky, rád si počtu v diskusi pod článkem!</p>	
