<properties
   pageTitle="Jak aplikace se přidají do služby Azure Active Directory."
   description="Tento článek popisuje, jak se přidávají aplikací na instanci služby Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Jak a proč aplikace se přidají do Azure AD

Jeden z původně matoucím kroků při prohlížení seznamu aplikací v instanci služby Azure Active Directory je pochopení aplikace odkud a proč je je k dispozici.  Tento článek poskytuje nejvyšší úrovně základní informace o tom, jak aplikace představují v adresáři a poskytnout kontext, které vám pomohou porozumět tomu, jak jste dostali v adresáři aplikace.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Na jaké služby Azure AD poskytuje aplikacím?

Aplikace se přidají do Azure AD můžete využít jednu nebo více služby, které poskytuje.  Zahrnout tyto služby:

* Aplikace a tak mohli ověřovat
* Ověřování uživatelů a ověření
* Jednotné přihlašování (SSO) pomocí federace nebo heslo
* Zřizování uživatelů a synchronizace
* Řízení přístupu na základě rolí; Použití adresáře definovat role aplikací k provádění rolí založeny ověření kredibility v aplikaci.
* služby autorizace oAuth (používané v Office 365 a dalších aplikací Microsoft povolit přístup k rozhraní API/zdrojů.)
* Publikování aplikací a proxy; Publikování na Internetu aplikace ze privátní sítě

## <a name="how-are-applications-represented-in-the-directory"></a>Jak reprezentuje aplikací v adresáři?

Aplikace představují v Azure AD pomocí 2 objekty: objekt aplikace a služby objekt.  Je jeden objekt aplikace registrované doma"" / "vlastník" nebo "publikování" adresář a jeden nebo více služby základní objekty, které představují aplikace v každém adresáři, ve kterém funguje.  

Objekt aplikace popisuje aplikace na Azure AD (služba více klienta) a může neobsahoval žádný z těchto věcí: (*Poznámka*: Toto není vyčerpávající.)

* Názvu, loga a aplikace Publisher
* Tajemství (symetrickou a/nebo asymetrické klíče slouží k ověření aplikace)
* Rozhraní API závislostí (oAuth)
* Rozhraní API/zdroje/obory publikované (oAuth)
* Role aplikace (RBAC)
* Jednotné přihlašování metadata a konfigurace (SSO)
* Uživatel zřizování metadat a konfigurace
* Proxy metadata a konfigurace

Jistina služby je záznamu aplikace v každé adresáře, kde aplikace funguje, včetně domácí adresář.  Hlavní služby:

* Odkazuje na objekt aplikace prostřednictvím vlastnost id aplikace
* Záznamy místní uživatelské účty a přiřazování rolí aplikace skupiny
* Oprávnění pro místní uživatele a správce záznamů poskytnuté do aplikace
    * Příklad: oprávnění pro aplikaci pro přístup k e-mailu konkrétní uživatele
* Záznamy místní zásady včetně podmíněné přístupu
* Místní alternativní místní nastavení záznamů pro aplikaci
    * Nároky transformace pravidel
    * Mapování atributů (zřizování uživatele)
    * Klient určitou aplikaci role (Pokud aplikace podporuje vlastní role)
    * Název/loga

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Příklad některých dalších funkcí objekty aplikace a služby objekty napříč adresáře

![Diagram znázorňující, jak aplikace objekty a objekty stávající s Azure AD instancí služby.][apps_service_principals_directory]

Jak vidíte v diagramu nahoře.  Microsoft udržuje dva adresáře interně (vlevo) používala publikování aplikací.

* Jeden pro Microsoft Apps (adresáře služeb Microsoft)
* Jeden pro předem integrované aplikace 3 stran (Galerie aplikací adresář)

Aplikace vydavatelích/dodavatelů integrace s Azure AD musí mít publikování adresář.  (Adresář některé SAAS).

Aplikace, které můžete přidat sebe, patří:

* Aplikace, kterou jste vytvořili (integrovaný s AAD)
* Aplikace připojení pro jednotného přihlašování
* Aplikace se publikována proxy server Azure AD aplikace.

### <a name="a-couple-of-notes-and-exceptions"></a>Několik poznámek a výjimky

* Ne všechny objekty služby přejděte na příkaz zpět objekty aplikace.  Huh? Jakmile původně vytvořenou Azure AD poskytované aplikace byly mnohem omezenější a jistinu služby dostatečné pro vytvoření identitu aplikace.  Původní služby základní byl uvnitř obrazce blíž k účtu služby Windows Server Active Directory.  Z toho důvodu je dál možné vytvořit objekty služby pomocí prostředí PowerShell Azure AD dokud nevytvoříte objekt aplikace.  Rozhraní API graf vyžaduje objekt aplikace před vytvořením služby základní.
* Všechny informace jsme je popsali výše se nyní zobrazí programově.  Toto jsou k dispozici pouze v uživatelském rozhraní:
    * Nároky transformace pravidel
    * Mapování atributů (zřizování uživatele)
