<properties pageTitle="Libovolná příkazy pro vytváření nový článek nebo aktualizujete stávající článek" description="Postup pro práci s Azure technické obsahu GitHub úložištích." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Libovolná příkazy pro vytváření nový článek nebo aktualizujete stávající článek


## <a name="standard-process-working-from-master"></a>Standardní proces (pracuju z předloha)
Postupujte podle kroků v tomto článku se vytvářet místní pobočky pracovní ve vašem počítači, můžete vytvářet nový článek oddílu technické si přečtěte následující dokumentaci azure.microsoft.com nebo aktualizovat existující článek.

1. Spusťte flám libovolná (nebo pole se příkazového řádku, který používáte pro libovolná).

 **Poznámka:** Pokud pracujete v úložišti veřejné, změňte azure obsah cena azure obsahu v seznamu příkazy.

2. Změňte azure obsah cena:

        cd azure-content-pr
3. Podívejte se na větvi předlohy:

        git checkout master

4. Vytvoření nové místní pracovní větve odvozeno z větvi předlohy:

        git pull upstream master:<working branch>


5. Přesunutí do nové pracovní větve:

        git checkout <working branch>

6. Informujte svůj vidlice vědět, že jste vytvořili větvi místní pracovní:

        git push origin <working branch>

7. Vytvořte nový článek nebo změnit existující článek. Pomocí Průzkumníka Windows a vytvořit nové soubory markdown, a použít Atom (http://atom.io) jako markdown editor. Po vytvořili nebo změnili článek a obrázky, přejděte k dalšímu kroku.

8. Přidání a potvrďte změny, které jste udělali:

        git add .
        git commit –m "<comment>"
        
   Nebo přidat pouze konkrétní soubory se změny:

        git add <file path>
        git commit –m "<comment>"

   Pokud jste odstranili soubory, musíte pomocí tohoto příkazu:
   
        git add --all
        git commit -m "<comment>"

9. Místní pobočky pracovní aktualizujte nejnovějšími změnami udělanými odesílání dat:

        git pull upstream master

10. Použít změny pro vaše vidlice na GitHub:

        git push origin <working branch>

12. Až budete připravení na odeslání obsah tak, aby větvi nadřazeného předlohy pro přípravu, ověření a/nebo publikování, v uživatelském rozhraní GitHub vytvoření žádost vložit z vaší vidlice větvi předlohy.

13. Pokud jste zaměstnanec práce v úložišti soukromé změny, které odesíláte jsou automaticky připravené a pracovní odkaz se žádost vložit. Kontrola fázované obsahu a odhlášení v žádosti o komentářích vyžádané přidáním komentář **#sign nepracovní** .  Tento údaj označuje, že změny připraveni být posunuto live.  Pokud nechcete, aby žádost vyžádané se dají přijímat – Pokud jsou jenom pracovní změny – přidáte poznámku **#hold nepracovní** žádost vložit.

14. Žádost o příjemce vložit kontroluje žádosti o vložit, poskytuje zpětnou vazbu, a/nebo přijímá žádosti o vložit. 

15. Pokud chcete ověřit, zda publikovaný článek nebo změny

 http://Azure.microsoft.com/documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Publikování

- Články jsou publikované na přibližně desáté hodiny dopoledne a 3:00 odp Tichomoří, pondělí pátek. Může trvat až 30 minut články online se zobrazí po publikování. Uvědomte si, že žádosti o vyžádané je nutno sloučit recenzenta žádost o vyžádané před změny může být součástí příštího naplánovaného publikování spustit. Potřebujete pracovat s svého vyžádané žádost o recenzenta předem zajistit že žádost o vyžádané sloučený pro konkrétní publikování spustit. V opačném PRs: budou posouzeny v pořadí, v jakém byly odeslány.

- Pokud jste zaměstnance práce v úložišti soukromé, všechny žádosti o vyžádané se vztahují ověřovacích pravidel, která nutné zvážit před nutno sloučit žádost vložit. 

## <a name="working-with-release-branches"></a>Práce s pobočky vydání

Při práci s větví vydání, je nejlepší způsob, jak vytvořit místní pobočky pracovní z větve vydání pomocí následující syntaxe příkazu:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Tím vytvoříte místní pobočky přímo z nadřazeného větve, že místní sloučení.

