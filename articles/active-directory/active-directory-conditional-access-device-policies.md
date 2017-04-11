<properties
    pageTitle="Zásady zařízení podmíněného přístupu pro služby Office 365 | Microsoft Azure"
    description="Podrobnosti o zařízení jak na základě podmínek pomocí řízení přístupu ke službám Office 365. I když pracovníci pracující s informacemi (IWs) služby přístup k Office 365 jako Exchange a Sharepointu Online v práci nebo ve škole ze své osobní zařízení, jejich správce IT chce přístup k být secure.IT správci můžete vytvořit zásady zařízení podmíněné přístupu k zabezpečení podnikové prostředky ve stejnou dobu povolení IWs na zařízení kompatibilní s přístup k službám."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Zásady zařízení podmíněného přístupu pro služby Office 365

Termín, "podmíněné přístup k" obsahuje mnoho podmínky přidružené například ověřeného uživatele Multi-Factor ověření zařízení, kompatibilní se standardem zařízení atd. Toto téma focusses především na základě zařízení podmínky k řízení přístupu ke službám Office 365. I když pracovníci pracující s informacemi (IWs) služby přístup k Office 365 jako Exchange a Sharepointu Online v práci nebo ve škole ze své osobní zařízení, jejich správce IT chce přístup bezpečný. Správci IT můžete vytvořit zásady zařízení podmíněné přístupu k zabezpečení podnikové prostředky ve stejnou dobu povolení IWs na zařízení kompatibilní s přístup k službám. Z portálu Microsoft Intune podmíněné access může konfigurovat zásady podmíněné přístupu k Office 365.

Azure Active Directory vynucuje zásady podmíněné přístupu a zabezpečit přístup ke službám Office 365. Správci klientů můžete vytvořit zásady podmíněné přístupu, která blokuje uživatele na zařízení není kompatibilní s získat přístup k služby O365. Uživatel musí odpovídat zásady společnosti zařízení před přístup ke službě. Správce případně můžete vytvořit zásady, které uživatelé budou jenom zápisu jejich zařízení k získání přístupu ke službě O365. Zásady může použít pro všechny uživatele v organizaci, nebo omezený na několik cílové skupiny a rozšířeného časem zahrnovat další cílové skupiny.

Předpokladem vynucení zásady zařízení je pro uživatele zaregistrujte svoje zařízení se službou Azure Active Directory registrace zařízení. Můžete se rozhodnout pro povolení vícefaktorové ověřování (MFA) pro registraci zařízení pomocí služby Azure Active Directory registrace zařízení. MFA je vhodné pro službu Azure Active Directory zařízení registrace. Jestliže je povolena MFA, uživatelé zaregistrovali služby Azure Active Directory zařízení zápisu jejich zařízení výzva pro druhé faktor ověření.

##<a name="how-does-conditional-access-policy-work"></a>Fungování podmíněné přístupu

Když uživateli žádosti o přístup k služby O365 z platformy podporovaného zařízení, ověří Azure Active Directory uživatele a zařízení, ze které uživatel spustí žádosti; a poskytuje přístup ke službě pouze v případě, že uživatel odpovídá zásad nastavení služby. Uživatelé, kteří nemají zápisu jejich zařízení jsou uvedeny nápravné pokyny týkající se zapsat a budou požadavkům pro přístup k podnikové služby O365. Uživatelé, kteří na iOS a Android se musí k zápisu jejich zařízení aplikaci portál společnosti. Když uživatel zapsán své zařízení, zařízení je registrovaný u Azure Active Directory a zápisu pro správu zařízení a dodržování předpisů. Zákazníci musí službu Azure Active Directory zařízení registrace spolu s Microsoft Intune povolit Správa mobilních zařízení pro služby Office 365. Zápis zařízení je předpoklad uživatelům přístup k Office 365 služby, když platí zásady zařízení.

Když uživatel úspěšně zapsán své zařízení, se stane důvěryhodným zařízení. Poskytuje Azure Active Directory-jednotné přihlášení do aplikace access společnosti a vynucuje podmíněné přístupu udělit přístup k služby nejen prvního času uživatel požádá o přístup, ale pokaždé, když uživatel požádá o prodloužení platnosti předplatného přístup. Uživatel bude být odepření přístupu ke službám dojde ke změně přihlašovací údaje, zařízení ztratilo či krádeže nebo zásady není splněná v době žádost o prodloužení.

## <a name="deployment-considerations"></a>Důležité informace o nasazení:
Musíte použít služby Azure Active Directory zařízení registrace pro registrace zařízení.

Po ověření pro místní uživatele požaduje Active Directory Federation Services (AD FS) (1.0 a nahoře). Vícefaktorové ověřování pro připojení pracovišti se nezdaří, když zprostředkovatele identit nedokáže vícefaktorové ověřování. Například služba AD FS 2.0 není Multi-Factor authentication podporou. Správce klienta musí zajistit, aby místní služba AD FS je MFA může a platnou metodu MFA aktivované před povolením MFA na službu Azure Active Directory zařízení registrace. Například služba AD FS v systému Windows Server 2012 R2 má funkce MFA. Před povolením MFA na služby Azure Active Directory registrace zařízení musí taky povolit metodu další platné ověřování (MFA) na server služby AD FS. Další informace o podporovaných metod MFA ve službě AD FS najdete v článku Konfigurace další metody ověřování pro službu AD FS.

## <a name="frequently-asked-questions-faq"></a>Nejčastější dotazy

Otázka: co je výchozí zásada vyloučených položek pro platformy nepodporované zařízení?

A: v současné době podmíněného přístupu selektivně platí zásady pro uživatele v iOS a Android. Aplikace na jiných platformách zařízení jsou ve výchozím nastavení ovlivněny podmíněné přístupu pro iOS a Android. Správce klienta, ale můžete přepsat globální zásadu Zakázat přístup uživatelům na nepodporované platformách.
Je na přehled rozšířit podmíněné přístupu uživatelů na jiných platformách, včetně Windows.

Otázka: když se podmíněné přístupu ke službám Office 365 prodlužuje na prohlížeči na základě aplikace (například OWA, SharePoint založená na prohlížeči).

A: v současné době se omezí na bohatých aplikací na zařízení podmíněné přístup ke službám Office 365. Je na přehled rozšiřte podmíněné přístupu uživatelům přístup ke službám z prohlížeče.
