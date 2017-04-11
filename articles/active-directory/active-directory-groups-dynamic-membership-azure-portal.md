
<properties
    pageTitle="Vytvářet složitější pravidla pro členství ve skupinách v náhledu Azure Active Directory pomocí atributy | Microsoft Azure"
    description="Jak vytvořit složitější pravidla pro dynamické skupina členství včetně podporovaná výraz pravidlo operátory a parametry."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Chcete-li vytvářet složitější pravidla pro členství ve skupinách v náhledu Azure Active Directory pomocí atributů

Portál Azure poskytuje možnost vytvářet složitější pravidla povolit složitější podle atributů dynamické členství ve náhled skupin služby Azure Active Directory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md) Tento článek obsahuje podrobnosti pravidlo atributy a syntaxi vytvoření upřesnit pravidla.

## <a name="to-create-the-advanced-rule"></a>Vytvoření rozšířeného pravidla

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

  ![Otevření Správa uživatelů](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Na zásuvné **Uživatelé a skupiny** vyberte **všechny skupiny**.

  ![Otevření zásuvné skupiny](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Na zásuvné **Uživatelé a skupiny – všechny skupiny** vyberte příkaz **Přidat** .

  ![Přidat novou skupinu](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Na zásuvné **skupiny** zadejte název a popis pro novou skupinu. Vyberte **Zadejte členství** **Dynamické uživatele** nebo **Dynamické zařízení**, podle toho, jestli chcete vytvořit pravidlo pro uživatele nebo zařízení a pak vyberte **Přidat dynamické dotazu**. Atributy používané pro zařízení pravidla najdete v článku [použití atributy k vytváření pravidel pro objekty zařízení](#using-attributes-to-create-rules-for-device-objects).

  ![Přidání pravidla dynamické členství](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Na zásuvné **dynamické členství pravidla** do pole **Přidat dynamické členství rozšířeného pravidla** zadejte pravidlo, stiskněte klávesu Enter a vyberte možnost **vytvořit** v dolní části zásuvné.

7. Výběrem možnosti **vytvořit** na zásuvné **skupiny** můžete vytvořit skupinu.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Stavba textu rozšířené pravidlo

Rozšířené pravidlo, které můžete vytvořit dynamické členství skupin je v podstatě binární výraz, který se skládá ze tří částí a výsledkem výsledek PRAVDA nebo NEPRAVDA. Jsou tři části:

- Levé parametrů
- Binární operátor
- Pravý konstanta

Dokončení rozšířené pravidlo vypadá nějak takto: (leftParameter binaryOperator "RightConstant"), kde levou a pravou kulatou závorku jsou potřeba pro celý binární výraz dvojité uvozovky jsou potřeba pro správné konstanty a syntaxe pro parametr levé je user.property. Rozšířené pravidlo mohou být tvořeny více binární výrazů oddělený- a, nebo a - ne logické operátory.

Následující příkladů správně vyrobeno rozšířeného pravidla:

- (user.department - eq "Prodej")- nebo (user.department - eq "Marketing")
- (user.department - eq "Prodej")- a - ne (user.jobTitle-obsahuje "SDE")

Úplný seznam podporovaných parametry a operátory pravidlo výrazů naleznete v tématu níže. Atributy používané pro zařízení pravidla najdete v článku [použití atributy k vytváření pravidel pro objekty zařízení](#using-attributes-to-create-rules-for-device-objects).

Celkovou délku textu rozšířeného pravidla nesmí překročit 2048 znaků.

> [AZURE.NOTE]
>Řetězec a regex operace jsou malá a velká písmena. Můžete taky provádět kontroly Null, používají $null konstanta, například user.department - eq $null.
Řetězec obsahující nabídky "by měly být ukončeny pomocí" znaků, například user.department - eq \`"Prodej".

## <a name="supported-expression-rule-operators"></a>Podporované výraz pravidlo operátory
V následující tabulce jsou uvedeny všechny operátory pravidlo podporované výrazů a jejich syntaxi se nemusí používat v těle rozšířeného pravidla:

| Operátor        | Syntaxe         |
|-----------------|----------------|
| Není rovno      | -ne            |
| Rovná se          | -eq            |
| Není začíná | -notStartsWith |
| Spouštění pracovních     | -startsWith    |
| Neobsahuje    | -notContains   |
| Obsahuje        | -obsahuje      |
| Neodpovídá       | -notMatch      |
| POZVYHLEDAT           | -podle         |


## <a name="query-error-remediation"></a>Chyba remediation dotazu
V následující tabulce jsou uvedeny potenciální chyby a jak je opravit, pokud se vyskytují

| Chyba analýzy dotazu     | Použití chyby       | Opravený použití             |
|-----------------------|-------------------|-----------------------------|
| Chyba: Atribut nejsou podporované.                                      | (user.invalidProperty - eq "Hodnotu")       | (user.department - eq "hodnotu")<br/>Vlastnosti by měly odpovídat jeden z [podporovaných seznam vlastností](#supported-properties).                          |
| Chyba: Operátor není podporována ve atribut.                       | (user.accountEnabled-obsahuje PRAVDA)                                                                               | (user.accountEnabled - eq PRAVDA)<br/>Vlastnost je typu boolean. Podporované operátory (-eq nebo - ne) na typ boolean ze seznamu.                                                                                                                                   |
| Chyba: Chyba kompilace dotazu.                                      | (user.department - eq "Prodej")- a (user.department - eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Prodej")- a (user.department - eq "Marketing")<br/>Logickým operátorem by měly odpovídat jeden ze seznamu podporovaných vlastnosti. (user.userPrincipalName-odpovídají ".*@domain.ext")or(user.userPrincipalName -odpovídají "@domain.ext$")Error v regulárních výrazů. |
| Chyba: Binární výraz není ve správném formátu.                     | (user.department – eq "Prodej") (user.department - eq "Prodej") (user.department eq "Prodej")                             | (user.accountEnabled - eq PRAVDA) – a (user.userPrincipalName-obsahuje"alias@domain")<br/>Dotaz obsahuje více chybám. Závorku není ve správném místě.                                                                                                                            |
| Chyba: Neznámé chybě při nastavení dynamické členství. | (user.accountEnabled - eq "True" a user.userPrincipalName-obsahuje"alias@domain")                               | (user.accountEnabled - eq PRAVDA) – a (user.userPrincipalName-obsahuje"alias@domain")<br/>Dotaz obsahuje více chybám. Závorku není ve správném místě.                                                                                                                            |

## <a name="supported-properties"></a>Podporované vlastnosti
Toto jsou všechny uživatele vlastnosti, které můžete použít v rozšířeného pravidla:

### <a name="properties-of-type-boolean"></a>Vlastnosti typu boolean

Povolené operátory

* -eq


* -ne


| Vlastnosti     | Povolené hodnoty  | Použití                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | true NEPRAVDA      | user.accountEnabled - eq PRAVDA)  |
| dirSyncEnabled | true false hodnota null | (user.dirSyncEnabled - eq PRAVDA) |

### <a name="properties-of-type-string"></a>Vlastnosti typu String

Povolené operátory

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -obsahuje


* -notContains


* -podle


* -notMatch

| Vlastnosti                 | Povolené hodnoty                                                                                        | Použití                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Město                       | Libovolná hodnota řetězec nebo $null                                                                           | (user.city - eq "hodnotu")                                   |
| země                    | Libovolná hodnota řetězec nebo $null                                                                            | (user.country - eq "hodnotu")                                |
| oddělení                 | Libovolná hodnota řetězec nebo $null                                                                          | (user.department - eq "hodnotu")                             |
| displayName                | Libovolnou hodnotu řetězce                                                                                 | (user.displayName - eq "hodnotu")                            |
| facsimileTelephoneNumber   | Libovolná hodnota řetězec nebo $null                                                                           | (user.facsimileTelephoneNumber - eq "hodnotu")               |
| givenName (jméno)                  | Libovolná hodnota řetězec nebo $null                                                                           | (user.givenName - eq "hodnotu")                              |
| Funkce                   | Libovolná hodnota řetězec nebo $null                                                                           | (user.jobTitle - eq "hodnotu")                               |
| pošta                       | Libovolná hodnota řetězec nebo $null (adresa SMTP uživatele)                                                  | (user.mail - eq "hodnotu")                                   |
| mailNickName               | Libovolnou hodnotu řetězce (mailový alias uživatele)                                                            | (user.mailNickName - eq "hodnotu")                           |
| mobilní                     | Libovolná hodnota řetězec nebo $null                                                                           | (user.mobile - eq "hodnotu")                                 |
| Objektu                   | Identifikátor GUID objektu uživatele                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Žádná DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Libovolná hodnota řetězec nebo $null                                                                            | (user.physicalDeliveryOfficeName - eq "hodnotu")             |
| PSČ                 | Libovolná hodnota řetězec nebo $null                                                                            | (user.postalCode - eq "hodnotu")                             |
| preferredLanguage          | Kód ISO 639-1                                                                                        | (user.preferredLanguage - eq "en US")                      |
| sipProxyAddress            | Libovolná hodnota řetězec nebo $null                                                                            | (user.sipProxyAddress - eq "hodnotu")                        |
| Stav                      | Libovolná hodnota řetězec nebo $null                                                                            | (user.state - eq "hodnotu")                                  |
| streetAddress              | Libovolná hodnota řetězec nebo $null                                                                            | (user.streetAddress - eq "hodnotu")                          |
| Surname (příjmení)                    | Libovolná hodnota řetězec nebo $null                                                                            | (user.surname - eq "hodnotu")                                |
| telephoneNumber            | Libovolná hodnota řetězec nebo $null                                                                            | (user.telephoneNumber - eq "hodnotu")                        |
| usageLocation              | Dva písmeny směrové číslo země                                                                           | (user.usageLocation - eq "USA")                             |
| userPrincipalName          | Libovolnou hodnotu řetězce                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | člen hosta $null                                                                                    | (user.userType - eq "Člena")                              |

### <a name="properties-of-type-string-collection"></a>Vlastnosti kolekce webů zadejte řetězec

Povolené operátory

* -obsahuje


* -notContains

| Poperties      | Povolené hodnoty                        | Použití                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Libovolnou hodnotu řetězce                      | (user.otherMails-obsahuje"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-obsahuje "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Rozšíření atributy a vlastní atributy
Rozšíření atributy a vlastní atributy podporuje pravidla dynamické členství.

Rozšíření atributy se synchronizují z na místní okno Server AD a prohlédněte formát "ExtensionAttributeX", kde X rovná se 1-15.
Příklad pravidlo, které používá atribut rozšíření

(user.extensionAttribute15 - eq "Marketing")

Vlastní atributy se synchronizují z místního nasazení systému Windows Server AD nebo z připojeného SaaS aplikace a na formát "user.extension_[GUID]\__ [atribut]", kde je jedinečný identifikátor v AAD aplikace, která vytvoří atribut v AAD [GUID] a [atribut] je název atribut, který byl vytvořen.
Příklad pravidlo, které používá vlastní atribut

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Název vlastní atributu najdete v adresáři pomocí dotazu uživatele atribut pomocí Průzkumníka grafu a vyhledávání pro název atributu.

## <a name="direct-reports-rule"></a>Pravidlo přímých podřízených
Teď můžete naplnit členové skupiny podle atributu správce uživatele.

**Konfigurace skupiny jako skupinu "Správce"**

1. Postupujte podle kroků 1 až 5 v [Rozšířené pravidlo vytvoříte](#to-create-the-advanced-rule)a vyberte **typ členství** **Dynamické**uživatele.

2. Na zásuvné **dynamické členství pravidla** zadejte pravidlo pomocí následující syntaxe:

    Přímé sestav pro *přímé podřízené {obectID_of_manager}*. Příklad platné pravidlo pro přímých podřízených

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    kde je "62e19b97-8b3d-4d4a-a106-4ce66896a863" objektu vedoucího. ID objektu najdete v Azure AD na **kartu profil** uživatele pro uživatele, kterému je správce.

3. Při uložení tohoto pravidla, všichni uživatelé, které vyhovují pravidlo spojí jako členy této skupiny. Může trvat několik minut skupiny původně plnit.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Pomocí atributů k vytváření pravidel pro objekty zařízení

Můžete taky vytvořit pravidlo, které slouží k výběru objektů zařízení pro členství ve skupině. Můžete použít následující atributy zařízení:

| Vlastnosti           | Povolené hodnoty                  | Použití                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| displayName          | libovolnou hodnotu řetězce                | (device.displayName - eq "Rudolf Iphone")                 |
| deviceOSType         | libovolnou hodnotu řetězce                | (device.deviceOSType - eq "IOS")                      |
| deviceOSVersion      | libovolnou hodnotu řetězce                | (zařízení. OSVersion - eq "9.1")                         |
| isDirSynced          | true false hodnota null                 | (device.isDirSynced - eq "true")                      |
| isManaged            | true false hodnota null                 | (device.isManaged – eq "false")                       |
| isCompliant          | true false hodnota null                 | (device.isCompliant – eq "true")                      |


## <a name="additional-information"></a>Další informace
Tyto články poskytují další informace o skupinách služby Azure Active Directory.

* [Zobrazit existující skupiny](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidání členů](active-directory-groups-create-azure-portal.md)
* [Správa nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Správa dynamických pravidel pro uživatele ve skupině](active-directory-groups-dynamic-membership-azure-portal.md)
