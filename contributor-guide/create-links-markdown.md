<properties
   pageTitle="Vytváření odkazů v článcích markdown" description="Vysvětluje, jak kódu crosslinks v markdown." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Propojení pokyny pro Azure technické obsah

### <a name="links-from-one-acom-article-to-another"></a>Propojení z jednoho článku ACOM do druhého

Pokud chcete vytvořit odkaz na vložený technický článek ACOM na jiný ACOM technické článek, použijte následující odkaz syntaxi:  

- Článek ve odkazů adresáře služby na jiný článek ve stejném adresáři služby:

  `[link text](article-name.md)`

- Článek obsahuje odkazy ze služby podadresáře na články, v kořenovém adresáři:

  `[link text](../article-name.md)`

- Článek v kořenovém adresáři odkazy na články podadresáře služby: 

  `[link text](./service-directory/article-name.md)`

- Článek služby podadresáře odkazy na články do jiného podadresáře služby:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Odkazy na ukotvení

Není potřeba vytvářet ukotvení - se automaticky vygenerují v době pro všechna záhlaví H2 publikování. Jediné, které je potřeba udělat, je vytvořit odkazy na oddíly H2.

- Pokud chcete vytvořit odkaz na nadpis v rámci stejného článku:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Pokud chcete vytvořit odkaz na ukotvení v jiném článku ve stejném podadresáře:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Pokud chcete vytvořit odkaz na ukotvení do jiného podadresáře služby:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Jedním ze způsobů automatizovat vytváření odkazů v článcích automaticky generované ukotvení odkazy je [MarkdownAnchorLinkGenerator – nástroj pro generování ukotvení odkazy pro ACOM ve správném formátu](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Obsahuje odkazy ze

Protože zahrnout soubory jsou umístěny v jiném adresáři, budete muset používat již relativní cesty, jak je ukázáno v následujícím příkladu. Pokud chcete vytvořit odkaz na článek ze souboru vložení, použijte tento formát:

    [link text](../articles/service-folder/article-name.md)
    
Další informace o tom, jak používat soubor zahrnout v [vlastní markdown rozšíření pokyny](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Odkazy v voliče

Pokud máte voleb, které jsou vložené do zahrnutí, použijete tento typ propojení: 

    > [AZURE. Výběr seznamu (Dropdown1 | Dropdown2)]     -  [(Text1 | Priklad1)](../articles/service-folder/article-name1.md)
    - [(Text1 | Priklad2)](../articles/service-folder/article-name2.md)
    - [(Text2 | Example3)](../articles/service-folder/article-name3.md)
    - [(Text2 | Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Styl odkazu odkazy

Odkazy na referenční stylů umožňuje přehlednější zdroje obsahu. Referenční odkazy styl nahraďte syntaxe odkaz vložené zjednodušené syntaxi, která umožňuje přesunutí dlouhé adresy URL na konci článku. Tady je příklad Daring Fireball:

Text vloženého:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Odkaz na konci tohoto článku:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Zkontrolujte, že obsahují mezery za dvojtečka před propojením. Po propojení na jiné technické články, pokud je zapomenete obsahovat mezery, bude v článku znalostní báze publikované porušený odkaz. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Odkaz na ACOM stránek, které nejsou součástí sadu technické dokumentů

Pokud chcete vytvořit odkaz na stránku ACOM (například stránku ceny, SLA stránky nebo čehokoli jiného, který není článku si přečtěte následující dokumentaci), používat absolutní adresa URL, ale vynechat národní prostředí. Cílem tady je, které fungují odkazy v GitHub a vykreslená portálu:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Odkaz na webu MSDN nebo TechNet

Když potřebujete propojení MSDN nebo TechNet, použijte celou odkaz na téma a odebrání en-us národní prostředí odkazu. 

### <a name="use-friendly-link-text-for-all-links"></a>Použití popisný odkaz textu hypertextových odkazů pro všechny odkazy

Slova, která zahrnuté odkazu by měl být popisný – tedy by měl být normální anglických slov nebo názvu stránky, kterou můžete vytvořit odkaz. Nepoužívejte "Kliknutím sem". Je neměli SEO a nemá dostatečně popsat cíl.

**Oprava:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Nesprávný:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>Odkazy

Nepoužívejte odkazy (naše systém přesměrování) v obsahu azure.microsoft.com. Budou bude použito pouze jako poslední možnost když potřebujete k vytvoření odkazu na stránku ještě nevíte, jehož adresu URL. Jsou potřeba téměř nikdy skutečně. Pro ACOM definovat název souboru, abyste věděli, co bude předem. Knihovna tématu, který není zatím publikovat můžete vytvořit odkaz, který používá téma GUID, takže nemusíte použít odkaz.

Pokud je nutné použít odkaz na webovou stránku, patří parametr P snažíme usnadnit jeho trvalé přesměrování:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Po vložení cílovou adresu URL do nástroje FWLink nezapomeňte Pokud cíl odkazu je knihovna ACOM, nebo na webu MSDN nebo TechNet odebrat národní prostředí.

## <a name="remember-the-azure-library-chrome"></a>Nezapomeňte chrome Azure knihovny.
Pokud chcete odkaz Azure knihovny téma, které jsou umístěná [tohoto](https://msdn.microsoft.com/library/azure)uzlu, nezapomeňte zadat Azure chrome v poli odkaz (/ azure /). Azure chrome sdílí možnosti navigace ACOM a zobrazí jenom Azure obsah knihovna MSDN. Správně obory odkaz vypadat takto:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

V opačném stránce budou k dispozici v zobrazení standardních MSDN se zobrazí celý strom MSDN.

### <a name="contributors-guide-links"></a>Přispěvatelům Průvodce odkazy

- [Článek Základní informace](./../README.md)
- [Index pokyny články](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
