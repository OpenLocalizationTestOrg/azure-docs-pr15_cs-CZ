<properties
    pageTitle="Vytvoření do webových aplikací v prostředí aplikace služby"
    description="Naučte se vytvářet web apps a aplikace služeb plány v prostředí aplikace služby"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Vytvoření do webových aplikací v prostředí aplikace služby

## <a name="overview"></a>Základní informace

Tento kurz ukazuje, jak vytvořit aplikaci služby plány a webových aplikací web apps v [Prostředí aplikace služeb](app-service-app-service-environment-intro.md) (pomocného mechanismu řízení). 

> [AZURE.NOTE] Pokud chcete Naučte se vytvářet web appu, ale nemusíte udělat v prostředí aplikace služby, přečtěte si článek [vytvořit web appu .NET](web-sites-dotnet-get-started.md) nebo některou ze souvisejících výukové programy pro další jazyky a rámce.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz se předpokládá, že jste vytvořili prostředí aplikace služby. Pokud můžete, ještě neudělali, najdete v článku [Vytvoření prostředí aplikace služby](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

1. Na [Portálu Azure](https://portal.azure.com/)klikněte na **Nový > Web + mobilní > Web Appu**. 

    ![][1]

2. Vyberte předplatné.  

    Pokud máte víc předplatných mějte na paměti, že se vytvářet aplikace ve vašem prostředí aplikace služby, musíte použít stejné předplatné, které jste použili při vytvoření prostředí. 

3. Vyberte nebo vytvořte novou skupinu zdroje.

    *Skupiny zdrojů* vám umožní spravovat související Azure zdroje jako celek a jsou užitečné při vytváření pravidla pro *řízení přístupu na základě rolí* (RBAC) pro aplikace. Další informace najdete v tématu [Správa Azure zdroje][ResourceGroups]. 

4. Vyberte nebo vytvořte plán služeb aplikací.

    *Plány aplikaci služby* je spravováno sady webových aplikací web apps.  Obvykle vyberete ceny, ceny platí pro plán služeb aplikací a ne jednotlivé aplikace. V pomocného mechanismu řízení platíte výpočetních instancí přidělit pomocného mechanismu řízení místo mít společně s vaší ASP.  Zobrazit počet výskytů do webových aplikací, které škálování instancí aplikace služby plán a ovlivní všechny webové aplikace do tohoto plánu.  Některé funkce, jako jsou sloty webu nebo VNET integrace mít taky zajištěný omezení množství v plánu.  Další informace najdete v tématu [Přehled plány aplikaci služby Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Můžete určit plány aplikaci služby ve vaší pomocného mechanismu řízení pohledem na umístění, které je uvedeno v části název plánu.  

    ![][5]

    Pokud chcete použít některý z plánů aplikaci služby, které už existuje ve vašem prostředí služby aplikace, vyberte jej. Pokud chcete vytvořit nový plán služeb aplikací, najdete v následující části tohoto kurzu, [Vytvořit plán služeb aplikací v prostředí aplikace služby](#createplan).

5. Zadejte název pro web app a potom klikněte na **vytvořit**. 

    Pokud vaše pomocného mechanismu řízení využívá externí VIP je adresa URL aplikace v pomocného mechanismu řízení: [*název webu*]. [*Název prostředí aplikace služby*]. p.azurewebsites.net místo [*název webu*]. azurewebsites.net
    
    Pokud vaše pomocného mechanismu řízení používá interní VIP a adresu URL aplikace v je pomocného mechanismu řízení: [*název webu*]. [*subdoménu zadané při vytváření pomocného mechanismu řízení*]   
    Když vyberete svůj ASP při vytváření pomocného mechanismu řízení uvidíte subdoména aktualizovat pod **jménem**

## <a name="createplan"></a>Vytvoření plánu aplikace služby

Při vytváření plán služeb aplikací v prostředí aplikace služby své volby pracovníka jsou různé v pomocného mechanismu řízení jsou žádné sdílené dalšími lidmi.  Pracovníky, které je nutné použít jsou ty, které jste dostali přidělené pomocného mechanismu řízení správcem.  To znamená, že pokud chcete vytvořit nový plán, musíte mít víc pracovníků přidělit váš pracovní fond pomocného mechanismu řízení než celkový počet výskytů přes všechny vaše plány už v tomto fondu kolegy.  Pokud nemáte dost dalšími lidmi ve vaší pracovní fondu pomocného mechanismu řízení k vytvoření plánu, budete muset pracovat s pomocného mechanismu řízení správce, aby si je přidat.

Dalším rozdílem plánech aplikaci služby hostované pomocí prostředí aplikace služby je nedostatku ceny výběru.  Pokud máte v prostředí aplikace služby platíte pro využití zdrojů použitých v systému a nemusíte přidané poplatky pro plány v prostředí.  Za normálních okolností při vytváření plánu aplikace služby vyberte ceny plán, který určuje vašeho účtu.  Prostředí aplikace služby je v podstatě soukromé umístění, kde můžete vytvářet obsahu.  Platíte prostředí a nechcete hostovat svůj obsah.

Následující pokyny ukazují, jak vytvořit plán služeb aplikací při vytváření do webových aplikací způsobem popsaným v předchozí části kurzu.

1. Klikněte na **Vytvořit nový** plán výběru uživatelského rozhraní a zadejte název svého plánu jenom běžným způsobem mimo pomocného mechanismu řízení.

2. Vyberte pomocného mechanismu řízení, který chcete použít v pro výběr umístění.

    Protože prostředí aplikace služby v podstatě umístění soukromé nasazení se zobrazuje v části umístění. 

    ![][2]

    Po výběru pomocného mechanismu řízení v ovládacím prvku pro výběr umístění vytvoření plánu aplikace služby UI aktualizuje.  Umístění teď zobrazí název systému pomocného mechanismu řízení a oblasti se nachází v a ceny pro výběr plánu je nahrazen příkazem ovládacího prvku Výběr fondu pracovníka.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Výběr fond kolegy

Obvykle v aplikaci služby Azure nebo mimo ni prostředí aplikace služby existuje 3 velikosti výpočetním, které jsou dostupné s výběrem plán vyhrazené cena.  Podobným způsobem pro pomocného mechanismu řízení můžete definovat až 3 fondů zaměstnanců a zadejte požadovanou velikost výpočetních se používá pro tento pracovní fond.  Co to znamená, že pro klienty pomocného mechanismu řízení je to je místo toho výběru ceny plán s velikostí výpočetním pro váš plán služeb aplikací, vyberte takzvanou *pracovníka fondu*.  

Výběr fondu pracovníka uživatelského rozhraní zobrazuje výpočetním velikost použitá pro tento pracovní fond pod názvem.  Dostupné množství odkazuje na výpočet kolik instancí jsou k dispozici pro použití v tomto fondu.  Celkové fond, může mít víc instancí než toto číslo, ale tato hodnota odkazuje na jednoduše kolik nejsou používá.  V případě potřeby upravte prostředí aplikace služby přidat více výpočetního zdroje informací najdete v článku [Konfigurace prostředí aplikace služby](app-service-web-configure-an-app-service-environment.md).

![][4]

V tomto příkladu se zobrazí pouze dva pracovní fondů k dispozici. Je proto, že správce pomocného mechanismu řízení pouze přidělit hosts do tyto dva pracovní skupiny.  Třetí by neprojevila se po VMs přidělit do ní.  

## <a name="after-web-app-creation"></a>Po vytvoření webové aplikace

Ke spuštění webových aplikací web apps a Správa aplikací služby plán pomocného mechanismu řízení, které je třeba vzít v úvahu několik v úvahu.  

Uvedených dříve, vlastník pomocného mechanismu řízení je zodpovědný za velikost systému a jako výsledek ty představují taky zodpovědný za zajistit, aby byl dostatečnou kapacitu k hostování požadované plány aplikaci služby. Pokud není k dispozici zaměstnanců, nebudete moct vytvářet plánu aplikace služby.  Toto je také hodnotu true škálování webovou aplikaci.  Pokud potřebujete další instance je třeba získat správce aplikace služby prostředí přidat více zaměstnanců.

Po vytvoření web app a plán služeb aplikace je vhodné zobrazit.  Pomocného mechanismu řízení je vždy musí mít nejméně 2 výskyty plánu aplikace služby stanovit odolnost proti chybám aplikace.  Změna velikosti plán služeb aplikací v pomocného mechanismu řízení je stejná jako normálně až plán aplikaci služby UI.  Další informace o měřítko, [jak změnit velikost do webových aplikací v prostředí aplikace služby](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
