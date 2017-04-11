<properties
    pageTitle="Hybridní Identity: Integrace s adresářem nástroje porovnání | Microsoft Azure"
    description="Toto je stránka obsahuje komplexní tabulku, která porovnává různé nástroje pro integraci adresářů, které můžete použít pro integraci adresářů."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Integrace služby directory Identity hybridní nástroje porovnání

Nástroje pro integraci adresářů let se pěstují a evolved.  Tento dokument je pomáhají zajistit konsolidované zobrazení těchto nástrojích a porovnání funkcí, které jsou k dispozici v každém.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect zahrne součástí a funkcí dříve vydávat Dirsync a AAD Sync. Tyto nástroje jsou už vydána jednotlivě a všechny budoucí vylepšení bude součástí aktualizace Azure AD Connect, abyste pořád věděli, kde získat funkci nejnovější aktualizaci.
>
>Nástroj DirSync a Azure AD Sync jsou změněny. Další informace najdete v [tomto poli](active-directory-aadconnect-dirsync-deprecated.md).


Použijte tento klíč pro každou tabulku.

● = Teď k dispozici  
FR = budoucí vydání  
PP = Public Preview  

## <a name="on-premises-to-cloud-synchronization"></a>Místní do cloudu synchronizace

| Funkce  | Připojení služby Azure Active Directory  | Synchronizační služba služby Azure Active Directory (AAD Sync) | Nástroj pro synchronizaci služby Azure Active Directory (DirSync)| Správce identit Forefront 2010 R2 (FIM) |Správce Microsoft identit 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Připojení k jedné místního AD struktury | ● | ● | ● | ● |● |
| Připojení k více místního AD strukturami |●  | ● |  | ● |● |
| Připojení k více místní Exchange organizacích | ● |  |  |  | |
| Připojení k adresář LDAP jediný místního | FR |  |  | ● |● |
| Připojení k více místního adresáře LDAP |FR  |  |  | ● |● |
| Připojení k místním prostředím AD a místního adresáře LDAP |FR  |  |  | ● |● |
| Připojení k vlastní systémy (tedy SQL Oracle, MySQL, atd.) | FR |  |  | ● |● |
| Synchronizace zákazníka definované atributy (rozšíření directory) | ● |  |  |  |  |
|Připojení k místní HR (tedy SAP, Oracle eBusiness PeopleSoft)| FR |  |  | ● |● |
|Podporuje FIM synchronizace pravidla a konektory: pro vytváření místní systémy.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Cloudu pro místní synchronizace

| Funkce  | Připojení služby Azure Active Directory  | Synchronizační služba služby Azure Active Directory | Nástroj pro synchronizaci služby Azure Active Directory (DirSync) | Správce identit Forefront 2010 R2 (FIM) |Správce Microsoft identit 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Zpětného zápisu zařízení | ● |  | ● |  ||
| Atribut zpětného zápisu (pro hybridní nasazení Exchange) | ● | ● | ● | ● |● |
| Zpětným uživatelé a skupiny objektů |  ●|  | |  ||
| Zpětným hesel (z Změna hesla a obnovení samoobslužných funkcí hesla (SSPR)) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Podpora funkcí ověřování

| Funkce  | Připojení služby Azure Active Directory | Synchronizační služba služby Azure Active Directory | Nástroj pro synchronizaci služby Azure Active Directory (DirSync) | Správce identit Forefront 2010 R2 (FIM) |Správce Microsoft identit 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Synchronizace hesel pro jeden místního AD struktury | ● | ● | ● |  ||
| Synchronizace hesel pro více místního AD strukturami |  ●| ● |  |  ||
| Jednotné přihlašování federace | ● | ● | ● | ● |● |
| Zpětným hesel (z SSPR a heslo změnit) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Instalace a nastavení

| Funkce  | Připojení služby Azure Active Directory  | Synchronizační služba služby Azure Active Directory | Nástroj pro synchronizaci služby Azure Active Directory (DirSync) | Správce Microsoft identit 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Podpora instalace u řadiče domény | ● | ● | ● |  |
| Podporuje instalaci SQL Express | ● | ● | ● |  |
| Snadná aktualizace z DirSync |● |  |  |  |
| Lokalizace činnosti koncového uživatele správce systému Windows Server jazycích k dispozici | ● | ● | ● |  |
|Lokalizace koncový uživatel činnosti koncového uživatele systému Windows Server jazycích k dispozici| |  |  |● |
| Podpora pro Windows Server 2008 a Windows Server 2008 R2 | ● pro synchronizaci, ne pro federaci| ● | ●  | ● |
| Podpora pro Windows Server 2012 a Windows Server 2012 R2 | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filtrování a konfigurace

Funkce  | Připojení služby Azure Active Directory | Synchronizační služba služby Azure Active Directory | Nástroj pro synchronizaci služby Azure Active Directory (DirSync) | Správce identit Forefront 2010 R2 (FIM)| Správce Microsoft identit 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filtrování na domény a organizačních jednotek | ● | ● | ● | ●  | ●
Filtrování hodnot objektu atributu | ● | ● | ● | ●| ●
Povolit, minimální nastavením atributů, které mají být synchronizovány (MinSync) | ● | ● |  ||
Povolit jiné službě šablony můžou být použity pro toky atribut |●  | ● |  ||
Povolit odebráním vyplývající z AD Azure AD atributy | ● | ● |  |  |
Povolit upřesňující nastavení pro atribut toků | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
