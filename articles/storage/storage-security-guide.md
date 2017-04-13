<properties
    pageTitle="Azure Průvodce zabezpečením úložiště | Microsoft Azure"
    description="Podrobné informace o mnoha metody zabezpečení Azure úložiště, včetně RBAC úložiště služby šifrování, šifrování na straně klienta, SMB 3.0 a šifrování disku Azure."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure Průvodce zabezpečením úložiště

##<a name="overview"></a>Základní informace

Azure úložiště umožňuje komplexní sady možnosti zabezpečení, které dohromady vývojářům umožňuje vytvářet zabezpečené aplikace. Přímo účet úložiště lze zabezpečit pomocí řízení přístupu na základě rolí a Azure Active Directory. Data lze zabezpečit mezi aplikací a Azure pomocí [Šifrování na straně klienta](storage-client-side-encryption.md), HTTPS nebo SMB 3.0. Data můžete nastavit automaticky zašifrovat zápisu k základnímu úložišti Azure pomocí [Úložiště služby šifrování (SSE)](storage-service-encryption.md). Šifrování pomocí [Šifrování disku Azure](../security/azure-security-disk-encryption.md)můžete nastavit OS a dat disků používaný virtuálních počítačích. Delegovaná přístup k objektům dat v úložišti Azure můžete k ní mít udělený používání [Sdílené podpisů přístup](storage-dotnet-shared-access-signature-part-1.md).

V tomto článku najdete základní informace o jednotlivých funkcích zabezpečení, které můžete používat s Azure úložiště. Jsou odkazy za předpokladu, že na články, které vám umožní podrobnosti každé funkce, takto můžete snadno dělat dále v každé téma vyšetřování.

Tady jsou tématy k uvedené v tomto článku:

