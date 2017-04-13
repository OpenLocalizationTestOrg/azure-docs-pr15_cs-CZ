<properties 
    pageTitle="Jak používat bodování profilů v Azure hledání | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu" 
    description="Ladění prostřednictvím bodování profilů v Azure vyhledávání hostovanou cloudu vyhledávací služby na Microsoft Azure hodnocení výsledků vyhledávání." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Použití bodování profilů v Azure hledání

Hodnocení profily jsou funkce Microsoft Azure hledání, které přizpůsobení výpočtu výsledků hledání, vliv na způsob řazení položek v seznamu výsledků hledání. Si můžete představit bodování profily jako způsob, jak modelu relevance tím položky, které splňují předdefinované kritéria. Předpokládejme například, že aplikace je online hotelu rezervace webem. Tím `location` pole výsledky hledání, které zahrnutí termínu jako Seattlu bude mít za následek vyšší skóre pro položky, které mají Seattlu v `location` pole. Všimněte si, že můžete mít víc než jeden bodování profil nebo žádný, pokud bodování výchozí aplikace.

Můžete experimentovat s bodování profily, můžete si stáhnout ukázku aplikace, která používá bodování profily Pokud chcete změnit pořadí řazení výsledků hledání. Vzorek tvoří aplikace konzoly – možná není velmi reálné pro vývoj aplikací v reálném světě – ale užitečné přesto jako prostředek učení. 

Ukázková aplikace ukazuje bodování chování při použití fiktivní dat s názvem `musicstoreindex`. Zjednodušení aplikaci ukázka usnadňuje bodování profily a upravovat dotazů a potom projeví okamžitě na pořadí pořadí při spuštění programu.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Zjistit předpoklady pro

Ukázková aplikace je napsané v jazyce C# pomocí aplikace Visual Studio 2013. Pokud ještě nemáte kopii Visual Studiu, zkuste bezplatné [Visual Studio 2013 Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) .

Budete potřebovat Azure předplatné a služby Azure vyhledávání k dokončení kurzu. Nápovědu k nastavení služby najdete v článku [Vytvoření vyhledávací služby na portálu](search-create-service-portal.md) .

