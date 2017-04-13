<properties
   pageTitle="Testování pera | Microsoft Azure"
   description="Tento článek obsahuje přehled průniku testování obrázku (pentest) a jak provádět pentest proti aplikace spuštěné v Azure infrastruktury."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Testování pero

Jednou z skvělých možností používat Microsoft Azure testování aplikací a nasazení je, že nemusíte sestavit infrastrukturu místní vývoji, otestujte a nasazení aplikace. Všechny infrastruktury se stará služby platformu Microsoft Azure. Nemusíte bát žádanek, získáním, "stáčení a překrývání" místní hardwaru.

Toto je skvělý – ale potřebujete a ujistěte se, provádět běžné zabezpečení pečlivostí. Jednou z co musíte udělat je průniku testovat aplikace nasadíte v Azure.

Možná už víte, že Microsoft provádí [průniku testování naše Azure prostředí](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Pomáhá vylepšováním naše platformy a provede naše akce z hlediska vylepšení ovládací prvky zabezpečení, úvodní informace o nové ovládací prvky zabezpečení a vylepšení naše procesy zabezpečení.

Jsme není pera testovat aplikace za vás, ale nám jasné, že se má a potřeba provést pera testování vlastní aplikace. Je dobré, proto, že můžete zvýšit zabezpečení aplikace, vám pomoci zabezpečit celý Azure ekosystému.

Když jste pera testování aplikací, může vypadat jako útok námi. Jsme [nepřetržitě sledovat](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) útoku vzorků a zahájí proces incidentu odpověď, pokud potřebujeme. Není pomoct vám i ho není Pomozte nám Pokud jsme aktivace incidentu odpověď kvůli vlastní termín péčí pera testování.

Co dělat?

Až budete připravení na pero testování aplikací hostované Azure, musíte a dejte nám vědět. Jakmile víme, že budete chtít provádět specifické testy jsme nebude omylem vypnutí (například blokování IP adresy, kterou jste testování z), jakož testy odpovídat Azure pera testování podmínkami a ujednáními.
Standardní testy, které mohou vykonávat patří:

- Testuje na koncových bodů vám pomůžou odhalit [horní 10 chyby otevřít webovou aplikaci zabezpečení projektu (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
- [Testování argumentu neurčité](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) koncových bodů
- [Skenování portu](https://en.wikipedia.org/wiki/Port_scanner) koncových bodů

Jedním typem test, který nelze provést je jakýkoli druh [útok](https://en.wikipedia.org/wiki/Denial-of-service_attack) služby (DoS). Platí to i zahájení útok DoS samotné nebo provádění související testů, které může určit, ukazují nebo simulovat libovolný typ DoS útoku.

Jste připravení začít pracovat s pera testování aplikace hostovaný v Microsoft Azure? Pokud ano a potom hlavy na myší na [Přehled Test průniku](https://security-forms.azure.com/penetration-testing/terms) stránku (a klikněte na tlačítko testování požádat o v dolní části na stránce vytvořit. Také najdete další informace o pera testování podmínky a ujednání a užitečné odkazy na tom, jak můžete nahlásit chyby zabezpečení týkající se Azure nebo jiné služby Microsoft.
