
<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určení strategie životního cyklu hybridní identity | Microsoft Azure"
    description="Umožňuje definovat úlohy správy identit hybridní podle dostupné možnosti pro jednotlivé fáze životního cyklu."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Určení strategie životního cyklu hybridní identity
V tomto úkolu definujete strategie řízení identity pro vaše hybridní identity řešení požadavkům firmy, které jste definovali v [úlohy správy identit hybridní určení](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).


Definování úlohy správy identit hybridní podle životním cyklu začátku do konce identity prezentovány dříve v tomto kroku, je nutné zvážit dostupné možnosti pro jednotlivé fáze životního cyklu.

## <a name="access-management-and-provisioning"></a>Správa přístupu a zřízení
Pomocí řešení správy přístup dobré účtu vaší organizace můžete sledovat přesně kdo bude mít přístup k jakým informacím v organizaci.

Řízení přístupu je důležité funkce systému zřizovací centralizované, jednoho místa. Kromě chránit citlivé informace, přístup k ovládacím prvkům vystavit existující účty, které mají neschválené povolení nebo pokud je už není potřeba. Pro řízení zastaralé účty, zřizovací systém odkazy společném informací o účtu s autoritativní informace o uživatelích, kteří patří účty. Autoritativní uživatelích identity je obvykle zachovaná v databázích a adresářů lidských zdrojů.

Účty v sofistikované podniky IT obsahuje stovky parametrů, které definují orgány, a tyto podrobnosti můžete ho řídil zřizovací systému. Noví uživatelé mohou být identifikovány s daty, která poskytují z důvěryhodného zdroje. Funkce schválení žádosti o přístup zahájí procesů, které schválení (nebo odmítnutí) zdroje zřizování pro ně.


