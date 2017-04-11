<properties
  pageTitle="Azure IoT sady a Azure Active Directory | Microsoft Azure"
  description="Popisuje, jak Azure IoT sadu používá Azure Active Directory ke správě oprávnění."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Oprávnění na webu azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Co se stane při přihlášení

Při prvním přihlášení na [azureiotsuite.com][lnk-azureiotsuite], webu určuje úrovně oprávnění můžete mít na základě vybraného klienta Azure Active Directory (AAD) a Azure předplatného.

1.  Webu nejdřív zjistí z Azure které AAD klienti patříte k načtení hodnot seznamu klientů zobrazené vedle přihlášeného uživatelské jméno. V současné době můžete webu pouze získat tokeny uživatelů pro jednoho klienta najednou. Jako výsledek při změně na jiném klientovi pomocí rozevíracího seznamu v pravém horním rohu webu znovu přihlásí vás k tomuto klientovi získat tokeny k tomuto klientovi.

2.  Pak webu zjistí z Azure které předplatná přidružený vybraného klienta. Při vytváření nové předkonfigurované řešení zobrazit dostupné předplatná.

3.  Nakonec webu načte všech zdrojů v předplatných a skupin zdrojů příznakem jako předkonfigurované řešení a naplní dlaždice na domovské stránce.

Následující oddíly popisují role, které řízení přístupu k předkonfigurované řešení.

## <a name="aad-roles"></a>Role AAD

Role AAD řídit řešení možnost poskytování automaticky předem nakonfigurovaná a Správa uživatelů v předem řešení.

Další informace o role správců můžete najít v AAD v tématu [přiřazení rolí správce v Azure AD][lnk-aad-admin], ale tento článek se zaměřuje na **Globální správce** a **Uživatele/člen domény** role používaného předkonfigurované řešení.

**Globální správce:** Může být mnoho globální správci na AAD klienta. Když vytvoříte AAD klienta, se ve výchozím nastavení globální správce tomuto klientovi. Globální správce může zřízení předkonfigurované řešení a přiřazenou roli **Správce** pro aplikaci uvnitř jejich AAD klienta. Ale pokud jiný uživatel ve stejném klientovi AAD vytvoří aplikace, výchozí role udělení globální správce je **IMPLICITNÍ ČÍST jenom**. Globální správci můžete přiřadit role pro aplikace pomocí [Azure klasické portál][lnk-classic-portal].

**Uživatele/člen domény:** Může být mnoho uživatelů/členy domény na AAD klienta. Uživatel domény můžete zřízení předkonfigurované řešení prostřednictvím [azureiotsuite.com] [ lnk-azureiotsuite] webu. Výchozí role, které jsou poskytovány pro aplikaci, kterou budou zřízení je **Správce**. Můžete vytvořit aplikaci pomocí skriptu build.cmd v [azure iot vzdáleného – sledování] [ lnk-rm-github-repo] nebo [azure-iot prediktivní údržbu] [ lnk-pm-github-repo] úložiště, ale výchozí role mají přístup je **IMPLICITNÍ jen pro čtení**, nemají oprávnění k přiřazení role. Pokud jiný uživatel v klientovi AAD vytvoří aplikace, kterému jsou přiřazené role **IMPLICITNÍ jen pro čtení** ve výchozím nastavení této aplikace. Nemají možnost přiřadit role pro aplikace. Proto nemůžete přidat uživatele nebo role pro uživatele aplikace i v případě, že zřízení.

**Hosta uživatele/Host:** Může být mnoho hosta uživatelé/hostů na AAD klienta. Uživatele typu Host mít omezenou sadu práv v klientovi AAD. V důsledku toho uživatele typu Host nelze vytvořit předkonfigurované řešení v klientovi AAD.

Další informace najdete v následujících zdrojích:

