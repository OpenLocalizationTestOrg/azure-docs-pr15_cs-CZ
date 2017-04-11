<properties
  pageTitle="Správa přístupu k aplikacím Azure AD pomocí |  Microsoft Azure"
  description="Popisuje, jak Azure Active Directory umožňuje organizacím určit aplikace, ke kterým má každý uživatel přístup."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Správa přístupu k aplikacím

Správu průběžného přístupu hodnocení použití a vytváření sestav nadále výzvu po aplikace je součástí systému identity vaší organizace. V mnoha případech muset udělat probíhající aktivní úlohu při správě přístup ke svým aplikacím správce IT nebo technickou podporu. V některých případech přiřazení provést týmem obecné nebo oddělení IT. Často se má rozhodnutí přiřazení delegovat obchodní rozhodnutí makeru vyžadování schválení před něco v ní IT přiřazení.  Jinými organizacemi investovat při integraci s existující automatického identita a přístup systému správy, jako řízení přístupu na základě rolí (RBAC) nebo pomocí řízení přístupu na základě atribut (ABAC). Integrace a vývoj pravidla jsou obvykle specializované a drahé. Sledování nebo po nahlášení na některý z řízení přístupu je investiční samostatné nákladné a složité.

## <a name="how-does-azure-active-directory-help"></a>Jak pomáhá Azure Active Directory?

 Azure AD podporuje správa rozsáhlých přístupu pro nakonfigurované aplikace umožňuje organizacím snadno dosáhnout zásady správné přístupu od přiřazení automatické, podle atributů (ABAC nebo RBAC scénáře) delegováním a včetně správu. S Azure AD můžete snadno dosáhnout složité zásady kombinace více modely správy pro jednu aplikaci a můžete dokonce opakované použití pravidel pro správu v aplikacích s stejné cílové skupiny.

 - [Přidat nové nebo existující aplikace](active-directory-sso-integrate-saas-apps.md)


 Přiřazení aplikace Azure AD se zaměřuje na dva způsoby primární přiřazení:

- **Jednotlivé přiřazení** Správce IT s oprávněními správce globální adresář můžete vybrat jednotlivé uživatelské účty a udělit jim přístup k aplikaci.
- **Přiřazení na základě skupiny (zaplacené Azure AD)** Správce IT s oprávněními správce globální adresář můžete přiřadit skupiny aplikace. Přístup k určitým uživatelům, je určený podle toho, zda jsou členy této skupiny při pokusu o přístup k aplikaci. Jinými slovy správce můžete efektivně vytvářet pravidlo přiřazení informující o "každý aktuální člen skupiny přiřazené má přístup k aplikaci". Pomocí této možnosti přiřazení správci můžete využít některé možností Azure AD skupiny správy, včetně [podle atributů dynamické skupiny](active-directory-accessmanagement-manage-groups.md), externího systému skupiny (například na adresářová služba Active Directory nebo Workday) nebo správce Správa přístupových práv nebo ztlumí-provoz Správa přístupových práv skupin. Jedinou skupinu můžete snadno přidělovat více aplikací zajistit, že aplikace s přiřazení spřažení můžete sdílet pravidla přiřazení snížení složitost celkové správy. Dejte pozor, které vnořené skupiny, které nejsou podporované členství pro přiřazení Skupina založená na aplikace v současné době.

Pomocí těchto dvou přiřazení režimů, správci dosáhnete všechny žádoucí přiřazení řízení přístupu.

S Azure AD použití a vytváření sestav funkce přiřazení je plně integrovaný, umožňuje správcům snadno vykázat stav přiřazení, přiřazení chyb a dokonce i použití.

## <a name="complex-application-assignment-with-azure-ad"></a>Přiřazení složité aplikace s Azure AD

