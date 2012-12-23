---
18. Funktionsaufrufe innerhalb einer Schleife sind langsam
---

Beispiel
________

	<?php
	
	function theCalc($a,$b)
	{
		$c = $a * $b;
		$c = $c * $c;
		$c = $c / ($a*$b);
		return $c;
	}
	
	$times	= array();
	$result	= array();
	
	for($i = 1; $i <= 100; $i++)
	{
		$value 		= $i + 2;
		
		$start		= (double)microtime(true);
		$result[$i]	= theCalc($i,$value);
		$end		= (double)microtime(true);
		
		$times[$i]['func']['time']  = $end - $start;
		
		unset($end);
		unset($start);
		
		$start		= (double)microtime(true);
		$c 			= $i * $value;
		$c 			= $c * $c;
		$c 			= $c / ($i*$value);
		$result[$i] = $c;
		$end		= (double)microtime(true);
			
		$times[$i]['code']['time']  = $end - $start;
		$times[$i]['diff']			= (double)$times[$i]['func']['time'] - $times[$i]['code']['time'];
	
		
	}
	
	foreach($times as $key=>$data)
	{
		$tmp['diff'] += (double)$data['diff'];
		$tmp['code'] += (double)$data['code']['time'];
		$tmp['func'] += (double)$data['func']['time'];
		echo $key."  |  ".$data['func']['time']." | ".$data['code']['time']." | ".$data['diff']."<br>";
	}
	
	$tmp['diff'] = (double)$tmp['diff']/100;
	$tmp['code'] = (double)$tmp['code']/100;
	$tmp['func'] = (double)$tmp['func']/100;
	
	echo "<br>".$tmp['code']." | ".$tmp['func']." | ".$tmp['diff']."<br>";
	?>
	
Kommentar
--------
Ich persönlich bin der Meinung das die Variante mit Funktionsaufruf ist wesentlich übersichtlicher.

Performance
-----------
Aufgrund der Messungen ist kein Zuwachs an Performance erkennbar.

Ergebnis
---------
siehe data.md

Fazit
------
Der Aufruf einer Funktion innerhalb einer Schleife ist nach meinen Messungen nicht langsamer als die Ausführung des gleichen Codes innerhalb der Schleife.
Im Gegenteil der Aufruf einer Funktion innerhalb einer Schleife ist meinen Messungen nach sogar etwas schnell als der Code.

