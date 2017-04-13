<properties 
    pageTitle="Azure RemoteApp – jak šířka pásma nebo kvalitu dojít práce pohromadě? | Microsoft Azure"
    description="Zjistěte, jak šířka pásma v Azure RemoteApp může ovlivnit kvalitu vašeho uživatelského prostředí."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – jak šířka pásma nebo kvalitu dojít práce pohromadě?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Když se díváte [celkové šířky pásma sítě](remoteapp-bandwidth.md) potřebných pro Azure RemoteApp, mějte na paměti následující faktory: Toto jsou všechny část dynamické systému, které mají vliv na celkovou uživatelské prostředí. 

- **K dispozici šířka pásma nebo aktuální stav sítě** – sadu parametrů (kolísání ztráty latence) ve stejné síti v daném okamžiku může být ovlivněné aplikace streamování prostředí, což znamená snížené celkové uživatelského prostředí. Dostupné ve svojí síti šířky pásma je funkce přetížení, náhodné ztráty, latence, protože všechny parametry vliv na ovládací prvek mechanismus přetížení, která zase ovládací prvky rychlosti přenosu Chcete-li předejít konflikty.  Například míru ztrát sítě nebo sítě s vysokou latencí pokusy uživatelům chybná i v síti s šířkou pásma 1 000 MB. Ztráta a latence lišit v závislosti na počtu uživatelů, kteří jsou ve stejné síti a co tito uživatelé na projektu vedou (například sledování videa, stahování nebo nahrávání velkých souborů, tisk).
- **Použití scénář** - možnosti závisí na co uživatelé na projektu vedou jako osoby a skupiny ve stejné síti. Například čtení jeden snímek vyžaduje pouze jeden snímek aktualizovat; Pokud uživatel skims a posunuje nad obsahem textový dokument, potřebují vyšší počet snímky mají být aktualizovány sekundu. Komunikace a zpátky k serveru v tomto scénáři postupně používat větší šířku pásma sítě. Zvažte také příklad krajní: více uživatelů jsou sledování videa vysokém rozlišení (třeba 4K rozlišení), podržte HD konferenčních hovorů, 3D her video nebo práce v systémech CAD. Všechny tyto můžete pak dělat i síť skutečně vysoké pásma prakticky nelze použít.
- **Rozlišení obrazovky a počet obrazovek** - větší šířku pásma sítě požaduje pro její úplné aktualizace obrazovky větší než menších obrazovkách. Základní technologii znamená poměrně výborně kódování a přenos jenom oblasti obrazovky, které byly aktualizovány, ale jednou za čas, třeba aktualizovat na celé obrazovce. Pokud má uživatel vyšší rozlišení obrazovky (například 4K rozlišení), tuto aktualizaci vyžaduje větší šířku pásma než obrazovka s nižším rozlišením (třeba 1024x768px). Tato stejné logika platí při použití více než jednu obrazovku pro přesměrování. Šířka pásma musí zvětšíte s počtem obrazovky.
- **Schránka a zařízení přesměrování** – jedná se o problém není zřejmé, ale v mnoha případech Pokud uživatel uloží velký shluk data do schránky, trvá trochu času pro tyto informace pro přenos z klienta vzdálené plochy na server. Podřízené prostředí můžou mít vliv na zkušeností před odesláním obsah schránky. Platí pro přesměrování – Pokud skener nebo webové kamery vytvoří velké množství dat, kterou je nechat zasílat odesílání dat za účelem serveru nebo tiskárnu musí přijímání dokumentu velké místní úložiště, musí být k dispozici aplikace spuštěné v cloudu, aby zkopírujte velké soubory, mohou uživatelé všimnout vynechání snímků nebo dočasně "ukotvený" video protože data potřebná pro přesměrování zařízení roste šířka pásma potřebuje. 

Při vyhodnocení potřeb šířky pásma, zkontrolujte, že zvažte všechny z následujících skutečností fungují jako systém.

Teď vraťte se do [článku šířky pásma hlavní síť](remoteapp-bandwidth.md)nebo přejděte k testování [Šířka pásma](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Víc se uč
- [Odhad využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp – testování přístupnosti šířku pásma s některé běžné scénáře](remoteapp-bandwidthtests.md)

- [Azure RemoteApp šířku pásma sítě – obecné pokyny (když nemůžete testovat vlastní)](remoteapp-bandwidthguidelines.md)