Zvažte aplikaci, třeba služby Salesforce. V mnoha organizacích služby Salesforce primárně používají pracovníci organizace marketingu a prodeje. Často členové marketingového týmu mají vysoce výsadní přístup k služby Salesforce, zatímco členové prodejní tým mají omezený přístup. V mnoha případech obecných populaci pracovníci pracující s informacemi mají omezený přístup k aplikaci. Výjimky následujícími pravidly zvětšit věcech. Je často výsadního vedoucí týmů marketingu nebo prodeje chcete udělit přístup uživatelů nebo změnit jejich role nezávisle na tyto obecné pravidla.

S Azure AD může být aplikacích, například služby Salesforce předkonfigurovaná pro jednotné přihlašování (SSO) a automatické zřizování. Po konfiguraci aplikace Správce může trvat jednorázové akci, kterou chcete vytvořit a přiřadit příslušné skupiny. V tomto příkladu by mohl správce spustit následující přiřazení:

- [Dynamické skupiny](active-directory-accessmanagement-manage-groups.md) je to možné definovat automaticky pro všechny členy týmů marketingu a prodeje pomocí atributů jako oddělení nebo role:

    - Všichni členové marketingové skupin by přiřazené k roli "marketingové" v služby Salesforce

    - Všichni členové prodejní tým přiřazené skupin by být "prodej" role v služby Salesforce Další vylepšení použít více skupin, které představují místní prodejní týmy přiřazená k rolím různé služby Salesforce.

- Povolit mechanismus výjimek, nelze vytvořit skupinu samoobslužných funkcí pro každou roli. Jako skupinu samoobslužné lze například vytvořit skupinu "Služby Salesforce marketing výjimce". Skupiny můžete přidělovat marketingové rolí služby Salesforce a marketingového týmu vedoucí postavení půjde vlastníci. Členové týmu marketingové vedoucí postavení může přidat nebo odebrat uživatele, nastavení zásad spojení nebo i schválit nebo zamítnout jednotliví uživatelé žádosti o připojení. Toto je podporováno prostřednictvím informace pracovníka příslušné možnosti nevyžaduje zvláštní vzdělávání vlastníky a členy.

V tomto případě všech přiřazených uživatelům by být automaticky zřízení do služby Salesforce, jako se přidají do různých skupin, do kterých jejich přiřazování rolí by aktualizují v služby Salesforce. Uživatelé by nebudou moct prozkoumat a přístup k služby Salesforce prostřednictvím aplikace access panelech systému Microsoft Office webových klientů, nebo i tak, že přejdete na jejich organizace přihlašovací stránku služby Salesforce. Správci by mohli snadno zobrazit stav použití a přiřazení pomocí Azure AD vykazování.

Správci můžou využívat [Azure AD podmíněného přístupu](active-directory-conditional-access.md) k nastavit zásady přístupu pro konkrétní rolí. Tyto zásady může obsahovat, zda je povolen přístup mimo podnikovém prostředí a dosáhnout přístupu v různých případech i Vícefaktorové ověřování nebo zařízení požadavky.

## <a name="how-can-i-get-started"></a>Jak můžu začít?

První, pokud už nepoužíváte Azure AD a vy jste správce IT:

 - [Vyzkoušejte si to!](https://azure.microsoft.com/trial/get-started-active-directory/) -registraci bezplatnou 30denní zkušební verzi a nasazení prvního cloudové řešení za v části 5 minut pomocí tohoto odkazu

Azure AD k funkcím podporující nástroj sdílení účtu, patří:

- [Přiřazení skupiny](active-directory-accessmanagement-self-service-group-management.md)
- Přidání aplikací na Azure AD
- Začínáme s přiřazení
- Nejčastější dotazy týkající se přiřazování aplikací
- [Sestavy řídicího panelu použití aplikace](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Kde můžu najít další?

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Ochrana aplikace s podmíněným přístupem](active-directory-conditional-access.md)
- [Samoobslužné skupiny správy/SSAA](active-directory-accessmanagement-self-service-group-management.md)