-   [Správa rovině zabezpečení](#management-plane-security) – zabezpečení účtu úložiště

    Správa rovině se skládá ze zdroje sloužící ke správě účtu úložiště. V této části podíváme nasazení modelu správce prostředků Azure a jak pomocí řízení přístupu na základě rolí (RBAC) k řízení přístupu k účtům úložiště. Budeme se taky o správě kódu účtu Key úložiště a jak je obnovit.

-   [Data rovině zabezpečení](#data-plane-security) – zabezpečení přístupu k datům

    V tomto oddílu se podíváme na povolení přístupu k objektům vlastních dat ve vašem účtu úložiště, jako je objektů BLOB, soubory, dotazů a tabulek, pomocí sdílené podpisy přístup a uložené zásady přístupu. Budeme se zabývat těmito oblastmi přidružení služby úroveň zabezpečení a přidružení účtu úroveň zabezpečení. Ukážeme si taky, jak omezit přístup k určité IP adresy (nebo rozsah IP adresy), jak omezit protokol sloužící k HTTPS a jak odvolat přístup podpis sdílené bez nutnosti čekání vypršení platnosti.

-   [Šifrování při přenosu šifrovaná](#encryption-in-transit)

    Tato část popisuje, jak zajistit data při přenosu nebo stmívání Azure úložiště. Podíváme doporučené použití HTTPS a šifrování používaný SMB 3.0 pro Azure sdílené složky. Jsme bude taky podívejte se na straně klienta šifrování, které umožňuje šifrování data před bude převedena do úložiště v klientské aplikaci a dešifrování data po přenosu z úložiště.

-   [Šifrování na ostatní](#encryption-at-rest)

    Bude budeme úložiště služby šifrování (SSE) a jak ji povolit účtu úložiště, výsledkem je vaše objektů BLOB blok objektů BLOB stránky a připojit automaticky šifrovány zápisu Azure úložiště objektů BLOB. Podíváme se také na tom, jak můžete použít šifrování disku Azure a prozkoumat základní rozdíly a případech šifrování disku a SSE versus šifrování na straně klienta. Podíváme bude stručně na FIPS dodržování předpisů pro vládní organizace počítače.

-   Použití [Analýzy úložiště](#storage-analytics) auditování přístupu Azure úložiště

    Tato část popisuje, jak najít informace v protokolech analýzy úložiště žádost. Můžeme se podívejte se na skutečné úložiště analýzy dat protokolu a zjistěte, jak ověřit pravost žádost s klíč účtu úložiště, s sdílených přístup podpis, zda anonymně a jestli ho úspěšný nebo.

-   [Povolení používání CORS klientů pomocí prohlížeče](#Cross-Origin-Resource-Sharing-CORS)

    Tato část pojednává o tom, jak povolit více origin zdroje sdílení (CORS). Podíváme doménami přístup a jak se zachází s možnostmi CORS integrované v úložišti Azure.

##<a name="management-plane-security"></a>Správa rovině zabezpečení

Správa rovině se skládá z operacích, které ovlivňují přímo účet úložiště. Můžete například vytvořit nebo odstraněním účtu úložiště, získáte seznam účtů úložiště v předplatné, načtení klíčů účtu úložiště nebo obnovovat klíče účtu úložiště.

Při vytváření nového účtu úložiště vyberte model nasazení Classic nebo správce prostředků. Klasický model vytvoření zdrojů v Azure pouze umožňuje vše nebo nic přístup k předplatnému a následně účtu úložiště.

Tento průvodce se zaměřuje na modelu správce prostředků, což je doporučené prostředky pro vytvoření účtů úložiště. Se správce prostředků úložiště účty, nikoli procesu udělení přístupu celý předplatného můžete řídit přístup na víc konečných úrovně rovině správy pomocí řízení přístupu na základě rolí (RBAC).

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Jak zajistit účtu úložiště pomocí řízení přístupu na základě rolí (RBAC)

Promluvme si o novinky RBAC a jak ho používat. Každý Azure předplatné má Azure Active Directory. Uživatelé, skupiny a aplikací z adresáře můžete k ní mít udělený přístup k přidávání a používání zdrojů v Azure předplatné, které používají nasazení modelu správce prostředků. To je uvedená jako řízení přístupu na základě rolí (RBAC). Ke správě tento přístup, můžete [Azure portál](https://portal.azure.com/), [rozhraní příkazového řádku Azure nástroje](../xplat-cli-install.md), [prostředí PowerShell](../powershell-install-configure.md)nebo [Azure úložiště zdroje poskytovatele REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx).

S modelem správce prostředků vložíte účtu úložiště zdroje skupiny a řízení přístupu k rovině správy účtu konkrétní úložiště služby Azure Active Directory. Například můžete uživatelům určité možnost přístupové klávesy účtu úložiště, zatímco ostatní uživatelé uvidí informace o účtu úložiště, ale nemáte přístup ke klíči účtu úložiště.

####<a name="granting-access"></a>Udělení přístupu

Přístup, která přiřazením odpovídající roli RBAC uživatelé, skupiny a aplikace na pravém obor. Udělení přístupu k celé předplatného, přiřadit roli na úrovni předplatného. Udělit přístup k všechny zdroje ve skupině zdroje udělením oprávnění ke skupině prostředků samotné. Můžete také přiřadit konkrétní rolí určitého zdroje, jako je třeba úložiště účty.

Tady jsou hlavní body, které byste měli vědět o přístup k operace správy účtu úložiště Azure pomocí RBAC:

-   Pokud přiřadíte access, v podstatě přiřadit roli k tomuto účtu, který chcete mít přístup. Můžete řídit přístup k operace sloužící ke správě tohoto účtu úložiště, ale ne k objektům dat do účtu. Například udělit oprávnění k načtení vlastnosti úložiště účtem (třeba redundance), ale ne do kontejneru nebo dat v kontejneru uvnitř úložiště objektů Blob.

-   Někdo mít oprávnění k přístupu k objektům dat do účtu úložiště můžete udělit oprávnění ke čtení klíče účtu úložiště a uživatele můžete pomocí těchto kláves přístup objektů BLOB, fronty, tabulky a soubory.

-   Role můžete přiřadit konkrétní uživatelský účet, skupinu uživatelů, nebo na určitou aplikaci.

-   Každou roli obsahuje seznam akcí a ne akce. Role přispěvatele virtuální počítač má například akci "listKeys" umožňující klávesy účtu úložiště ke čtení. Přispěvatel obsahuje "Není akce" třeba aktualizace přístupu pro uživatele ve službě Active Directory.

-   Role pro úložiště zahrnout (ale nejsou omezené na) následující:

    -   Vlastník – můžete spravují všechno, co, včetně přístupu.

    -   Přispěvatel – můžou dělat cokoliv vlastník můžete kromě přiřadit přístup. Uživatel, který tato role můžete zobrazit a obnovit klíče účtu úložiště. Pomocí klávesy účtu úložiště se můžou dostat k datové objekty.

    -   Čtečky – prohlížením informace o účtu úložiště, s výjimkou tajemství. Například pokud přiřadit roli pomocí čtečky oprávnění na účtu úložiště někomu můžete zobrazit vlastnosti účtu úložiště, ale nedaly proveďte požadované změny vlastností a zobrazte klávesy účtu úložiště.

    -   Přispěvatel účtu úložiště – můžete spravují účtu úložiště – můžete číst skupiny zdrojů a materiály, předplatného a vytváření a Správa nasazení skupina zdroje předplatného. Můžete taky přístup k účtu klíče úložiště, které zároveň znamená, že se můžou dostat k rovině data.

    -   Správce přístup uživatelů – budou spravovat přístup uživatelů k tomuto účtu úložiště. Jsou například udělit přístup čtečka určitému uživateli.

    -   Virtuální počítač přispěvatelů – můžete spravují virtuálních počítačích, ale není úložiště účet, ke kterému jsou připojené. Tato role může seznam klávesy účtu úložiště, což znamená, že uživatele, kterým přiřadíte tato role může aktualizovat rovině data.

        Uživateli vytvořit virtuální počítač, mají mít možnost vytvářet odpovídající virtuální pevný disk soubor účtu úložiště. Aby je dostala, budou muset mít možnost načíst klíč účtu úložiště a rozhraní API vytváření OM předat. Toto oprávnění musí mít proto tak jejich seznamu klávesy účtu úložiště.

- Možnost definovat vlastní role je funkce, která umožňuje vytváření sady akcí ze seznamu dostupných akcí, které lze provádět na Azure zdroje.

- Uživatel musí nastavit v Azure Active Directory před přiřadit roli na ně.

- Můžete vytvořit sestavu kdo udělena/odvolán jaký typ přístupu k a od kterého a na jaké rozsahu pomocí prostředí PowerShell nebo Azure rozhraní příkazového řádku.

####<a name="resources"></a>Zdroje informací

-   [Řízení přístupu na základě rolí Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    Tento článek vysvětluje řízení přístupu na základě rolí Azure Active Directory a jak to funguje.

-   [RBAC: Integrované role](../active-directory/role-based-access-built-in-roles.md)

    Tento článek obsahuje podrobnosti všechny dostupné v RBAC předdefinované role.

-   [Principy správce prostředků nasazení a klasické nasazení](../resource-manager-deployment-model.md)

    Tento článek vysvětluje správce prostředků nasazení a modelů klasické nasazení a vysvětluje výhody používání skupin Správce zdrojů a zdrojů

-   [Azure výpočetním, sítě a poskytovatelů úložiště v části Azure správce zdrojů](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    Tento článek vysvětluje, výpočet Azure, sítě a úložiště poskytovatelů fungování v části model správce prostředků.

-   [Správa řízení přístupu na základě rolí pomocí rozhraní REST API](../active-directory/role-based-access-control-manage-access-rest.md)

    Tento článek ukazuje, jak používat rozhraní REST API pro správu RBAC.

-   [Azure úložiště zdroje poskytovatele REST API odkaz](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Toto je odkaz pro rozhraní API, můžete použít ke správě účtu úložiště programově.

-   [Příručka pro vývojáře na auth pomocí rozhraní API Správce prostředků Azure](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    Tento článek ukazuje, jak ověření pomocí rozhraní API Správce prostředků.

-   [Řízení přístupu na základě rolí pro službu Microsoft Azure z Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Toto je odkaz na video na kanál 9 z konference Ignite 2015 MS. V této relaci, budou si o přístup k řízení a možnosti vytváření sestav v Azure a prozkoumání doporučené postupy kolem zabezpečení přístupu k předplatným Azure pomocí služby Azure Active Directory.

###<a name="managing-your-storage-account-keys"></a>Správa kódu účtu Key úložiště

Klávesy účtu úložiště jsou řetězce 512-bit vytvořil Azure, která spolu s název účtu úložiště mohou sloužit k přístupu k objektům dat uložených v úložišti účtu, objekty například BLOB, entity v tabulce, zpráv a souborů na sdílet soubory Azure. Řízení přístupu k úložiště účtu klíče ovládací prvky přístup k rovině dat pro jeho účet úložiště.

Každý účet úložiště má dva klíče označovány jako "Klíč 1" a "2" [Azure portál](http://portal.azure.com/) a klíčových v rutiny prostředí PowerShell. Toto je možné znovu generovat ručně pomocí několika metod, včetně, ale bez omezení použití [Azure portál](https://portal.azure.com/), Powershellu, rozhraní příkazového řádku Azure nebo programově pomocí knihovnu .NET úložiště klienta nebo REST API Azure úložiště služby.

Existuje libovolný počet důvodů, proč obnovit kódu účtu Key úložiště.

-   Obnovit je může pravidelně z bezpečnostních důvodů.

-   Klíče účtu úložiště chcete obnovit, pokud někdo podařilo proniknout do aplikace a načíst klávesu pevně kódovaná nebo uložený v souboru konfigurace jim plný přístup ke svému účtu úložiště.

-   Další písmena klíčů je Pokud váš tým používá aplikace Explorer úložiště, která uchovává klíč účtu úložiště a jedné od členů týmu ponechá. Aplikaci byste pokračovat v práci, bude mít přístup ke svému účtu úložiště až budou pryč. Toto je skutečně primární důvod, proč vytvořeného úroveň účtu sdílené přístup podpisy – přidružení zabezpečení úrovní účtů můžete použít místo ukládání přístupových kláves v souboru konfigurace.

####<a name="key-regeneration-plan"></a>Obnovování klíčů plán

Nechcete jenom obnovit klávesu používané bez plánování. Pokud to uděláte, může oříznutí veškerý přístup k tomuto účtu úložiště, což může způsobit hlavní přerušení. Z tohoto důvodu existují dva klíče. Měli obnovit jen jednu klávesu současně.

Před obnovit klíče se ujistěte, že máte seznam všech vašich aplikací, které jsou závislé na účtu úložiště, stejně jako jakékoli jiné služby, které používáte v Azure. Například pokud používáte Azure Media Services, které jsou závislé na vašem účtu úložiště, musí znovu synchronizujete přístupových kláves s vašimi službami médií po obnovit klávesu. Pokud používáte všech aplikací, jako je explorer úložiště, musíte zadat nové klíče pro tyto aplikace taky. Všimněte si, že máte VMs jejichž virtuální pevný disk soubory uložené v účtu úložiště, budou se nevztahuje obnovení účtu klávesy úložiště.

Můžete obnovit kódu Key na portálu Azure. Jakmile klíče vygenerují může trvat až 10 minut synchronizovat přes úložiště služby.

Jakmile budete připraveni, tady je obecný postup s podrobnostmi jak změňte kód. V tomto případě předpokladu je momentálně používáte klíč 1 a budete měnit všechno můžete klíč 2.

1.  Obnovit klíč 2 zajistit, aby byla v bezpečí. Lze provést na portálu Azure.

2.  Ve všech aplikací, kde je uložena klávesu úložiště změna klíče úložiště použít novou hodnotu 2 klíče. Otestujte a publikování aplikace.

3.  Po všech aplikací a služeb jsou výše a úspěšným, obnovit klíč 1. Zajistíte tak, že kdokoli komu jste to ještě zadaných výslovně nový klíč už mají přístup k tomuto účtu úložiště.

Pokud aktuálně používáte klíč 2, můžete budete postupovat stejně, ale Převrátit klíčové názvy.

Můžete migrovat přes několik dnů, změna každé aplikaci pro použití nového klíče a publikováním. Po všem poznámkám hotovi, by měl pak vrátit a obnovit původní klíč tak, aby mi už funguje.

Další možností je zahrnout klíč účtu úložiště v [Azure klíč trezoru](https://azure.microsoft.com/services/key-vault/) jako tajná a aplikace načíst klíč odtud. Potom až obnovit klávesu a aktualizovat trezoru klíč Azure, aplikace nebudete muset znovu nasadit, protože jsou vyzvedne nový klíč z trezoru klíč Azure automaticky. Všimněte si, že máte aplikaci číst klíč pokaždé, když ho potřebujete nebo mezipaměti v paměti a pokud se nezdaří při použití, klávesu znovu načíst z trezoru klíč Azure.

Pomocí klávesy trezoru Azure taky přidá další úroveň zabezpečení pro úložiště klíče. Pokud tímto způsobem se ukládání nemusíte vůbec starat klíčové pevně kódovaná úložiště v souboru konfigurace, která odebere tento způsob případného někdo získání přístupu ke klíči bez konkrétní oprávnění.

Další výhodou používání Azure klíč trezoru je, že můžete taky přístup k můžete řídit klíče pomocí služby Azure Active Directory. To znamená, že můžete udělit přístup k hrstku aplikací, které potřebují k načtení klávesy z trezoru klíč Azure a vědět, že ostatní aplikace nebude moct přistupovat klávesy bez jejich udělení oprávnění konkrétně.

Poznámka: doporučujeme použít pouze jeden z klíčů ve všech vašich aplikací ve stejnou dobu. Pokud používáte klíč 1 v některých místa a klíč 2 v jiných, nebude moct otočit kódu Key bez ztráty přístupu některé aplikace.

####<a name="resources"></a>Zdroje informací

-   [Informace o účtech Azure úložiště](storage-create-storage-account.md#regenerate-storage-access-keys)

    V tomto článku najdete přehled úložiště účty a pojednává o zobrazování, kopírování a obnovení úložiště přístupových kláves z verze.

-   [Azure úložiště zdroje poskytovatele REST API odkaz](https://msdn.microsoft.com/library/mt163683.aspx)

    Tento článek obsahuje odkazy na konkrétní články o načítání klíče účtu úložiště a obnovení účtu klávesy úložiště pro účet Azure pomocí rozhraní REST API. Poznámka: Toto je správce prostředků úložiště účty.

-   [Operace úložiště účtů](https://msdn.microsoft.com/library/ee460790.aspx)

    Tento článek Správce úložiště služby REST API odkazu obsahuje odkazy na konkrétní články o načítání a obnovení účtu klíče úložiště pomocí rozhraní REST API. Poznámka: Toto se týká účty klasické úložiště.

-   [Nepoužívejte klíčové správy – správa přístupu k datům úložišti Azure pomocí Azure AD](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    Tento článek ukazuje, jak pomocí řízení přístupu k úložišti Azure klíče trezoru klíč Azure pomocí služby Active Directory. Také ukazuje, jak používat úloha Azure Automation k opětovnému vytvoření klávesy na hodinu.

##<a name="data-plane-security"></a>Zabezpečení rovině dat

Zabezpečení rovině dat odkazuje na metody používá k zabezpečení dat objektů uložených v úložišti Azure – objektů BLOB, fronty, tabulky a soubory. Jsme viděli metod k šifrování dat a zabezpečení během přenosu dat, ale jak přejdete k povolení přístupu k objektům?

V podstatě dvěma způsoby pro řízení přístupu k datové objekty, které sami. První je pomocí řízení přístupu k účtu klávesy úložiště a druhý používá sdílené podpisy přístup udělení přístupu k určité datové objekty pro konkrétní časový úsek.

Jedinou výjimkou je potřeba pamatovat je, že je povolit veřejné přístup k vaší objektů BLOB nastavením úroveň přístupu pro kontejner obsahující objekty BLOB příslušným způsobem. Pokud nastavení přístupu k objektů Blob nebo Container kontejneru povolují veřejné přístup pro čtení pro objekty BLOB v tomto kontejneru. To znamená, že každý, kdo má adresa URL odkazující na objektů blob v tomto kontejneru ho otevřete v prohlížeči bez podpisem sdílených přístup a bez kláves účtu úložiště.

###<a name="storage-account-keys"></a>Klávesy účtu úložiště

Klávesy účtu úložiště jsou řetězce 512-bit vytvořil Azure, která spolu s název účtu úložiště mohou sloužit k přístupu k objektům dat uložených v úložišti účtu.

Můžete číst objektů BLOB, zapisovat do fronty, vytvářet tabulky a upravovat soubory. V mnoha tyto akce lze provést pomocí portálu Azure nebo pomocí jedné z mnoha Průzkumníka úložišť aplikací. Můžete také vytvořit kód použít rozhraní REST API nebo jedna z úložiště klienta knihoven provádět tyto operace.

Jak je popsáno v části [Správa rovině zabezpečení](#management-plane-security), můžete umožnit přístup k klávesy úložiště účtu úložiště klasické tím, že úplný přístup k předplatnému Azure. Přístup ke klíči úložiště úložiště účtu pomocí Správce prostředků Azure modelu možné ovládat pomocí řízení přístupu na základě rolí (RBAC).

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Jak přístup k objektům ve vašem účtu pomocí sdílené podpisy přístup a uložené zásady přístupu delegáta

Přístup k podpisu sdílené je řetězec obsahující tokenu zabezpečení, který může být připojen identifikátor URI, který vám umožní přístup delegáta, aby úložiště objektů a určete omezení například oprávnění a rozsah kalendářních dat/časů přístup.

Můžete udělit přístup k objektů BLOB, kontejnerů, fronty zprávy, soubory a tabulky. S tabulkami je skutečně udělit oprávnění k přístupu k oblasti entity v tabulce zadáním oddíl a řádek klíčové oblasti, do kterých chcete, aby uživatel měl přístup. Například, pokud máte data uložená s klíč oddílu zeměpisné státu, byste mohli dát někdo přístup pouze data k Kalifornie.

Jiný příklad může dejte webovou aplikaci token přidružení zabezpečení, který umožňuje psaní položky do fronty a dát pracovník role aplikace token přidružení zabezpečení se zprávy ve frontě a jejich zpracování. Nebo byste mohli dát zákazníků token přidružení zabezpečení můžete použít k odeslání obrázků do kontejneru v úložišti objektů Blob a dát webové aplikace oprávnění ke čtení tyto obrázky. V obou případech oddělení pochybnosti – jednotlivých aplikací může být zadán jenom přístup, který potřebují k provedení příslušný úkol. To je možné prostřednictvím sdílených podpisy přístup.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Proč chcete používat sdílené podpisy přístup

Proč byste měli být místo jenom poskytování svůj klíč účtu úložiště, což je to tím usnadnit použijte přidružení zabezpečení? Poskytování svůj klíč účtu úložiště je chcete se podělit o klíči království úložiště. Zajišťuje úplný přístup. Uživatel může pomocí kláves a nahrajte jejich celý knihovna ke svému účtu úložiště. Také se může nahraďte infekce antivirová verzí souborů nebo ukrást vaše data. Pojmenování pryč neomezený přístup ke svému účtu úložiště je něco, co by se neměl brát mírným.

S sdílených podpisy přístup dodáte klienta jenom informace o oprávněních požadovaných pro omezené množství času. Například pokud někdo se odesílá objektů blob ke svému účtu, můžete udělit jim přístup pro zápis jenom dostatek času nahrát objektů blob (v závislosti na velikosti objektů blob, samozřejmě). A pokud změníte názor, můžete odvolat, aby přístup.

Navíc můžete určit, že žádosti pomocí přidružení zabezpečení je omezen na IP adresu nebo Azure mimo rozsah IP adres. Můžete taky požadovat, aby žádostech používající určitý protokol (HTTPS nebo protokolu HTTP/HTTPS). To znamená, pokud chcete povolit přenosy HTTPS, můžete nastavit jiné požadovaný protokol HTTPS pouze a budou blokovány přenosy protokolu HTTP.

####<a name="definition-of-a-shared-access-signature"></a>Definice sdíleného přístupu podpisu

Přístup k podpisu sdílené je sada parametry dotazu přidaným k adresa URL odkazující na zdroje

který obsahuje informace o přístupu povolené a doba, u kterého je povolen přístup. Tady je příklad; Tento identifikátor URI poskytuje přístup pro čtení objektů blob pět minut. Poznámka: aby přidružení zabezpečení parametry dotazu musí být kódování URL například % 3A dvojtečka (:) nebo % 20 prostoru pro.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Jak Access podpis sdílené ověření úložiště službou Azure

Když služba úložiště obdrží žádost, trvá vstupní parametry dotazu a vytvoří podpis stejným způsobem jako volající program. Potom porovná dva podpisy. Pokud souhlasí, můžete zkontrolovat služba úložiště na úložiště služby verzi, ujistěte se, že je platný, ověřte, jestli jsou v okně zadaný aktuální datum a čas, ujistěte se, že požadovaný přístup odpovídá žádost, atd.

Například se adresa URL výše uvedené, pokud adresa URL byla přejdete na soubor místo objektů blob, tento požadavek by nepovede, protože ho Určuje, že podpis sdílené aplikace Access pro objekt blob. Pokud příkaz ZBÝVAJÍCÍ volání aktualizovat objektů blob, by nepovede, protože Access podpis sdílené Určuje, že je povolen pouze pro čtení přístup.

####<a name="types-of-shared-access-signatures"></a>Typy podpisů sdílený přístup

-   Přidružení služby úroveň zabezpečení mohou sloužit k přístup k určitým prostředkům ve účtu úložiště. Některé příklady jsou načtení seznamu objektů BLOB v kontejneru, stahování objektů blob, aktualizace entita v tabulce, přidávání zpráv do fronty nebo uložení souboru do sdílené složky.

-   Přidružení zabezpečení úroveň účtu mohou sloužit k všechno, co přidružení služby úroveň zabezpečení se dá použít pro přístup. Kromě toho můžete zobrazit možnosti a prostředky, které nejsou povoleny přidružením úrovni služeb, jako je možnost vytvořit kontejnery, tabulek, dotazů a sdílených souborů. Přístup k více služeb můžete taky určit najednou. Například někdo dáte přístup ke službě objektů BLOB i soubory ve vašem účtu úložiště.

####<a name="creating-an-sas-uri"></a>Vytvoření přidružení zabezpečení URI

1.  Můžete vytvořit ad hoc URI na službu, definování všechny parametry dotazu pokaždé, když.

    To je ale skutečně flexibilní, ale pokud logické nastavení parametrů, které jsou podobné pokaždé, když, pomocí uložené zásady přístupu je výhodnější.

2.  Můžete vytvořit zásady přístupu uložené pro celý container, sdílené složky, tabulku nebo fronty. Pak můžete to jako základ pro vytvoření URI přidružení zabezpečení. Oprávnění podle uložené zásady přístupu můžete snadno odvolat. Můžete mít až na 5 zásady definované na každý kontejneru fronty, tabulky a sdílení souborů.

    Například pokud budete pryč mít více lidem číst objektů BLOB v určitém kontejneru, můžete vytvořit pro uložené přístup tak, že "udělit přístup pro čtení" a další nastavení, které budou shodovat s pokaždé, když. Pak můžete vytvořit přidružení zabezpečení URI pomocí nastavení zásady přístupu uloženy a zadání vypršení platnosti datum a čas. Výhodou je, že nemusíte zadat všechny parametry dotazu pokaždé, když.

####<a name="revocation"></a>Odvolání

Předpokládejme ohroženo vaší přidružení zabezpečení nebo chcete změnit kvůli podnikové zabezpečení nebo zákonných předpisů. Jak se odvolat přístup k prostředku pomocí tohoto přidružení zabezpečení? To záleží na tom, jak jste vytvořili URI přidružení zabezpečení.

Pokud používáte ad hoc URI, máte tři možnosti. Můžete problémů přidružení zabezpečení tokeny zásadám krátkých a jednoduše počkejte přidružení zabezpečení platnosti. Můžete přejmenovat nebo odstranit zdroj (za předpokladu, že byl token omezené na jeden objekt). Můžete změnit klávesy účtu úložiště. Poslední možnost může mít velký dopad, podle toho, kolik služby používáte tohoto účtu úložiště a pravděpodobně se něco, co chcete udělat bez plánování.

Pokud používáte přidružení zabezpečení odvozeno z uložené zásady přístupu, aplikace access můžete odebrat tak, že odvolání přístupu uložené – můžete jednoduše změnit tak, aby již vypršela nebo ho můžete úplně odebrat. Tím se projeví okamžitě a zruší platnost každé přidružení zabezpečení vytvořených pomocí této zásady přístupu uložený. Aktualizace nebo odstranění přístupu uložené může tabulky dopad uživatelům přístup k této konkrétní kontejneru sdílené složky nebo fronty prostřednictvím přidružení zabezpečení, ale pokud klienti jsou napsali tak požádají nová přidružení zabezpečení při ztratí starý to fungovalo správně.

Protože pomocí přidružení zabezpečení odvozeno z uložené zásady přístupu poskytuje možnost okamžitě zrušit tento přidružení zabezpečení, je doporučený osvědčený postup vždy používat uložené Pokud je to možné zásady přístupu.

####<a name="resources"></a>Zdroje informací

Podrobnější informace o používání sdílené podpisy přístup a uložené zásady přístupu, dokončeno s příklady naleznete v následujících článcích:

-   Toto jsou článků odkaz.

    -   [Služba přidružení zabezpečení](https://msdn.microsoft.com/library/dn140256.aspx)

        Tento článek uvádí příklady použití přidružení služby úroveň zabezpečení s objekty BLOB, zpráv, rozsahy tabulky a soubory.

    -   [Stavba službu přidružení zabezpečení](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Vytvoření účtu přidružení zabezpečení](https://msdn.microsoft.com/library/mt584140.aspx)

-   Toto jsou výukové programy pro použití knihovnu klienta .NET k vytvoření sdílené podpisy přístup a uložené zásady přístupu.

    -   [Používání podpisů sdílený přístup (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md)

    -   [Sdílené podpisy Accessu, část 2: Vytvoření a používání přidružení zabezpečení se službou objektů Blob](storage-dotnet-shared-access-signature-part-2.md)

        Tento článek obsahuje vysvětlení přidružení zabezpečení modelu příklady sdílené podpisy přístup a doporučení pro doporučený postup pomocí přidružení. Odvolání oprávnění udělena rovněž popisované je.

-   Omezení přístupu podle IP adresy (IP ACL)

    -   [Co je koncový bod seznam řízení přístupu (ACL)?](../virtual-network/virtual-networks-acl.md)

    -   [Stavba službu přidružení zabezpečení](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Je toto článek odkaz pro přidružení zabezpečení úrovni služeb; obsahuje příklady IP ACLing.

    -   [Vytvoření účtu přidružení zabezpečení](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Je toto článek odkaz pro úroveň účtu přidružení zabezpečení; obsahuje příklady IP ACLing.

-   Ověřování

    -    [Ověřování služby Azure úložiště](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Sdílené přístup podpisy Začínáme kurzu

    -   [Přidružení zabezpečení Začínáme kurzu](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Šifrování při přenosu šifrovaná

###<a name="transport-level-encryption--using-https"></a>Šifrování transportní – pomocí HTTPS

Další krok, který by měl provádět zajistit bezpečnost dat Azure úložiště je šifrování dat mezi klientem a úložišti Azure. První doporučení je vždy použít protokol [HTTPS](https://en.wikipedia.org/wiki/HTTPS) , které umožňuje zabezpečená komunikace přes veřejnou Internet.

Při volání rozhraní REST API nebo přístupu k objektům v úložišti byste měli vždy používat HTTPS. **Sdílené podpisy přístup**nichž lze delegování přístupu k objektům Azure úložiště, se týkají taky, jednu z možností můžete určit, že při používání sdílené podpisů přístup zajistit, že kdokoli odesláním odkazy s přidružení zabezpečení tokeny bude používat správné protokol lze použít pouze protokol HTTPS.

####<a name="resources"></a>Zdroje informací

-   [Povolení HTTPS pro aplikaci služby Azure aplikace](../app-service-web/web-sites-configure-ssl-certificate.md)

    Tento článek popisuje, jak povolit HTTPS pro webovou aplikaci Azure.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Pomocí šifrování během přenosu s sdílených souborů Azure

Azure úložiště souborů podporuje HTTPS při používání rozhraní REST API, ale další běžně používaných jako sdílené složky SMB přiřazen virtuálního počítače. Plány SMB 2.1 nepodporuje šifrování, takže připojení jsou jenom u povolených ve stejné oblasti v Azure. Však SMB 3.0 podporuje šifrování a lze použít s Windows serveru 2012 R2, Windows 8, Windows 8.1 a Windows 10, povolení více oblastí přístup a dokonce přístup na stolním počítači.

Všimněte si, že během Azure sdílené složky můžete použít se systémem Unix, klient Linux SMB ještě nepodporuje šifrování, tak, aby access je jenom u povolených v Azure oblasti. Podpora šifrování Linux je přehled zodpovědný za SMB funkce vývojáře Linux. Při přidávání šifrování, bude mít stejné možnosti pro přístup ke sdílení souborů Azure na Linux stejně jako pro systém Windows.

####<a name="resources"></a>Zdroje informací

-   [Použití úložiště souborů Azure s Linux](storage-how-to-use-files-linux.md)

    Tento článek ukazuje, jak připojit Azure sdílené složce v systému Linux a nahrávání nebo stahování souborů.

-   [Začínáme s úložiště souborů Azure v systému Windows](storage-dotnet-how-to-use-files.md)

    Tento článek obsahuje přehled Azure sdílené složky a jak připojit a jejich použití pomocí prostředí PowerShell a .NET.

-   [Úložiště uvnitř Azure souborů](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    Tento článek obsahuje podrobné technické informace o šifrování SMB 3.0 a oznamuje všeobecně dostupná úložiště souborů Azure.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Použití šifrování na straně klienta k zabezpečení dat, které budete odesílat k základnímu úložišti

Další možnost, která vám pomůže zajistit, že vaše data jsou zabezpečené při přenosu mezi klientské aplikace a úložiště je šifrování na straně klienta. Data musí být zašifrovaný před převáděných do úložiště Azure. Po načtení dat z Azure úložiště, je dešifrovat data po přijetí na straně klienta. I když data musí být zašifrovaný přejdete přes lince, doporučujeme použít taky HTTPS, protože obsahuje kontrolu integrity dat integrované nápovědy, která se zmírnění chyby sítě došlo k ovlivnění integrity dat.

Šifrování na straně klienta je také metoda šifrování dat v klidu, jako jsou data uložena v šifrované podobě. Podíváme to podrobnější v části o [šifrování u ostatních](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Šifrování na ostatní

Existují tři Azure funkce, které jsou zdrojem šifrování na ostatních. Azure šifrování disku slouží k šifrování disků OS a data v IaaS virtuálních počítačích. Ostatní dvě – šifrování na straně klienta a SSE – jsou oba používaný k šifrování dat v úložišti Azure. Pojďme podívejte se na každý z nich, pak se srovnáním a najdete v článku když každý z nich můžete použít.

Při použití šifrování na straně klienta můžete k šifrování data při přenosu šifrovaná (který je uložen také formou šifrované v úložišti), je možná budete chtít jednoduše pomocí HTTPS během převodu a nějak pro data, která chcete automaticky zašifrovat při ukládání. Existují dva způsoby – můžete to udělat šifrování disku Azure a SSE. Jednu se používá k přímo šifrování data na OS a data discích používaný VMs a druhý je používaný k šifrování dat došlo k úložišti objektů Blob Azure zápisu.

###<a name="storage-service-encryption-sse"></a>Úložiště služby šifrování (SSE)

SSE umožňuje požadovat, aby služba úložiště automaticky zašifrovat data při psaní k základnímu úložišti Azure. Při načtení dat z Azure úložiště ho bude dešifrovat tak, že služba úložiště před vrácením. Umožňuje zabezpečit data bez nutnosti upravit kód nebo přidání kódu pro všechny aplikace.

Toto je údaj vztahující se k účtu celé úložiště. Můžete povolit a tuto funkci zakázat změnou hodnoty nastavení. K tomuto účelu můžete portálu Azure, Powershellu, rozhraní příkazového řádku Azure, úložiště zdroje poskytovatele REST API nebo knihovnu .NET úložiště klienta. Ve výchozím nastavení je SSE vypnou a zůstanou vypnuté.

V současné době klíčů pro šifrování spravuje Microsoft. Jsme vygenerování klíčů původně a správa zabezpečeného úložiště klávesy, stejně jako běžný otočení definované interní zásady společnosti Microsoft. V budoucnu bude získat možnost spravovat vlastní šifrovacího klíče a zadejte cestu migrace z klíčů Microsoft Správa přístupových práv k zákazníka Správa přístupových práv klíče.

Tato funkce je k dispozici u standardních a Premium úložiště účtů vytvořených pomocí nasazení modelu správce prostředků. SSE platí jenom pro blokovat objektů BLOB, objekty BLOB stránky a připojte objektů BLOB. Jiné typy dat, včetně tabulek, dotazů a soubory, nebude zašifrován.

Data zašifrován pouze pokud je povoleno SSE a data zapisuje k úložišti objektů Blob. Povolení nebo zakázání SSE nemá vliv na existující data. Jinými slovy Pokud povolíte šifrování, nelze přejít zpět a šifrování data, která už existuje; ani bude dešifrovat data, která už existuje, když zakážete SSE.

Pokud chcete tuto funkci lze použít s účtem klasické úložiště, můžete vytvořit nový účet správce prostředků úložiště a zkopírujte data do nového účtu pomocí AzCopy. 

###<a name="client-side-encryption"></a>Šifrování na straně klienta

Společnost Microsoft uvedené šifrování na straně klienta při projednávat šifrování data při přenosu šifrovaná. Tato funkce umožňuje programově šifrování dat v klientské aplikaci před odesláním přes lince měly zapisovat k základnímu úložišti Azure a programově dešifrovat data po načtení z Azure úložiště.

To poskytnout šifrování při přenosu šifrovaná, ale také poskytuje funkce šifrování na ostatních. Všimněte si, že i když data musí být zašifrovaný na cestě, pořád doporučujeme používat HTTPS umožní využít výhod kontrolu integrity předdefinované dat, které zmírnit chyby sítě došlo k ovlivnění integrity dat.

Pokud používáte webové aplikace, která jsou uložená objektů BLOB a obnovení objektů BLOB a chcete, aby aplikace a dat je co nejbezpečnější je například ze kdy to můžete použít. V takovém případě použijete šifrování na straně klienta. Přenosy mezi klientem a služba objektů Blob Azure jsou zašifrované zdroje a nikdo interpretovat data při přenosu šifrovaná a znovuvytvoření do soukromé objektů BLOB.

Šifrování na straně klienta je integrovaná v Java a .NET úložiště klienta knihoven, které zase používat trezoru API Azure klíče, díky poměrně snadná můžete provádět. Proces šifrování a dešifrování data používá Obálka techniku a ukládá používá šifrování ve všech objektů úložiště metadat. Například pro objekty BLOB, uloží se v metadatech objektů blob pro fronty, přidá se každá zpráva fronty.

Šifrování samotné generovat a Správa šifrovacího klíče. Můžete taky použít zkratku generovaných knihovnu Azure úložiště klienta nebo budete moci vygenerování klíčů trezoru Key Azure. Šifrovacího klíče mohou být uloženy v úložišti klíčové místní nebo mohou být uloženy v trezoru klíč Azure. Azure trezoru klíč umožňuje udělit přístup k tajemství v Azure klíč trezoru vybraným uživatelům pomocí služby Azure Active Directory. To znamená, že nejen kdokoli může číst trezoru klíč Azure a načíst klávesy, které používáte pro šifrování na straně klienta.

####<a name="resources"></a>Zdroje informací

-   [Šifrování a dešifrování objektů BLOB v úložišti tabulek Microsoft Azure pomocí trezoru klíč Azure](storage-encrypt-decrypt-blobs-key-vault.md)

    Tento článek ukazuje, jak pomocí šifrování na straně klienta služby Azure klíč trezoru, včetně postupu vytvořte KEK a uložte ho do trezoru pomocí Powershellu.

-   [Šifrování na straně klienta a Azure klíčové trezoru úložištěm Microsoft Azure](storage-client-side-encryption.md)

    Tento článek vysvětluje klientských šifrování a uvádí příklady použití knihovně úložiště klienta postup pro šifrování a dešifrování zdroje ze stránky služby na čtyři úložiště. Také pojednává o Azure klíč trezoru.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Pomocí šifrování disku Azure šifrování disků používaný virtuálních počítačích

Azure šifrování disku je novou funkci, která je aktuálně v náhledu. Tato funkce umožňuje šifrování s operačním systémem od disků dat používaných tak, že IaaS virtuálního počítače. Pro systém Windows jednotky šifrování pomocí technologie šifrování standardní nástroje BitLocker. Linux jsou šifrované disků pomocí technologie DM Crypt. To je integrována s Azure klíč trezoru umožňuje řízení a Správa šifrovacího klíče disku.

Řešení šifrování disku Azure podporuje následující tři zákazníka šifrování scénáře:

-   Povolte šifrování nové IaaS VMs vytvořené z zákazníka zašifrovaných souborů virtuálního pevného disku a pokud zákazník šifrovací klíče, které jsou uložené v Azure klíč trezoru.

-   Povolte šifrování nové IaaS VMs vytvořená z Azure Marketplace.

-   Povolte šifrování existující IaaS VMs spuštěná v Azure.

>[AZURE.NOTE] Linux VMs spuštěná v Azure nebo nové Linux VMs vytvořené ze služby obrázky v Azure Marketplace šifrování disku operační systém není podporované aktuálně. Šifrování s operačním systémem hlasitost Linux VMs je podporována pouze u VMs, které byly šifrované místních a nahráli Azure. Toto omezení platí jenom pro OS disk. podpora šifrování objemů dat pro OM Linux.

Řešení podporuje následující IaaS VMs pro verzi veřejných preview-li v Microsoft Azure povoleno:

-   Integrace se službou Azure klíčové trezoru

-   Standardní [A, D a VMs IaaS řady G](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   Povolte šifrování IaaS VMs vytvořené pomocí [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) modelu

-   Všechny Azure veřejné [oblastí](https://azure.microsoft.com/regions/)

Tato funkce zaručuje, že je všechna data na disku virtuálního počítače šifrovaná v klidu v úložišti Azure.

####<a name="resources"></a>Zdroje informací

-   [Šifrování Azure disku pro Windows a Linux IaaS virtuálních počítačích](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    Tento článek popisuje verzi preview Azure disku šifrování a obsahuje odkaz pro stažení dokument white paper.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Porovnání šifrování Azure disku, SSE a šifrování na straně klienta

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs a jejich soubory virtuální pevný disk

Disků používaný IaaS VMs doporučujeme vám použít šifrování disku Azure. Můžete zapnout SSE šifrování virtuální pevný disk soubory, které se používají k zálohování těchto discích v úložišti Azure, ale jenom šifruje nově ručně psaných data. Znamená to, že když vytvoříte virtuálního počítače a pak ji povolit SSE na úložiště, který obsahuje soubor virtuálního pevného disku, budou šifrovány pouze změny, nikoli původní soubor virtuální pevný disk.

Pokud vytvoříte virtuálního počítače pomocí obrázek z webu Azure Marketplace Azure provádí [omezené kopírovat](https://en.wikipedia.org/wiki/Object_copying) obrázek ke svému účtu úložiště v úložišti Azure a není zašifrovaných, i když máte SSE povolené. Po OM vytvoří a spustí aktualizace obrazu, začnou SSE šifrování data. Z toho důvodu je vhodné použít šifrování disku Azure na VMs vytvořené ze služby obrázky v Azure Marketplace Pokud požadavkům plně šifrovaná.

Pokud předem šifrované OM do Azure načíst z místního, budete moct nahrajte šifrovacího klíče Azure klíč trezoru a používejte dál jednotné šifrování pro tuto OM, že jste používali místní. Zpracování tento scénář je povoleno Azure šifrování disku.

Pokud máte bez šifrování virtuální pevný disk z místního, můžete uložit do Galerie jako vlastní obrázek a zřízení OM z něho. Pokud provedete pomocí šablon správce prostředků, můžete požádat zapněte šifrování disku Azure při spuštění nahoru OM.

Když přidáte disk dat a připojit ji na OM, můžete zapnout šifrování disku Azure tohoto disku data. Bude nejdřív šifrování disku data místně a vrstvy management service bude proveďte pustí zápisu proti úložiště, musí být zašifrovaný obsah úložiště.

####<a name="client-side-encryption"></a>Šifrování na straně klienta####

Šifrování na straně klienta je nejbezpečnější způsob šifrování dat, protože šifruje před dopravní a jsou šifrovány data na ostatních. Vyžaduje však přidání kódu pro vaše aplikace pomocí úložiště, která nemusí chcete udělat. V těchto případech můžete HTTPs pro vaše data při přenosu šifrovaná a SSE šifrování data na ostatních.

Pomocí klienta šifrování dá zašifrovat entity tabulky, zpráv a objekty BLOB. S SSE zašifrovat lze pouze objektů BLOB. Pokud potřebujete tabulku a fronty data šifrování, byste měli použít šifrování na straně klienta.

Šifrování na straně klienta se spravuje úplně aplikace. To je nejbezpečnějším způsobem, ale vyžadují, abyste programové změny aplikace a zavedený správy klíčů procesů. Pokud chcete zvýšit zabezpečení během přenosu a chcete šifrovat uložená data toto použijete.

Šifrování na straně klienta je větší zatížení v klientském počítači a je potřeba počítat v škálovatelnost plánů, zejména pokud jsou šifrování a přenosu velké množství dat.

####<a name="storage-service-encryption-sse"></a>Úložiště služby šifrování (SSE)

SSE se spravuje Azure úložiště. Použití SSE neposkytuje zabezpečuje data při přenosu šifrovaná, ale šifrování dat zapsané k základnímu úložišti Azure. Je žádný vliv na výkon při použití této funkce.

Můžete jenom šifrování objektů BLOB blok, přidat objektů BLOB a stránky objektů BLOB pomocí SSE. Pokud potřebujete k šifrování dat v tabulce nebo fronty dat, zvažte použití šifrování na straně klienta.

Pokud máte archivu nebo knihovny virtuální pevný disk souborů, které můžete použít jako základ pro vytvoření nového virtuálních počítačích, vytvořte nový účet úložiště, povolení SSE a pak nahrajte soubory virtuální pevný disk k tomuto účtu. Tyto soubory virtuálního pevného disku, budou šifrovány službou Azure úložiště.

Pokud máte šifrování disku Azure aktivované disků OM a SSE povolili na účtu úložiště blokování souborů virtuálního pevného disku, budou fungovat správně; Výsledkem bude žádná data nově psané šifrovány dvakrát.

##<a name="storage-analytics"></a>Úložiště analýzy

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Používání technologie pro analýzu úložiště ke sledování typ ověření

Pro každý účet úložiště můžete povolit analýzy úložiště Azure k provádění protokolování a uložit metriky data. Toto je skvělým nástrojem používat, když chcete zkontrolovat měřítka účtu úložiště nebo třeba Poradce při potížích s účtem úložiště, protože máte problémy s výkonem.

Další část data, která se zobrazí v protokolech analýzy úložiště je na metodě ověřování používané někdo při přístupu k úložiště. Například s úložiště objektů Blob uvidíte používají přístup podpis sdílené nebo klávesy účtu úložiště, zda v případě objektů blob přistupovat byl veřejné.

Může to být skutečně užitečné, pokud jsou úzce zabezpečení přístup k základnímu úložišti. Například v úložišti objektů Blob můžete nastavit všechny kontejnerů soukromé a implementovat použití služby přidružení zabezpečení v celém aplikace. Pak můžete zkontrolovat, že protokoly pravidelně zjištění, jestli vaše objektů blob se otevřít prostřednictvím klíče účtu úložiště, které mohou poukazovat porušení zabezpečení, nebo pokud objekty BLOB jsou veřejné, ale by neměly být.

####<a name="what-do-the-logs-look-like"></a>Jak protokoly vypadají?

Po povolení metriky úložišť účtu a protokolování portálu Azure technologie pro analýzu dat začne nahromadit rychle. Protokolování a metriky pro každou službu je nezávislý; protokolování je zapsat pouze při aktivity v tomto účtu úložiště během metriky se Zaprotokolují každou minutu, každou hodinu nebo každý den, v závislosti na tom, jak ji nakonfigurovat.

Protokoly ukládají do bloku objektů BLOB v kontejneru s názvem $logs účtu úložiště. Tento kontejner se automaticky vytvoří při zapnuté funkci analýzy úložiště. Po vytvoření tento kontejner můžete nelze ji odstranit, i když odstraníte jeho obsah.

V části kontejneru $logs je složka pro každou službu a potom podsložky existují pro rok/měsíc/den/hodinu. V části hodiny číslují jednoduše protokoly. Jde o struktuře adresářů bude vypadat takto:

![Zobrazení souborů protokolu](./media/storage-security-guide/image1.png)

Každý požadavek k základnímu úložišti Azure je přihlášení k lyncu. Tady je snímek soubor protokolu zobrazující první pár polí.

![Snímek souboru protokolu](./media/storage-security-guide/image2.png)

Uvidíte, že můžete protokoly sledovat jakýkoli druh volání k účtu úložiště.

####<a name="what-are-all-of-those-fields-for"></a>Jaké jsou všech těchto polí pro?

Je článek uvedené v zdroje pod, který obsahuje seznam mnoho polí v protokolech a jejich použití. Tady je seznam polí v pořadí:

![Snímek polí v souboru protokolu](./media/storage-security-guide/image3.png)

Jsme zajímá položky pro GetBlob a jak ověřen, takže potřebujeme Hledat položky, které odpovídají typu operace "Get-Blob" a kontrola stavu žádosti (4<sup>tém</sup> sloupci) a typ ověření (8<sup>tém</sup> sloupci).

Například v několik prvních řádků v seznamu výše uvedené, stav žádosti o je "ÚSPĚCH" a typ ověření "ověřen". To znamená, že žádosti byla ověřena pomocí klíč účtu úložiště.

####<a name="how-are-my-blobs-being-authenticated"></a>Jak jsou při ověřování Moje objektů BLOB?

Máme tři případy, které jsme zajímají.

1.  Objektů blob je veřejné a pracuje pomocí adresy URL bez podpisu sdílené aplikace Access. V tomto případě stavu žádosti je "AnonymousSuccess" a typ ověření je "anonymní".

    1.0; 2015-11 – 17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200 124; 37; **anonymní**; mystorage...

2.  Objektů blob je soukromá a byla použita s sdílených přístup podpisem. V tomto případě stavu žádosti je "SASSuccess" a "přidružení zabezpečení" je typ ověření.

    1.0; 2015-11 – 16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200 416; 64; **přidružení zabezpečení**; mystorage...

3.  Klíč úložiště byla použita k přístupu objektů blob je soukromé. V tomto případě stavu žádosti je "**Úspěch**" a typ ověření je "**ověření**".

    1.0; 2015-11 – 16T18:32:24.3174537Z; GetBlob; **Úspěšné**; 206 59; 22; **ověření**; mystorage...

Microsoft Analyzer zprávy můžete použít k prohlížení a analýze tyto protokoly. Hledání a filtrování možnosti zahrnuje. Můžete například chtít hledat výskyty GetBlob zobrazíte při použití, co můžete očekávat, tedy a ujistěte se, někdo nepřistupuje ke svému účtu úložiště nevhodně.

####<a name="resources"></a>Zdroje informací

-   [Úložiště analýzy](storage-analytics.md)

    Tento článek je přehled úložiště technologie pro analýzu a jak je povolit.

-   [Formát protokolu analýzy úložiště](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    Tento článek ukazuje formát ukládání analýzy protokolu a podrobné informace o dostupných polí, včetně – typ ověřování, který označuje typ ověřování žádosti o.

-   [Sledování úložiště účtu na portálu Azure](storage-monitor-storage-account.md)

    Tento článek ukazuje, jak konfigurovat sledování metriky a protokolování účtu úložiště.

-   [Při použití metriky úložišť Azure protokolování, AzCopy a zprávy Analyzer Poradce při potížích začátku do konce](storage-e2e-troubleshooting.md)

    Tento článek pojednává o řešení potíží pomocí analýzy úložiště a ukazuje, jak používat Microsoft Analyzer zprávy.

-   [Příručka provozní Analyzer Microsoft zprávy](https://technet.microsoft.com/library/jj649776.aspx)

    Tento článek je odkaz na Microsoft Analyzer zprávy a obsahuje odkazy kurz úvodní a souhrnné funkce.

##<a name="cross-origin-resource-sharing-cors"></a>Sdílení (CORS) Origin více zdrojů

###<a name="cross-domain-access-of-resources"></a>Access doménami zdrojů

U webový prohlížeč aplikaci jednu doménu, kvůli kterému žádost HTTP zdroje z jinou doménu, je místo toho žádost HTTP křížově origin. Příklad stránky HTML podávané množství od contoso.com žádá o jpeg hostitelem fabrikam.blob.core.windows.net. Z bezpečnostních důvodů omezit prohlížeče požadavků HTTP křížově origin zahájená z v rámci skriptů, jako třeba JavaScript. To znamená, že požaduje-li některé kód v JavaScriptu na webovou stránku na contoso.com, které jpeg na fabrikam.blob.core.windows.net, nepovolí prohlížeči žádost.

Jaký to má dělat s Azure úložiště? Dobře, pokud ukládáte statické materiálů, jako jsou JSON nebo XML datových souborů v úložišti objektů Blob pomocí účtu úložiště jako Fabrikam majetku bude fabrikam.blob.core.windows.net a contoso.com webové aplikace nebude možné k nim získat přístup pomocí jazyka JavaScript, protože domény se liší. Toto je PRAVDA, pokud chcete zavolat jednu z úložiště služby Azure – například úložiště tabulek – které vrací JSON dat na zpracování klientem JavaScript.

####<a name="possible-solutions"></a>Možné řešení

Jedním ze způsobů tento problém vyřešíte je přiřadit fabrikam.blob.core.windows.net vlastní doménu, třeba "storage.contoso.com". Je to, že můžete nastavit pouze vlastní domény k účtu jednoho úložiště. Co když aktiva jsou uložené ve více účtů úložiště?

Další způsob, jak tento problém vyřešíte má mít webové aplikace fungovat jako proxy serveru pro volání úložiště. To znamená, pokud se nahrávání souboru k úložišti objektů Blob, webové aplikace by buď napsat místně a pak adresu zkopírovat k úložišti objektů Blob nebo ho by všechno načíst do paměti a potom ho k úložišti objektů Blob. Případně můžete napsat vyhrazené webové aplikace (například rozhraní API webových), která odešle soubory místně a zapíše k úložišti objektů Blob. V obou případech musíte účtu pro tuto funkci při určování škálovatelnost potřebuje.

####<a name="how-can-cors-help"></a>Jak může pomoci CORS?

Azure úložiště umožňuje povolit CORS – křížové sdílení zdrojů Origin. Pro každý účet úložiště můžete určit domény, které mají přístup k prostředkům v tomto účtu úložiště. V našem případě výše uvedených jsme například povolte CORS na účtu úložiště fabrikam.blob.core.windows.net a nakonfigurujte ji povolit přístup k contoso.com. Potom contoso.com webové aplikace přímo přístup k prostředkům ve fabrikam.blob.core.windows.net.

Poznámka je CORS umožňujícímu přístup, ale nenabízí ověřování, který je potřebný pro všechny jiné veřejné přístup úložiště zdrojů. To znamená, že objekty BLOB můžete jenom přístup, pokud jsou veřejné nebo obsahovat sdílené přístup podpis pojmenování odpovídající oprávnění. Tabulek, dotazů a soubory nemají přístup veřejné a požadovat přidružení zabezpečení.

Ve výchozím nastavení je vypnutá, CORS na všechny služby. Povolit CORS pomocí rozhraní REST API nebo knihovně úložiště klienta volání metod k nastavení zásad služby. Po výběru zahrnete CORS pravidlo, které je ve formátu XML. Tady je příklad CORS pravidlo, které byl nastaven myší nastavení vlastnosti služby pro službu objektů Blob účtu úložiště. Můžete provádět operace použití knihovně úložiště klienta nebo rozhraní REST API pro Azure úložiště.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Tady je význam každý řádek:

-   **AllowedOrigins** Toto říká domény které neodpovídající můžete požádat o a přijímat data z úložiště služby. Říká, že contoso.com a fabrikam.com můžete požádat o data z úložiště objektů Blob účtu konkrétní úložiště. Můžete vytvořit také to na zástupný znak (\*) umožňuje všech domén pro přístup k žádosti o.

-   **AllowedMethods** Toto je seznam metod (slovesa žádost HTTP), které lze použít při vytváření žádosti. V tomto příkladu jsou povoleny pouze umístění a NAČÍST. To můžete nastavit, aby zástupný znak (\*) umožňuje všechny metody se nemusí používat.

-   **AllowedHeaders** Toto je žádost o záhlaví, které původní doméně můžete zadat při vytváření žádosti. V tomto příkladu jsou povoleny všechna záhlaví metadat počínaje x-ms metadata, x ms meta cílové a x ms meta abc. Zástupný znak (\*) označuje, že jsou povoleny všechny záhlaví začínající na s předponou zadaný.

-   **ExposedHeaders** To ukazuje, které odpověď záhlaví by měl zveřejněné příslušným prohlížeč Vystavitel žádost. V tomto příkladu libovolné záhlaví počínaje "x-ms - meta-" k dispozici.

-   **MaxAgeInSeconds** Toto je maximální počet hodin, že bude v prohlížeči mezipaměti výstupem žádost možnosti. (Další informace o výstupem žádost, najdete v první z níže uvedených článků.)

####<a name="resources"></a>Zdroje informací

Další informace o CORS a jak jej povolit podívejte se na tyto materiály.

-   [Sdílení (CORS) Podpora služby Azure úložiště na Azure.com Origin více zdrojů](storage-cors-support.md)

    Tento článek obsahuje přehled CORS a nastavení pravidel pro různé úložiště služby.

-   [Sdílení (CORS) Podpora služby Azure úložiště na webu MSDN Origin více zdrojů](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Toto je odkaz si přečtěte následující dokumentaci pro podporu CORS úložiště služby Azure. To obsahuje odkazy na články použití u každé úložiště služby ukazuje příklad a vysvětluje každý prvek v souboru CORS.

-   [Microsoft Azure úložiště: Úvodní informace o CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Toto je odkaz na článek počáteční blogu oznamující CORS a znázorňující, jak se používá.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Nejčastější dotazy k úložišti Azure zabezpečení

1.  **Jak lze ověřit integrita, pokud nelze použít protokol HTTPS se mi přenos nebo stmívání Azure úložiště objektů BLOB?**

    Pokud z nějakého důvodu, které musíte použít protokol HTTP místo HTTPS a práci s objekty BLOB blok, můžete kontrolu MD5 k ověření integrity objektů BLOB převáděných. To vám pomůže s ochranou ze sítě/transport layer chyby, ale ne nutně s zprostředkovatel útoky.

    Pokud používáte HTTPS, který zajišťuje zabezpečení na úrovni přenosu, bude pomocí kontroly MD5 nadbytečné a zbytečnému.
    
    Další informace podívejte se na [Přehled MD5 objektů Blob Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Co dávat pozor FIPS-dodržování předpisů v rámci vláda USA?**

    Spojené státy Federal informace zpracování standardní (FIPS) definuje cryptographic algoritmů pro použití schváleno americké Federal government počítačové systémy pro ochranu citlivá data. Povolení FIPS režimem systému Windows server nebo plochy, bude operační systém, že bude použito pouze FIPS ověřit algoritmy šifrování. Pokud aplikace používá nevyhovujícím algoritmů, zruší se aplikace. Verze With.NET Framework 4.5.2 nebo vyšší, aplikace automaticky přepne algoritmů kryptografický použití kompatibilní se standardem FIPS algoritmů při počítače v režimu FIPS.

    Microsoft zůstane až jednotlivé zákazníky se rozhodnout, jestli chcete povolit režim FIPS. Jsme myslíte, že není atraktivní důvod pro zákazníky, kteří nejsou vyměřené poplatky za jeho government právní povolit režim FIPS ve výchozím nastavení.

    **Zdroje informací**

-   [Proč jsme není doporučující "Režim FIPS" přestal](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Tento článek blogu dává přehled o FIPS a vysvětluje, proč není umožňují režim FIPS ve výchozím nastavení.

-   [FIPS 140 ověření](https://technet.microsoft.com/library/cc750357.aspx)

    Tento článek obsahuje informace o tom, jak produkty společnosti Microsoft a cryptographic moduly v souladu s standardu FIPS pro USA vláda.

-   ["Systém šifrování: použití FIPS algoritmů kompatibilních pro šifrování, algoritmus hash a podepisování" vliv na nastavení zabezpečení v systému Windows XP a v novějších verzích Windows](https://support.microsoft.com/kb/811833)

    Tento článek pojednává o použití režimu FIPS ve starším počítače se systémem Windows.
