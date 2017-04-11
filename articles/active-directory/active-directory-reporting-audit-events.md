<properties
   pageTitle="Události sestavy auditování Azure Active Directory | Microsoft Azure"
   description="Auditovány události, které jsou dostupné pro zobrazení a stahování z Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Události sestavy auditování Azure Active Directory

*Tato dokumentace je součástí Příručky pro [Azure Active Directory vykazování] (aktivní adresáře vykazování guide.md).*

Sestavy auditování Azure Active Directory pomáhá zákazníkům identifikovat privilegovaných akcí, které byly v Azure Active Directory. Privilegovaných akce zahrnutí zvýšení změny (například vytváření role nebo resetování hesla), změna konfigurace zásad (například zásady pro hesla) a změny konfigurace adresáře (například změny nastavení federace domén). Sestavy protokolu auditování poskytují pro název události, actor, kdo provádí akce cílový prostředek ovlivňují změnit a datum a čas (UTC). Zákazníci se načtení seznamu události auditování pro Azure Active Directory pomocí [Portálu Azure](https://portal.azure.com/), jak je uvedeno v [zobrazení protokolů auditování](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Seznam událostí sestavy auditování
<!--- audit event descriptions should be in the past tense --->

Události                               | Popis události
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Uživatelské události**                      |
Přidání uživatele                             | Přidat uživatele do adresáře.
Odstranit uživatele                          | Odstranit uživatele z adresáře.
Nastavení vlastností licence               | Nastavení vlastnosti licence pro uživatele v adresáři.
Obnovení uživatelského hesla                  | Resetování hesla pro uživatele v adresáři.
Změna hesla uživatelského                 | Změnit heslo pro uživatele v adresáři.
Změna uživatelskou licenci                  | Ke změně licence přiřazené k uživatele v adresáři. Pokud chcete zobrazit, které licence aktualizovaly, podívejte se na vlastnostech [uživatele aktualizace](#update-user-attributes)
Aktualizovat uživatele                          | Aktualizovat uživatele v adresáři. Atributy, které mohou být aktualizovány [najdete níže](#update-user-attributes) .
Nastavení vynutit změnit heslo uživatele       | Nastavte vlastnost, který vynucuje uživatelům měnit své heslo při přihlášení.
Přihlašovací údaje uživatele pro aktualizaci        | Uživatel změnil heslo  
**Událostí skupiny**                     |
Přidání skupiny                            | Vytvoření skupiny v adresáři.
Aktualizace skupiny                         | Aktualizace skupiny v adresáři. Byly aktualizovány jaké vlastnosti skupiny zobrazíte najdete pod odkazy v části [Auditovány vlastnosti skupiny](#update-group-attributes)
Odstranění skupiny                         | Odstranění skupiny z adresáře.
Přidání člena do skupiny            | Přidání člena do skupiny v adresáři.
Odebrání člena ze skupiny         | Odebrání člena ze skupiny v adresáři.
CreateGroupSettings              | Nastavení vytvořené skupiny
UpdateGroupSettings          | Aktualizace nastavení skupiny. Zobrazit nastavení skupiny, které byly aktualizovány, najdete pod odkazy v části [Auditovány vlastnosti skupiny](#update-group-attributes)
DeleteGroupSettings            | Nastavení odstraněné skupiny
SetGroupLicense                  | Nastavení skupiny licenci.
SetGroupManagedBy              | Nastavení skupiny spravovat uživatelem
AddGroupMember               | Přidané člena do skupiny
RemoveGroupMember            | Odebrání člena ze skupiny
AddGroupOwner                | Přidané vlastník skupiny
RemoveGroupOwner                 | Odebrané vlastníka ze skupiny
**Události aplikace**               |
Přidání služby jistinu                | Hlavní název služby přidat do adresáře.
Odebrání služby jistinu             | Hlavní název služby odebrány z adresáře.
Přidání služby základní přihlašovací údaje    | Přidané přihlašovací údaje pro hlavní název služby.
Odebrání služby základní přihlašovací údaje | Odebrané pověření od služby základní.
Zadání výjimky delegování                 | Vytvoření [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři.
Položka nastavení delegování                 | Aktualizace [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři.
Odstranit položku delegování              | Odstraní [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři.
**Role události**                      |
Přidání člena role role              | Přidat uživatele k roli adresář.
Odebrání rolí členů z Role         | Odebrat uživatele z role adresář.
Nastavení společnosti kontaktní informace      | Nastavení předvoleb kontaktu úrovni společnosti. Jedná se o e-mailové adresy pro marketingové, jakož i technické při upozorňování na Microsoft Online Services.
Zadání výjimky delegování                 | Vytvoření [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) v adresáři.
Položka nastavení delegování                 | Aktualizace [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) v adresáři.
Odstranit položku delegování              | Odstraní [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) v adresáři.
AddSevicePrincipalOwner          | Přidané vlastníkovi jistinu služby.
RemoveSevicePrincipalOwner   | Vlastník odebrali jistinu služby.
AddApplication  | Přidáte aplikaci.
UpdateApplication | Aktualizace aplikace. Zobrazit nastavení, které aplikace byly aktualizovány, najdete pod odkazy v následující části [Auditovány vlastnosti aplikace](#update-application-attributes)
DeleteApplication | Odstranění aplikace.
RestoreApplication|Obnovení aplikace.
AddApplicationOwner   |     Přidání vlastník aplikace.
RemoveApplicationOwner    |     Vlastníky odeberte z aplikace.
**Role události**                         |
Přidání rolí členů role                 | Přidat uživatele k roli adresář.
Odebrání rolí členů z Role            | Odebrání uživatele z role adresář.
AddRoleDefinition               | Definice přidané role.
UpdateRoleDefinition                | Definice aktualizované role. Zobrazit nastavení role, které byly aktualizovány, najdete pod odkazy v části [Role definice vlastnosti auditovány](#update-role-definition-attributes)
DeleteRoleDefinition                | Odstraní definice role.
AddRoleAssignmentToRoleDefinition | Přiřazování rolí přidané do definice role.
RemoveRoleAssignmentFromRoleDefinition | Přiřazení role odebraná z definice role.
AddRoleFromTemplate     | Přidané role ze šablony.
UpdateRole  | Aktualizované role.
AddRoleScopeMemberToRole    | Přidané obory členům role.
RemoveRoleScopedMemberFromRole  | Z role odebere obory člena.
**Události zařízení (všechny nové zvláštní události)**                    |
AddDevice               | Přidání zařízení.
UpdateDevice            | Aktualizované zařízení. Zobrazit, jaké zařízení vlastnosti aktualizovaly, najdete pod odkazy v části [Vlastnosti zařízení Audited](#update-device-attributes)
DeleteDevice            | Odstraněné zařízení.
AddDeviceConfiguration      | Konfigurace přidané zařízení.
UpdateDeviceConfiguration   | Konfigurace aktualizované zařízení. Zobrazit vlastnosti konfigurace jaké zařízení byly aktualizovány, najdete pod odkazy [Vlastnosti konfigurace zařízení Audited](#update-device-configuration-attributes) v následující části
DeleteDeviceConfiguration   | Konfigurace odstraněné zařízení.
AddRegisteredOwner     | Přidané registrovaných vlastníkovi zařízení.
AddRegisteredUsers     | Přidali registrované uživatele do zařízení.
RemoveRegisteredOwner    | Registrovaná vlastníky odeberte ze zařízení.
RemoveRegisteredUsers    | Odebrání registrovaných uživatelů ze zařízení.
RemoveDeviceCredentials  | Odebrání zařízení pověření.
**B2B události**                       |
Nahrané dávka pozvánky.              | Správce má nahrát soubor obsahující pozvánky nechat zasílat uživatelům partnera.
Dávkové zpracování pozvánky.             | Soubor obsahující pozvánek pro uživatele partnera má zpracovat.
Pozvání externích uživatelů.                | Externí uživatel dostali pozvání k adresáři.
Uplatnění pozvání externích uživatelů.         | Externí uživatel má uplatnit jejich pozvání do adresáře.
Přidání externích uživatelů do skupiny.          | Externí uživatel přiřadila členství ve skupině v adresáři.
Přiřadíte externího uživatele k aplikaci. | Externí uživatel přiřadila přímý přístup k aplikaci.
Vytvoření virové klienta.               | Nového klienta vytvořeného v Azure AD pomocí zaruč_cena pozvánku.
Vytvoření virové uživatele.                 | Uživatel vytvořeného v stávajícímu klientovi v Azure AD pomocí zaruč_cena pozvánku.
**Pro správu jednotky (všechny nové zvláštní události)**             |
AddAdministrativeUnit   |   Přidávání správce jednotek.
UpdateAdministrativeUnit|   Aktualizace pro správu jednotky. Jaké vlastností pro správu jednotky aktualizovaných zobrazíte najdete pod odkazy v části [Vlastnosti pro správu Jednotková auditování](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   Odstranění správní jednotku.
AddMemberToAdministrativeUnit|  Přidání člena do správní jednotku.
RemoveMemberFromAdministrativeUnit|     Odebrání člena ze správní jednotku.
**Adresář události**                 |
Přidání partnera společnosti               | Přidat partnera k adresáři.
Odebrání partnera společnosti          | Odebrat partnera z adresáře.
DemotePartner   |   Snížení úrovně partnera.
Přidání domény společnosti                | Doménu přidali do adresáře.
Odebrání domény společnosti           | Odebrání domény z adresáře.
Aktualizace domény                        | Aktualizace doménou v adresáři. Zobrazit vlastnosti domény které byly aktualizovány, najdete pod odkazy v části [Vlastnosti domény auditovány](#update-domain-attributes)
Ověření domény nastavení            | Změnit výchozí doménu pro společnosti.
Nastavení společnosti kontaktní informace      | Nastavení předvoleb kontaktu úrovni společnosti. Jedná se o e-mailové adresy pro marketingové, jakož i technické při upozorňování na Microsoft Online Services.
Nastavení federace domén    | Aktualizace nastavení federace pro doménu.
Ověření domény                        | Ověření domény v adresáři.
Ověření domény ověřenou e-mailu         | Ověření domény v adresáři pomocí ověření e-mailu.
Nastavit příznak DirSyncEnabled společnosti   | Nastavte vlastnost, která povolí adresář Azure AD Sync.
Nastavení zásad hesla                  | Nastavení délky a znak omezení pro uživatelská hesla.
Informace o nastavení společnosti              | Aktualizované informace o úrovni společnosti. V tématu rutiny Powershellu [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) další podrobnosti.
SetCompanyAllowedDataLocation  |    Nastavení společnosti povolené umístění dat.
SetCompanyDirSyncEnabled    |   Nastavte příznak DirSyncEnabled.
SetCompanyDirSyncFeature |  Nastavte DirSync funkce.
SetCompanyInformation |   Informace o nastavení společnosti.
SetCompanyMultiNationalEnabled    |     Nastavte společnosti mezinárodní zapnutou.
SetDirectoryFeatureOnTenant   |     Funkce adresáře sadu na klienta.
SetTenantLicenseProperties  |   Nastavení vlastností licenci klienta.
CreateCompanySettings   |   Vytvoření nastavení společnosti
UpdateCompanySettings   |   Aktualizujte svá nastavení společnosti. Zobrazit vlastnosti jaké společnosti byly aktualizovány, najdete pod odkazy [auditovány společnosti vlastnosti](#update-company-attributes) v části
DeleteCompanySettings   |   Odstranit nastavení společnosti
SetAccidentalDeletionThreshold   |  Nastavte prahovou hodnotu nechtěným odstraněním.
SetRightsManagementProperties   |   Nastavení vlastností správy práva.
PurgeRightsManagementProperties     |   Odstranění vlastností správy přístupových práv.
UpdateExternalSecrets   |   Aktualizace externích tajemství
**Události zásad (všechny nové zvláštní události)**           |
AddPolicy   |   Přidání zásad.
UpdatePolicy    |   Zásady aktualizujte.
DeletePolicy    |   Odstraňte zásadu.
AddDefaultPolicyApplication     |   Přidání zásad aplikace.
AddDefaultPolicyServicePrincipal    |   Přidejte zásady jistinu služby.
RemoveDefaultPolicyApplication  |   Odebrání zásad z aplikace.
RemoveDefaultPolicyServicePrincipal     |   Odebrání zásad jistinu služby.
RemovePolicyCredentials     |   Odebrání zásad pověření.

## <a name="audit-report-retention"></a>Sestavy auditování uchovávání informací
Události sestava auditování Azure AD uchovávají pro 180 dnů. Další informace o uchovávání informací v sestavách najdete v tématu [Zásady uchovávání informací sestavy Active Directory Azure](active-directory-reporting-retention.md).

Zákazníkům, kteří ukládat své auditování události delší dobu uchovávání informací rozhraní API sestav mohou sloužit k pravidelně začlenit auditování událostí do samostatný datový úložiště. Další informace najdete v článku [Začínáme s rozhraní API sestav](active-directory-reporting-api-getting-started.md) .

## <a name="properties-included-with-each-audit-event"></a>Vlastnosti zahrnutý v sadě každou událost auditování

Vlastnost      | Popis
------------- | --------------------------------------------------------------
Datum a čas | Datum a čas výskytu události auditu
Objekt actor         | Uživatel nebo jistinu služby, kterou provést akci
Akce        | Akce, které bylo provedeno
Cíl        | Uživatel nebo jistinu služby, které akci bylo provedeno na


## <a name="update-user-attributes"></a>"Aktualizovat uživatele" atributy
Události auditování "Aktualizace uživatele" obsahuje další informace o aktualizovaných jaké atributy uživatele. U každého atributu předchozí hodnotu a nová hodnota je však započítávány.

Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Uživatele můžete přihlásit.
AssignedLicense                     | Všechny licence přiřazené uživateli.
AssignedPlan                        | Service plány vrácené se uživateli přiřadí licence.
LicenseAssignmentDetail             | Podrobnosti o se uživateli přiřadí licence. Například pokud skupina založená licencování byl použit, by obsahoval skupinu, které přidělit licence.
Mobilní telefon                              | Mobilní telefon uživatele.
OtherMail                           | Alternativní e-mailovou adresu uživatele.
OtherMobile                         | Uživatele alternativní mobilního telefonu.
StrongAuthenticationMethod          | Seznam metod ověřování nakonfigurovány uživatelem Vícefaktorové ověřování, například kód hlasové volání, služby SMS nebo ověření v mobilní aplikaci.
StrongAuthenticationRequirement     | Pokud vynutit Vícefaktorové ověřování povolit nebo zakázat pro tohoto uživatele.
StrongAuthenticationUserDetails     | Uživatele telefonní číslo alternativní telefonní číslo a e-mailovou adresu sloužit k ověření Vícefaktorové ověřování a heslo resetovat.
StrongAuthenticationPhoneAppDetail  | Podrobnosti forof v aplikacích pro telefon registrované provádět 2FA přihlášení
TelephoneNumber                     | Telefonní číslo uživatele.
AlternativeSecurityId               | Alternativní zabezpečení ID objektu.
CreationType                        |Vytvoření metody uživatele (nebo pozvání virové).
InviteTicket                        |Seznam lístků pozvání pro uživatele.
InviteReplyUrl                      |Seznam adres URL odpovědět po přijetí pozvánky.
InviteResources                     |Seznam zdrojů, ke kterým uživatel pozván.
LastDirSyncTime                     |Čas poslední objekt byl aktualizován kvůli synchronizace z autoritativní adresáře (zákazník, místní).
MSExchRemoteRecipientType           |Mapy příjemců typům MSO. Podívejte se do [MSO příjemců typy] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx typů příjemců
PreferredDataLocation               |Upřednostňované umístění pro uživatele, skupiny, kontaktu, veřejné složky nebo dat mobilního zařízení.
ProxyAddresses                      |Adresa prodloužením doby příjemců objektu serveru Exchange rozpoznán v cizího poštovního systému.
StsRefreshTokensValidFrom           |Mělo aktualizovat informace o tokenu odvolání – všechny aktualizace tokeny STS vydané před Tentokrát jsou považovány za konec platnosti.
UserPrincipalName                   |Hlavní název uživatele, který je internetovému stylu přihlašovací jméno pro uživatele.
UserState                           |Stav uživatel čeká na schválení/PendingAcceptance/přijaté/PendingVerification.
UserStateChangedOn                  |Časové razítko poslední změny UserState. Slouží k spouštění pracovních postupů životního cyklu.
UserType                            |Typ uživatele. Člen (0), hosta (1), Viral(2).

## <a name="update-group-attributes"></a>Atributy "Aktualizace skupiny"
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Klasifikace            | Klasifikace pro skupinu jednotné (HBI, MBI atd.).
Popis               | Čitelné popisná věty o objektu.  
DisplayName               |Zobrazovaný název objektu
DirSyncEnabled            |Označuje, zda synchronizace z autoritativní adresáře (zákazník, místní).
GroupLicenseAssignment    |Přiřazení licencí skupiny
GroupType                 |Typ skupiny, jednotné (0)
IsMembershipRuleLocked    |Označuje, že vlastnost MembershipRule nastavil službu pro správu samoobslužné skupiny a uživatelé nemohli měnit. Použít pouze u nichž GroupType obsahuje GroupType.DynamicMembership skupiny
IsPublic                  |Příznak označující, pokud je skupina soukromá /.
LastDirSyncTime           |Čas poslední objekt byl aktualizován důsledku synchronizace z autoritativní adresáře (místního nasazení zákazníka).
Pošta                      |Primární e-mailovou adresu
MailEnabled               |Označuje, zda má tato skupina možnost e-mailu.
MailNickname              |Zástupný pro konkrétní objekt knihy adresu, obvykle část názvu e-mailu předchozí "@" symbol.   
MembershipRule            |Řetězec vyjadřující kritéria pomocí služby správy samoobslužné skupiny a určit členy, které by měla patřit k této skupině. Další informace najdete v článku IsMembershipRuleLocked. Platí jenom pro skupiny nichž GroupType obsahuje GroupType.DynamicMembership.
MembershipRuleProcessingState| Hodnota výčtu definovaná samoobslužné skupiny správy definování stav členství zpracování pro tuto skupinu. Platí jenom pro skupiny nichž GroupType obsahuje GroupType.DynamicMembership.
ProxyAddresses            |Adresa, podle kterého příjemců objektu serveru Exchange rozpoznala v cizího poštovního systému.
RenewedDateTime           |Záznam časové razítko ze kdy naposledy obnovil skupiny.   
SecurityEnabled           |Označuje, zda členství ve skupině může ovlivnit rozhodnutí o ověření.
WellKnownObject           |Zobrazuje objektu adresáře označíte jako jednu z předem definovaných sadu.

## <a name="update-device-attributes"></a>"Aktualizovat zařízení" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Označuje, zda může ověřovat jistinu zabezpečení.
CloudAccountEnabled | Označuje, zda může ověřovat jistinu zabezpečení. Pokud je zařízení standardní místního nasazení který napsal InTune.
CloudDeviceOSType | Typ zařízení na základě operačního systému například Windows RT iOS. Když nastavil do cloudové služby (třeba Intune), Tenhle atribut stane autoritativní v adresáři DeviceOSType.
CloudDeviceOSVersion | Verze operačního systému. Když nastavil do cloudové služby (třeba Intune), Tenhle atribut stane autoritativní v adresáři DeviceOSVersion.
CloudDisplayName | Jako hodnotu atributu displayName LDAP. Když nastavil do cloudové služby (třeba Intune), bude Tenhle atribut autoritativní v adresáři displayName.
CloudCreated |Označuje, zda objekt vytvořil cloudovým službám.
CompliantUntil | K čemu spuštění se považuje za kompatibilní se standardem zařízení.
DeviceMetadata |Vlastní metadat pro zařízení
DeviceObjectVersion | Tenhle atribut slouží k identifikaci verze schématu zařízení.
DeviceOSType |Typ zařízení na základě operačního systému například Windows RT iOS. Napsal službu registrace a mají být aktualizovány tak, že MDM služba Správa nebo Služba tokenů zabezpečení částečné služba pro správu.
DeviceOSVersion |Verze operačního systému. Napsal službu registrace a mají být aktualizovány tak, že MDM služba Správa nebo Služba tokenů zabezpečení částečné služba pro správu.
DevicePhysicalIds |Vícehodnotový atribut určená pro ukládání identifikátory fyzické zařízení. Může se jednat o BIOS ID, TPM thumbprints, hardwaru konkrétní ID, atd.
DirSyncEnabled | Označuje, zda autoritativní (zákazníkovi, na místní) dochází k synchronizaci adresářů.
DisplayName |Zobrazovaný název objektu
IsCompliant | Tenhle atribut je sloužící ke správě stav správy mobilních zařízení zařízení.
IsManaged |Tenhle atribut slouží k označení, že zařízení se spravuje obláčkem MDM.
LastDirSyncTime |Čas poslední objekt byl aktualizován kvůli synchronizace z autoritativní adresáře (místního nasazení zákazníka).

## <a name="update-device-configuration-attributes"></a>"Aktualizovat konfigurace zařízení" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Maximální počet dní, po které zařízení může být neaktivní dříve, než je považován za pro odstranění.
RegistrationQuota   |Zásady umožňují omezit počet registrace zařízení povolených pro jednoho uživatele.

## <a name="update-service-principal-configuration-attributes"></a>"Aktualizovat služby základní konfiguraci" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Označuje, zda může ověřovat jistinu zabezpečení.
AppPrincipalId | Externí, definované aplikací identity pro jistinu zabezpečení.
DisplayName |Zobrazovaný název objektu
ServicePrincipalName    | Služby základní jméno obsahující "název/autorita", kde název určuje hodnotu třídy aplikace a autorita obsahuje aspoň hostname [: port] nebo "název", která určuje identifikátor pro jistinu služby.

## <a name="update-app-attributes"></a>"Aktualizaci" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Sada adresy (přesměrování adresy URL), které jsou přiřazené k hlavní název služby.
ID aplikace                               |ID aplikace aplikace   
AppIdentifierUri | Aplikace identifikátor URI, který identifikuje aplikace.  Obvykle je adresa URL aplikace access.
AppLogoUrl |Adresa url pro obrázek loga aplikace uložené v CDN.
AvailableToOtherTenants | True aplikace je program více klienta (může být použity tak, že ostatní klienti).
DisplayName | Zobrazovaný název název aplikace
Oprávnění |Seznam nároky aplikace.
ExternalUserAccountDelegationsAllowed |Příznak, označuje, zda zdrojů aplikace je důvěryhodných a můžete vytvořit položky delegování u externích uživatelských účtů.
GroupMembershipClaims |Členství ve skupinách deklarace zásad.
PublicClient | True, pokud klienta nemůžete udržovat tajná (tedy utajení client OAuth2.0)
RecordConsentConditions | Typy souhlas podmínek, definované podmínky smlouvy: žádné (0), SilentConsentForPartnerManagedApp(1). Tato hodnota bude zveřejněným v schéma rozhraní API grafu a může být pouze nastavit nebo změnit tak, že správce klienta.
RequiredResourceAccess |Obsah XML hodnoty RequiredResourceAccess vlastnosti.
Web Appu |True, označuje, že tuto aplikaci je web app.
WwwHomepage |Primární webovou stránku.

## <a name="update-role-attributes"></a>"Aktualizovat roli" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Sada adresy (přesměrování adresy URL), které jsou přiřazené k hlavní název služby.
BelongsToFirstLoginObjectSet |Pokud je hodnota true, označuje, že tento objekt patří do sady objektů muset povolit přihlášení první správce nového klienta.
Předdefinované |Označuje, zda je životnost objektu vlastněná systému.
Popis | Čitelné popisná věty o objektu.  
DisplayName |Zobrazovaný název objektu
MailNickname | Zástupný pro konkrétní objekt knihy adresu, obvykle část názvu e-mailu předchozí "@" symbol.
RoleDisabled |Označuje, zda roli měly být ignorovány pro účely kontrol přístup.
RoleTemplateId | Identita šablony rolí.
ServiceInfo |Specifických pro službu zřizovací informace, které mohou být využívané MOAC a/nebo jiné služby výskyty (typy stejném nebo jiném služby).
TaskSetScopeReference |Určuje TaskSet a sadu obory přidružené k roli nebo šablony role.
ValidationError |Informace o publikovaných federované službou popisující nepřechodná, specifických pro službu chyba týkající se vlastnosti a odkaz z správce akce přidružená k objektu řešení.
WellKnownObject |Zobrazuje objektu adresáře označíte jako jednu z předem definovaných sadu.

## <a name="update-role-definition-attributes"></a>"Aktualizujte definici Role" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Kolekce oborů se tak mohli ověřovat, které mohou odkazovat k přiřazení tohoto RoleDefinition jistinu zabezpečení.
DisplayName     |Zobrazovaný název objektu
GrantedPermissions  |Oprávnění udělena tato RoleDefinition.


## <a name="update-administrative-unit-attributes"></a>"Aktualizovat pro správu jednotku" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Popis |   Tato vlastnost se aktualizuje při změně popis pro správu jednotky.
DisplayName |   Tato vlastnost se aktualizuje při změně název pro správu jednotky.

## <a name="update-company-attributes"></a>Atributy "Aktualizace společnosti"
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Umístění, ve kterém budou moci zřídit uživatelé společnosti.
AuthorizedServiceInstance   | Názvy instancí služby, ke kterým může být nasazené na plán.
DirSyncEnabled |Označuje, zda autoritativní (zákazníkovi, na místní) dochází k synchronizaci adresářů.
DirSyncStatus |Označuje, zda synchronizace adresu knihy objekty v tomto klientovi kontextu autoritativní (zákazníkovi, na místní) directory. rozšíření vlastnost DirSyncEnabled u společnosti objektů.
DirSyncFeatures |Příznak ke sledování sadu funkcí zapnutým a vypnutým dirsync klienta.
DirectoryFeatures |Adresář povolit nebo zakázat funkce.
DirSyncConfiguration |Obsahuje všechny DirSync konfigurace specifické pro aktuální klienta.
DisplayName |Zobrazovaný název objektu
IsMnc |Příznak typu Boolean nastavena na "hodnotu true" iff, které společnosti aktivované řešení funkci mezinárodní společnosti.
ObjectSettings | Souhrn nastavení k dispozici rozsahu objektu.
PartnerCommerceUrl |Adresa URL commerce web partnera.
PartnerHelpUrl |Adresa URL nápovědy web partnera.
PartnerSupportEmail | Adresa URL partnera podpory e-mailu.
PartnerSupportTelephone |Adresa URL pro telefon podpory partnera.
PartnerSupportUrl |Adresa URL partnerský web podpory.
StrongAuthenticationDetails |Podrobnosti související s StrongAuthentication.
StrongAuthenticationPolicy |Silné ověřování zásady společnosti.
TechnicalNotificationMail |E-mailovou adresu sdělit technické problémy týkající se ze společnosti.
TelephoneNumber |Telefonní čísla, která splňují E.123 doporučení ITU.
TenantType |Typ klienta. Pokud není zadán tato hodnota, je klient společnosti. V ostatních případech jsou možné hodnoty: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Sadu názvů domén DNS vázaný na společnost.

## <a name="update-domain-attributes"></a>"Aktualizace domény" atributy
Atribut                           | Popis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Funkce    |Bit příznaků s popisem funkcí domény, pokud existuje.
Výchozí |Označuje, jestli domény je výchozí hodnota. například výchozí UserPrincipalName přípona při správce vytvoří nový uživatel v MOAC.
Počáteční |Označuje, zda domény na počáteční doménu společnosti, jak přidělené OCP. Na počáteční doménu je jedinečný domény Microsoft Online domény; e.g.contoso3.microsoftonline.com.
LiveType    |Typ odpovídající Windows Live obor názvů, pokud existuje.
Jméno    | Identifikátor URI koncového bodu.
PasswordNotificationWindowDays  |Počet dní před uživateli platnosti hesla upozornění.
PasswordValidityPeriodDays  | Počet dní, po které hesla je vhodný k předtím, než je třeba změnit.

Auditování záznamy jsou požadované ovládací prvek pro mnoho dodržování předpisů. Zákazníkům, kteří používají Azure Active Directory auditování sestavy podle svých dodržování předpisů je vhodné zákazník předložit kopii tohoto tématu nápovědy kopii zprávy exportovaného auditu zákazníka vysvětlit podrobnosti sestavy. Pokud má auditor znát dodržování předpisů aktuálně splňující Azure, odkázat auditor [dodržování předpisů stránku](https://azure.microsoft.com/support/trust-center/compliance/) Microsoft Azure Trust Center.
