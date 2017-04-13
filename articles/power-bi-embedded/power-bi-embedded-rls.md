<properties
   pageTitle="Úroveň řádku cenného papíru s Power BI vložený"
   description="Podrobnosti o úrovni řádek cenného papíru s Power BI vložený"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Zabezpečení na úrovni řádku s Power BI vložený

Zabezpečení na úrovni řádku (RLS) mohou sloužit k omezení přístupu uživatelů pro konkrétní data v rámci sestavy nebo přehledu výkonnostních datovou sadu, umožňující několika různých uživatelé můžou používat stejnou sestavu při všechny vidět různé datové. Power BI vložený nyní podporuje nakonfigurována RLS datové sady.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Aby šlo využít RLS, je důležité že pochopit tři hlavní koncepty; Uživatelé, role a pravidla. Podívejme se podrobněji na každý:

**Uživatelé** – jedná se o skutečné koncoví uživatelé zobrazení sestavy. V Power BI vložený uživatelé jsou označeny vlastnost username v Token aplikace.

**Role** – uživatelé patří k rolím. Role je kontejner pro pravidla a může vypadat třeba "Vedoucí prodeje" nebo "Prodejce". V Power BI vložený uživatelé jsou označeny vlastnost role v aplikaci tokenu.

**Pravidla** – role pravidla a pravidla jsou skutečné filtry, které mají být použity pro data. To můžou skládat například "země = USA" nebo něco mnohem dynamičtější.

### <a name="example"></a>Příklad

Ve zbytku tohoto článku nabízíme příklad vytváření RLS a jinými, vložené aplikace. Náš příklad používá [Maloobchodní analýzy ukázkový](http://go.microsoft.com/fwlink/?LinkID=780547) soubor PBIX.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Našimi ukázkovými maloobchodní analýzy prodejů pro všechna úložiště v konkrétní maloobchodního řetězce. Bez RLS, bez ohledu na to, které oblast správce přihlásí a zobrazení sestavy, vidí stejná data. Vedení zjistil vedoucího oblast měli vidět jenom prodeje pro úložiště, které spravují a k tomuto účelu používáme RLS.

RLS je vytvořený v Power BI Desktop. Při otevření sady dat a sestav, jsme přepněte do zobrazení diagramu zobrazíte schématu:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Všimněte si s toto schéma několik věcí:

-   Všechny míry, jako je **Celkový prodej**, jsou uloženy v tabulce faktů **prodeje** .
-   Existují čtyři další související dimenze tabulky: **položky**, **čas**, **ukládání**a **oblast**.
-   Šipky na řádcích relace označují jaký způsob filtry můžete přetékat z jedné tabulky do dalšího. Například pokud filtr je umístěn dokončen **včas [datum]**, v aktuálním schématu ho jenom filtrovat dolů hodnoty v tabulce **Sales** . Ostatní tabulky by bude to mít vliv tak, že tento filtr od všechny šipky na řádcích relace přejděte k tabulce prodejů a ne pryč.
-   **Oblast** tabulka uvádí vnímat vedoucí pro každou oblast:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Na základě této schématu, pokud na **Správce oblast** sloupce v tabulce oblast použít filtr a shody, které filtrovat uživatele při zobrazení sestavy, tento filtr se filtrovat i v případě dolů **úložiště** a **Prodej** tabulky, které chcete zobrazit pouze data pro tuto konkrétní oblast správce.

Tady je způsob:

1.  Na kartě modelování klikněte na **Spravovat role**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Vytvořte novou roli **Správce**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  V tabulce **oblast** zadat Tento výraz jazyka DAX: **[oblast Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Abyste měli jistotu, že tato pravidla pracují na kartě **modelování** klikněte na **Zobrazit jako role**a potom zadejte následující údaje:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    V sestavách se nyní zobrazí data jako, pokud jste přihlášení jako **Andrew Ma**.

Použití filtru, bude způsob, jakým kroků uvedených v tomto filtrovat dolů všechny záznamy z tabulek **oblast**, **ukládání**a **Prodej** . Z důvodu směru filtr na vztahy mezi **prodeje** a **čas**však nebude tabulky **Sales** a **položky**a **položky** a **čas** filtrována dolů.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Mohou být pro tohoto požadavku na ok, však nechceme správci zobrazit položky, u kterých tito uživatelé nemají žádné prodeje, jsme pravděpodobně zapnout obousměrný křížovém filtrování relace a tok filtru zabezpečení v obou směrech. Lze to úpravou vztah mezi **prodeje** a **položky**, třeba takto:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Teď filtry můžete taky flow z tabulky Sales v tabulce **položky** :

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Poznámka:** Pokud používáte režimu DirectQuery pro vaše data, bude třeba povolit obousměrný křížové filtrování podle těchto dvou možností:

1.  **Soubor** -> **možností a nastavení** -> **Funkce** -> **Povolit křížové filtrování v obou směrech DirectQuery**.
2.  **Soubor** -> **možností a nastavení** -> **DirectQuery** -> **povolit neomezený míra v režimu DirectQuery**.


Další informace o obousměrný křížovém filtrování, stáhněte si dokument [obousměrný křížovém filtrování v SQL Server Analysis Services 2016 a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) .

To zalamuje nahoru všechny práce, kterou je potřeba provést v Power BI Desktop, ale je, že jeden další část práce, která je třeba udělat, aby RLS pravidla jsme v Power BI vložený definované práce. Uživatelé ověření a oprávnění v aplikaci a aplikace tokeny používají udělit přístup k sestavě konkrétní Power BI vložený. Power BI vložený nemá žádné konkrétní informace, kdo je vaše uživatele. Pro RLS pracovat musíte se předat některé další kontext jako součást aplikace token:
-   **uživatelské jméno** (volitelné) – použít s RLS Toto je řetězec, který slouží k identifikaci uživatele při použití RLS pravidel. V tématu Použití řádek vložený zabezpečení na úrovni s Power BI
-   **role** – řetězec obsahující role vyberte při použití pravidel řádku úrovně zabezpečení. Pokud předávání více než jednu roli, by měl být předané jako řetězců.

Pokud vlastnost uživatelské jméno je k dispozici, musíte taky předáte alespoň jednu hodnotu v jednotlivých rolích.

Úplné aplikace token bude vypadat nějak takhle:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Teď se všechny části dohromady, při přihlášení do naší aplikace zobrazíte sestavu, budou pouze měli zobrazíte data, která mohou zobrazit, podle našeho řádku úrovně zabezpečení.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Viz taky
[Řádek úrovně zabezpečení (RLS) s výkonem](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
