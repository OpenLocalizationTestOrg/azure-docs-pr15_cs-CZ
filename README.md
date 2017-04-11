# <a name="azure-technical-documentation-contributor-guide"></a>Příručka Azure technické si přečtěte následující dokumentaci přispěvatelů

Nalezení úložiště GitHub, která bude obsahovat zdrojem technické si přečtěte následující dokumentaci publikovaný do centra Azure si přečtěte následující dokumentaci na [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation).

Toto úložiště také obsahuje pokyny k přispívání do naší technické dokumentace.  Seznam článků v příručce přispěvatelům najdete v článku [index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Přispívání do si přečtěte následující dokumentaci Azure

Děkujeme za váš zájem si přečtěte následující dokumentaci Azure.

* [Způsoby, jak chcete přispět svými](#ways-to-contribute)
* [Chování](#code-of-conduct)
* [Informace o své příspěvky Azure obsahu](#about-your-contributions-to-azure-content)
* [Úložiště organizace](#repository-organization)
* [Použití GitHub, libovolná a toto úložiště](#use-github-git-and-this-repository)
* [Jak používat markdown formátovat téma](#how-to-use-markdown-to-format-your-topic)
* [Zpětnou vazbu, poznámky a podporu](./contributor-guide/feedback-and-comments.md)
* [Další materiály](#more-resources)
* [Index článků průvodce všechny přispěvatelů](./contributor-guide/contributor-guide-index.md) (otevře novou stránku)

## <a name="ways-to-contribute"></a>Způsoby, jak chcete přispět svými 

Můžete do ní můžete přidávat [Azure dokumentaci](http://azure.microsoft.com/documentation/) k několika různými způsoby:

* Přispívat na [diskusní fóra](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Odeslání komentářů Disqus na konci článku.
* Snadno může do odborných článků v uživatelském rozhraní GitHub přispívat. Najít článek v této úložiště nebo naleznete v článku na [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) a klikněte na odkaz v článku znalostní báze, přejde k tomuto zdroji GitHub článku.
* Pokud vytváříte zásadní změny k existujícímu článku, přidání nebo změna obrázků nebo přispívání nový článek potřebujete rozvětvené tohoto úložiště, nainstalujte libovolná flám, Markdown číselník a zjistěte, některé příkazy libovolná.

##<a name="code-of-conduct"></a>Chování

Tento projekt přijala [Microsoft otevřít zdroj chování](https://opensource.microsoft.com/codeofconduct/). Další informace najdete v článku [Kód vedení FAQ](https://opensource.microsoft.com/codeofconduct/faq/) nebo kontakt [opencode@microsoft.com](mailto:opencode@microsoft.com) s jakékoli další dotazy nebo komentáře.

##<a name="about-your-contributions-to-azure-content"></a>Informace o své příspěvky Azure obsahu

###<a name="minor-corrections"></a>Menší oprav

Menší oprav nebo vysvětlivky, které odesíláte si přečtěte následující dokumentaci a kód příklady v tomto repo jsou překryty [Azure webu podmínky použití (rovině)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Větší odeslání

Pokud odešlete žádost vyžádané změnami nové nebo významné si přečtěte následující dokumentaci a příklady kódu jsme uživateli odešleme komentář v GitHub s žádostí o odeslání online příspěvek licenční smlouvy (CLA), pokud jste v jednom z těchto skupin:

* Členové skupiny otevřít technologie společnosti Microsoft.
* Přispěvatelům, kteří nepracují společnosti Microsoft.

Chcete-li dokončit formuláři online před jsme přijetí žádosti o vyžádané potřebujeme.

Úplné informace jsou k dispozici [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Úložiště organizace

Obsah v úložišti azure obsah spočívá v organizaci si přečtěte následující dokumentaci na [Azure.Microsoft.com](http://azure.microsoft.com). Toto úložiště obsahuje dva kořenové složky:

### <a name="articles"></a>\articles

Složka *\articles* obsahuje článků si přečtěte následující dokumentaci markdown soubory s příponou *.md* ve formátu.

Publikování článků v kořenovém adresáři na Azure.Microsoft.com na cestě *http://azure.microsoft.com/documentation/articles/ {článek název bez md} /*.

* **Článek názvy souborů:** Viz [náš pokyny pro pojmenování souborů](./contributor-guide/file-names-and-locations.md).

Články do vlastní složky služby jsou publikované na Azure.Microsoft.com na cestě *http://azure.microsoft.com/documentation/articles/service-folder/ {článek název bez md} /*

* **Médií podsložky:** *\Articles* složka obsahuje složku *\media* kořenový adresář článek mediální soubory, do kterého jsou podsložky s obrázky pro každý článek.  Složky služby obsahují složku samostatné média článků v rámci každé ze složek služby. Obrázek složky článku jsou pojmenované stejně jako souboru článek bez přípony souboru *.md* .

### <a name="includes"></a>\includes

Můžete vytvořit opakovaně použitelné části obsahu mají být součástí jedné nebo více články. Zobrazit [vlastní přípony používán naše technický obsah](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown šablony

Tato složka obsahuje šablony naše standardní markdown základní markdown formátování, které potřebujete pro článek.

### <a name="contributor-guide"></a>\contributor-Guide

Tato složka obsahuje článků, které jsou součástí Příručky pro naše přispěvatelům.  

## <a name="use-github-git-and-this-repository"></a>Použití GitHub, libovolná a toto úložiště

Informace o přispívat, jak používat uživatelské rozhraní GitHub přispívat malé změny a jak stůl a klonovat úložiště pro větší příspěvky najdete v tématu [instalace a nastavení nástroje pro vytváření GitHub](./contributor-guide/tools-and-setup.md).

Pokud si nainstalujete GitBash a zvolte pracovat v místním počítači, kroků pro vytvoření nové místní pobočky pracovní, změny a odeslání změny zpět hlavní větev jsou uvedené v [Libovolná příkazy pro vytváření nový článek nebo aktualizujete stávající článek](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Poboček

Doporučujeme vytvoření místních poboček pracovní zaměřených obor konkrétní změny. Jednotlivých větví by měl být omezená jeden koncept/článku jak zjednodušit průběhu prací a omezit možnost sloučení konflikty.  Následující úsilí jsou vhodné obor pro novou větev:

* Nový článek (a související obrázky)
* Pravopis a gramatika úpravy na článek.
* Použití jedné změny formátování velkých sady výsledků články (například nové autorských práv zápatí).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Jak používat markdown formátovat téma

Všechny články v tomto úložišti pomocí GitHub flavored markdown.  Tady je seznam zdrojů.

- [Základní informace o markdown](https://help.github.com/articles/markdown-basics/)

- [Tisknutelné markdown cheatsheet](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Náš seznam markdown editory podívejte se na [Nástroje a téma Instalace](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Článek metadat

Článek metadat umožňuje některé funkce na webové stránce azure.microsoft.com, například autor autorství přispěvatelů autorství, navigace s popisem, článek popisy a SEO optimalizace, jakož i sestav Microsoft používá k vyhodnocení výkonu obsah. Ano je důležité metadata! [Tady je návod, jak zajistit metadata probíhá vpravo](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Popisky

Automatické popisky přiřazené na žádosti o softwaru a služeb spravovat žádosti o pracovním postupu vyžádané a pomůže vám dá vědět, co se děje s žádostí o vyžádané:

* Licenční smlouva příspěvek související
    * cla není povinné: změna je poměrně menší a nevyžaduje přihlaste CLA.
    * povinné cla: relativně velká a vyžaduje se přihlásit CLA rozsah změny.
    * přihlášení cla: Přispěvatel podepsána CLA, tak žádost Vložit můžete nyní Přechod vpřed ke kontrole.
* Pilíře popisky: popisky jako PnP, moderní aplikace a ORS můžete uspořádat žádosti o vyžádané interní organizací, která je potřeba zkontrolovat žádost vložit.
* Změna odeslaná k vytváření: Autor byla oznámení o požadavek na zjištění stavu úkolů vložit.

## <a name="more-resources"></a>Další materiály

V tématu [index příručky naše přispěvatelů](./contributor-guide/contributor-guide-index.md) pro všechny naše témata pokyny.
