<properties
    pageTitle="Nastavení zásad podmíněné přístupu na základě zařízení připojené služby Azure Active Directory aplikací | Microsoft Azure"
    description="Nastavení zásad podmíněný přístupu zařízení pro Azure AD připojení aplikace."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Nastavení zásad podmíněné přístupu na základě zařízení připojené služby Azure Active Directory aplikací


Azure Active Directory (Azure AD) zařízení přístupu na základě podmíněné vám mohou pomoci chránit zdroje organizace od:

- Z neznámou nebo nespravované zařízení pokusy o přístup.
- Zařízení, která neodpovídají zásady zabezpečení vaší organizace.

Základní informace o podmíněném přístupu najdete v článku [podmíněného přístupu Azure Active Directory](active-directory-conditional-access.md).

Můžete nastavit podmíněné přístupu na základě zařízení zásady ochrany tyto aplikace:

- Office 365 SharePoint Online, můžete chránit vaše organizace webům a dokumentům
- Office 365 Exchange Online, můžete chránit vaše organizace e-mailu
- Software jako aplikace služby (SaaS), které jsou připojené k Azure AD pro ověřování
- Místní aplikace, které jsou publikované pomocí služby Azure AD aplikace Proxy

Nastavení zásad podmíněné přístupu na základě zařízení, na portálu Azure, přejděte na určitou aplikaci v adresáři.


  ![Seznam aplikací v adresáři Azure portálu] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Aplikace")


Vyberte požadovanou aplikaci a pak klikněte na kartu **Konfigurovat** nastavení zásad podmíněné přístup.  


  ![Konfigurace aplikace] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Zařízení na základě pravidel přístupu")




**Chcete-li nastavit zásady podmíněný přístupu na základě zařízení, v části **zařízení na základě pravidel přístupu** , **Povolit pravidla přístupu**, vyberte.**

Zásady podmíněné přístupu na základě zařízení má tři součásti:

- **Použití**. Rozsah uživatelů, které zásad.

- **Pravidla zařízení**. Podmínky zařízení musí být splněné před má přístup k aplikaci.

- **Vynucení aplikace**. Klientské aplikace (nativní versus prohlížeče) zásady se týká.

  ![Tři komponent zásady přístupu na základě zařízení] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Zařízení na základě pravidel přístupu")


## <a name="select-the-users-the-policy-applies-to"></a>Vyberte uživatele, které zásad

V části **Použít** můžete vybrat rozsah uživatelů, které platí pro tuto zásadu.

Když vytvoříte rozsahem zásady přístupu pro uživatele máte dvě možnosti:

- **Všichni uživatelé**. Zásadu použijte všem uživatelům, kteří mají přístup aplikace.

- **Skupiny**. Omezení zásady pro uživatele, kteří patří do určité skupiny.

![Zásady použít pro všechny uživatele nebo skupiny] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Použití")


 Vyloučit uživatele z zásadu, **s výjimkou** zaškrtněte políčko. To je užitečné, když potřebujete udělení oprávnění k určitému uživateli dočasně přístup k aplikaci. Tuto možnost vyberte, pokud někteří uživatelé ještě zařízení, která nejsou jste připraveni na podmíněný přístup. Zařízení nemusí ještě zaregistrovaná nebo mají být mimo dodržování předpisů.


## <a name="select-the-conditions-that-devices-must-meet"></a>Vyberte podmínky, které musí být splněné zařízení

Použití **Pravidel zařízení** nastavit podmínky zařízení musí být splněné pro přístup k aplikaci.

Podmíněné přístupu na základě zařízení můžete nastavit pro tyto typy zařízení:

- Aktualizace výročí Windows 10, Windows 8.1 a Windows 7
- Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008 R2
- zařízení s iOS (iPad, iPhone)
- Zařízení s androidem

Podpora pro Mac je brzy k dispozici.

  ![Použít zásadu zařízení] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Aplikace")

 >[AZURE.NOTE] Informace o rozdílech mezi doméně Azure AD připojen zařízení a najdete v tématu [použití Windows 10 zařízení ve vaší firmě](active-directory-azureadjoin-windows10-devices.md).

