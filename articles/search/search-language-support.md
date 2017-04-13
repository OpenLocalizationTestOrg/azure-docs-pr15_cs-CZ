<properties
   pageTitle="Vytvoření indexu u dokumentů v různých jazycích v Azure hledání | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
   description=" Azure hledání podporuje 56 jazyků využívání jazyk analyzátory z Lucene a zpracování přirozeného jazyka technologie od Microsoftu."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Vytvoření indexu u dokumentů v různých jazycích v Azure hledání
> [AZURE.SELECTOR]
- [Portál](search-language-support.md)
- [ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Unleashing power jazyk analyzátory je stejně snadná jako nastavení jednu vlastnost s možností vyhledávání polem v definici indexu. Teď můžete udělat tento krok na portálu.

Tady jsou snímky obrazovek listy Azure portál mají být vyhledávány Azure, povolit uživatelům definovat schématem index. Z tohoto zásuvné uživatelé vytvořte všechna pole a nastavte vlastnost analyzátor pro každý z nich.

> [AZURE.IMPORTANT] Pouze nastavit jazyk analyzer během definice pole podle při vytváření nového indexu z důvodu nahoru nebo při přidávání nového pole do existující index. Zkontrolujte, že zadáte plně všech atributů, včetně Průvodce analýzou při vytváření pole. Nebudete moct upravit vlastnosti nebo změnit typ analyzer uložte provedené změny.

## <a name="define-a-new-field-definition"></a>Definování definici nové pole

1. Přihlaste se k [Portálu Azure](https://portal.azure.com) a otevřete služby zásuvné vyhledávací služby.
2. Klikněte na **Přidat indexu** v řádku nabídek v horní části řídicího panelu služeb začít nový index nebo otevřete existující index u nových polí, kterou chcete přidat do existující index nastavit analýza.
3. Zásuvné pole se zobrazí, která nabízí možnosti pro definování schématu index, včetně kartě Analyzer použít pro výběr analyzer jazyka.
4. V polích začněte definici pole názvem, vyberte datový typ a nastavíte atributy, které můžete použít označení pole celý text s možností vyhledávání, zobrazitelné ve výsledcích vyhledávání ve výsledcích hledání použitelné v podmínka navigační struktury seřaditelné a tak dále. 
5. Přejde na další pole, otevřete kartu **Analýza** . 

   
![][1]
*Chcete-li vybrat analyzer, klikněte na kartu Analyzer na zásuvné pole*

## <a name="choose-an-analyzer"></a>Klikněte analýza

6. Pomocí posuvníku najděte pole, které definujete. 
7. Pokud jste to ještě označili pole jako s možností vyhledávání, zaškrtněte políčko teď Pokud chcete označit jako **vhodné pro prohledávání**.
8. Klikněte na oblast analyzátor zobrazíte seznam dostupných analyzátory.
9. Klikněte na analýza, které chcete použít.

![][2]
*Vyberte jeden z podporovaných analyzátory pro jednotlivá pole*

Ve výchozím nastavení všechna pole s možností vyhledávání pomocí [Standardní Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) , což je bez ohledu na jazyk. Pokud chcete zobrazit úplný seznam podporovaných analyzátory, najdete v článku [Podpora jazyků v Azure vyhledávání](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Po výběru jazyka analyzer pro pole se použije při každém požadavku indexování a vyhledávání pro toto pole. Vystavení dotazu proti více polí pomocí různých analyzátory dotaz bude zpracovat nezávisle na sobě správné analyzátory u každého pole.

Mnoho webu a mobilní aplikace sloužit uživatelů ve světě pomocí různých jazycích. Je možné definovat rejstříku pro situace takto vytvořením pole pro jednotlivé jazyky podporované.

![][3]
*Definice indexu s polem popis pro jednotlivé jazyky podporované*

Pokud ho znáte language agent vydání dotazu můžete s konkrétním poli pomocí dotazu parametr **searchFields** oborem požadavku hledání. Následující dotaz bude pouze vystavený popis v polština:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Dotaz můžete indexu z portálu pomocí **Průzkumníka hledání** můžete vložit do dotazu podobná té nahoře. Hledání explorer je k dispozici v řádku nabídek v zásuvné služby. Další informace najdete v článku [dotazu Azure vyhledávacím odkazu na portálu](search-explorer.md) .

Někdy language agent vydání dotazu neznámý, v takovém případě dotazu může být vystavený všechna pole současně. V případě potřeby můžete předvoleb pro zajištění výsledků určitého jazyka definované pomocí [bodování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx). V následujícím příkladu bude mít shody najdete v popisu anglicky skóre vyšší relativní odpovídající polština a Francouzština:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Pokud jste vývojáři .NET, nezapomeňte, že můžete nakonfigurovat pomocí [Azure hledání .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)analyzátory jazyk. Nejnovější verze podporuje i analyzátory jazyka Microsoft.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
