<properties
   pageTitle="Doporučené postupy pro podniky přesunutí do Azure | Microsoft Azure"
   description="Popisuje scaffold využívající podniky zajistit zabezpečené a spravovatelné prostředí."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise scaffold - zásady správného řízení obvyklé předplatného

Podniky stále můžete veřejné cloudu pro jeho flexibilitu a flexibilitu. Budou se používají v cloudu pevnosti generovat výnosy nebo optimalizaci zdroje pro firmy. Microsoft Azure nabízí velký počet služeb, že podniky můžete sestavit jako stavební bloky, který bude řešit celou řadou pracovního vytížení a aplikací. 

Ale vědět, kde začít často není snadné. Jakmile se rozhodnete používat Azure běžně nastanou na pár otázek:

- "Jak můžu odpovídají naše zákonné požadavky zůstala zachovaná svrchovanost data v některých zemích?"
- "Jak zjistím, že někdo nemění omylem důležitých systémových?"
- "Jak poznám co každý zdroj podporuje tak, aby se účtu a účtu přesně zpátky?"

Potenciální zákazník prázdné předplatného se žádné stráž profilů je individuálního. Tento mezeru můžete omezovat přesunout do Azure.

Tento článek obsahuje výchozí bod pro odborníci adresy nutnosti zásady správného řízení a zůstatek s nutností flexibilitu. Uvádí koncepci scaffold organizace, která provede organizace při provádění a správu jejich Azure předplatná. 

## <a name="need-for-governance"></a>Potřebujete pro zásady správného řízení

Při přesunu Azure, musíte adresu v tématu Zásady správného řízení nejdříve možné zajistit úspěšná využívání cloudu v rámci organizace. Bohužel čas a byrokracie vytváření systému komplexní řízení znamená, že některé obchodní skupiny přejít přímo dodavatelé bez týkající se organizace IT. Tento postup můžete nechat podniku otevřené chybami Pokud nejsou správně spravovat zdroje. Vlastnosti veřejného cloudových – flexibilitu flexibilitu a na základě spotřebu ceny - jsou důležité pro firmy skupiny, které je potřeba rychle požadavkům zákazníků (interní i externí). Ale podnikové potřebuje k zajištění efektivně ochrany dat a systémů.

Ve skutečnosti vygenerovaných slouží k vytvoření základ strukturu. Scaffold příručky obecné osnovy a poskytuje ukotvení body trvalejší systémy v počítačích chcete připojit. Pole organizace scaffold je stejná: množiny flexibilní ovládací prvky a Azure funkcí, které poskytují strukturu prostředí a ukotvení služby založená na veřejné cloudu. Poskytuje tvůrci (IT a obchodní skupiny) foundation a vytvořte nové služby.

Scaffold vychází z postupy, pomocí kterých jsme shromáždili z mnoha závazky s klienty různých velikostí. Tyto klientů sahají od malých organizacích vývoj řešení v cloudu, aby Fortune 500 podniky a dodavatelů nezávislé softwaru, kteří jsou migrace a vytváření řešení v cloudu. Pole organizace scaffold je "purpose-built" být flexibilní kvůli podpoře tradiční pracovních vytížení a aktivní úloh; například vývojáři vytváření aplikací software jako-service (SaaS) založené na Azure možnosti.

Pole organizace scaffold má být základem každé nové předplatné v Azure. Umožňuje správcům zajistěte, aby pracovního vytížení požadavkům minimální zásady správného řízení organizace bez brání rychle schůzky vlastní cíle obchodní skupiny a vývojáři.

> [AZURE.IMPORTANT] Zásady správného řízení je nezbytné k úspěchu Azure. Tento článek zaměřuje technické provádění scaffold organizace, ale jenom nedotýká na širší vztahy mezi složkami a proces. Zásady řízení toku shora dolů a je určený podle firmy chce k dosažení. Samozřejmě vytvoření modelu zásad správného řízení pro Azure včetně zástupců IT, ale důležitější by měl mít silných znázornění z firmy skupiny Vedoucí a zabezpečení a řízení rizik. V konečném scaffold organizace se zabývá zmírnění rizik firmy usnadnit poslání a cílů organizace.

