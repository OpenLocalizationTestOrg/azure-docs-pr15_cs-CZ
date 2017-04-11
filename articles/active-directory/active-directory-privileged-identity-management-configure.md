<properties
    pageTitle="Správa identit Azure AD oprávněními | Microsoft Azure"
    description="Téma, které vysvětluje, co je Azure AD privilegovaných Správa identit a používání osobních zlepšit cloudu zabezpečení."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Správa identit Azure AD oprávněními

S Azure Active Directory (AD) privilegovaných Správa identit můžete spravovat ovládací prvek a sledovat přístup ve vaší organizaci. Jedná se o přístupu k prostředkům v Azure AD a dalších online služeb společnosti Microsoft jako je Office 365 nebo Microsoft Intune.  

> [AZURE.NOTE] Privilegovaných Správa identit je k dispozici pouze v edici Premium P2 služby Azure Active Directory. Další informace najdete v tématu [edicí Azure Active Directory](active-directory-editions.md).

Organizace se snaží minimalizovat počet lidí, kteří mají přístup k zabezpečení informací nebo zdroje, protože, snižuje pravděpodobnost se zlými úmysly začíná tento přístup. Uživatelé dál potřebovat provádět privilegovaných operace v Azure, Office 365 nebo SaaS aplikace. Organizace poskytnout přístup privilegovaných uživatelů v Azure AD bez sledovat, co dělají tyto uživatele se jeho oprávnění správce. Správa identit Azure AD oprávněními pomůže vyřešit toto riziko.  

Správa identit Azure AD oprávněními vám pomůže:  

- Zjistit, které uživatelé jsou správci služby Azure AD
- Povolit na vyžádání "time" pro správu přístup ke službám Microsoft Online Services, jako je Office 365 a Intune
- Získání sestav o zjišťování Správce zobrazení historie a změny přiřazení správce
- Získat oznámení o přístup k roli privilegovaných

Správa identit privilegovaných služby Azure AD můžete spravovat vestavěné organizační role Azure AD, včetně:  

- Globální správce
- Správce fakturace
- Správce služeb  
- Správce uživatelů
- Správce hesel

## <a name="just-in-time-administrator-access"></a>Pouze v aplikaci access správce času

Z historických důvodů mohli byste přiřadit uživatele roli správce prostřednictvím portálu pro Azure klasické nebo prostředí Windows PowerShell. Výsledkem je stane jako **trvalé správce**, vždy aktivní v přiřazenou roli. Správa identit Azure AD oprávněními zavádí koncept **měl správce**. Možný Správce by měl být uživatelé, kteří potřebují mít přístup privilegovaných a poté, ale ne každý den. Role není aktivní, dokud uživatel potřebuje přístup, a pak dokončete proces aktivace a stát se správcem aktivní pro přednastaveném počtu čas.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Povolení privilegovaných Správa identit adresáře

Můžete začít používat Azure AD privilegovaných Správa identit [Azure portálu](https://portal.azure.com/).

>[AZURE.NOTE] Musí být globálním správcem s účtem organizace (například @yourdomain.com), není svým účtem Microsoft (například @outlook.com), povolit Azure AD privilegovaných Správa identit pro adresář.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace víc než jeden adresář, vyberte svoje uživatelské jméno v pravém horním rohu portálu Azure. Vyberte adresář, které budete používat Azure AD privilegovaných Správa identit.
3. Vyberte **Další služby** a pomocí textového pole filtru můžete vyhledávat **Azure AD privilegovaných Správa identit**.
4. **Připnout do řídicího panelu** a potom klikněte na **vytvořit**. Otevře se aplikace privilegovaných správy identit.

Pokud jste kdy se první osoba používat Azure AD privilegovaných Správa identit v adresáři, pak [Průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md) vás provede prostředí počáteční přiřazení. Až to automaticky změní první **Správce zabezpečení** a **privilegovaných role správce** v adresáři.

Pouze privilegovaných role správce dá řídit přístup pro dalšími správci. Můžete [ostatním uživatelům umožnit spravovat správce osobních informací](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Privilegovaných Správa identit řídicího panelu

Správce identit Azure AD oprávněními poskytuje řídicího panelu poskytující důležité informace, jako je třeba:

- Upozornění, které poukázat příležitosti ke zlepšení zabezpečení
- Počet uživatelů přiřazených k jednotlivým rolím privilegovaných  
- Počet měl a trvalé správci
- Recenze průběžného přístupu

![Řídicího panelu Správce osobních informací – snímek][2]

## <a name="privileged-role-management"></a>Správa privilegovaných rolí

S Azure AD privilegovaných správy identit můžete ovládat správci přidávání a odebírání správců trvalých nebo měl k jednotlivým rolím.

![Správci přidat nebo odebrat správce osobních informací – snímek][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurace nastavení aktivace rolí

Použití [Nastavení role](active-directory-privileged-identity-management-how-to-change-default-settings.md) lze konfigurovat vlastnosti aktivace měl role včetně:

- Aktivace období rolí
- Oznámení o aktivaci rolí
- Informace, které uživatel musí zadat během procesu aktivace rolí  

![Snímek obrazovky nastavení – správce aktivace – správce osobních informací][4]

Všimněte si, že tlačítka pro **Vícefaktorové ověřování** na obrázku zakázány. U některých, vysoce privilegované role, jsme vyžadují MFA zesílený ochranu.

## <a name="role-activation"></a>Aktivace rolí  

[Aktivace roli](active-directory-privileged-identity-management-how-to-activate-role.md)měl správce požadavky čas – mez "aktivace" rolí. Aktivace mohou být požadována možnost **Aktivovat Moje role** v Azure AD privilegovaných Správa identit.

Kdo chce pro aktivaci role správce musí inicializace Azure AD privilegovaných Správa identit Azure portálu.

Aktivace role je přizpůsobit. V části Nastavení správce osobních informací můžete určit délku aktivačním kódem a co informace správce musí zajistit aktivovat roli.

![Aktivace role správce osobních informací správce žádost – snímek][5]

## <a name="review-role-activity"></a>Revize role aktivity

Existují dva způsoby, jak sledovat, jak se privilegovaných role používají zaměstnanců a správce. První možnost používá [auditování historie](active-directory-privileged-identity-management-how-to-use-audit-log.md). Historie auditu protokoly sledování změn v privilegovaných přiřazování a rolí aktivace historie.

![Správce osobních informací aktivace historie - snímek][6]

Druhou možností je nastavení pravidelných [kontroluje přístup](active-directory-privileged-identity-management-how-to-start-security-review.md). Tyto recenze přístupu můžete provedly a přiřazené Revidující (třeba týmu) nebo zaměstnanců můžete zkontrolovat sami. Toto je nejlepší způsob, jak sledovat kdo stále vyžaduje přístup a kdo už dělá.


## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
