<properties
    pageTitle="Seznam kompatibilních Azure AD federace"
    description="Tato stránka obsahuje Zprostředkovatelé identit jiní, kteří mohou sloužit k implementace jednotného přihlašování."
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Seznam kompatibilních Azure AD federace
Azure Active Directory obsahuje jednotného přihlašování a rozšířené zabezpečení aplikace access pro Office 365 a dalších služeb Microsoft Online pro hybridní a jen cloudu implementace bez nutnosti jakékoli řešení jiných výrobců. Office 365, jako je většina Online službám společnosti Microsoft, je integrována s Azure Active Directory pro, adresářovými službami a tak mohli ověřovat. Azure Active Directory také poskytuje jednotného přihlašování k tisícům SaaS aplikací a místní webových aplikací. Přečtěte si téma Galerie aplikace služby Azure Active Directory pro podporovaných aplikací SaaS.

V organizacích, které mají investovat v řešení-služby Microsoft federation Toto téma obsahuje pokyny ke konfiguraci jednotného přihlašování pro svoje uživatele služby Active Directory pro Windows Server se službami Microsoft Online s použitím poskytovatele identity společnosti Microsoft z "Azure Active Directory federation kompatibility seznamu" dole. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Skupina počítač Oxford](http://oxfordcomputergroup.com/)třetí strany, společnosti Microsoft, testováno tyto jednoho přihlašování zkušenosti pomocí Zprostředkovatelé identit jiní vůči sadě běžné případy použití se službou Azure Active Directory.

Návod, jak můžete rychle poskytovatele identit třetích stran uvedený tady, získáte Oxford počítač skupiny na [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Skupiny počítačů Oxford testováno pouze funkce federace tyto scénářů jednotného přihlašování. Skupiny počítačů Oxford nebyla provedena všechny testování synchronizace, dvojúrovňové ověřování, součástí těchto scénářů jednotného přihlašování atd.

>Použití přihlašovací tak, že alternativní ID UPN není taky testováno v této aplikaci.



- [Azure Active Directory](#azure-active-directory)
- [Optimální IDM virtuální Identity serveru Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [Můžete IBM federované správce identit 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [Certifikační Autorita SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [Správce přístupu NetIQ 4.0.1](#netiq-access-manager-401) 
- [VELKÁ IP s Správce zásady přístupu velká – IP verze 11.3 x – 11.6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [Portálu pracovního prostoru VMware verze 2.1](#vmware-workspace-portal-version-21) 
- [Přihlaste se a přejděte 5.3](#signampgo-53) 
- [IceWall federace verze 3.0](#icewall-federation-version-30) 
- [Certifikační Autorita zabezpečené cloudu](#ca-secure-cloud) 
- [V7.1 Dell jeden Identity cloudu přístup správce](#dell-one-identity-cloud-access-manager-v71) 
- [Jednoduchá AuthAnvil přihlášení 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Protože jsou tyto údaje produkty jiných výrobců, společnost Microsoft neposkytuje podporu pro nasazení, konfiguraci, Poradce při potížích, nejlepší postupy, atd problémy a otázky týkající se tyto Zprostředkovatelé identit jiní. Pro podporu a dotazy týkající se tyto Zprostředkovatelé identit jiní kontaktujte přímo podporované třetí strany.

>Zprostředkovatelé identit třetích stran byly testováno spolupráce pomocí WS federování a licenci WS zabezpečení protokoly pouze cloudovým službám Microsoftu. Testování neobsahuje pomocí protokolu SAML.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory můžete ověřování uživatelů federování s vaší Active Directory místní nebo bez server místní federaci použitím synchronizaci hesel. 

Následující obrázek je matice podpora scénářů pro tento přihlášení: 


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|
|Moderní aplikace ADAL například Office 2016| Podporované|Žádná|

Další informace o použití služby Azure Active Directory se službou AD FS najdete v článku [Active Directory Federation Services služby AD FS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Další informace o použití služby Azure Active Directory s synchronizaci hesel najdete v článku [Azure AD Connect](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimální IDM virtuální Identity serveru Federation Services 
Optimální IDM virtuální Identity serveru Federation Services můžete ověřování uživatelů, které se nacházejí v aktivním adresářích místní zákazníků.

Následující obrázek je scénáře podporují matice této jedné přihlášení:


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientskými aplikacemi například Lyncu, předplatné Office, CRM |  Podporované |Integrované ověřování systému Windows|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Pro další informace o přístupu klienta zásad najdete v článku [omezení přístupu k Office 365 služeb na základě polohy klienta.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 používá často používaný standardní identity WS Federation k zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientskými aplikacemi například Lyncu, předplatné Office, CRM |  Podporované |Nikdo (starší verze třeba upgradovat na 6.11|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

PingFederate pokyny ke konfiguraci to STS poskytovat prostředí přihlášení uživatele služby Active Directory, stáhněte si soubor pdf [zde.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

PingFederate pokyny ke konfiguraci tato služba tokenů zabezpečení a služby Active Directory uživatelům poskytovat prostředí přihlašování, najdete v článku [zde.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x implementuje často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

PingFederate pokyny ke konfiguraci tato služba tokenů zabezpečení a služby Active Directory uživatelům poskytovat prostředí přihlašování, najdete v článku [zde.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify pomáhá poskytovat federované jediného přihlašování rozhraní pro Office 365 bez nutnosti hostingu místního federačního serveru.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Řízení přístupu klienta nepodporuje 

Další informace o Centrify najdete v tématu [zde.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>Můžete IBM federované správce identit 6.2.2 
IBM můžete federované Identity správce 6.2.2 s IBM zabezpečení aplikace Access správce pro 1.4 aplikace Microsoft používá často používaný standardní identit WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o IBM můžete federované správce identit najdete v tématu [IBM zabezpečení aplikace Access správce for Applications Microsoftu.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 používá často používaný směrodatnou identity WS federace/WS – zabezpečení poskytovat prostředí přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o SecureAuth najdete v tématu [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>Certifikační Autorita SiteMinder 12.52 SP1 kumulativní verze 4
Certifikační Autorita SiteMinder federace 12.52 používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o CA SiteMinder najdete v tématu [CA SiteMinder federace.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne cloudu federace služby (CFS) 3.0 používá často používaný standardní identit WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Integrované ověřování systému Windows|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o RadiantOne CFS najdete v tématu [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta používá často používaný směrodatnou identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 


| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Integrované ověřování systému Windows vyžaduje nastavení další webový server a Okta aplikace.|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Integrované ověřování systému Windows|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o Okta najdete v tématu [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin při zkoušce v květen 2014 používá často používaný standardní identit WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Integrované ověřování systému Windows|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Integrované ověřování systému Windows|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o OneLogin najdete v tématu [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>Správce přístupu NetIQ 4.0.1 
Správce přístupu NetIQ 4.0.1 používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |* Podporované smlouvy Kerberos|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Integrované ověřování systému Windows není podporovaná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

* NetIQ podporuje ověřování protokolem Kerberos prostřednictvím konfigurace Kerberos smlouvy.  V této konfiguraci požádejte o pomoc kontaktujte NetIQ nebo zobrazení Průvodce nastavením. Další informace o správce NetIQ přístupu najdete v tématu [Správce přístupu NetIQ.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>VELKÁ IP s Správce zásady přístupu IP velká verze 11.3 x – 11.6 x 
VELKÁ IP s zásad správce přístupu, (vylepšeného řízení spotřeby) IP velká verze 11,3 x – 11.6 x používá často používaný standardní identit SAML poskytovat prostředí přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení: 

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Nepodporovaná funkce |Nepodporovaná funkce|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o správce zásad velká IP přístupu najdete v tématu [Správce zásady přístupu velká IP.](https://f5.com/products/modules/access-policy-manager) 

Správce zásady přístupu velká IP pokyny ke konfiguraci to služba tokenů zabezpečení poskytovat prostředí přihlašování Active Directory uživatelům, stáhněte si pdf [zde.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>Portálu pracovního prostoru VMware verze 2.1 
Portálu pracovního prostoru VMware verze 2.1 používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Integrované ověřování systému Windows není podporovaná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM | Podporované |Integrované ověřování systému Windows není podporovaná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o portálu pracovního prostoru VMware verze 2.1, stáhněte si soubor pdf [zde.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Přihlaste se a přejděte 5.3 
Přihlaste se a přejděte 5.3 implementuje často používaný identity WS federace/WS – zabezpečení Standardní zadal jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Protokol Kerberos smlouvy podporované |
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM | Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|


Symbol & go 5.3 podporuje ověřování protokolem Kerberos prostřednictvím konfigurace Kerberos smlouvy.  V této konfiguraci požádejte o pomoc, kontaktujte prosím Ilex nebo zobrazení Průvodce nastavením [zde.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall federace verze 3.0 
IceWall federace verze 3.0 používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Integrované ověřování systému Windows není podporovaná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM | Podporované |Integrované ověřování systému Windows není podporovaná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o IceWall federace najdete v tématu [tady](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) a [zde.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>Certifikační Autorita zabezpečené cloudu 

Certifikační Autority zabezpečené cloudu používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Integrované ověřování systému Windows není podporovaná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM | Podporované |Integrované ověřování systému Windows není podporovaná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o zabezpečení cloudové CA, najdete v článku [cloudu zabezpečené certifikační Autority.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>V7.1 Dell jeden Identity cloudu přístup správce 
Správce aplikace Access cloudu jeden identit Dell používá často používaný standardní identit WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Žádná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM |  Podporované |Žádná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|

Další informace o Dell jeden Identity cloudu správce přístupu najdete v tématu [správce přístup cloudu jeden identit Dell.](http://software.dell.com/products/cloud-access-manager)

 Pokyny ke konfiguraci tato služba tokenů zabezpečení poskytovat prostředí přihlášení uživatelé Office 365 najdete v tématu [Konfigurace uživatele Office 365.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>Jednoduchá AuthAnvil přihlášení 4.5 
AuthAnvil jeden znak na 4.5 používá často používaný standardní identity WS federace/WS – zabezpečení zadání jednotného přihlašování a atribut exchange framework.

Následující obrázek je matice scénář podporu pro tento jednoho přihlášení:

| Klient |Podpora  |Výjimky|
| --------- | --------- |--------- |
| Webových klientů, jako je Web Access serveru Exchange a Sharepointu Online | Podporované |Integrované ověřování systému Windows není podporovaná|
| Formátovaný klientské aplikace například Lyncu, předplatné Office, CRM | Podporované |Integrované ověřování systému Windows není podporovaná|
| E-mailu ve formátu RTF klientů, jako je Outlook a ActiveSync |  Podporované |Žádná|


Další informace najdete v tématu [AuthAnvil jednotné přihlašování.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
