<properties
    pageTitle="Publikování aplikace v Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak publikovat aplikací a prostředků v Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Jak publikovat aplikace v RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Jakmile vytvoříte kolekci RemoteApp, budete muset publikování aplikací nebo zdroje, které mají být k dispozici pro uživatele. Obrázky šablon součástí vašeho předplatného, stačí pár aplikace publikovány ve výchozím nastavení – sdílení dalších aplikací, je potřeba publikovat.

> [AZURE.NOTE] Potřebujete se aktualizovat aplikaci pro? Musíte aktualizovat [Obrázek](remoteapp-update.md) nejdřív.

Na kartě **publikování** na portálu klikněte na **Publikovat**. Můžete přidat aplikaci z nabídky **Start** obrázek šablony, nebo zadejte cestu k nainstalovanou aplikaci na obrázek šablony. Pokud se rozhodnete přidat z nabídky **Start** , zvolte aplikaci publikovat ze seznamu. Pokud se rozhodnete zadejte cestu k aplikaci, zadejte název aplikace a cestu k aplikaci. Pomocí proměnných v parametru path – například "systémová_jednotka %" místo "c:\".

> [AZURE.NOTE] Pokud chcete přidat aplikaci z nabídky **Start** , musíte mít *Přidat tuto aplikaci a * *Start* * nabídka na obrázek šablony.* V opačném RemoteApp uvidí jenom k čemu *je* v nabídce **Start** a bude odpovědi. 

>Aby zkontrolovala, jestli že je aplikace v nabídce **Start** , umístěte soubor zástupce - **lnk** - ve složce Menu\Programs %systemdrive%\ProgramData\Microsoft\Windows\Start.

> Pokud zapomněli jste přidání aplikace do nabídky **Start** po vytvoření šablony, zvolte Přidat cestu k aplikaci. (Nebo obnovit obrázek šablony, ale, úplně o něco víc práce.)


 