Na následujícím obrázku jsou popsány součásti scaffold. Základem závisí na plnou plán pro oddělení, účtů a předplatné. Pilíře tvořeny zásady správce prostředků a silných naming standardy. Zbytek scaffold pochází z core Azure funkce a funkce, které povolit zabezpečené a spravovatelné prostředí.

![součásti scaffold](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure roste rychle od svého uvedení 2008. Tento růst vyžaduje Microsoft konstrukce týmy přehodnotit jejich přístup pro správu a nasazení služeb. Správce prostředků Azure modelu byla zavedená v 2014 a nahradí modelu klasické nasazení. Správce prostředků umožňuje organizacím snadněji nasazení, uspořádání a řízení Azure zdroje. Správce prostředků obsahuje paralelního zpracování při vytváření zdroje pro rychlejší nasazení složité, vzájemně závislých řešení. Obsahuje taky řízení podrobného přístupu a možnost značku zdroje s metadaty. Microsoft doporučuje vytvořit všechny zdroje prostřednictvím modelu správce prostředků. Scaffold organizace je explicitně určený pro správce prostředků modelu.

## <a name="define-your-hierarchy"></a>Definování hierarchii

Základem scaffold jsou přihlášení Azure Enterprise (a portálu). Registrační enterprise definuje obrazci a pomocí služby Azure ve společnosti a je základní struktury řízení. V rámci organizace smlouvy souhlasím zákazníci se dál rozdělit prostředí do oddělení účty a nakonec předplatná. Předplatné Azure je základní jednotka, kde jsou obsaženy všechny zdroje. Definuje také několik meze Azure, jako je třeba počet jádra, zdrojů, atd.

![hierarchie](./media/resource-manager-subscription-governance/agreement.png)

Každý organizace se liší a hierarchie v předchozí obrázek umožňuje významné pružnost při uspořádání Azure v rámci podniku. Před implementací pokyny obsažené v tomto dokumentu, měli byste model hierarchii a pochopit dopad na fakturace, přístup k prostředku a složitosti.

Jsou tři běžné vzorce pro Azure přihlášení:

- **Funkční** vzorek

    ![funkčnosti](./media/resource-manager-subscription-governance/functional.png)

- **Organizační jednotka** vzorek 

    ![Business](./media/resource-manager-subscription-governance/business.png)

- **Zeměpisná** vzorek

    ![zeměpisná](./media/resource-manager-subscription-governance/geographic.png)

Použití scaffold na úrovni předplatné prodloužit řízení požadavků podniku do předplatné.

## <a name="naming-standards"></a>Pojmenování standardy

První pilíře scaffold je standardního pojmenování. Dobře navržená názvový standardy umožňují určit zdroje na portálu na směnky a v rámci skriptů. Největší pravděpodobností už máte pojmenování standardy pro místní infrastrukturu. Při přidávání Azure prostředí, by měl rozšíříte normy názvový Azure zdroje. Pojmenování standardní usnadnění efektivnějšímu řízení prostředí na všech úrovních.

> [AZURE.TIP]
>
> - Zkontrolujte a přijmout where možné [vzorků a postupy pokyny](./guidance/guidance-naming-conventions.md). Tyto pokyny vám pomůže se rozhodnout na smysluplné standardní pojmenování.
> - Použití camelCasing pro názvy zdrojů (například myResourceGroup a vnetNetworkName). Poznámka: Existují určité zdroje, jako je třeba úložiště účty, kde je toto jediná možnost používat malými písmeny (a jiné speciální znaky).
> - Zvažte použití zásad správce prostředků Azure (popsané v následující části) k jejímu vynucení naming standardy.
 
## <a name="policies-and-auditing"></a>Zásady a auditování

Druhého pilíře scaffold zahrnuje vytvoření [zásady správce prostředků Azure](resource-manager-policy.md) a [auditování protokolu činnosti](resource-group-audit.md). Správce prostředků zásady poskytují možnost řízení rizik v Azure. Můžete definovat zásad, které zajistit, aby zůstala zachovaná svrchovanost data omezení, vynucení nebo auditování určité akce. 

- Zásady je výchozí **Povolit** systém. Řízení akcí definování a přiřazení zásady prostředky, které odmítnout nebo auditovat akce na zdroje.
- Zásady popsané definice zásad v jazyce definice zásad (Pokud pak podmínky).
- Vytváření zásad s JSON (Javascript Object Notation) formátovaný soubory. Po definování zásad, můžete ji přiřadit určitý obor: předplatné, skupina zdroje. nebo zdroje.

Zásady mít několik akcí, které umožňují jemně odstupňovaná přístupu k vaší scénáře. Jsou tyto akce:

-   **Odepřít**: blokuje žádost o zdroj
-   **Auditování**: umožňuje žádost, ale do přidá čára protokolu činnosti (nichž lze zajistit upozornění nebo aktivovat runbooks)
-   **Přidávací**: Přidá do zdroje zadané informace. Pokud na zdroj není značku "CostCenter", například přidáte značka zmíněná s výchozí hodnotou.

### <a name="common-uses-of-resource-manager-policies"></a>Společné použití zásad správce prostředků

Azure zásady správce prostředků jsou výkonný nástroj v Azure sady nástrojů. Umožňují zabránit neočekávané náklady, k identifikaci nákladové středisko pro zdroje prostřednictvím označování a ověřit, zda jsou splněny stávající1 požadavky. Když zásad, se sloučí s předdefinované funkce auditování, můžete odpovídat složité a flexibilní řešení. Dovolit zásady společnosti, které poskytují ovládací prvky pro pracovního vytížení "Tradiční IT" a "Agilní" úloh; například vývoj aplikací zákazníka. Nejběžnější vzorků zobrazované zásad jsou:

- **Svrchovanost data Geo dodržování předpisů /** - Azure poskytuje oblasti po celém světě. Podniky často chcete určit, kde zdroje vytvářejí (ať už zajistit zůstala zachovaná svrchovanost data nebo zajistěte, aby zdrojů se vytvářejí zavřít spotřebitele ukončit zdrojů).
- **Správa náklady** - předplatné Azure může obsahovat prostředků mnoho typů a měřítko. Sdruženími často chcete zajistit, že standardní předplatná nepoužívání zbytečně velký zdroje, které můžete nákladů stovky dolary zabere nejméně jeden měsíc.
- **Výchozí zásady správného řízení prostřednictvím požadované značky** - vyžadující značky je jednou z funkcí nejběžnější a vysoce požadované. Použití zásad správce prostředků Azure podniky se ujistěte, že je zdroj řádně podporovat příznakem. Jsou nejčastější značky: oddělení, vlastníka zdroje a prostředí typu (například - výrobní test, vývoj)

**Příklady**

"Tradiční IT" předplatné pro řádek podnikových aplikací

-   Vynucení oddělení a vlastník značky u všech zdrojů
-   Vytváření zdroje k oblasti Severní Americe omezit
-   Omezit možnost vytvořit VMs řady G a HDInsight clusterů

"Aktivní" prostředí pro organizační jednotku vytváření aplikací cloudu

- Požadavkům svrchovanost data, umožňují vytvářet zdroje jen v určité oblasti.
- Vynucení prostředí značky u všech zdrojů. Pokud zdroje je vytvořená bez značku, připojte **prostředí: Neznámý** značku tomuto zdroji.
- Auditování při zdroje vytvářejí mimo Severní Americe, ale nezabrání.
- Auditování při vytvoření maximum nákladové zdroje.

> [AZURE.TIP]
>
> Nejběžnější využívání zásady správce prostředků v organizacích je ovládací prvek *kde* zdrojů lze vytvořit a *Jaké* typy zdrojů lze vytvořit. Kromě poskytování ovládacích prvků na *místo, kam* a *Jaké*mnoho podniky umožňuje zásady zajistit zdroje příslušná metadata k fakturování zpět spotřebu. Doporučujeme použití zásad na úrovni předplatné pro:
>
> - Svrchovanost data GEO dodržování předpisů /
> - Řízení nákladů
> - Požadované značky (ROZHODNUTY tak, že obchodních potřeb, například BillTo, vlastník aplikace)
>
> Můžete použít další zásady nižší úrovně obor.
 
### <a name="audit---what-happened"></a>Auditování – příčinách?

Pokud chcete zobrazit, jak funguje prostředí, budete muset auditování činnosti uživatelů. Většina typů zdrojů v Azure vytvořit diagnostických protokolů, které můžete analyzovat pomocí nástroje protokolu nebo sady správy operace Azure. Shromáždění protokolů činnosti přes víc předplatných poskytovat oddělení nebo zobrazení organizace. Auditování záznamů jsou důležitým nástrojem diagnostické a důležité mechanismus aktivační události v Azure prostředí.

Protokoly aktivity ze Správce prostředků nasazení umožňují určit **operací** , jako když umístěte a kdo provádí. Protokoly aktivity můžete shromažďovat a agregovat pomocí nástroje jako protokolu analýzy.

## <a name="resource-tags"></a>Značky zdroje

Jak uživatelé ve vaší organizaci přidání zdrojů do předplatného, bude stále důležité přidružení zdroje k odpovídající oddělení, zákazníka a prostředí. Metadata můžete připojit k prostředkům prostřednictvím [značky](resource-group-using-tags.md). Pomocí značek jsou uvedeny informace o zdroji nebo vlastník. Značky umožňují nejen agregovat seskupení zdrojů různými způsoby, ale použijte tato data pro účely zpětné. Můžete označit zdroje s až 15 klíč: dvojice. 

Značky zdroje jsou flexibilní a by měl být připojen většina prostředků. Příklady běžných značek zdroje:

- BillTo
- Oddělení (nebo organizační jednotka)
- Prostředí (výrobní fázi, vývoj)
- Osy (osy Web, vrstva aplikace)
- Vlastník aplikace
- Název projektu

![značky](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Další příklady značek naleznete v tématu [Doporučené pojmenování konvence pro Azure zdroje](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Vytvoření značek strategie, která označuje v rámci předplatného jaká metadata potřebné pro firmy, finance, zabezpečení, řízení rizik a celkové správy prostředí. Zvažte zásadu, která ukládá značky pro:
>
> - Skupiny zdrojů
> - Úložiště
> - Virtuálních počítačích
> - Aplikace služby prostředí/webových serverů

## <a name="resource-group"></a>Pole Skupina zdroje 

Správce prostředků umožňuje umístění zdroje do smysluplného skupin příbuznosti Správa fakturace nebo přírodní. Jak jsme zmínili dříve, Azure obsahuje dva modely nasazení. V předchozích modelu Klasický byla základní jednotka správy předplatného. Bylo obtížné rozdělí zdroje v rámci předplatného, které vedly k vytvoření velkého množství předplatná. S modelem správce prostředků jsme viděli Úvod skupiny zdrojů. Skupiny prostředků jsou kontejnery zdrojů, které máte společné životního cyklu nebo sdílet atribut například "všech serverů SQL Server" nebo "Aplikace A".

Skupiny zdrojů nemůže být obsažen v sobě a zdroje můžete jenom patří do skupiny jeden zdroj. Některé akce můžete použít u všech zdrojů v skupina zdroje. Odstranění skupiny zdrojů například odebere všechny zdroje v rámci skupiny zdrojů. Obvykle umístěte celou aplikaci nebo systémové ve stejné skupině zdroje. Tři úrovně aplikace s názvem Contoso webové aplikace by například obsahovat webového serveru, aplikační server a SQL server ve stejné skupině prostředků.

> [AZURE.TIP]
>
> Jak uspořádání skupin zdroje se může lišit z úloh "Tradiční IT" k "aktivní" pracovních vytížení
>
> - "Tradiční IT" pracovního vytížení jsou nejčastěji seskupená podle položky v rámci stejné životního cyklu, například aplikace. Seskupení tak, že aplikace umožňuje správa jednotlivých aplikací.
> - "Aktivní IT" pracovního vytížení převážně zaměřují na externí cloudu zákazníka webových aplikací. Skupiny zdrojů odrážet vrstvy nasazení (například osy webové aplikace osy) a řízení.

## <a name="role-based-access-control"></a>Řízení přístupu na základě rolí

Pravděpodobně pokládáte sami "kdo má přístup k prostředkům?" a "jak se dá nastavit přístup?" Povolení nebo zákaz přístup na portál Azure a řízení přístupu k prostředkům ve portálu je velmi důležité. 

Pokud byla původně Azure, řízení přístupu k předplatnému byly základní: spolu správce nebo správce. Přístup k předplatnému v dialogu přístup modelu předpokládané klasické pro všechny zdroje na portálu. Tento chybějící ovládacího prvku jemně odstupňovaná vedly k šíření předplatná poskytují úroveň ovládacího prvku rozumné přístup pro zápis Azure.

Již není potřeba této šíření předplatná. Řízení přístupu na základě rolí můžete přiřadit uživatelům standardní rolím (například "čtečka" a "Autor" běžného role). Můžete také definovat vlastní role.

> [AZURE.TIP]
>  
> - Připojení vaší firemní identity úložiště přihlašovacích údajů (nejčastěji služby Active Directory) pro službu Azure Active Directory pomocí nástroje AD Connect.
> - Ovládací prvek správce/Co – správce předplatného pomocí spravovaného identity. **Není** přiřaďte nové vlastník předplatného správce/Co – správce. Zajistit práva **vlastníka** do skupiny nebo jednotlivé místo toho použijte RBAC role.
> - Přidání Azure uživatelů do skupiny (například aplikace X vlastníci) ve službě Active Directory. K zajištění členy skupiny příslušná oprávnění ke správě skupina zdroje obsahující aplikaci používají synchronizované skupiny.
> - Postupujte podle zásady poskytování **nejnižších možných oprávnění** požadovaných pro očekávané práci. Příklad:
>     - Nasazení: Skupiny, který je možné instalovat zdroje.
>     - Správa virtuálního počítače: Skupinu, která dokáže restartovat VMs (pro operace)

## <a name="azure-resource-locks"></a>Uzamčení Azure prostředků

Jak vaše organizace přidá služby základní k předplatnému, bude stále důležitější zajistit, že tyto služby má k dispozici vyhnout firmy narušením. [Uzamčení prostředků](resource-group-lock-resources.md) umožňují omezit operace na místo, kam úpravami nebo odstraněním by ovlivňovat významné aplikace nebo cloudové infrastruktury vysoké hodnoty zdroje. Uzamčení můžete použít k předplatnému, skupina zdroje nebo zdroje. Obvykle použijete zámků webů základní zdroje, jako jsou virtuálních sítí, brány a úložiště účty. 

Uzamčení prostředků momentálně podporují dvou hodnot: CanNotDelete a jen pro čtení. CanNotDelete znamená, že uživatelé (s příslušnou právy) pořád číst nebo upravit zdroje ale nelze odstranit. Jen pro čtení znamená, že uživatelé nelze odstranit nebo upravit zdroj.

Vytvoření nebo odstranění zámky správy, musíte mít přístup k `Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce.
Předdefinované rolí jenom vlastník a správce přístup uživatelů udělena tato akce.

> [AZURE.TIP] Základní síť možnosti by měly být chráněny s zámků webů. Nechtěným odstraněním brány na webu VPN by katastrofální předplatné Azure. Azure neumožňuje odstranit virtuální sítě, který se používá, ale použití více omezení je užitečné opatření. Zásady jsou také klíčové pro zachování příslušných ovládacích prvků. Doporučujeme použít **CanNotDelete** zámkem zásad, které se používají.
>
> - Virtuální sítě: CanNotDelete
> - Skupina zabezpečení sítě: CanNotDelete
> - Zásady: CanNotDelete

## <a name="core-networking-resources"></a>Základní síťový prostředky

Přístup k prostředkům mohou být interní (v rámci podnikové sítě) nebo externí (přes internet). Není těžké uživatelům ve vaší organizaci omylem umístění zdroje ve špatném místě a potenciálně nebezpečný přístup otevírat. Stejně jako u místních zařízení, musíte přidat podniky příslušných ovládacích prvků zajistit, aby uživatelům Azure rozhodovat pravém. Pro správu předplatného budou popsána základní prostředky, které poskytují základní ovládací prvek aplikace access. Základní prostředky tvořeny:

- **Virtuální sítě** jsou objekty kontejneru pro podsítě. Když není nezbytných často používá se při připojení aplikace interní podnikové prostředky.
- **Skupiny zabezpečení sítě** se podobají bránu firewall a zadat pravidla jak zdroje můžete "mluvit" v síti. Poskytují podrobného publikum nemůže ovládat způsob / Pokud podsítě (nebo virtuálního počítače) můžete připojení k Internetu nebo jiných podsítí ve stejné síti virtuální.

![základní sítě](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Vytvořte virtuální sítě snaží o externím pracovního vytížení a interních úloh. Tento přístup snižuje pravděpodobnost omylem uvedení virtuálních počítačích, které jsou určené pro interní úloh v externí vystaveného místa.
> - Skupiny zabezpečení sítě je považován za kritický k této konfiguraci. Minimálně blokovat přístup k Internetu vnitřní virtuální sítě a blokovat přístup k podnikové síti externí virtuální sítě.

### <a name="automation"></a>Automatizace

Správa zdrojů jednotlivě je časově náročný a potenciálně chyby chybám pro určité operace. Azure poskytuje různé možnosti automatizaci včetně Azure automatizaci, použití logických operátorů aplikací a funkcí Azure. [Automatizace Azure](./automation/automation-intro.md) umožňuje správcům vytvářet a definovat runbooks zpracovávání běžné úkoly v řízení zdrojů. Vytvoření runbooks pomocí editoru kódu prostředí PowerShell nebo grafický editor. Můžete vytvářet složité vícestupňové pracovní postupy. Azure automatizaci často používá k zpracovat běžné úkoly ATP vypnutí nepoužité zdroje, vytvoření zdrojů v odpovědi na konkrétní aktivační událost aniž by musel zásahu člověka.

> [AZURE.TIP]
>
> - Vytvořte účet Azure automatizaci a zkontrolujte dostupné runbooks (řádek grafických a příkaz) k dispozici v [Galerii postupu Runbook](./automation/automation-runbook-gallery.md).
> - Import a přizpůsobení klíčové runbooks pro vlastní potřebu.
>
> Běžné situace je možnost zahájení nebo ukončení virtuálních počítačích podle plánu. Existuje runbooks příklad, které jsou dostupné v galerii, jak řešit tento scénář a vás naučí, jak rozbalte.


## <a name="azure-security-center"></a>Centrum zabezpečení Azure

Možná jednu největších blokování do cloudu zavádění okno zjištění pochybností přes zabezpečení. Správci IT rizika a zabezpečení oddělení nutné ověřit, že jsou zdroje v Azure zabezpečené. 

[Centrum zabezpečení Azure](./security-center/security-center-intro.md) obsahuje ústřední zobrazení stavu zabezpečení zdrojů v předplatných a doporučení, která zabránit hostují zdroje. Umožňuje je podrobnějších zásad (například použití zásad specifickým skupinám prostředků, které umožňují enterprise přizpůsobit své postoje rizika, které jsou adresování). Centrum zabezpečení Azure nakonec je otevřít platformu, která umožňuje partnery společnosti Microsoft a nezávislí dodavatelé softwaru vytvářet software, zapojení do Centrum zabezpečení Azure zlepšit jeho možnosti. 

> [AZURE.TIP]
>
> Centrum zabezpečení Azure aktivované ve výchozím nastavení každého předplatného. Shromažďování dat z virtuálních počítačích umožňuje Centrum zabezpečení Azure instalace jeho zástupce a začněte shromažďování dat však musíte povolit.
>
> ![shromažďování dat](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Další kroky

- Teď, když jste se naučili o řízení předplatného, je čas zobrazíte těmito doporučeními v praxi. Příklady [provádění zásady správného řízení Azure předplatného](resource-manager-subscription-examples.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) přispěla k v tomto tématu.*
