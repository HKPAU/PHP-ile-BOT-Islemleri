# PHP-ile-BOT-Islemleri
<!DOCTYPE html>
<html lang="tr-TR">
<head>
	<meta http-equiv="Content-Type" content="text/html ; charset=utf-8">
	<meta http-equiv="Content-Language" content="tr">
	<meta charset="utf-8">

	<script type="text/javascript" src="javaeklentisi.js" language="javascript"></script>
	<title></title> 
</head>

<body>

	<?php


	$ch 	= curl_init();
	curl_setopt($ch, CURLOPT_URL, "https://www.mynet.com/");
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	$sonuc	= curl_exec($ch);
	curl_close($ch);
	//echo $sonuc;

	preg_match_all('/src="(.*?)" /', $sonuc, $dizi);//Resim İceriklerini Yakalamak icin Düzenli İfade Yazdık......
	/*
	echo "<pre>";
	print_r($dizi[1]);
	echo "</pre>";
	*/

	$temizdizi	= array_unique($dizi[1]);//Resimleri Depoladıgımız Dizi de tekrar eden icerik varsa onu temizledik......
	/*
	echo "<pre>";
	print_r($temizdizi);
	echo "</pre>";
	*/
	
	
	foreach($temizdizi as $degerler) {
		$uzantibul	= substr($degerler, -4);//Resim İceriklerinin Uzantısını Bulduk.....
		//echo $uzantibul . "<br/>";
		if(($uzantibul == ".jpg") or ($uzantibul == "jpeg") or ($uzantibul == ".png") or ($uzantibul == ".gif")){
			//echo "<img src='" . $degerler . "'><br/>"; => Resimleri Ekrana Yazdırma İslemi....

			$parcala	= explode("/", $degerler);//Bize Gerekli olan resim isimelrini almak icin url'i parçaladık....
			/*
			echo "<pre>";
			print_r($parcala);
			echo "</pre>";
			*/
			$sonelemanbul	= end($parcala);//Bize gerekli olan resim adları dizinim son elemanlarında gizli oldugu için end metodunu kullandık......

			//echo $sonelemanbul . "<br/>";

			$yeniuzantibul	= substr($sonelemanbul, -4);
			if($yeniuzantibul == "jpeg"){
				$uzantiolustur	= "." . $yeniuzantibul;
			}else{
				$uzantiolustur 	= $yeniuzantibul;
			}

			$isim 		= md5(uniqid(mt_rand()));
			$olustur	= $isim . $uzantiolustur;

			//echo $olustur . "<br/>";

			$dosya	= @file_get_contents($degerler);
			if($dosya){
				file_put_contents($olustur, $dosya);
			}
		}
	}



	?>

</body>
</html>
