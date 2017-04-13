<properties
   pageTitle="Publikování aplikací jednotlivým uživatelům Azure RemoteApp sbírky (verze Preview) | Microsoft Azure"
   description="Zjistěte, jak publikovat aplikací pro jednotlivé uživatele, ne v závislosti na skupin v Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Publikování aplikací jednotlivým uživatelům Azure RemoteApp sbírky (verze Preview)

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Tento článek vysvětluje, jak publikovat aplikace jednotliví uživatelé v kolekci Azure RemoteApp. Toto je nové funkce v Azure RemoteApp aktuálně v "soukromé náhled" a k dispozici pouze pro vybrané jednotlivá k vyzkoušení.

Původně povolených Azure RemoteApp pouze jeden způsob "publikování" aplikací: Správce by publikování aplikací z obrázku a by být viditelné pro všechny uživatele v kolekci.

Běžné situace je zahrnout mnoho aplikací do jednoho obrázku a nasazení jednu kolekci ke snížení nákladů řízení. Oftentimes ne všechny aplikace jsou důležité pro všechny uživatele – Správci raději publikujte aplikace jednotlivým uživatelům, aby se nezobrazuje nepotřebných aplikací ve své aplikaci kanálu.

To je teď možné v Azure RemoteApp – aktuálně jako funkce omezené náhled. Tady je stručný souhrn nových funkcí:

1. Kolekce můžete nastavit do jedné ze dvou režimů:
 
  - původní "kolekce režimu", kde můžete zobrazit všechny uživatele v kolekci všechny publikované aplikace. Toto je výchozí režim.
  - nové "režim aplikace", kde uživatelé vidí jenom aplikace, které byly explicitně přiřazená

2. V okamžiku, kdy režim aplikace lze povolit pouze pomocí rutin prostředí RemoteApp PowerShell Azure.

  - Když nastaven do režimu aplikace, přiřazení uživatelských v kolekci nelze spravován během portálu Azure. Přiřazení uživatelských musí spravován během rutiny prostředí PowerShell.

3. Uživatelům se zobrazí pouze aplikací publikovaných na ně. Však je dál možné pro uživatele ke spuštění dalších aplikací dostupné v obraze přístupem přímo v operačním systému.
  - Tato funkce Microsoft neposkytuje žádnou zabezpečené uzamčení aplikací. pouze omezuje viditelnost v aplikaci kanálu.
  - Pokud potřebujete Izolovat uživatele z aplikací, musíte pomocí samostatných kolekcí pro danou.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Jak získat rutiny prostředí RemoteApp PowerShell Azure

Pokud chcete vyzkoušet nové funkce náhledu, musíte pomocí rutin prostředí PowerShell Azure. Momentálně není možné povolit nové režim publikování aplikace pomocí portálu pro správu Azure.

Nejdřív zkontrolujte, jestli že máte nainstalovaný [modul Azure Powershellu](../powershell-install-configure.md) .

Potom spusťte konzolu Powershellu v režimu správce a spusťte následující rutinu:

        Add-AzureAccount

Zobrazí výzvu k zadání Azure uživatelského jména a hesla. Když se přihlásíte, budete moct spustí Azure RemoteApp rutiny hledání předplatné Azure.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Jak se dá zjistit, jaký režim kolekce je v

Spusťte následující rutinu:

        Get-AzureRemoteAppCollection <collectionName>

![Zaškrtněte políčko kolekce režim](./media/remoteapp-perapp/araacllelvel.png)

Vlastnost AclLevel může mít tyto hodnoty:

- Kolekce: publikování původní režim. Všichni uživatelé najdete že všechny publikované aplikace.
- Aplikace: publikování nový režim. Uživatelé vidí jenom aplikace publikovaných na ně.

## <a name="how-to-switch-to-application-publishing-mode"></a>Jak se dá přepnout do režimu aplikace publikování

Spusťte následující rutinu:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Zachová aplikace publikování stav: původně všechny původní publikované aplikace se zobrazí všem uživatelům.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Jak získat seznam uživatelů, kteří vidí dané aplikace

Spusťte následující rutinu:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Seznam všech uživatelů, kteří vidí aplikace.

Poznámka: Zobrazí se aplikace aliases (s názvem "aplikace aliasu" v syntaxi výše) spuštěním Get AzureRemoteAppProgram - Název_kolekce <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Jak přiřadit aplikace pro uživatele

Spusťte následující rutinu:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Uživatel se nyní zobrazí aplikace v klientovi Azure RemoteApp a budou moct připojit k němu.

## <a name="how-to-remove-an-application-from-a-user"></a>Jak odebrat aplikaci od uživatele

Spusťte následující rutinu:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Poskytování zpětné vazby
Názory a návrhy týkající se tato funkce náhledu vážíme. Vyplňte [průzkum](http://www.instant.ly/s/FDdrb) a dejte nám vědět, co si myslíte.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Nedošlo je moct vyzkoušet funkce Zobrazit?
Pokud jste to ještě se zúčastnili náhled ještě, stiskněte klávesovou zkratku tento [průzkum](http://www.instant.ly/s/AY83p) požádat o přístup.
