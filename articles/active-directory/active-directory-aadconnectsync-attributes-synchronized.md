<properties
    pageTitle="Azure AD Connect synchronizace: atributy synchronizovat s Azure Active Directory | Microsoft Azure"
    description="Seznam atributy, které jsou synchronizovány pro službu Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect synchronizace: atributy synchronizovat s Azure Active Directory
Toto téma uvádí atributy, které jsou synchronizovány Azure AD Connect synchronizace.  
Atributy jsou seskupená podle související Azure AD aplikace.

## <a name="attributes-to-synchronize"></a>Atributy pro synchronizaci
Běžné otázky je *co seznam minimální atributů synchronizovat*. Výchozí a doporučený postup je výchozí atributy tak celou GAL (globální adresář) můžete vytvořen v cloudu a všechny funkce vstoupit úloh Office 365. V některých případech existují některé atributy, které vaše organizace nemá být synchronizované do cloudu od obsahovat tyto atributy citlivé nebo Umožňujících (identifikovatelné osobní údaje) dat, třeba v tomto příkladu:  
![Chybné atributy](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

V tomto případě začínat seznam atributů v tomto tématu a identifikovat ty atributy, které by obsahoval citlivé nebo Umožňujících dat a nelze synchronizovat. Potom zrušte zaškrtnutí políčka tyto atributy během instalace pomocí [aplikace Azure AD a atribut filtrování](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

>[AZURE.WARNING] Při následné zrušení jejich atributy, doporučujeme nutná opatrnost a pouze zrušte zaškrtnutí políčka tyto atributy opravdu nelze synchronizovat. Unselecting další atributy může mít negativní vliv na funkce.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Název atributu| Uživatel| Komentář |
| --- | :-: | --- |
| accountEnabled| X| Určuje, zda je povoleno účet.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| pwdLastSet| X| technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| sourceAnchor| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| usageLocation| X| technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X| Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|

## <a name="exchange-online"></a>Exchange Online

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| Pomocník pro| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| KN| X|  | X|  |
| co| X| X|  |  |
| společnosti| X| X|  |  |
| countryCode| X| X|  |  |
| oddělení| X| X|  |  |
| Popis| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName (jméno)| X| X|  |  |
| Telefon domů| X| X|  |  |
| informace o| X| X| X| Tenhle atribut není aktuálně spotřebované množství pro skupiny.|
| Pole Iniciály| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Správce| X| X|  |  |
| člena|  |  | X|  |
| mobilní| X| X|  |  |
| msDS HABSeniorityIndex| X| X| X|  |
| msDS PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Tenhle atribut není aktuálně využívané Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Tenhle atribut není aktuálně využívané Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Tenhle atribut není aktuálně využívané Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Tenhle atribut není aktuálně využívané Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Tenhle atribut není aktuálně využívané Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| operátor| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| PSČ| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Odvozeno z groupType|
| okně| X| X|  |  |
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Název| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| společnosti| X| X|  |  |
| countryCode| X| X|  |  |
| oddělení| X| X|  |  |
| Popis| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName (jméno)| X| X|  |  |
| hideDLMembership|  |  | X|  |
| Telefon domů| X| X|  |  |
| informace o| X| X| X|  |
| pole Iniciály| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| pošta| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| Správce| X| X|  |  |
| člena|  |  | X|  |
| middleName| X| X|  |  |
| mobilní| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| operátor| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| PSČ| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Odvozeno z groupType|
| okně| X| X|  |  |
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Název| X| X|  |  |
| unauthOrig| X| X| X|  |
| Adresa URL| X| X|  |  |
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X|  |  | Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| společnosti| X| X|  |  |
| oddělení| X| X|  |  |
| Popis| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName (jméno)| X| X|  |  |
| Telefon domů| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| pošta| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Správce| X| X|  |  |
| člena|  |  | X|  |
| mobilní| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| atribut msRTCSIP-ApplicationOptions| X|  |  |  |
| atribut msRTCSIP-DeploymentLocator| X| X|  |  |
| atribut msRTCSIP řádku| X| X|  |  |
| atribut msRTCSIP-OptionFlags| X| X|  |  |
| atribut msRTCSIP-OwnerUrn| X|  |  |  |
| atribut msRTCSIP-PrimaryUserAddress| X| X|  |  |
| atribut msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| PSČ| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| securityEnabled|  |  | X| Odvozeno z groupType|
| okně| X| X|  |  |
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Název| X| X|  |  |
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X|  |  | Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| CN| X|  | X| Běžné název nebo alias takové. Nejčastěji používané předpona hodnotu [hromadné].|
| displayName| X| X| X| Řetězec, který odpovídá názvu často zobrazuje popisný název (křestní jméno příjmení).|
| pošta| X| X| X| úplnou e-mailovou adresu.|
| člena|  |  | X|  |
| objectSID| X|  | X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| proxyAddresses| X| X| X| technický vlastnost. Využívána Azure AD. Obsahuje všechny sekundární e-mailových adres pro uživatele.|
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny.|
| securityEnabled|  |  | X| Odvozeno z groupType.|
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X|  |  | Tento UPN je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|

## <a name="intune"></a>Intune

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Popis| X| X| X|  |
| displayName| X| X| X|  |
| pošta| X| X| X|  |
| mailnickname| X| X| X|  |
| člena|  |  | X|  |
| objectSID| X|  | X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| securityEnabled|  |  | X| Odvozeno z groupType|
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X|  |  | Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| společnosti| X| X|  |  |
| countryCode| X| X|  |  |
| Popis| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName (jméno)| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Správce| X| X|  |  |
| člena|  |  | X|  |
| mobilní| X| X|  |  |
| objectSID| X|  | X| technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| PSČ| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| securityEnabled|  |  | X| Odvozeno z groupType|
| okně| X| X|  |  |
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| Název| X| X|  |  |
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X|  |  | Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|

## <a name="3rd-party-applications"></a>aplikace 3.
Tato skupina je sada atributy použité jako minimální atributy potřebné pro obecný pracovní zátěž nebo aplikací. Můžete použít pro úlohu není uvedená v jiné části nebo aplikace jiných výrobců. Používá se výslovně tyto věci:

- Yammer (pouze pro uživatele je spotřebované množství)
- [Hybridní Business Business (B2B) křížově organizační scénáře možností spolupráce nabízená zdrojů, jako jsou služby SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Tato skupina je sada atributy, které lze použít, pokud není použit adresář Azure AD pro podporu Office 365, Dynamics nebo Intune. Má kratších seznamů základní atributy.

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Určuje, zda je povoleno účet.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| givenName (jméno)| X| X|  |  |
| pošta| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| člena|  |  | X|  |
| objectSID| X|  |  | technický vlastnost. Identifikátor uživatele AD využít ke správě synchronizace mezi Azure AD a AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | technický vlastnost. Slouží k vědět, kdy platnost už vydané tokeny. Používat synchronizace hesel a federace.|
| okně| X| X|  |  |
| sourceAnchor| X| X| X| technický vlastnost. Neměnný identifikátor Udržovat vztah mezi přidá a Azure AD.|
| usageLocation| X|  |  | technický vlastnost. Země uživatele. Použít pro přiřazení licencí.|
| userPrincipalName| X|  |  | Přípony je ID přihlášení uživatele. Nejčastěji používané stejný příkaz jako [hromadné] hodnotu.|

## <a name="windows-10"></a>Windows 10
Windows 10 doméně computer(device) synchronizuje některé atributy Azure AD. Další informace o scénáře najdete v článku [připojení zařízení Azure AD pro Windows 10 doméně dojde](active-directory-azureadjoin-devices-group-policy.md). Tyto atributy vždy synchronizovat a Windows 10 se nezobrazí jako aplikaci, kterou můžete zrušit zaškrtnutí položky. Windows 10 doméně počítač bude označen tím, že atribut userCertificate vyplněné.

| Název atributu| Zařízení| Komentář |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Hodnota pevně kódovaná doméně počítačů. |
| displayName | X| |
| MS-DS-CreatorSID | X| Taky se mu říká registeredOwnerReference.|
| objectGUID | X| Taky se mu říká deviceID.|
| objectSID | X| Taky se mu říká onPremisesSecurityIdentifier.|
| operačního systému | X| Taky se mu říká deviceOSType.|
| operatingSystemVersion | X| Taky se mu říká deviceOSVersion.|
| userCertificate | X| |

Tyto atributy pro **uživatele** jsou kromě dalších aplikací, který jste zvolili.  

| Název atributu| Uživatel| Komentář |
| --- | :-: | --- |
| domainFQDN| X| Taky se mu říká Název_domény_dns. Například contoso.com.|
| domainNetBios| X| Taky se mu říká Název_netbios. Například CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Hybridní zpětným Exchange
Tyto atributy se zpět z Azure AD došlo k zápisu místní služby Active Directory vyberete povolit **hybridní nasazení Exchange**. V závislosti na vaší verzí Exchange může synchronizovat méně atributy.

| Název atributu| Uživatel| Kontakt| Skupina| Komentář |
| --- | :-: | :-: | :-: | --- |
| msDS ExternalDirectoryObjectID| X|  |  | Odvozeno z cloudAnchor v Azure AD. Tenhle atribut je nového v Exchange 2016.|
| msExchArchiveStatus| X|  |  | Online archivu: Umožňuje zákazníkům k archivaci pošty.|
| msExchBlockedSendersHash| X|  |  | Filtrování: Data zapisuje zpět místním prostředím filtrování a online bezpečných a blokovaných odesílatele dat z klientů.|
| msExchSafeRecipientsHash| X|  |  | Filtrování: Data zapisuje zpět místním prostředím filtrování a online bezpečných a blokovaných odesílatele dat z klientů.|
| msExchSafeSendersHash| X|  |  | Filtrování: Data zapisuje zpět místním prostředím filtrování a online bezpečných a blokovaných odesílatele dat z klientů.|
| msExchUCVoiceMailSettings| X|  |  | Povolení jednotné zasílání zpráv (UM) – Online hlasová pošta: Microsoft Lync Server pomocí připojuje integrace naznačit Lync Server místní jestli má uživatel hlasové pošty v online službám.|
| msExchUserHoldPolicies| X|  |  | Blokování z důvodu soudních sporů: Umožňuje cloudovým službám a zjistit určující, kteří uživatelé jsou ve skupinovém rámečku, podržte při řešení soudních sporů.|
| proxyAddresses| X| X| X| Pouze x500 vložit adresu z Exchange Online.|

## <a name="device-writeback"></a>Zpětného zápisu zařízení
Objekty zařízení vytvořené ve službě Active Directory. Tyto objekty může být připojen k Azure AD zařízení nebo doméně počítačích Windows 10.

| Název atributu| Zařízení| Komentář |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| rozlišující název | X| |
| msDS CloudAnchor | X| |
| msDS DeviceID | X| |
| msDS DeviceObjectVersion | X| |
| msDS DeviceOSType | X| |
| msDS DeviceOSVersion | X| |
| msDS DevicePhysicalIDs | X| |
| msDS KeyCredentialLink | X| Jenom s Windows serveru 2016 AD schématu |
| msDS IsCompliant | X| |
| msDS IsEnabled | X| |
| msDS IsManaged | X| |
| msDS RegisteredOwner | X| |


## <a name="notes"></a>Poznámky

- Pokud chcete použít alternativní ID, atributu userPrincipalName místní synchronizovat s onPremisesUserPrincipalName atribut Azure AD. Atribut alternativní ID pro poštu příklad se synchronizuje s Azure AD atributu userPrincipalName.
- V seznamech nad typ objektu **uživatele** týká rovněž typu objektu **iNetOrgPerson**.

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
