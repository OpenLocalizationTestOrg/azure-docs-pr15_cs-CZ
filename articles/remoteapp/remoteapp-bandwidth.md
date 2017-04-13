
<properties 
    pageTitle="Odhad využití šířky pásma sítě Azure RemoteApp | Microsoft Azure"
    description="Další informace o požadavcích na šířku pásma sítě pro vaše kolekce Azure RemoteApp a aplikace."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Odhad využití šířky pásma sítě Azure RemoteApp 

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp používá pro komunikaci mezi spuštěné v Azure cloudu a vaši uživatelé vzdálené plochy RDP (Protocol). Tento článek obsahuje některé základní pokyny, které můžete použít k odhadu využití této sítě a potenciálně vyhodnocení využití šířky pásma sítě za Azure RemoteApp uživatele.

Odhad využití šířky pásma za uživatele je velmi složité a vyžaduje aplikaci více aplikací současně multitasking scénáře kde aplikací můžou mít vliv na dalších uživatelů výkonu podle jejich služba pro šířku pásma sítě. I typ klienta vzdálené plochy (například Mac klient versus HTML5 klienta) může vést k výsledkům různých pásma. Můžete pracovat pomocí těchto komplikace, jsme budete rozdělí scénáře použití několika běžných kategorií replikovat reálný scénáře. (Je-li reálné situace je samozřejmě kombinaci kategorie a se liší podle uživatele.)

Předtím, než budeme pokračovat další - musí Všimněte si, že předpokladu, že RDP poskytuje dobré pracovníků prostředí pro většinu použití scénářů v sítích s latence pod 120 ms a šířku pásma víc než 5 MB – to je založená na RDP na možnost dynamicky nastavit pomocí dostupných šířka pásma nebo šířku pásma odhadovaná aplikace. Tento článek přesahuje, můžou být "většina scénáře použití" se podívat, okraje, kde začít scénáře unwind a uživatelské rozhraní se začne snížit.

Teď rezervovat v následujících článcích informace, včetně faktory vzít v úvahu, doporučení podle směrného plánu a co jsme neobsahovala v naší odhady.

- [Jak šířka pásma nebo kvalitu dojít práce pohromadě?](remoteapp-bandwidthexperience.md)
- [Testování šířku pásma s některé běžné scénáře](remoteapp-bandwidthtests.md)
- [Rychlé pravidla Pokud nemáte čas nebo možnost test](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Co nejsou jsme včetně?

Když si prohlédnete navrhované testů a naše celkové (a admittedly obecný) doporučení, mějte na paměti, že jsou několika faktorech, které budeme považuje za. Například komplikace uživatelské prostředí poskytovanou asymetrické přírodní nahrávání a stahování šířky pásma. Asymetrické přírodní většina sítě Wi-Fi ovlivní navíc týkající se výkonu a povědomí prostředí uživatele. Interaktivní scénářích může podřízené přenosy přednostně nižší než nejvyšší úrovně, které mohou zvýšení počtu ztraceny videoklipu nebo zvukového snímky a tedy mít vliv na uživatele povědomí streamování prostředí. Můžete použít vlastní pokusy najdete v článku co je dobré pro případ použití konkrétní a sítě.

Sice probereme tato přesměrování zařízení, jsme not vzít v úvahu dopad šířku pásma sítě přenosy způsobená připojených zařízení, například úložiště tiskárny, skenery, webové kamery a jiná zařízení USB. Efekt tato zařízení obvykle dočasně špičky potřeb šířky pásma a nezmizí po dokončení úkolu. Ale pokud mít často, vyžadující šířky pásma může být docela výrazná.

Taky není probereme tato jak jeden uživatel může být ovlivněné ostatním uživatelům ve stejné síti. Například jeden uživatel jinými video 4K síti 100 MB/s ovlivní výrazně ostatním uživatelům ve stejné síti pokusíte stejné úkolu. Bohužel získá postupně obtížnější určit dopad souběžné použití dát běžné nebo zahrnující všechna doporučení o jak systém provádí v agregační. Můžete říkáme, stačí technologii protokol bude nejlépe využijete dostupné síťové šířky pásma, že máte její omezení.