| Správa fází životního cyklu          | Místní nasazení                                                                                                                                                                                                                                                       | Cloud                                                                                                                                                                                                                                                                                                                     | Hybridní                                                                                   |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Správa účtu a zřízení | Pomocí roli server® Active Directory Domain Services (AD DS), můžete vytvořit scalable zabezpečené a spravovatelné infrastrukturu pro uživatele a řízení zdrojů a podporují adresáři aplikací, jako je Microsoft® Exchange Server. <br><br> [Zřízení skupiny ve službě AD DS prostřednictvím Správce Identity](https://technet.microsoft.com/library/ff686261.aspx) <br>[Zřizování uživatelů ve službě AD DS](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Můžete správci řízení přístupu spravovat přístup uživatelů k sdílených z bezpečnostních důvodů. Ve službě Active Directory je spravován řízení přístupu na úrovni objektu tak, že nastavení různé úrovně přístupu nebo oprávnění k objektům, například Úplné řízení, zapsat, číst nebo žádný přístup. Řízení přístupu ve službě Active Directory definuje jak jednotlivým uživatelům můžete použít objektů služby Active Directory. Ve výchozím nastavení jsou nastavené oprávnění k objektům ve službě Active Directory nejbezpečnější nastavení. | Budete muset vytvořte účet všem uživatelům, kteří budou přístup ke cloudové službě společnosti Microsoft. Můžete taky změnit uživatelské účty nebo odstraňte je, když jste už nepotřebujete. Ve výchozím nastavení uživatelé nemají oprávnění správce, ale můžete volitelně přiřadit. Další informace najdete v tématu [Správa uživatelů v Azure AD](active-directory-create-users.md). <br><br> V rámci služby Azure Active Directory je jednou z hlavních funkcí možnost spravovat přístup ke zdrojům. Tyto materiály může být součástí v adresáři, stejně jako v případě oprávnění ke správě objekty prostřednictvím rolí v adresáři nebo zdroje, které jsou mimo v adresáři, například SaaS aplikací, Azure služeb a Sharepointových webech nebo na místní zdroje. <br><br> Při přístupu Centrum z Azure Active Directory společnosti je řešení pro správu skupiny zabezpečení. Vlastníka zdroje (nebo správce v adresáři) můžete přiřadit skupinu, do určité přístup zprava prostředky, které vlastní. Členové skupiny se dozvíte přístup a vlastníka zdroje můžete udělit oprávnění ke správě seznamu členy skupiny někomu jinému – třeba oddělení správce nebo správce technickou podporu<br> <br> Správa skupin v tématu Azure AD poskytuje další informace o správě přístup prostřednictvím skupin.| Rozšíření služby Active Directory do cloudu prostřednictvím synchronizace a federace |

## <a name="role-based-access-control"></a>Řízení přístupu na základě rolí
Přístupu na základě rolí řídit (RBAC) používá role a zřízení zásady chcete zjistit, otestujte a prosazování obchodních procesů a pravidla pro udělení přístupu uživatelům. Klíčové správci vytvořit zřizovací zásady přiřadit role uživatelů a které definují sady nároky na materiály k těmto rolím. RBAC rozšiřuje řešení správy identit pomocí softwarová procesy a zmenšení uživatele ručních zřizovací procesu.
Azure AD RBAC umožňuje společnosti omezení počtu operacích, které jednotlivce můžete udělat, když má přístup k portálu Správa Azure. Pomocí RBAC k řízení přístupu k portálu správci IT ca přístup delegáta pomocí následujících přístupů řízení přístupu:

- **Přiřazení role skupina založená**: můžete přiřadit přístup Azure AD skupin, které můžete synchronizují z místní služby Active Directory. Umožňuje využívat existující investice, které vaše organizace provedla nástrojů a procesů pro správu skupin. Můžete taky použít funkci delegované skupiny správy z Azure AD Premium.
- **Využití integrované role v Azure**: můžete použít tři role – vlastník skupiny přispěvatelů a Reader, který je k zajištění uživatelům a skupinám oprávnění provádět pouze úkoly, kterou potřebují pro svou práci.
- **Granular přístupu k prostředkům**: můžete přiřadit role uživatelům a skupinám u určitého předplatného, skupina zdroje nebo konkrétního zdroje Azure například na webu nebo v databázi. Tímto způsobem můžete zajistit, aby uživatelé měli přístup k všechny zdroje, které potřebují a bez přístupu k prostředkům, které není nutné spravovat.

## <a name="provisioning-and-other-customization-options"></a>Vytváření a další možnosti úprav
Váš tým umožňuje plány pro firmy a požadavky rozhodnout, kolik přizpůsobení řešení totožnost. Například velké organizace může vyžadovat rozfázovaném zavádění plán pro pracovní postupy a vlastní adaptéry založené na časová osa pro postupně zřizování aplikace, které jsou velmi sloužícího zeměpisných. Jiný plán přizpůsobení může poskytovat dva nebo více aplikací pro zřízení v celé organizaci, po úspěšném testování. Interakce uživatelů aplikaci můžete přizpůsobit a postupy pro vytváření zdrojů lze změnit tak, aby zahrnoval automatizované zřizování.

Můžete deprovision odebrat služby nebo součásti. Zrušení zajišťování účet například znamená, že účet se odstraní ze zdroje.

Hybridní model zřizování zdroje kombinuje žádosti a na základě rolí přístupů, které podporuje Azure AD. Pro podmnožinu zaměstnanců a spravovaných systémů možná budete chtít na podnik automatizovat přístupu pomocí přiřazení na základě rolí. Obchodní mohou také úchyt pro všechny ostatní žádosti o přístup nebo výjimky prostřednictvím založené na žádost o modelu Některé společnosti může začínat ruční přiřazení a vyvíjet směrem k modelu hybridní s záměr nasazení plně na základě rolí v budoucnu.

Jiných společností může najděte nepraktické pracovních důvodů dosáhnout zřizování dokončení na základě rolí a obrázku hybridní přístup jako požadovaného cíl. Pořád jiných společností může být vyhovující zřizování pouze na základě žádost a nebudete chtít investovat další plánování řízené úsilí definovat a správa na základě rolí, automatické vytváření zásad.

## <a name="license-management"></a>Správa licencí
Správa licencí na základě skupiny v Azure AD umožňuje správcům přiřazení uživatelů do skupiny zabezpečení a Azure AD automaticky přiřadí licence všem členům skupiny. Pokud je uživatel následně přidat nebo odebrat ze skupiny, licence bude automaticky přiřazení nebo odebrání podle potřeby.

Synchronizovat ze skupiny můžete použít místní AD nebo spravovat v Azure AD. Párování to s Azure AD premium Samoobslužná správa skupiny můžete snadno delegovat přiřazení licencí pro příslušný rozhodování. Si můžete jistí, že problémy, například konfliktů licencí a chybějící data umístění automaticky řazení.

## <a name="self-regulating-user-administration"></a>Samoregulačního Správa uživatelů
Po spuštění organizaci zřízení zdroje přes všechny interní organizace implementovat samoregulačního možnost správy uživatelů. Hranice organizace můžete zaznamenat výhody a výhody zřizování uživatelů. V tomto prostředí změny ve stavu uživatele automaticky projeví ve přístupových práv přes hranice organizace a zeměpisných. Můžete omezit zřizovací náklady a zjednodušit procesy přístup a schválení. Provedení provádí potenciál provádění řízení přístupu na základě rolí pro správu přístupu začátku do konce ve vaší organizaci. Můžete snížit náklady na správu prostřednictvím automatické postupy pro upravující zřizování uživatelů. Můžete zlepšit zabezpečení díky automatizaci vynucení zásady zabezpečení a zjednodušit a soustředit Správa životního cyklu uživatelů a zdrojů zřizování pro velké uživatele populace.

>[AZURE.NOTE]
Další informace najdete v tématu Nastavení Azure AD pro správu samoobslužné aplikace access

Na základě licence (nárok na základě) Azure AD služeb práce aktivací předplatného ve vašem klientovi adresářové/služby Azure AD. Jakmile je aktivní předplatné možností služby můžete spravuje správce adresáře/služby a používat licencované uživatele. Další informace najdete v tématu Jak funguje Azure AD licencování práce?
Integrace se službou zprostředkovatelé jiných 3.

Azure Active Directory obsahuje jednotného přihlašování a rozšířené zabezpečení aplikace access do tisíce SaaS i místní webových aplikací. Podrobný seznam galerie aplikace služby Azure Active Directory pro podporovaných aplikací SaaS najdete v tématu Azure Active Directory federation kompatibility seznamu: Zprostředkovatelé identit třetích stran, kteří mohou sloužit k implementace jednotného přihlašování

## <a name="define-synchronization-management"></a>Definování řízení synchronizace
Integrace adresářů místní s Azure AD umožňuje uživatelům produktivnější poskytnutím běžné identity pro přístup ke cloudu a místní zdroje. Tento integrací uživatele a organizace můžete využít z následujících akcí:

- Uživatelé, kteří mají společné identity hybridní poskytne organizace přes místního nebo cloudového služeb využívání služby Active Directory pro Windows Server a potom připojení pro službu Azure Active Directory.
- Správci můžou poskytnout podmíněného přístupu na základě prostředku aplikace, zařízení a identita uživatele, umístění v síti a vícefaktorové ověřování.
- Uživatele můžete využít identitu běžné prostřednictvím účtů v Azure AD do Office 365, Intune, SaaS aplikace a aplikace jiných výrobců.
- Vývojáři můžete vytvářet aplikace, které běžné model identit integrace aplikace do služby Active Directory místní nebo Azure získáte cloudu

Na následujícím obrázku obsahuje příklad všeobecný přehled o synchronizaci identity.

![](./media/hybrid-id-design-considerations/identitysync.png)

Proces synchronizace identity

Přečtěte si v následující tabulce a umožňuje porovnávat možností synchronizace:

| Možnosti řízení synchronizace          | Výhody                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Nevýhody                                                                                                                                                                                                                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Na základě synchronizace (přes DirSync nebo AADConnect) | Uživatelé a skupiny synchronizují z místní a cloudu <br>  **Zásady řízení**: zásady účtů můžete nastavit pomocí služby Active Directory, která umožňuje správce Správa zásady pro hesla workstation, omezení, ovládací prvky uzamčení a další, aniž byste museli provádět další úkoly v cloudu.  <br>  **Řízení přístupu**: omezit přístup ke cloudové službě tak, aby služby můžete k nim získat přístup prostřednictvím podnikové prostředí prostřednictvím serverům online nebo obojí. <br>  Omezenou podporu volání: Pokud uživatelé méně hesla si zapamatovat, jsou menší pravděpodobně v žádném případě nezapomeňte je. <br>  Zabezpečení: Identity uživatele a informace jsou chráněny, protože všechny servery a služby používané v jednotné přihlašování, jsou standardní a řídit místní. <br>  Podpora pro silné ověřování: silné ověřování (nazývané také dvojúrovňové ověřování) můžete použít ke službě cloud. Ale pokud používáte silné ověřování, musí použijete jednotného přihlašování. |                                                                                                                                                                                                                                                                                                |
| Na základě federace (prostřednictvím služby AD FS)           | Povolit tak, že služba tokenů zabezpečení (Služba tokenů zabezpečení). Při konfiguraci STS poskytnout přístup přihlašování cloudové službě Microsoftu, bude vytvářet federovaný vztah důvěryhodnosti mezi místní STS a federované domény, kterou jste zadali do vašeho tenanta Azure AD. <br> Koncoví uživatelé používat stejnou sadu přihlašovacích údajů získat přístup k více zdrojů <br>koncoví uživatelé nemají udržovat více sad přihlašovacích údajů. Ještě uživatelé potřebovat zadejte své přihlašovací údaje pro každého z nich účastní prostředky., B2B B2C příklady použití podporované.                                                                                                                                                                                                                                                                                                                                                                                                             | Vyžaduje speciální pracovníci pro nasazení a údržba vyhrazené místní servery služby AD FS. Pokud budete chtít používat službu AD FS pro vaše Služba tokenů zabezpečení jsou omezení při používání silné ověřování. Další informace najdete v tématu [Konfigurace rozšířené možnosti pro službu AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

>[AZURE.NOTE]
Další informace najdete [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).


## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
