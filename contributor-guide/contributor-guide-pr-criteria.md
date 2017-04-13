# <a name="quality-criteria-for-pull-request-review"></a>Kritéria kvality vyžádané požadavku na zkontrolovat

Tato kritéria jsou určeny pro autorů, kteří vytvářejí a spravují technické články a vložit žádost o recenzentům, kteří kontrola žádostí o obsahu vložit. Pokud vaši žádost vyžádané neposkytuje [automatického sloučení](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), je kontrolovaná recenzenta žádost o lidské vložit. Vložit žádost o recenzentů obecně prohlédněte si jenom novinky nebo změněné. Vložit žádost o recenzentů vyhodnocení změny v žádosti o vyžádané podle blokování a blokování kvality revize položky uvedené v tomto článku.

## <a name="blocking-content-quality-items"></a>Blokování kvalitu obsahu položky

Aktualizace v žádosti o vyžádané musí splňovat následující kritéria ke sloučení. Recenzenti vyžádané žádost o zadání názoru v vyžádané žádost o komentáře pro tyto položky a typ `#hold-off` v žádosti o vyžádané vrátí ho (autor) zpětnou vazbu.

| Kategorie | Zlepšení kvality kontrola položky |
|----------|---------------------|
|Zjistit předpoklady pro| Popisky "ověření bylo úspěšné" a "Připravené – k-hromadné korespondence" přiřazené PR.|
|Zjistit předpoklady pro| Žádost o vložit nemůžete blokovány konflikt hromadné korespondence.|
|Integrita repo|    Vložit požadavek obsahuje bez zjevných obsahu regrese.|
|Integrita repo|    Žádost o vyžádané nezahrnuje vložený repo nebo neobvyklé, nadbytečné soubory.|
|Integrita repo |Vložit žádost obsahuje míň než 100 změněné soubory, pokud cena úmyslně aktualizuje větví vydání ze předlohu. (Ve skutečnosti cena by měl obsahovat zcela nižší než, ale po 100 změněné soubory GitHub nezobrazí diffs).|
|Pojmenování |[Pokyny pro pojmenování souborů](file-names-and-locations.md)postupujte podle názvů souborů pro nové soubory.|
|Pojmenování |Nové složky k tomu sledovat repo [pokyny pro pojmenování složky](file-names-and-locations.md#folder-names-in-the-repo).|
|Obsah    |Tento článek je dokumentem technické a proto správné obsahu kanálu. Zobrazit [co přejde where pokyny](content-channel-guidance.md).|
|Obsah    |Je předmětem technický dokument určený pro technické článek. Zobrazit [co přejde where pokyny](content-channel-guidance.md).|
|Obsah    |Tento článek obsahuje úvodní odstavec a procedurální nebo koncepční textu obsahu. V článku musí obsahovat dostatečné, dokončeno obsahu pro lepší viditelnost vlastní, jako je článek. Nesmí být menší část informace. (Výjimku: "Limity" tematickým v kontextu velké článek, který obsahuje seznam všech limity službu.)|
|Obsah| Mají být číslovány prvky, které by měl být číslované seznamy, prvky, které by měl být neuspořádané seznamy s odrážkami. Toto je základní použitelnosti.|
|Obsah| Musí vetted s potenciálního zákazníka recenzentovi cena neobvyklé nebo nové grafiky, informační architektura nebo struktury nebo očividně nestandardní návrhů. Družstev, která jsou experimentování s novinek třeba plátno problém/řešení nebo plán na místě pro vyhodnocení pokusy.|
|Funkce návrh nebo webu| Switchers slouží jenom pro přepínání mezi různými verzemi stejného článku.|
|Funkce návrh nebo webu| Názvy switchered článků obsahují informace, které odlišuje každý článek z jiných článků v sadě switchered.|
|Funkce návrh nebo webu| Ruční vytvořil nejsou povoleny obsahy. V článku musí spolehnout H2s pro jeho obsahu na stránce.|
|Funkce návrh nebo webu| Pokud existují H2 nadpisy, článek obsahuje aspoň dvě H2 záhlaví. Použití jedné položky H2 vytváří jednou položkou článek obsahu. H2 nadpisy musí používat před H3 nadpisy zajistit, aby že se vytvoří obsah.|
|Markdown| HTML: Zdroje obsahu neobsahuje HTML na úrovni blok – menší textu, HTML je povolený – třeba horní, dolní index, speciální znaky a další menší věci, které se nedá dosáhnout pomocí markdown. Tabulky ve formátu HTML jsou povoleny jenom v případě, že tabulka obsahuje seznam s odrážkami nebo číslované seznamy, ale je to obvykle údaj, který obsahu musí být zjednodušené tak zdroje můžete kódovaný v markdown.|
|Markdown   |Vlastní markdown prvky se používají v případě potřeby. Ex: Jsou kódovaný poznámek pomocí AZURE. Všimněte si přípony, ne jako prostý text.|
|SEO    |"& #124; Požaduje identifikátor webu Microsoft Azure"|
|SEO    |Název H1 obsahuje dostatečné informace popisující obsah v článku jak ho mají odlišit od jiných Azure články a přiřadit klíčová slova pravděpodobně zákazníka. Jako název H1 sestava "Přehled" je například selhalo.|
|Terminologie| Použití zkratka ARM, V1 nebo V2 jako odkazy na klasickou a správce prostředků nasazení modely je blokování položka terminologie.|
|Obrázky |Animované obrázky GIF nejsou povolené v repo.|
|Obrázky | Obrázky mít vymazat rozlišení, neobsahují chybně napsaná slova a obsahovat žádné osobních údajů | 
|Pracovní|Náhled článek musí být čisté na pracovní. Nesmí obsahovat žádné zjevných chyby ve formátování: <br><br>-S odrážkami nebo číslovaný seznam, který se zobrazí jako odstavce <br> -Doručení s kódem v bloku kódu zobrazené částečně v bloku kódu a částečně mimo něj <br>-Číslované kroky správně očíslovaný kvůli vadná odsazení|

## <a name="non-blocking-content-quality-items"></a>Blokování bez obsahu položky kvality

U těchto položek vyžádané žádost o recenzentů poskytnout zpětnou vazbu a pokyny pro Autor ke zpracování s problémů, které požadavek na pozdější vložit. Tuto zpětnou vazbu neblokuje rozhodnutí o sloučit. Autoři mají zpracovat do 3 pracovních dnů pomocí opravy.

| Kategorie | Zlepšení kvality kontrola položky |
|----------|---------------------|
|Obsah|Články, "Další kroky" má na konci 1 až 3 relevantní a atraktivní další kroky. Informace musí obsahovat krátký text, který vám pomůže zákazníka pochopit, proč jsou důležité dalším krokům. (Nové články pouze). Příklad: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Obsah|Kontrolu pravopisu, gramatiky i jiných problémů psaní - vyžádané žádost o recenzentů může svůj názor na několik menší problémy jako není blokování názory. Pokud existuje více než několika redakční problémy, přihlaste se recenzentů upravit žádost o v článku úpravy po publikace.|
|Obrázky|Obrázky pomocí správné popisek styl a barvu a snímky obrazovek použít správný styl ohraničení a zástupný symbol. [V tématu pokyny obrázek](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Obrázky|Obrázky obsahovat alternativní text. [V tématu pokyny obrázek](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Funkce návrh nebo webu|H2 nadpisy, když vykreslování obsahu na stránce v ideálním případě zalamován víc než 2 řádky. Delší nadpisy proveďte v článku obsahu těžší ke kontrole.|
|Konvence stylu|Všechny nadpisy a záhlaví jsou věty za Azure styl.|
|Proces|Pokud žádost o vyžádané může mít snadno byla změnili konfiguraci využívat PRmerger automatizaci, recenzentů vyžádané žádost o poskytnutí zpětné vazby autorovi o tom, jak použít větví, aby se změny může nutno sloučit automaticky. Najdete [v článku etiketa cena](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically).|
|Proces|Když odstranit nebo přejmenovat článek, zkontrolujte, že budete pokračovat další. Vyžádat vyžádání recenzentů bychom přidat následující komentář a odkaz ve formuláři:<br><br>*Zkontrolujte, zda jste postupovali podle obrázku v příručce přispěvatelů pro odstranění články: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Související

- [Vložit žádost o etiketu a osvědčené postupy pro Microsoft přispěvatelů](contributor-guide-pull-request-etiquette.md)

- [Vyžádat žádost o automatické komentář](contributor-guide-pull-request-comments.md)
