<properties
    pageTitle="Správa licencí Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak fungují programy v Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Jak licencování funguje v Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Takže jste nastavili služby Azure RemoteApp vytvořené šablony a jste připraveni publikovat aplikace uživatelům. Ale přesto se řídí jeden co chcete zjistit: licencí. Jak funguje jenom licencování práce pro RemoteApp a aplikace, které sdílíte prostřednictvím RemoteApp?

RemoteApp nevyžaduje Windows licence nebo vzdálené plochy CAL. Vaše předplatné stará kresbu žirafí RemoteApp samotné. (Podívejte se na podrobnosti o [cenách plánů](https://azure.microsoft.com/pricing/details/remoteapp).)

Pokud používáte některou z obrázků, které je součástí vašeho předplatného, můžete sdílet některé z aplikací na tomto obrázku aniž by musel samostatnou licenci. Například pokud používáte Windows serveru 2012 R2 šablony k vytváření své sbírky, můžete sdílet System Center Endpoint Protection s jinými uživateli. Pouze výjimkou tohoto pravidla se Office 365 ProPlus, které vyžadují samostatné předplatné Office 2013, do kterého ve výrobním kolekce nejde sdílet.

Pokud chcete použít obrázek šablony Office 365, které jsou součástí RemoteApp, musíte mít *existující* plán Office 365 ProPlus. Platí totéž i pro aplikaci Office 365, které publikujete pomocí vlastní šablonu. Je potřeba aktivovat aplikace s vlastními předplatného. To platí pro zkušební verzi a placené předplatné. Pokud chcete použít obrázek šablony Office 365 během zkušební verze, *a ještě nemáte předplatné*, přejděte na stránku Office 365 [si zaregistrovali](https://go.microsoft.com/fwlink/p/?LinkID=403802) ke zkušebnímu předplatnému. V tématu [jak RemoteApp a Office spolupracují?](remoteapp-o365.md) Další informace.

Pokud během zkušební období nechcete získání zkušební předplatné Office 365, použijte Office 2013 Professional Plus šablony obrázek, který je součástí RemoteApp. Tento obrázek šablony lze použít pouze pro 30 dní a nelze převést na placené kolekce.

Pro ostatní aplikace budete muset zkontrolujte, jestli máte licenci k sdílení aplikace.

Který dává smysl správné? Můžete publikovat aplikace, které máte ze zákona oprávnění ke sdílení. A potřebujete, aby zkontrolovala, jestli že máte skutečně nárok sdílení aplikací.

Dejte pozor CAL nebo multilicence smlouva nelze použít v cloudu kolekci. Multilicence smlouvu *můžete* použít k aktivaci aplikace ve vaší kolekci hybridní (s výjimkou Office). Stačí nainstalovat na obrázek šablony z multilicence média. Sledování informací z aplikace dodavatele nainstalovat licencí v prostředí Vzdálená plocha.