- [Vytváření nebo úpravy uživatelů v Azure AD][lnk-create-edit-users]
- [Přiřazení rolí aplikace v AAD][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Role správců Azure předplatného

Role správců Azure řídit možnost mapovat předplatné Azure AD klienta.

Můžete zjistit víc o rolích Azure spolu správce, Správce služeb a účtů správce v článku [Přidání nebo změna Azure spolu správce, Správce služeb a účtů správce][lnk-admin-roles].

## <a name="application-roles"></a>Role aplikací

Role aplikací pomocí řízení přístupu k zařízení ve vašem předkonfigurované řešení.

Existují dva definované a implicitní rolí definované v aplikaci, která se vytvoří při zřizování předkonfigurované řešení.

-   **Správce:** Úplné řízení, spravovat, nebo zařízení

-   **Jen pro čtení:** Má zobrazování zařízení

-   **IMPLICITNÍ jen pro čtení:** To je stejný jako jen pro čtení, ale uděleno všem uživatelům klienta AAD. To byla pro usnadnění průběhu vývoje. Odebrání tato role změnou [RolePermissions.cs] [ lnk-resource-cs] zdrojový soubor.

### <a name="changing-application-roles-for-a-user"></a>Změna rolí aplikací pro uživatele

Následující postup slouží k nastavení uživatele ve službě Active Directory jako správce vašeho předkonfigurované řešení.

Musíte být globální správce AAD změnit role uživatele:

1. Přejděte na [portál Azure klasické][lnk-classic-portal].

2. Vyberte **služby Active Directory**.

3. Klikněte na název vašeho klienta AAD (Toto je na adresář, který jste se rozhodli na azureiotsuite.com při zřizování řešení).

4. Klikněte na **aplikace**.

5. Klikněte na název aplikace, která odpovídá názvu předkonfigurované řešení. Pokud aplikace v seznamu nevidíte, přepněte **Zobrazit** rozevírací dolů **aplikací vlastní naší společnosti** a klepněte na značku zaškrtnutí.

7. Klikněte na **uživatele**.

8. Vyberte uživatele, kterého chcete přepnout role.

9. Klikněte na **přiřadit** a vyberte roli (například **správu**) chcete uživateli přiřadit, klikněte na značku zaškrtnutí.

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Jsem Správce služby a chcete změnit mapování adresářů mezi svoje předplatné a konkrétní AAD klienta. Jak můžu udělat to?

1. Přejděte na [portál Azure klasické][lnk-classic-portal], v seznamu služby na levé straně klikněte na **Nastavení** .

2. Vyberte předplatné byste chtěli změnit mapování adresář.

3. Klikněte na **Upravit adresář**.

4. Vyberte **adresář** , kterou chcete použít v rozevíracím seznamu. Klikněte na tlačítko vpřed.

5. Potvrďte mapování adresář a ovlivněné dalších správců. Všimněte si, že pokud přesunujete z jiného adresáře, všech dalších správců z původního adresáře se odeberou.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Jsem uživatele/členem domény na klientovi AAD a jste vytvořili předkonfigurované řešení. Jak můžu získat přiřazené role pro aplikaci?

Zeptejte se globální správce můžete přiřadit jako globální správce na AAD klientovi, aby vám udělil oprávnění přiřadit role pro uživatele nebo požádejte přiřadit roli globálního správce. Pokud chcete změnit klienta AAD předkonfigurované řešení byla nasazena, najdete v článku další otázku.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Jak se přepíná mezi AAD klienta, které jsou přiřazené Moje vzdálené monitorovací předkonfigurované řešení a aplikací?

Můžete spustit na nasazení cloudu z <https://github.com/Azure/azure-iot-remote-monitoring> a přeinstalujte s nově vytvořený tenant AAD. Protože se ve výchozím nastavení globální správce při vytváření nového klienta AAD, bude mít přístup k přidání uživatelů a přiřazování rolí tyto uživatele.

1. Vytvoření nového adresáře AAD na [portálu Správa Azure][lnk-classic-portal].

2. Přejděte na <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Spuštění `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (například `build.cmd cloud debug myRMSolution`)

4. Po zobrazení výzvy, nastavte **tenantid** být nově vytvořený tenant místo předchozí klienta.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Chci změnit správce služeb nebo správce spolupracovat při přihlášení k lyncu organizační účtem

Najdete v článku podpora [Změna správce služby a dalších správce při přihlášení k lyncu organizační účtem][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Proč se mi zobrazují tato chyba? "Váš účet nemá příslušná oprávnění k vytvoření řešení. Zkontrolujte pomocí svého účtu správce nebo zkuste s jiným účtem."

Podívejte se na následující obrázek:

![][img-flowchart]

> [AZURE.NOTE] Pokud budete pokračovat zobrazíte Chyba za ověření se jako globální správce v klientovi AAD spolu správce u předplatného a požádejte správce účet odebrat uživatele a znovu přiřadit potřebná oprávnění v následujícím pořadí: přidejte uživatele jako globální správce a potom přidejte uživatele jako spolu správce Azure předplatného. Pokud potíže potrvají, obraťte se prosím [Nápověda a podpora][lnk-help-support].

**Proč vidím tuto chybu, pokud máte předplatné Azure?** *Předplatné Azure je potřeba k vytvoření předkonfigurovaná řešení. Vytvořit účet bezplatnou zkušební verzi v jenom pár minut.*

Pokud jste si jistí, jestli že máte předplatné Azure, ověřte klienta mapování pro vaše předplatné a zajistěte, aby že správné klienta je vybraná v rozevíracím seznamu. Pokud jste ověřit správnost požadované klienta, postupujte podle výše uvedených diagramu a ověřte mapování předplatného a tohoto klienta AAD.

## <a name="next-steps"></a>Další kroky

Přečtěte si víc o IoT sadu, najdete v článku jak se dá [Přizpůsobit předkonfigurované řešení][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
