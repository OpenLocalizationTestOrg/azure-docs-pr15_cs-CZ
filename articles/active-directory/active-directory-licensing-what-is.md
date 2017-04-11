<properties
    pageTitle="Co je licence Microsoft Azure Active Directory? | Microsoft Azure"
    description="Popis licencování Microsoft Azure Active Directory, jak na to, jak začít a doporučené postupy, včetně služeb Office 365, Microsoft Intune a Azure Active Directory Premium a Základní edice"
    services="active-directory"
      keywords="Azure AD licencování"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Co je licence Microsoft Azure Active Directory?

##<a name="description"></a>Popis
Azure Active Directory (Azure AD) je Identity společnosti Microsoft jako řešení služby (IDaaS) a platformy. Azure AD je k dispozici v počet funkční a technické verze od Azure AD volno, což je dostupné pro všechny služby Microsoft jako je Office 365, Dynamics, Microsoft Intune a Azure (Azure AD nevytvoří všechny spotřebu poplatky v tomto režimu), Azure AD zaplatili verzemi třeba Enterprise mobilitu sadu EMS (), Azure AD Premium a Basic, jakož i Azure Multi-Factor Authentication (MFA). V mnoha službám Microsoft online services, jako je většina Azure AD zaplacený verze jsou poskytovány uživatelská oprávnění jsou v Office 365, Microsoft Intune a Azure AD. V těchto případech služby nákupu představuje u jednoho nebo víc předplatných a každý předplatné zahrnuje před nákupem počet licencí ve vašem klientovi. Nároky na uživatele se dosahuje prostřednictvím přiřazení licencí, vytvoření propojení mezi uživatelem a produkt, povolení součástí služby pro uživatele a jinými jednu předplacený licence.

