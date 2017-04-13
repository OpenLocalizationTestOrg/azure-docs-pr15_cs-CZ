<properties
   pageTitle="Správce prostředků služby struktury clusteru - spřažení | Microsoft Azure"
   description="Základní informace o konfiguraci spřažení struktury služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurace a používání služba spřažení v struktury služby

Spřažení je ovládací prvek, který je k dispozici hlavně k usnadnění přechodu větší monolitický aplikace do cloudu a microservices světě. Který říká, že jej lze také v některých případech jako legitimní optimalizace pro zvýšení výkonu služby, když to mít vliv straně.

Řekněme, že jste dosažení větší aplikace, nebo který právě není určen s microservices vám struktury služby. Toto jsou skutečně běžné a jsme nechali jste několik zákazníků (interní i externí) v tomto případě. Spuštění zvednutí přes celou aplikací v prostředí, zobrazuje se sbalí a spuštění. Pak začněte rozdělit do různých menší služeb, že všechny vzájemné komunikaci.

Potom je "Oops...". "Oops" obvykle spadá do jedné z těchto kategorií:

1. Některé komponenty X v aplikaci monolitický nechali nezdokumentovaný závislost na součásti Y a můžeme jednoduše vypnou a zůstanou do samostatných služby. Protože tyto běží v různých uzlech clusteru, budou tyto nefunkční.
2.  Komunikovat pomocí následujících kroků (místní pojmenované kanály | sdílené paměti | soubory na disku), ale skutečně potřebuju mohli aktualizovat nezávisle zrychlení lze trochu. Budete odeberu pevných závislost později.
3.  Všechno, co je v pořádku, ale zjistíte, že tyto dvě složky jsou ve skutečnosti velmi chatty/výkonu citlivé. Po jejich přesunutí do samostatných služby celkový výkon aplikace tanked nebo latence zvýšit. Celková aplikace není v důsledku toho splnění očekávání.

V těchto případech jsme nechcete, aby ztratíte naše refaktoringu práce a nechcete, aby se pořád vracet monolitu, ale potřebujeme některé smysl místo. To bude trvat buď jsme změnit návrh komponenty pro práci přirozeným jako služby, dokud nebo jsme řešení očekávání výkonu jiným způsobem, pokud je to možné.

Co dělat? Můžete taky zkusit zapnutí spřažení.

## <a name="how-to-configure-affinity"></a>Postup při konfiguraci spřažení
Pokud chcete nastavit spřažení, definujete spřažení vztah mezi dva různé služby. Můžete si spřažení jako "ukazatel ve tvaru" jedné služby v jiném a sdělují "tuto službu lze spustit pouze pokud běží služba." Někdy označovány spřažení jako vztah nadřazenosti nebo podřízenosti (kde ukážete podsložky v nadřazené). Spřažení zaručuje, že repliky nebo instancí jeden služby jsou umístěná na stejné uzly jako repliky nebo instance jiného.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Možnosti různých spřažení
Affinity představuje prostřednictvím jeden z několika režimy korelace a má dvěma různými způsoby. Nejběžnější režimu spřažení takto označujeme NonAlignedAffinity. V NonAlignedAffinity repliky nebo výskyty různých službách jsou umístěny ve stejné uzlech. V režimu je AlignedAffinity. Zarovnanými spřažení je užitečná pouze stavové služby. Konfigurace dvě stavové služby, které mají zarovnaný spřažení zaručuje, že základní barvy těchto služeb jsou umístěny ve stejné uzlech jako každý jiný. Způsobí taky každou dvojici druhotné pro tyto služby umístit na stejný uzlů. Je také možné (když méně běžné) konfigurace NonAlignedAffinity stavové služby. Pro NonAlignedAffinity různých kopie dvě stavové služby by být použít pro společné umísťování ve stejném uzlech, ale bez by být pokusu zarovnat jejich základní barvy nebo druhotné.

![Režimy spřažení a jejich efekty][Image1]

### <a name="best-effort-desired-state"></a>Nejlepší plánování řízené úsilí žádoucí stavu
Existuje několik rozdílů mezi affinity a monolitický architektury. Řada z nich jsou, protože vztahu spřažení je nejlepší plánování řízené úsilí. Služby v relaci spřažení se zásadně různé entity, které může selhat a přesouvat nezávisle na sobě. Existují také pro proč může přerušit spřažení relace. Například kapacitu kde jenom některé z objektů služby v relaci spřažení vejde na daný uzel. V těchto případech Ačkoli je vztah spřažení na místě, ji nelze vynutit kvůli jiných omezení. Pokud je možné k jejímu vynucení všechny ostatní omezení a spřažení později porušení omezení spřažení automaticky opravován.  

### <a name="chains-vs-stars"></a>Chains (řetězy) porovnání hvězdiček
Dnes nemůžeme Chains (řetězy) modelu spřažení relace. Co to znamená, je to služba, která je dítěte některou relaci spřažení nemůžou být nadřazený objekt u jiného spřažení relace. Pokud chcete modelovat tohoto typu relace, máte efektivně model jako hvězdy, nikoli řetězce. K tomuto účelu by být nadřazena nejnižší podřízené do nadřazené "střední" podsložky místo. V závislosti na ujednání služby může být nutné vytvoření služby "zástupný symbol" sloužit jako nadřazené pro více dětí.

![Chains (řetězy) a hvězdičky v kontextu spřažení relace][Image2]

Dalším krokem je potřeba pamatovat k relacím spřažení dnes je, že pocházejí směrová. To znamená, že pravidlo "spřažení" pouze vynucuje, je podsložky, kde je nadřazený. Pokud například nadřazeného neočekávaně selhání na jiný uzel potom správce prostředků clusteru nemá vašeho názoru ve skutečnosti že je něco, co špatně dokud zjistí, že podsložky není umístěn s nadřazený objekt; relace není nevynucují okamžitě.

### <a name="partitioning-support"></a>Rozdělení podpory
Poslední oznámení o spřažení se tento spřažení relace nejsou podporovány, kde je nadřazený oddíly. Toto je něco, co může podporujeme nakonec, ale ještě dnes není povolená.

## <a name="next-steps"></a>Další kroky
- Pro další informace o dalších možností konfigurace služeb podívejte se na téma na ostatní clusteru Správce konfigurace k dispozici [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)
- Důvody kde lidé používají spřažení, například omezení služeb malý nastavte strojů a chcete agregovat načíst sada služeb, lépe podporují prostřednictvím skupiny aplikací. Podívejte se na [Skupiny aplikací](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