* Další podrobné informace o jistinu služby a objekty aplikace naleznete v dokumentaci rozhraní REST API Azure AD grafu.  *Tip*: si přečtěte následující dokumentaci rozhraní API Azure AD graf je nejbližší věc na odkaz schématu pro Azure AD, která je aktuálně dostupná.  
    * [Aplikace](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Hlavní služby](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Jak se přidávají aplikace Moje Azure AD instanci?
Kterou aplikaci můžete přidat Azure AD mnoha způsoby:

* Přidat aplikaci z [Galerie aplikací Azure Active Directory](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Znaménko nahoru nebo do 3rd aplikace jiných výrobců integrovaný s Azure Active Directory (například: [Smartsheet](https://app.smartsheet.com/b/home) nebo [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Během znaménko nahoru/uživatelům se zobrazí dotaz, a udělte oprávnění pro aplikaci pro přístup k jejich profilu a jiná oprávnění.  Kdy se první osoba udělit souhlas způsobí, že služba jistinu představující aplikaci přidat do adresáře.
* Přihlaste se nahoru nebo na webu Microsoft online services, jako je [Office 365](http://products.office.com/)
    * Když pořídí předplatné Office 365 nebo začít zkušební verzi nebo další služby objekty vytvořené v adresáři představující různé služby, které se používá k poskytování všechny funkce spojený s Office 365.
    * Některých služeb Office 365 jako SharePoint vytvořit objekty služeb na základě probíhající umožňuje zabezpečená komunikace mezi součástmi včetně pracovní postupy.
* Přidání aplikace vyvíjíte v portálu pro správu Azure viz: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Přidání, najdete v článku aplikace, které vyvíjíte pomocí aplikace Visual Studio:
    * [Metody ověřování ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Připojené služby](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Přidání aplikace používání [Proxy aplikace Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx) pomocí
* Připojení aplikace pro jednotné přihlašování pomocí SAML nebo heslo jednotné přihlašování
* Dalších včetně různých prostředí developer v Azure/in a rozhraní API aplikace explorer dojde přes centra pro vývojáře

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kdo má oprávnění přidávat aplikace Moje Azure AD instanci?

Jenom globální správci tyto možnosti:

* Přidání aplikací z Galerie aplikace Azure AD (předem integrované 3 stran aplikace)
* Publikování aplikace pomocí Azure Proxy AD aplikace

Všichni uživatelé ve vašem adresáři oprávnění přidávat aplikace, které jsou vývoj a volnost prostřednictvím aplikace, které budou sdílet/přidělit přístup k jejich organizace datům.  *Nezapomeňte přihlašovací uživatele nahoru/aplikace a udělení oprávnění, můžete mít v hlavní název služby vzniku.*

Tento týkající se může původně zvuku, ale zachová v paměti následující kroky:

* Aplikace byly možnost využít služby Active Directory pro Windows Server pro ověřování uživatelů pro mnoho roků bez nutnosti aplikace je registrován/zaznamenané v adresáři.  Organizace teď bude lepší viditelnost přesně kolik aplikace používají adresář a what for.
* Bez nutnosti správce řízený úsilím procesu publikování/registrace aplikace.  S Active Directory Federation Services je pravděpodobné, kterou chcete přidat aplikaci jako druhou stranou za jménem vývojáři měli správce.  Teď můžete vývojáři samoobslužné.
* Uživatelé podepisování/až aplikací pomocí jejich účty organizace pro obchodní účely je dobré.  Pokud následně opustí organizaci dojde ke ztrátě přístup k účtu v aplikaci, kterou ho používali.
* Problémy záznam jaká data bylo nasdílené aplikace, ke kterému je dobré.  Data jsou přenosné víc než kdy dříve a Vymazat záznam kteří s aplikací, které sdílí jaká data je užitečné.
* Aplikace, kteří používají Azure AD pro oAuth rozhodnout přesně jaká oprávnění, které uživatelé můžou udělit na aplikace a který vyžaduje oprávnění správce souhlas s.  By měly vrátit bez oznamující, že můžete jenom správci souhlas s větší obory a větší oprávnění.
* Uživatele, které jsou přidávání a povolení aplikace přístup ke svým datům auditovány události, můžete zobrazit sestavy auditování v portálu Azure Managment a zjistit, jak jsme přidali v adresáři aplikace.

**Poznámka:** *Společnosti Microsoft, vlastní činnosti pomocí výchozí konfigurace pro mnoho měsíců teď.*

Spolu s ostatními, která říká je možné zabránit uživatelům v adresáři v přidání aplikací a od výkonu volnost nad jaké informace sdílejí s aplikacemi změnou konfiguraci služby Directory na portálu Správa Azure.  Následující konfigurace přístupné v portálu pro správu Azure na kartě "Konfigurovat" do adresáře.

![Snímek obrazovky uživatelského rozhraní pro konfiguraci nastavení integrovanou aplikaci][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Další informace o tom, jak přidat aplikace Azure AD a jak konfigurovat přihlašovacích údajů pro aplikace.

* [Zjistěte, jak integrovat aplikaci AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx) vývojáře:
* [Revize ukázkový kód pro aplikace integrovaný s Azure Active Directory na Github](https://github.com/AzureADSamples) vývojáře:
* Vývojáři a v oboru IT: [Revize dokumentace rozhraní REST API pro Azure Active Directory grafu rozhraní API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT: [Naučte se používat Azure Active Directory předem integrované aplikace z Galerie aplikace](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* [Výukové programy pro konfiguraci konkrétní předem integrované aplikace](https://msdn.microsoft.com/library/azure/dn893637.aspx) v oboru IT:
* IT: [Přečtěte si, jak publikovat aplikace pomocí Proxy aplikace Azure Active Directory](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Viz taky

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