[AZURE. ZAHRNOUT [musíte mít účet Azure tento kurz:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Stáhněte si aplikaci ukázka

Přejděte na [Ukázku profily bodování Azure hledání](https://azuresearchscoringprofiles.codeplex.com/) na webu codeplex ke stažení ukázkových aplikace popsaných v tomto kurzu.

Na kartě zdrojového kódu klikněte na **Stáhnout** získat soubor zip řešení. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>Úprava app.config

1. Po extrahujte soubory otevřete řešení ve Visual Studiu úpravy konfiguračního souboru.
1. V Průzkumníku poklikejte **app.config**. Tento soubor Určuje koncový bod služby a `api-key` slouží k ověření žádost. Tyto hodnoty můžete získat z portálu Microsoft klasické.
1. Přihlaste se k [portálu Azure](https://portal.azure.com).
1. Přejděte na řídicí panel služeb mají být vyhledávány Azure.
1. Klikněte na dlaždici **Vlastnosti** zkopírujte adresu URL služby
1. Klikněte na dlaždici **klíče** zkopírovat `api-key`.

Po dokončení přidávání adresu URL a `api-key` na app.config, nastavení aplikace by měl vypadat takto:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Prozkoumání aplikace

Téměř připraveni k vytvoření a spuštění aplikace, ale před provedením, podívejte se na JSON soubory slouží k vytvoření a naplnění index.

**Schema.JSON** definuje index, včetně bodování profily, které jsou zvýrazněny v Tato ukázka. Oznámení, že schéma obsahuje všechny sloupce použité v rejstříku, včetně vyhledávání, jako například `margin`, využívající bodování profilu. Hodnocení profilu syntaxe jsou popsány v [Přidat bodování profilu do indexu vyhledávání Azure](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1 3.json** obsahuje data, 246 alba přes hrstku žánry. Data je tvořen kombinací skutečné album a hledání umělců informace s fiktivní polí jako `price` a `margin` použité ke znázornění vyhledávání operací. Datové soubory odpovídat index a jsou odeslány Azure vyhledávací služby. Po odeslání a indexované dat můžete zadejte dotazů u ní.

**Program.cs** provede tyto operace:

- Otevře okno konzoly.

- Připojení k vyhledávání Azure pomocí adresy URL služby a `api-key`.

- Odstraní `musicstoreindex` pokud existuje.

- Vytvoří novou `musicstoreindex` použití schema.json souborů.

- Vyplní index pomocí datové soubory.

- Dotazy index pomocí čtyř dotazů. Všimněte si, že bodování profily jsou zadány jako parametr dotazu. Všechny dotazy z hledat stejné termín "nejlepší". První dotaz ukazuje bodování výchozí. Zbývající tři dotazů pomocí bodování profilu.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Vytvoření a spuštění aplikace

Vylučte připojení nebo sestavení odkaz problémy, vytvoření a spuštění aplikace zajistit neexistují problémy pro práci se první. Měli byste vidět aplikace konzoly otevřete na pozadí. Všechny čtyři dotazy provést postup bez pozastavení. V mnoha sítě celý program spustí než 15 sekund. Pokud aplikaci konzoly obsahuje zprávou "Hotovo. Pokračujte stisknutím klávesy enter", program byla úspěšně dokončena. 

Pokud chcete porovnat dotaz se spustí, můžete označit, kopírování, vkládání výsledků dotazu z konzoly a jejich vložením do souboru aplikace Excel. 

Následující obrázek znázorňuje výsledky první tři dotazů vedle sebe. Všechny dotazy používají stejné klíčová slova nejlépe, které se zobrazí v mnoha album názvy.

   ![][10]

Použití první query bodování výchozí. Vzhledem k tomu hledaný výraz se zobrazí pouze v album názvy a zadána žádná další kritéria, budou vráceny položek, které mají "nejlepší" v názvu album v takovém pořadí, ve kterém je najde vyhledávací služby. 

Druhý dotaz používá bodování profilu, ale Všimněte si, kterou profilu měli žádný vliv. Výsledky jsou stejné jako první dotaz. Je to proto bodování profilu zvyšuje pole (žánr), který není germane do dotazu. Hledaný termín "nejlepší" neexistuje v libovolné pole "žánr" libovolného dokumentu. Po bodování profilu žádný vliv, výsledky jsou stejná jako výchozí bodování.  

Třetí dotazu je první evidence bodování dopad profilu. Hledaný termín je pořád nejlepší tak, aby se stejná skupina alba pracujeme, ale že bodování profilu poskytuje další kritéria, která zvyšuje "hodnocení" a "poslední aktualizaci", některé položky jsou pohybu vyšší v seznamu.

Následující obrázek znázorňuje dotazu čtvrtou a poslední zesilován "marže". Pole "marže je není s možností vyhledávání a nelze zobrazit ve výsledcích hledání. "Marže" jsme přidali ručně do tabulky a názorně bod, který položek pomocí vyšší okraje neprojevila vyšší v seznamu výsledků hledání. 

   ![][9]

Teď, když budete mít experimented s bodování profily, zkuste změnit program, který chcete použít jiný dotaz syntaxi bodování profily nebo bohatší data. Odkazy v následující části poskytují další informace.

<a id="next-steps"></a>
## <a name="next-steps"></a>Další kroky

Další informace o bodování profily. Další informace najdete v článku [Přidání hodnocení profilu do indexu vyhledávání Azure](http://msdn.microsoft.com/library/azure/dn798928.aspx) .

Další informace o syntaxi a dotaz parametrů hledání. Podrobnosti najdete v části [Hledat dokumenty Azure hledání REST API ()](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Potřebujete ke kroku zpět a další informace o vytváření indexů? [V tomto videu](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) pro seznámení se základy.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 