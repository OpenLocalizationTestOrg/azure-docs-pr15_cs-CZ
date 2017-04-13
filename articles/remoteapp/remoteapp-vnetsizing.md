
<properties
    pageTitle="Změna velikosti informace pro VNET v Azure RemoteApp | Microsoft Azure"
    description="Další informace o požadavcích na IP adresu Azure RemoteApp s VNET"
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



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informace pro změnu velikosti pro VNET v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Pokud používáte Azure RemoteApp s virtuální sítě (VNET), používá RemoteApp IP adresy v rámci podsítě. Podle měřítka RemoteApp služby, musíte zajistit, aby vaší podsítě dost IP adresy dostupné pro RemoteApp virtuálních počítačích. Během návod pro změnu velikosti není dokonalý, jak RemoteApp dynamicky otáčí nahoru a otáčí dolů virtuálních počítačích v rámci kolekce, pomůže vám odhadu vaší podsítě oblasti. To je zvlášť důležité, když službu RemoteApp je umístěn v VNET, nelze zvětšíte podsítě bez odebrání RemoteApp.

Pro každý RemoteApp kolekce, kterou chcete spustit maximální kapacita byste si měli 100 IP adresy dostupné. Například pokud budete mít jednu kolekci RemoteApp na standardní plánu a chcete mít maximální 500 uživatelů, měli byste mít 100 IP adres pro tuto kolekci. Podobně potřebujete 100 IP adres pro kolekci RemoteApp ve základní plán, který má 800 uživatelů. Pokud budete chtít, aby méně (menší než maximální hodnota), můžete zmenšit požadovaných na kolekci IP adres. Požadavek velikost minimální podsítě je 30 IP adresy (/ 27).

Rezervování souborů tyto informace zajistit vaší VNET je nakonfigurované a pracovní propertly:

- [Migrace z osobního VNET do Azure VNET](remoteapp-migratevnet.md)
- [Ověření VNET Azure pomocí služby Azure RemoteApp](remoteapp-vnet.md)
