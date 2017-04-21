# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Kroky při vyřadit nebo změna názvu technický článek ACOM

Tento návod je určený pro podniky, kteří jsou označeny jako Autor příspěvku, který je potřeba vyřadit z části technické si přečtěte následující dokumentaci azure.microsoft.com. Postup také platí přejmenování souboru.

Pokud jste členem naší Azure komunity a nevíte, jestli že je článek má vyřadit z nějakého důvodu, prosím komentář v toku komentář Disqus článek chcete, aby autor, že něco není správné snažit se článek.

Autoři systémy potřeba pomocí několika kroků řádně vyřadit obsah tak, aby uživatelé webu neměli setkat i v případě chybná při jsme vyřadit obsah z webu. Odstranění v článku nebo změna jeho název musí být poslední věcí, které se stane!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Krok 1: Nastavte v článku na Ne index/bez sledovat a znovu publikovat (doporučeno)

První věc, které byste měli udělat je znovu publikovat v článku takto bez index a ne několik týdny před skutečně odstranit. Je to nejlepší praktické cvičení "před práce" pro do důchodu obsahu. Tím odebere v článku hledání engine indexy tak, aby lidé nenajde v článku hledání. [Zobrazit vnitřní wikiwebu podrobnosti.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Krok 2: Správa příchozí odkazy (povinné)

Zjistěte, zda jsou všechny příchozí odkazy společnosti Microsoft k obsahu. Často blogy, fóra a další obsah na webu odkazy na články. Často můžete pracovat s blogu vlastníci změnit tyto odkazy a můžete odebrat nebo aktualizujte propojení na fórum příspěvky. Nástroje pro web analytics můžou obsahovat, pokud existují nějaké vysoký provoz příchozí odkazy, budete potřebovat pro správu tímto způsobem.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Krok 3: Odebrání všech crosslinks v článku znalostní báze technické obsahu úložiště (povinné)

Nejsou založeny na přesměrování starat o crosslinks z další články. Aktualizovat nebo odebrat křížové odkazy na článek odstraňování a přejmenovávání, včetně v článcích vlastní někým jiným.

1. Zajistit, že pracujete v aktuální místní větev – spuštění `git pull upstream master` (nebo odpovídající variant k tomuto příkazu.

2.  Skenování složce azure – obsah cena/články a složce azure obsah cena/zahrnuje některá články a obsahuje tento odkaz na článek, který chcete stáhnout a buď odebrat crosslinks nebo nahraďte je vhodné nové crosslinks. Můžete použít hledání a nahrazení nástroje hledání crosslinks, pokud nemáte nainstalovaný. Pokud nemáte, můžete použít prostředí Windows PowerShell zdarma! Tady je postup, jak pomocí Powershellu najdete crosslinks:

 na. Spusťte Windows PowerShell.

 b. Na příkazovém řádku prostředí PowerShell změňte azure obsah pr\articles složky:

 `cd azure-content-pr\articles`

 c. Zadejte tento příkaz, který bude seznam všechny soubory, které obsahují odkaz na článek, který chcete odstranit:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Pokud chcete odeslat seznam názvů souborů do textového souboru (v tomto případu, pojmenované psoutput.txt), můžete:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Přidání uložte provedené změny, použít pro svůj vidlice a vytvoření žádosti o vyžádané přesunout změny ze svého vidlice předlohy pobočky hlavní úložiště.

## <a name="step-4-update-the-fwlink-tool-required"></a>Krok 4: Aktualizace nástroj FWLink (povinné)

Nástroj odkaz vyhledejte všechny odkazy, které může přejděte na článek. Přesuňte ukazatel myši všechny odkazy na nahrazení obsahu; Pokud nejste na alias, který vlastní odkaz, k ní připojte. Pokud vlastníci neaktualizují na odkaz, soubor lístek s MSCOM obsahuje odkaz změnit. Další informace – [interní wikiwebu](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Krok 5: Odebrání všech crosslinks na článek z jiných stránek na azure.microsoft.com a vytvořte přesměrování vyřazené stránky, pokud odpovídající (povinné)

Budete muset pracovat s osoba, která spravuje a aktualizovat cílové stránky si přečtěte následující dokumentaci pro službu pro tuto část. Pokud nevíte, který je tato osoba, kontaktujte svého partnera obsahu týmu. Osoba, která spravuje a aktualizovat cílové stránky dokumentu nutné provést dva kroky:

1. Ve Visual Studiu prohledejte **celý** ACOM webového řešení pro křížové odkazy na souboru, který má vyřadit. Křížové odkazy odeberte nebo nahraďte aktualizované křížový odkaz. Musíte odebrat odkazy HTML, jakož i řetězce související prostředků pro odkazy HTML. Další informace – najdete v článku [interní wikiwebu](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Pokud článek náhradu neexistuje, vytvořte přesměrování. Další informace – najdete v článku [interní wikiwebu](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Zkontrolujte změny do úložiště.

## <a name="step-6-retire-the-article"></a>Krok 6: Vyřadit v článku

Po dokončení předchozího postupu a tyto změny se živou, můžete odstranit následujícím článku z úložiště. 

**Důležité:** Při odstraňování souborů, je nutné použít `git add --all` příkaz.

## <a name="step-7-remove-links-from-msdn-required"></a>Krok 7: Odebrání odkazů na webu MSDN (povinné)

Zkontrolujte nástroj q & a obsahu pro přerušení propojení s tématem vyřazené nebo přejmenování souboru a odebrat a oprava odkazů ve všech témat MSDN vliv.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Krok 8: Odebrání mezipaměti stránky z vyhledávacích webů (pouze v případě vytvářejte)

Tento jen dělat, když obsah je třeba rychle odebrat z důvodu problémů zákaznické právní nebo špatných. Na osvědčené postupy z Google odstraněné stránky normální prioritou má jenom zacházet natural vyhledávacího stroje procesy. Přejděte na tyto webové stránky z vyhledávacích odebrat webové stránky v mezipaměti:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Přispěvatelům Průvodce odkazy

- [Článek Základní informace](./../README.md)
- [Index pokyny články](./contributor-guide-index.md)
