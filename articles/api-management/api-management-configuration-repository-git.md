<properties 
    pageTitle="Uložení a konfiguraci konfigurace služby Správa rozhraní API pomocí libovolná" 
    description="Zjistěte, jak můžete uložit a nakonfigurovat konfigurace služby Správa rozhraní API pomocí libovolná." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Uložení a konfiguraci konfigurace služby Správa rozhraní API pomocí libovolná

>[AZURE.IMPORTANT] Libovolná konfigurace správy rozhraní API momentálně v náhledu. Funkčně dokončení, ale je v náhledu, protože jsme aktivně hledají svůj názor týkající se této funkce. Je možné, že můžeme jazycích změnit v odpovědi zákazníků, proto doporučujeme není v závislosti na tuto funkci pro použití v provozním prostředí. Pokud máte svůj názor nebo dotazy, dejte nám prosím vědět na `apimgmt@microsoft.com`.

Jednotlivé instance služby správy rozhraní API udržuje konfigurační databáze, který obsahuje informace o konfiguraci a metadata pro instance služby. Změna nastavení portálu Publisheru, pomocí rutiny prostředí PowerShell nebo voláním rozhraní REST API můžete instanci služby provedeny změny. Kromě těchto postupů můžete spravovat konfigurace instanci služby pomocí libovolná, jako například povolení scénáře správy služby:

-   Konfigurace správy verzí – stažení a uložení různých verzí konfigurace služby
-   Hromadné změny konfigurace – změňte více částí konfigurace služby ve vaší místní úložiště a integrace změny na server s jedinou operaci
-   Známé toolchain libovolná či pracovního postupu – použít libovolná nástrojů a pracovních postupů, které již máte zkušenosti s

Na následujícím obrázku vidíte přehled o různých způsobech konfigurace instanci služby správy rozhraní API.

![Konfigurace libovolná][api-management-git-configure]

Při změnách ke službě pomocí portálu Publisheru, rutiny prostředí PowerShell nebo rozhraní REST API, spravujete vy vaše služby konfigurační databáze pomocí `https://{name}.management.azure-api.net` koncový bod, jak je znázorněno na pravé straně diagramu. V levé části diagram ukazuje, jak můžete spravovat pomocí libovolná konfigurace služby a úložiště libovolná službě c:\ `https://{name}.scm.azure-api.net`.

Podle těchto kroků poskytují přehled správy vaší instanci služby správy rozhraní API pomocí libovolná.

1.  Povolení přístupu ke libovolná ve službě
2.  Uložení databáze konfigurace služby do libovolná úložiště
3.  Klonovat repo libovolná do místního počítače
4.  Přetáhněte nejnovější repo dolů na místním počítači a změny potvrdit a nabízených zpět do svého repo
5.  Nasazení změn ze svého repo do konfigurační databáze služby

Tento článek popisuje, jak povolit a používat libovolná ke správě konfigurace služby a obsahuje odkaz souborů a složek v úložišti libovolná.

## <a name="to-enable-git-access"></a>Chcete-li povolit přístup libovolná

Můžete rychle zobrazit stav konfiguraci libovolná zobrazením libovolná ikonu v pravém horním rohu portálu aplikace publisher. V tomto příkladu není zatím povolené libovolná přístup.

![Libovolná stavu][api-management-git-icon-enable]

Prohlížení a nakonfiguroval nastavení konfigurace libovolná, můžete klikněte na ikonu libovolná nebo klikněte na nabídku **zabezpečení** a přejděte na kartu **Konfigurace úložiště** .

![Povolení libovolná][api-management-enable-git]

Povolení přístupu libovolná, zaškrtněte políčko **Povolit libovolná přístup** .

Po okamžiku uložení změn a zobrazí se potvrzovací zpráva. Všimněte si, že libovolná ikonu změnil na barvu, kterou chcete označit, že je povolený přístup libovolná a stavová zpráva teď označuje, že, která obsahuje neuložené změny úložiště. Důvodem je Správa rozhraní API služeb konfigurační databáze dosud nebyl uložen úložiště.

![Libovolná povolena][api-management-git-enabled]

