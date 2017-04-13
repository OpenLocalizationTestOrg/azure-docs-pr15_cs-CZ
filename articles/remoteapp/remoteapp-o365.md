
<properties
    pageTitle="Používat Office s Azure RemoteApp | Microsoft Azure" 
    description="Zjistěte, jak Office a Azure RemoteApp spolupracují?"
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

# <a name="using-office-with-azure-remoteapp"></a>Používat Office s Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Máte dvě možnosti pro hostování aplikací Office v Azure RemoteApp: Office 365 ProPlus nebo Office 2013 Professional Plus zkušební verze.

**Přece víte, že máme nový lepší článků, které budou brzy bude k dispozici, nahradí se to? Podívejte se na [použití vaše předplatné Office 365 s Azure RemoteApp](remoteapp-officesubscription.md). Zahrnuje všechny potřebné informace, které potřebujete pro používání Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Můžete vytvořit kolekci RemoteApp pomocí Office 365 ProPlus šablony. Tato možnost umožňuje rozšířit RemoteApp služby Office 365. Máte existující plán předplatné a vaši uživatelé musí mít licenci pro službu Office 365 ProPlus, buď samostatně nebo prostřednictvím Office 365 služby plány.

RemoteApp podporuje aktivace Office 365 sdílené počítače. Pokud povolíte sdílené aktivace počítače a použijte [Nástroj pro nasazení Office](http://www.microsoft.com/download/details.aspx?id=36778) pro instalaci, Office 365 ProPlus nainstaluje bez aktivace. Když uživatel znaménka do v kolekci obsahující Office 365, Office kontroluje, pokud byla zajištěna uživatele pro Office 365 ProPlus. Pokud ano, se Office dočasně aktivuje Office 365 ProPlus - tuto aktivaci potrvají do této značky uživatelé mimo služby.

Použít aktivace sdílené počítače aplikace Office 365, musíte vytvořit [vlastní šablonu](remoteapp-create-custom-image.md) a instalace Office 365 ProPlus, [těchto](https://technet.microsoft.com/library/dn782858.aspx)pokynů.

Může spravovat licence uživatelů Office 365 na [Portálu pro správu Office 365](https://portal.office365.com/). Přečtěte si další informace o různých [plánech služby Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Zkušební verze Office 2013 Professional Plus
Během 30denní zkušební verzi RemoteApp můžete Office 2013 Professional Plus (zkušební) šablony můžete vytvořit kolekci RemoteApp. Můžou uživatelům přiřadit k této zkušební kolekci pomocí své práce účty služby Azure Active Directory nebo Microsoft. Vyžaduje se žádné další předplatného.

Toto je Skvělá volba zahájení tires a získat dobrý pocit pro Office v RemoteApp. Tato možnost je však určené k vyhodnocení a testování pouze. Kolekce RemoteApp vytvořené pomocí Office 2013 Professional Plus (zkušební) šablony nemůže být, které přešly do režimu výrobní a přestane být platná na konci zkušební období.

## <a name="switching-from-trial-to-production"></a>Přechod z zkušební verze na výrobní
Při spuštění 30denní bezplatnou zkušební verzi poznámky v části RemoteApp portálu se dozvíte, jak dlouho vám zbývá ve zkušební verzi než budete muset přechodu na placené účet. Můžete aktivovat svůj účet a přepněte do režimu výrobní pomocí odkazu v tuto poznámku.

Při aktivaci účtu, bude to mít vliv všechny kolekce RemoteApp ve vašem účtu.

- Kolekce, které se systémem Windows serveru 2012 R2 nebo Office 365 ProPlus obrázky šablon bude bezproblémový přechod na výroby. Všechna uživatelská data a nastavení, včetně probíhající relací, zůstane nedotčený.
- Pokud jste nahráli obrázky vlastní šablonu, kolekcí pomocí těchto obrázků se také bezproblémový přechod.
- Office 2013 Professional Plus (zkušební) šablony je určená pro vyhodnocení pouze. Kolekce s Tento obrázek šablony, nemůžete které přešly výroby. Převedou se ve stavu "zakázané".


Přechod do režimu výrobní není tak, že po uplynutí zkušebního období, vaše kolekce RemoteApp přestane. Nedělejte si starosti: nastavení a uživatelská data budou uloženy jiného 90 dní, abyste mohli dál aktivace služby a přepněte do režimu výrobní bez ztráty dat.
