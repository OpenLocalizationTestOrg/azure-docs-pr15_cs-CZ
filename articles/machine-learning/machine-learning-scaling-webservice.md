<properties
   pageTitle="Změna měřítka webová služba | Microsoft Azure"
   description="Zjistěte, jak zobrazit webové služby zvýšením souběžné a přidávání nových koncové body."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="výuka, webové služby Azure počítače operationalization měřítka koncový bod, souběžné"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Změna měřítka webové služby

>[AZURE.NOTE] Toto téma popisuje technik pro klasické počítače výukové webové služby. 

Ve výchozím nastavení jednotlivých publikované webové služby konfigurace podporuje 20 souběžně prováděné žádosti a může být dosahovat 200 souběžně prováděné žádosti. Během portálu Azure klasické umožňuje nastavit tuto hodnotu, výukové počítače Azure optimalizuje automaticky nastavení poskytnout nejlepší možný výkon webové službě a portálu hodnota ignorován. 

Pokud chcete volání rozhraní API se zatížením vyšší než hodnota Maximum současné volání 200 podporovat, je třeba vytvořit více koncové body na stejné webové služby. Můžete pak náhodně distribuovat zatížení ve všech z nich.

## <a name="add-new-endpoints-for-same-web-service"></a>Přidání nového koncové body stejné webové služby

Změna velikosti webové služby je běžných úkolů. Několik důvodů, proč zobrazit mají podporu více než 200 souběžně prováděné žádosti, zvětšit dostupnost prostřednictvím více koncové body nebo poskytují samostatné koncové body webové služby. Zvětšit měřítko přidáním dalších koncové body stejné webové služby prostřednictvím [portálu pro Azure klasické](https://manage.windowsazure.com/) nebo portálu [Azure počítače výukové webové služby](https://services.azureml.net/) .

Další informace o přidávání nových koncové body najdete v článku [Vytvoření koncové body](machine-learning-create-endpoint.md).

Mějte na paměti, použitém vysoké souběžné počet může být poškozující, pokud nejsou volání rozhraní API s odpovídajícím způsobem vysoká rychlost. Jestliže umístíte nízkou zatížení na rozhraní API nakonfigurovány velkém zatížení, se může zobrazit latence Občasná časové limity a/nebo vrcholy pole.

Obrázek rozhraní API se obvykle používají v situacích, kde zhoršeným latence žádoucí. Latence tady znamená dobu potřebuje pro rozhraní API dokončete jednu žádost a nemá účet pro všechny zpoždění sítě. Řekněme, že máte rozhraní API s latence 50 ms. Plně využívat k dispozici objemu omezení úroveň Vysoké a Max současné volání = 20, budete muset volat toto rozhraní API 20 * 1 000 / 50 = 400 časy sekundu. Rozšíření to dál, Max současné volání 200 vám umožní volání časy rozhraní API 4000 sekundu za předpokladu, že latence 50 ms.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
