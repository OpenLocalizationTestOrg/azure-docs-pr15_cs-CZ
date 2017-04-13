
<properties
    pageTitle="Nikdy ukládat citlivá data na vlastní obrázky pro Azure RemoteApp | Microsoft Azure"
    description="Seznamte se s pokyny pro ukládání dat do vlastní obrázky v Azure RemoteApp"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Nikdy ukládat citlivá data na vlastní obrázky

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Pokud hostujete vlastní aplikaci v Azure RemoteApp cílem prvního kroku je vytvořit vlastní obrázek. Vytvořit instance OM, které slouží aplikací uživatelům používáme vlastní obrázek. Vlastní obrázek smí obsahovat jen aplikací a nikdy citlivá data, která může dojít ke ztrátě, například SQL databáze, týkající se pracovníků soubory nebo jinak datové soubory jako QuickBooks společnosti soubory. Všechna citlivá data by měly nacházet externí Azure RemoteApp na souborovém serveru, jiné Azure OM, nebo v SQL Azure. Obrázek by měl hostovat jenom aplikace, ke kterému je připojen ke zdroji dat, která představuje data. Kontrola [požadavků pro obrázky s Azure RemoteApp](remoteapp-imagereqs.md) Další informace. 

Pokud chcete zjistit, proč byste neměli ukládat citlivá data, budete muset pochopit, jak funguje Azure RemoteApp. Při vytvoření nebo aktualizace na pozadí více kopií obrázku nebo klony kolekce vzniká. Tyto OM instance jsou přesné kopie vlastní obrázek; Když uživatelé aplikace jsou propojeny na jeden z těchto případů OM. Ale stejné instanci není zaručené neměli užitečné, protože jsou dočasnou. Instance OM hostingu aplikace jsou dočasnou a lze odstraněna nebo odstranit na základě, například při aktualizaci kolekce. 

Když máte k dispozici v kolekci a uživatelé začnou připojovat k VMs, uživatelská data je trvalý a chráněny, protože se dokument uloží na samostatném úložiště v rámci virtuálního pevného disku, že název [disku profilu uživatele (UDP)](remoteapp-upd.md), což je profilů uživatelů v c:\users\<uživatelského profilu >. Při spuštění aplikace, je UDP připojených a považován stejně jako místní profil uživatele operační systém. Přečtěte si další informace o [Azure RemoteApp uloží uživatelských dat a nastavení](remoteapp-upd.md).

Ukázková data, která byste neměli nacházet na obrázku:

- Sdílet data pro uživatele k přístupu
- Databáze SQL nebo QuickBooks DB
- Všechna data v D:\

Ukázková data, která mohou být umístěny ve výchozím nastavení profilu zkopírovala do každého uživatele UDP:

- Konfigurace soubory na uživatele
- Skripty, které by uživatelé potřebují zachovány v jejich UDP

Klíčové body:

- Nikdy úložiště citlivá data, která může dojít ke ztrátě na ikonu při vytváření vlastní obrázek.
- Citlivá data měli vždy umístěny na serveru samostatného souboru samostatné OM Azure v cloudu a vždy externí na OM instance, který je hostitelem vašich aplikací v rámci Azure RemoteApp. 
- Uživatelská data se uloží a trvá disk profilu uživatele (UDP)


