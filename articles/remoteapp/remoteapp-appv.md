<properties
    pageTitle="Používání aplikací V aplikaci s Azure RemoteApp | Microsoft Azure"
    description="Informace o používání aplikací V aplikaci v Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Používání aplikací V aplikaci v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

V aplikaci aplikace můžete použít v kolekci hybridní Azure RemoteApp, která vyžaduje připojení k doméně.

Než začnete, zkontrolujte, že instalace klienta V aplikaci 5.1 s nejnovějšími aktualizacemi. Bude potřeba vytvořit [vlastní obrázek](remoteapp-create-custom-image.md) , který obsahuje klienta V aplikaci.  

Není těžké si pomocí stávající infrastrukturu App V Azure RemoteApp. Vzhledem k tomu kolekce hybridní nasazení do Azure VNET, který má přístup k vaší domény řadiče domény a VMs jsou domény připojen, můžete využít své existující v aplikaci pro infrastrukturu a nasazení způsoby easyily hostitelské V aplikaci aplikace v Azure RemoteApp. Tady je několik tipů, které je třeba vědět v závislosti na použitém typu nasazení V aplikaci, kterou Teď máte:

| Konfigurace možností |                       | Kladné                                                               | Záporné                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Metoda doručení       | Přenos (na vyžádání) | Aplikace je vždycky nejnovější a nové                                     | První latence času                                                                                    |
|                       | Připojit               | Nejrychlejší; aplikace již existuje v OM                              | Nárůst velikosti. - přijímá místa obrázek (127 GB limit)                                                           |
| Aplikace umístění úložiště  | Sdílený obsah        | Spustí aplikaci v paměti instance Azure RemoteApp                         | Eats paměti a dobré připojení k datové proudy serveru (soubor), kde jsou uložená v aplikaci                      |
|                       | Disku (v mezipaměti)         | Rychlé spuštění. Aplikace nezávislé na dostupnost zdroje obsahu | Nárůst velikosti. - přijímá místo na obrázek (127 GB limit)                                                           |
| Zaměření na             | Uživatel                  | Vyžaduje úplné samostatné aplikace V infrastrukturu                          |                                                                                                       |
|                       | Globální (počítač)      |  Předem publikovat nebo obrázku pomocí publikování na serveru                         |  Potřebujete aktualizovat Azure obrázek, pokud chcete aktualizovat aplikaci (velké). Zabírá místo na obrázek. |

 Po vytvoření vlastní obrázek kolekci hybridní publikování aplikace, přiřaďte uživatelům a moct užít dobrý pocit existující V aplikaci aplikace hostované v Azure RemoteApp Doručená do libovolného zařízení kdekoli.