[Zkuste Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Portál Správa Azure AD je součástí Azure klasické portál. Při použití Azure AD nevyžaduje žádný Azure nákup, přístup k tomuto portálu vyžaduje aktivní předplatné Azure nebo [Azure zkušební předplatné](https://azure.microsoft.com/pricing/free-trial/).

Obecných přehled funkcí služby Azure AD najdete v článku [Co je Azure AD](active-directory-whatis.md).
[Další informace o úrovních služby Azure AD](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Předplatná Azure platba podle příjmů se liší: když taky představující v adresáři, těchto předplatných povolit vytváření Azure zdrojů a namapujte je na způsob platby. V tomto případě se žádná licence počty přidružený k předplatnému. Přidružení uživatelů k předplatnému, přístup uživatelů k řízení zdrojů předplatné dosáhnete udělením oprávnění k ovládání na Azure zdroje namapované k předplatnému.


##<a name="how-does-azure-ad-licensing-work"></a>Jak funguje Azure AD licencování

Na základě licence (nárok na základě) Azure AD služeb práce aktivací předplatného ve vašem klientovi adresářové/služby Azure AD. Po aktivní předplatné můžete možností služby spravuje správci adresáře/služeb a používat licencované uživatele.

Při nákupu nebo aktivaci Enterprise mobilita sadu Azure AD Premium nebo Azure AD základní adresáři se aktualizuje k předplatnému, včetně jeho platnosti a předplacený licence. Vaše informace o předplatném včetně stavu, další událost životního cyklu a počet licencí přiřazené nebo k dispozici je k dispozici prostřednictvím portálu Azure klasický klikněte v části licence pro určitých adresáři. Toto je také na vhodné místo pro správu přiřazení licencí.

Každého předplatného se skládá z jedné nebo více služby plánů, každé mapování úroveň však započítávány funkčnosti typu služby; například Azure AD Azure MFA, Microsoft Intune, Exchange Online nebo SharePoint Online. Správa licencí Azure AD nevyžaduje Správa úrovně plán služeb. Tím se liší od Office 365, který závisí na tento režim rozšířené konfigurace spravovat přístup k zahrnutých službách. Azure AD závisí na v konfiguraci služby k povolení funkcí a správě jednotlivých oprávnění.

Informace o předplatném Azure AD se obecně spravuje Azure klasické portálu, klikněte v části licence pro určitý adresář. Azure AD předplatná, s výjimkou Azure AD Premium, se nezobrazí v portálu Microsoft Office.

> [AZURE.IMPORTANT] Azure AD Premium a Basic a Enterprise mobilita sadu předplatná se omezuje na jejich zřizování adresáře/klienta. Předplatná nelze rozdělení mezi adresáři nebo slouží k nárok uživatelů v ostatních adresářů. Přesouvání mezi adresářů předplatné je možné, ale vyžaduje poskytnutí požadavek podpory můžete nebo zrušení a znovu zakoupit v případě přímý nákup.

> Při nákupu Azure AD nebo Enterprise mobilita sadu prostřednictvím multilicenčního programu Aktivace předplatného proběhne automaticky při smlouvu obsahuje jiných služeb Microsoft Online, například Office 365.

Placená Azure AD funkce rozsahu šířka adresář. Jako příklad lze uvést:
- Skupina založená přiřazení k aplikacím, které jsou povoleny v dané aplikace, které spravujete.
- Pokročilé a možnosti správy samoobslužné skupiny jsou k dispozici v adresáři konfiguraci nebo konkrétní skupinou.
- Na kartě zasílání zpráv o chybách jsou Premium zabezpečení sestavy
- Zjišťování aplikací cloudu se objeví v portálu Azure identitou.

###<a name="assigning-licenses"></a>Přiřazování licencí
Získání předplatné je vše, co potřebujete ke konfiguraci placené funkcí, pomocí svého Azure AD zaplacený funkce vyžaduje distribuci licencí, které chcete doprava osob. Obecně všichni uživatelé, kteří mají mít přístup k nebo kteří se spravuje přes Azure AD vyplacené funkce musí mít přiřazené licence. Přiřazení licencí je mapování mezi uživatelem a placené služby, jako je Azure AD Premium, základní nebo Suite mobilita organizace.

Správa určující, kteří uživatelé v adresáři vašeho by měl mít licenci je velmi jednoduché. Lze provést přiřazením chcete skupinu vytvořit pravidla přiřazení pomocí portálu Správa Azure AD nebo přiřazením licencí přímo do doprava osob prostřednictvím portálu, prostředí PowerShell nebo rozhraní API. Po přiřazení licencí do skupiny, všem členům skupiny přiřadíte licenci. Pokud uživatelé po přidání nebo odebrání ze skupiny bude přiřazeno nebo odebrání odpovídající licenci. Přiřazení skupiny můžete použít jakékoli dostupné Správa skupiny a je v souladu s přiřazení Skupina založená na aplikace. Použití poddotazu můžete nastavit pravidla tak, aby se všechny uživatele v adresáři se automaticky získá, zajistit, aby všichni s odpovídající funkci licenci nebo dokonce delegovat rozhodnutí o dalších správců v organizaci.

Přiřazení licence skupinový všichni uživatelé chybí místa využívání zdědí umístění adresáře během přiřazení. Toto umístění můžete kdykoli změnit správcem. V případech, kdy automatické přiřazení selže kvůli chybě budou obsahovat informace o uživateli podle typu licence státu.

##<a name="getting-started-with-azure-ad-licensing"></a>Začínáme s Azure AD licencování

Začínáme s Azure AD je jednoduché, můžete kdykoli vytvořit adresáře jako součást registruje k bezplatná Azure zkušební verzi. [Další informace o přihlašování jako organizace](sign-up-organization.md). Následující vám můžou pomoct Ujistěte se, že adresáři nejlépe zarovnán další služby od Microsoftu může jinými nebo plánujete využívat a vaše cíle na získání službu.

Tady je několik doporučené postupy:
- Pokud už používáte organizační služeb společnosti Microsoft, máte už adresáře služby Azure AD. V tomto případě by měl dál používat stejnou adresáře zaměstnavatele s ostatními službami tak, aby Správa identit základní, včetně zřizování a hybridní jednotné přihlašování, můžete využít přes služby. Uživatelé budou mít jednoho přihlašování a budou využívat bohatší funkce přes služby. Jako výsledek Pokud se rozhodnete koupit Azure AD zaplacený služby pro pracovníků, doporučujeme provést pomocí stejného adresáře.
- Pokud se chystáte používat Azure AD pro jinou skupinu uživatelů (partnery zákazníci a tak dál) nebo pokud chcete vyhodnocení služby Azure AD a chtěli to udělat v izolace služby Výroba nebo pokud hledáte nastavení prostředí izolovaného prostoru služeb, doporučujeme, abyste nejprve vytvořit nový adresář prostřednictvím portálu klasické Azure Azure. [Další informace o vytvoření nového Azure AD adresáře Azure klasické portálu](active-directory-licensing-directory-independence.md). Nový adresář se vytvoří pomocí svého účtu jako externího uživatele s oprávněními globálního správce. Když se přihlásit k portálu Azure klasické k tomuto účtu, budete moct zobrazit tento adresář a zpřístupnit všechny úlohy správy adresář. Doporučujeme, abyste si vytvořili místního účtu s příslušná oprávnění ke správě další služby od Microsoftu (ty, není k dispozici prostřednictvím portálu Azure klasické). [Další informace o vytváření uživatelských účtů v Azure AD](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD podporuje "externích uživatelů," kterých jsou uživatelské účty v instanci Azure AD vytvořené pomocí účtu Microsoft účtu (MSA) nebo identitu Azure AD z jiného adresáře. Během jsme zaneprázdněné rozšíření tuto funkci do všech organizační služeb společnosti Microsoft, teď tyto účty nejsou podporované v některé z této služby prostředí; Příklad portálu správy Office 365 aktuálně nepodporuje tyto uživatele. V důsledku toho externí uživatelé s oprávněními Microsoft nebude moct získat přístup k portálu správy Office 365 vůbec, zatímco externích uživatelů z ostatních adresářů Azure AD budou ignorovat. V takovém případě pouze uživatele místního účtu, Azure AD nebo Office 365 adresáře, kde uživatel byla původně vytvořen budou dostupné prostřednictvím těchto prostředí.

Jak je uvedeno, Azure AD obsahuje různé verze na placené. Tyto verze mají některé vedlejší rozdíly v jejich dostupnost nákupu:


| Produktu   | EA/VL     | Otevřít  |   CSP |   Práva k užívání MPN  |   Přímý nákup | Zkušební verze |
|---|---|---|---|---|---|---|
| Organizace – mobilita Suite |   X | X | X | X |  |      X |
| Azure AD Premium  | X | X | X |   | X | X |
| Azure AD Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Vyberte jeden nebo víc licencí pokusů
 Ve všech případech je možné aktivovat Azure AD Premium nebo Enterprise mobilita sadu zkušební předplatné tak, že vyberete konkrétní zkušební verze, který chcete na kartě licencí ve vašem adresáři. Buď zkušební verze obsahuje 30denní předplatné 100 licencí.

![Plány zkušební licenci Azure Active Directory](./media/active-directory-licensing-what-is/trial_plans.png)

![Plány zkušební licenci Enterprise mobilita Suite](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Plány aktivní zkušební licenci](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Přiřazovat licence
Když je aktivní předplatné, by měl přiřadit licenci sami sobě a aktualizujte prohlížeč zajistit, aby že se vám zobrazuje všechny funkce. Dalším krokem je chcete přiřadit licence uživatelů, které bude nutné získat přístup k nebo nezahrnou zaplacený Azure AD funkce. Jsme výše uvedené v "Přiřazení licencí", je nejlepší způsob, jak to udělat identifikovat skupině představující požadovaný cílové skupiny a přiřaďte licence. Tímto způsobem uživatelé, kteří jsou přidat i odebrat ze skupiny prostřednictvím svého životního cyklu přiřazená nebo odebrali licence.

Pokud chcete přiřadit licenci skupiny nebo jednotlivým uživatelům, vyberte plán licence, kterou chcete přiřadit a na panelu s příkazy klikněte na **přiřadit** .

![Plány aktivní zkušební licenci](./media/active-directory-licensing-what-is/assign_licenses.png)

V dialogovém okně přiřazení pro vybraný plán můžete jednou vybrat uživatele a jejich přidávání nastavili **přiřadit** sloupec na pravé straně. Můžete procházet uživatelů ze seznamu nebo hledání konkrétních osob pomocí při hledání sklo horní doprava zájmů uživatele. Přiřadit skupiny, vyberte "Skupiny" v nabídce **Zobrazit** a klikněte na tlačítko Zkontrolovat na pravé straně aktualizovat přiřazení, která se zobrazí.

![Přiřazování licencí do skupin](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Teď můžete hledat nebo procházet skupiny a přidejte je do sloupce **přiřadit** stejným způsobem. Můžete tyto přiřadit kombinaci uživatelů a skupin v jedné operace. Dokončete proces přiřazení kliknutím na tlačítko zaškrtněte v pravém dolním rohu stránky.

![Zpráva o postupu přiřazení licence](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Je-li skupinu přiřazen, jeho členy dědit licencí než 30 minut, ale obvykle během několika minut 1-2.

Chyby přiřazení může dojít během přiřazení licencí Azure AD, ale jsou relativně zřídka. Potenciální chyby přiřazení jsou omezené na:
- Přiřazení konflikt – Pokud uživatel dříve přiřazené licence, která není kompatibilní s aktuální licenci. V tomto případě přiřazení nových licencí vyžaduje odebírání na předchozí snímek.
- Překročená dostupné licence – Pokud se počet uživatelů přiřazených skupinám překročit dostupné licence, budou obsahovat stavu přiřazení uživatelů selhání přiřazení z důvodu chybějící licence.

###<a name="view-assigned-licenses"></a>Zobrazení přiřazené licence

Souhrnné zobrazení přiřazené licence včetně události životního cyklu předplatného dostupný přiřazené a další se zobrazí na kartě **licence** .

![Zobrazení počtu přiřazené licence](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Podrobný seznam přiřazených uživatelům a skupinám, včetně přiřazení stavu a cesta (přímé nebo zděděných z jedné nebo více skupin) je k dispozici při přechodu do plánu licenci.

![Zobrazení podrobností o licence přiřazené licence plánu](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Odebrání licence je zrovna tak jednoduché, jako je přiřazení. Pokud je uživatel přímo přiřazen nebo přiřazené skupiny, můžete odebrat licenci Výběr typu licence, vyberte **Odebrat**, přidání uživatele nebo skupinu, do seznamu odebrat a potvrzení akce. Můžete taky můžete otevřete typu licence, vybrat určitého uživatele nebo skupiny a klepněte na **Odebrat** na panelu s příkazy. Ukončit dědění uživatele licence ze skupiny, jednoduše odeberte uživatele ze skupiny.

###<a name="extending-trials"></a>Rozšíření pokusů

Vyzkoušení rozšíření zákazníci jsou k dispozici jako samoobslužných funkcí pomocí portálu Office 365. Jako správce zákazníka můžete přejít na [portál Office](https://portal.office.com/#Billing) (přístup závisí na oprávněních k portálu Microsoft Office) a vyberte zkušební verzi Azure AD Premium. Klikněte na odkaz **prodloužit zkušební verzi** a postupujte podle pokynů. Budete muset zadat platební kartou, ale nebude platit.

![Rozšíření zkušební licenci na portálu Office](./media/active-directory-licensing-what-is/extend_license_trial.png)

Zákazníci taky můžete požádat o prodloužení zkušebního období tím, že odešlete žádost o podporu. Správce zákazníka můžete přejít Office 365 portálu [Stránka podpory](http://aka.ms/extendAADtrial) (přístup závisí na oprávněních pro stránku podpory Office). Na této stránce vyberte "Předplatná a pokusy" ve skupinovém rámečku funkce a "Zkušební verze otázky" v části příznak. Nakonec zadejte informace o případech

![Rozšíření zkušební licenci pomocí žádost o podporu](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Další kroky

Teď budete pravděpodobně chtít konfigurace a používání některých Azure AD prémiových funkcí.

- [Resetování hesla samoobslužných funkcí](active-directory-manage-passwords.md)
- [Správa skupiny samoobslužných funkcí](active-directory-accessmanagement-self-service-group-management.md)
- [Zdraví Azure AD Connect](active-directory-aadconnect-health.md)
- [Přiřazení skupiny aplikací](active-directory-manage-groups.md)
- [Azure Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication.md)
- [Přímý nákup licencí Azure AD Premium](http://aka.ms/buyaadp)
