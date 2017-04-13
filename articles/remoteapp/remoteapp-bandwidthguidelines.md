<properties 
    pageTitle="Azure RemoteApp šířku pásma sítě – obecné pokyny | Microsoft Azure"
    description="Princip několik pokynů základní síťový šířky pásma pro vaše kolekce Azure RemoteApp a aplikace."
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
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp šířku pásma sítě – obecné pokyny (když nemůžete testovat vlastní)

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Pokud nemáte čas nebo možnost Spustit [testy šířku pásma sítě](remoteapp-bandwidthtests.md) pro Azure RemoteApp, tady je několik poměrně obecných pokynů, které vám pomohou při odhadu šířky pásma sítě za uživatele.

Pokud máte kombinaci podobnému sledu nedoporučujeme všechno, co menší než (nebo se rovná) 10 MB/s jako šířka pásma minimální moderní aplikací v prostředí vzdálených připojeného k Internetu. (I když se, jak je popsáno, nebude to zajistit vyšší než průměrná uživatelského prostředí.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Komplexní PowerPointu, jednoduché aplikace PowerPoint

Azure RemoteApp znamená nejlépe v místní síti 100 MB. Na profil sítě 10 MB/s víc než 5 %, po kolísání nad 120 ms se uživateli zobrazí prostředí průměr. Na 1 MB/s, různých je výrazná – například v prezentaci, kterou uživatel nemusí animované přechody vůbec nezobrazovaly protože přeskočilo snímky.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer smíšený PDF, PDF, Text

10 MB/s síťový profil je tomu vašemu nejbližší LAN ve většině aspekty. 1 MB/s poskytne OK zkušenosti, i když může být některé kolísání když bude uživatel posouvat i když jsou obrázky na obrazovce.

## <a name="flash-video-youtube"></a>Formát Flash video (YouTube)

100 MB/s LAN poskytuje nejlepší možnosti, zatímco 10 MB/s je přijatelné (význam jsme zachovávat frekvenci snímků, ale kolísání zvýšení). Na 1 MB/s kolísání je velmi vysoké a výrazná.

## <a name="word-typing-word-remote-input"></a>Word psaní (vstup vzdálené Word)
Toto je scénáři využití šířky pásma minimum. Na 256 KB/s nabízíme horší základním jako místní sítě.

## <a name="learn-more"></a>Víc se uč
- [Odhad využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp – jak šířka pásma nebo kvalitu dojít práce pohromadě?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting šířku pásma s některé běžné scénáře](remoteapp-bandwidthtests.md)