Máte dvě možnosti pro zařízení pravidla:

- **Všechna zařízení musí odpovídat požadavkům**. Všechny platformy zařízení, které přístup k aplikaci musí odpovídat požadavkům. Zařízení, spuštěné v operačním systému platformy, které nepodporují podmíněné přístupu na základě zařízení je přístup odepřen.

- **Pouze vybrané zařízení musí odpovídat požadavkům**. Jenom platformy konkrétní zařízení musí odpovídat požadavkům. Dalších platformách nebo jiné platformy, které přístup k aplikaci mít přístup.

  ![Nastavení oboru pro zařízení pravidla] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Aplikace")

Azure AD připojen zařízení jsou kompatibilní, pokud jsou označeny jako **kompatibilní se standardem** v adresáři Intune nebo systémem správy mobilních zařízení jiných výrobců, které lze integrovat s Azure AD.

Doméně zařízení není kompatibilní s pokud:

- Máte zaregistrované v rámci Azure AD. Mnoho organizací považovat za důvěryhodný zařízení doméně zařízení.
- Je označen jako **kompatibilní se standardem** v Azure AD 2016 Centrum Správce konfigurace systému.

 ![Doméně zařízení, které jsou kompatibilní] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Pravidla zařízení")

Osobní zařízení Windows jsou kompatibilní, pokud jsou označeny jako **kompatibilní se standardem** v adresáři Intune nebo systémem správy mobilních zařízení jiných výrobců, které lze integrovat s Azure AD.

Zařízení Windows bez jsou kompatibilní Pokud jsou označeny jako **kompatibilní se standardem** v adresáři tak, že Intune.

 >[AZURE.NOTE] Další informace o tom, jak nastavit Azure AD zařízení se zákonnými požadavky v různých systémů najdete v článku [podmíněného přístupu Azure Active Directory](active-directory-conditional-access.md).


Můžete vybrat jeden nebo více platformy zařízení pro zásady přístupu na základě zařízení. Jedná se o Android, iOS, Windows Mobile (Windows 8.1 telefony a tablety) a Windows (všechna ostatní Windows zařízení, včetně všech zařízeních s Windows 10).
Hodnocení zásad probíhá pouze na vybrané platformách. Pokud zařízení, které pokusí o přístup spuštěný není jeden z vybraných platformách zařízení přístup k aplikaci, pokud má uživatel přístup. Bez zásad zařízení je vyhodnocen.

![Vyberte platformách pro zařízení pravidla] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Pravidla zařízení")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Nastavení zásad hodnocení typ aplikace

V části **Použití zámku** nastavte typ aplikace, které zásady se vyhodnotí pro uživatele nebo přístup zařízení.

Máte dvě možnosti pro typ aplikace zahrnout:

- Prohlížeč a nativní aplikace
- Nativní aplikace

![Zvolte prohlížeče nebo nativní aplikace] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Aplikace")

K jejímu vynucení zásady přístupu pro aplikace, vyberte **prohlížeče a nativní aplikací**. Můžete zahrnout:

- Prohlížeče (například Microsoft Edge ve Windows 10) nebo Safari v iOS.
- Aplikace, použijte Active Directory ověřování knihovny (ADAL) na libovolnou platformu nebo používající WebAccountManager (WAM) rozhraní API ve Windows 10.

>[AZURE.NOTE] Další informace o podporu prohlížeče a další informace pro uživatele, který používá na zařízení založený, certifikační autorita chráněné aplikace najdete v článku [podmíněného přístupu Azure Active Directory](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Chránit e-mailovou adresu z aplikace založené na Exchange ActiveSync

Aplikace Office 365 Exchange Online můžete Exchange ActiveSync k blokování e-mailu přístup k aplikacím pošta založená na Exchange ActiveSync.

![Možnosti dodržování předpisů Exchange ActiveSync] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Aplikace")

![Vyžadovat kompatibilní se standardem zařízení pro přístup k e-mailu] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Aplikace")


## <a name="next-steps"></a>Další kroky

- [Azure Active Directory podmíněného přístupu](active-directory-conditional-access.md)