>[AZURE.IMPORTANT] Všechny tajemství, které nejsou definované a vlastnosti budou uložené v úložišti, zůstanou v jeho historie dokud zakázat a znovu povolit libovolná přístup. Vlastnosti poskytnutí zabezpečeného místo pro správu konstantní hodnoty řetězce, včetně tajemství ve všech konfigurace rozhraní API a zásady, abyste pokaždé nemuseli uložit přímo do zásad příkazy. Další informace najdete v tématu [použití vlastností v zásady správy rozhraní API Azure](api-management-howto-properties.md).

Informace o povolení nebo zakázání libovolná připojení přes rozhraní REST API najdete v článku [Povolení nebo zakázání libovolná připojení přes rozhraní REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Do úložiště libovolná konfiguraci služby

Cílem prvního kroku před klonováním úložiště je uložit aktuální stav konfiguraci služby do úložiště. Klikněte na tlačítko **Uložit konfiguraci do úložiště**.

![Uložení konfigurace][api-management-save-configuration]

Proveďte požadované změny na obrazovce pro potvrzení a klikněte na **Ok** uložte.

![Uložení konfigurace][api-management-save-configuration-confirm]

Za několik okamžiků konfigurace uloží a stav konfigurace úložiště je zobrazen, včetně data a času poslední změny konfigurace a poslední synchronizace mezi konfiguraci služby a úložiště.

![Konfigurace stavu][api-management-configuration-status]

Po konfiguraci se uloží do úložiště, můžete klonovat.

Další informace o provádění operaci pomocí rozhraní REST API najdete v článku [Potvrdit konfigurace snímek pomocí rozhraní REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Klonovat úložiště do místního počítače

Klonovat úložišti, potřebujete adresu URL vašeho úložiště, uživatelské jméno a heslo. Uživatelské jméno a adresu URL se zobrazují v horní části karty **Konfigurace úložiště** .

![Libovolná klonovat][api-management-configuration-git-clone]

V dolní části karty **Konfigurace úložiště** je generováno heslo.

![Generování hesla][api-management-generate-password]

Generovat hesla, nejdřív ověřte, zda **vypršení** platnosti požadované datum a čas a potom klikněte na **Generovat tokenu**.

![Heslo][api-management-password]

>[AZURE.IMPORTANT] Poznamenejte si toto heslo. Jakmile opustit tuto stránku heslo se znovu nezobrazí.

Následující příklady nástrojem libovolná flám z [Libovolná pro Windows](http://www.git-scm.com/downloads) , ale můžete použít libovolný libovolná nástroj, který máte zkušenosti s.

Otevřete nástroj libovolná do požadované složky a spusťte tento příkaz klonovat úložiště libovolná do místního počítače pomocí příkazu poskytovanou portálu aplikace publisher.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Zadejte uživatelské jméno a heslo po zobrazení výzvy.

Pokud se zobrazí všechny chyby, pokuste se změnit svůj `git clone` příkaz zahrnout uživatelské jméno a heslo, jak je vidět v následujícím příkladu.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Pokud to obsahuje chybu, zkuste kódování heslo část příkazu adres URL. Jeden rychlý způsob, jak to udělat je otevřít Visual Studiu a tento příkaz **Příkazového podokna**. **Příkazové podokno**otevřete neotevírejte žádné řešení nebo projekt ve Visual Studiu (nebo vytvořit novou aplikaci prázdné konzoly) a zvolte **Windows** **Okamžité** z nabídky **ladění** .

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Vytvoření příkazu Libovolná pomocí zakódovaný hesla spolu s umístěním uživatelské jméno a úložiště.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Po klonovat úložiště můžete zobrazit a pracovat s ním v systému souborů místní. Další informace najdete v článku Principy [souborů a složek strukturu místní úložiště libovolná](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Pro informování místní úložiště pomocí aktuální instance konfiguraci služby

Pokud provedete změny instanci aplikace rozhraní API Správa služby na portálu publisher nebo pomocí rozhraní REST API, je nutné uložit tyto změny úložiště než místní úložiště můžete aktualizovat nejnovějšími změnami. K tomuto účelu na kartě **Konfigurace úložiště** na portálu Publisheru klikněte na **Uložit konfiguraci do úložiště** a vydá tento příkaz v místním úložišti.

    git pull

Před spuštěním `git pull` zajistit, aby se ve složce pro místní úložiště. Pokud jste právě dokončili `git clone` příkaz, musíte změnit v adresáři vaší repo pomocí příkazu jako následující a pak.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Jestli chcete použít změny z místní repo repo serveru

Pokud chcete použít změny z místní úložiště pro server úložiště, musí potvrdit změny a pak nabízená do úložiště serveru. Uložte provedené změny, otevřete nástroj command libovolná, přepněte do adresáře vaší místní úložiště a následující příkazy.

    git add --all
    git commit -m "Description of your changes"

Posunout všechny potvrzení na server, spusťte tento příkaz.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Nasazení změny konfigurace služby Správa rozhraní API instanci služby

Jakmile místní změny potvrzeného a posune úložišti server, můžete nasadit instanci služby správy rozhraní API.

![Nasazení][api-management-configuration-deploy]

Další informace o provádění operaci pomocí rozhraní REST API najdete v článku [nasazení libovolná změny konfigurační databáze pomocí rozhraní REST API](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Odkaz na místní úložiště libovolná struktury souborů a složek

Souborů a složek v úložišti místní libovolná obsahují informace o konfiguraci o instance služby.

| Položky                       | Popis                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| rozhraní api kteří nemají manažerskou kořenové složky | Obsahuje nejvyšší úrovně konfigurace instance služby                                  |
| rozhraní API složky                | Obsahuje postup nastavení rozhraní API v instanci služby                            |
| složku skupiny              | Obsahuje postup nastavení skupin v instanci služby                          |
| složku zásady            | Obsahuje zásady v instanci služby                                              |
| portalStyles složky        | Obsahuje konfiguraci vývojář vlastního nastavení portálu v instanci služby |
| Složka produkty            | Konfigurace produktů v instanci služby obsahuje                        |
| Složka šablony           | Obsahuje postup nastavení e-mailové šablony v instanci služby                 |

Všechny složky může obsahovat jeden nebo víc souborů a v některých případech jedné nebo více složek, třeba do složky pro každý rozhraní API, výrobek nebo skupiny. Soubory v každé ze složek jsou specifické pro typ entita popsaná název složky.

| Typ souboru | Účel                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Konfigurace informací o příslušné entity                  |
| ve formátu HTML      | Popisy entity často zobrazené v portálu pro vývojáře |
| XML       | Zásady příkazy                                                      |
| šablony stylů CSS       | Šablony stylů pro vývojáře portálu přizpůsobení                        |

Tyto soubory lze vytvořili, odstranit, upravit a spravovat na místní systém souborů a změny nasazeny zpět instance služby správy rozhraní API.

>[AZURE.NOTE] Následující entity se nenacházejí v úložišti libovolná a se nedají konfigurovat pomocí libovolná.
>
>-    Uživatelé
>-    Předplatná
>-    Vlastnosti
>-    Karta Vývojář v portálu entity než styly

### <a name="root-api-management-folder"></a>Rozhraní api kteří nemají manažerskou kořenové složky

Kořenovém `api-management` složka obsahuje `configuration.json` soubor, který obsahuje nejvyšší úrovně informace o instanci služby v v tomto formátu.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

První čtyři nastavení (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, a `UserRegistrationTermsConsentRequired`) mapy na následující nastavení na kartě **identit** v části **zabezpečení** .

| Nastavení identity                     | Map                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Přesměrovat anonymním uživatelům přihlašovací stránka** zaškrtávací políčko |
| UserRegistrationTerms                | **Podmínky použití na uživatele registrace** textové pole               |
| UserRegistrationTermsEnabled         | **Zobrazit podmínky použití na přihlašovací stránce** zaškrtávací políčko         |
| UserRegistrationTermsConsentRequired | **Vyžadovat souhlas** zaškrtávací políčko                          |

![Nastavení identity][api-management-identity-settings]

Další čtyři nastavení (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, a `DelegationValidationKey`) mapy na následující nastavení na kartě **delegování** v části **zabezpečení** .

| Delegování           | Map                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Zaškrtávací políčko **delegát přihlašovací & registrace**    |
| DelegationUrl                | Textové pole **Adresa URL koncového bodu delegování**        |
| DelegatedSubscriptionEnabled | Zaškrtávací políčko **předplatné s kódem product delegáta** |
| DelegationValidationKey      | Textové pole **Klíč ověření delegáta**        |

![Nastavení delegování][api-management-delegation-settings]

Nastavení konečného `$ref-policy`, namapuje na souboru globální zásad příkazy pro instanci služby.

### <a name="apis-folder"></a>rozhraní API složky

`apis` Složka obsahuje složku pro každou rozhraní API v instanci služby, která obsahuje následující položky.

-   `apis\<api name>\configuration.json`– To je konfigurace pro rozhraní API a obsahuje informace o adresy URL služby back-end a operací. Toto je stejné informace, které by být vrácena kdybyste volání [získat konkrétní rozhraní API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) pomocí `export=true` v `application/json` formát.
-   `apis\<api name>\api.description.html`– To je popis rozhraní API a odpovídat `description` vlastnosti [rozhraní API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`– Tato složka obsahuje `<operation name>.description.html` soubory, které namapovat operace rozhraní API systému. Každý soubor obsahuje popis jednotlivá v rozhraní API, která odpovídá `description` vlastnosti [entity operace](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) v rozhraní REST API.

### <a name="groups-folder"></a>složku skupiny

`groups` Složka obsahuje složku pro každou skupinu definované v instanci služby.

-   `groups\<group name>\configuration.json`– Jedná se o konfiguraci skupiny. Toto je stejné informace, které by být vrácena kdyby volání operaci [získat určité skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) .
-   `groups\<group name>\description.html`– To je popis skupiny a odpovídat `description` vlastnost [skupiny entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>složku zásady

`policies` Složka obsahuje příkazy zásad pro instanci služby.

-   `policies\global.xml`-obsahuje zásady definována v globální obor pro instanci služby.
-   `policies\apis\<api name>\`– Pokud máte všechny zásady definována v rozsahu rozhraní API, jsou obsaženy v této složce.
-   `policies\apis\<api name>\<operation name>\`Složka – Pokud máte všechny zásady definována v operaci obor, jsou obsaženy v této složce v `<operation name>.xml` soubory, které mapování na příkazy zásad pro každou akci.
-   `policies\products\`– Pokud máte všechny zásady definována v rozsah produktu, jsou obsaženy v této složce, která obsahuje `<product name>.xml` soubory, které mapování na příkazy zásad pro každý produkt.

### <a name="portalstyles-folder"></a>portalStyles složky

`portalStyles` Složka obsahuje konfigurace a styl stylů pro vývojáře portálu vlastní nastavení instance služby.

-   `portalStyles\configuration.json`-obsahuje názvy stylů použitých v portálu pro vývojáře
-   `portalStyles\<style name>.css`-Každý `<style name>.css` soubor obsahuje styly portálu pro vývojáře (`Preview.css` a `Production.css` ve výchozím nastavení).

### <a name="products-folder"></a>Složka produkty

`products` Složka obsahuje složku pro každý produkt v instanci služby definované.

-   `products\<product name>\configuration.json`– Jedná se o konfiguraci produktu. Toto je stejné informace, které by být vrácena, pokud by bylo potřeba zavolat operaci [získat určitý produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) .
-   `products\<product name>\product.description.html`– To je popis produktu a odpovídat `description` vlastnost [entity produktu](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) v rozhraní REST API.

### <a name="templates"></a>šablony

`templates` Složka obsahuje konfiguraci [e-mailové šablony](api-management-howto-configure-notifications.md) instance služby.

-   `<template name>\configuration.json`– Jedná se o konfiguraci e-mailové šablony.
-   `<template name>\body.html`– To je obsah šablony e-mailu.

## <a name="next-steps"></a>Další kroky

Informace o jiných způsobech ke správě instance služby najdete v tématu:

-   Správa instanci aplikace služby pomocí následující rutiny prostředí PowerShell
    -   [Nasazení služby reference pro rutiny prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Správa služby reference pro rutiny prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Správa instance služby na portálu aplikace publisher
    -   [Správa první rozhraní API](api-management-get-started.md)
-   Správa instanci aplikace služby pomocí rozhraní REST API
    -   [Odkaz rozhraní API správy REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Podívejte se na Videoukázku přehledu

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




