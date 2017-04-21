# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Vyžádat etiketu žádost a osvědčené postupy pro Microsoft přispěvatelům si přečtěte následující dokumentaci Azure

Publikování změn si přečtěte následující dokumentaci, odesílat žádosti o vložit z vašeho větev. Každý požadavek vložit musí být zkontrolována před slučovaných. V tomto článku se vysvětlit, jak by měl fungují s vyžádané žádost o recenzentů a jak můžete vytvořit vyžádané požadavky, které jsou snadněji a rychleji zobrazíte tak, aby fronty požadavků vyžádané funguje lépe pro všechny uživatele.

## <a name="working-with-pull-request-reviewers"></a>Práce s vyžádané žádost o recenzentů

Tady jsou základní informace, které byste měli vědět o práci s vyžádané žádost o recenzentů. 

- <b>Principy role revidující žádost o vložit. Revidující:</b>
  - Zajišťuje základní kvalitu obsahu
  - Zabrání Regrese v úložišti
  - Poskytuje zpětnou vazbu před sloučení

  Vložit žádost o recenzentů jsou v roli obsahu zásady správného řízení. Hlavní záměr je sloučit jednoduše něco jiného, co se odešle co nejdříve. Očekávejte názory, budete požádáni o aktualizovat, zejména u nové a velkém upravené články.

- <b>Připravte se pomocí svého recenzenta žádost o vyžádané:</b>
  - Pro vysokou prioritou vyžádané požadavky
  - Vložit požadavky vypršel časový limit a datovaná verzích
  - Vložit požadavky, které změňte nebo přidejte velké množství souborů

- <b>SLA vyžádané požádat o revize</b>

  V úložišti soukromé pokaždé, když žádosti o vyžádané zadá fronty požadavků Vložit s popiskem připraveno ke sloučení týmu pokusí prohlédnout žádost vložit do 12 hodin business (M + F, 8: 00 odp. k 5) a poskytnutí zpětné vazby nebo sloučení Pokud požaduje zpětná vazba. Tento SLA platí příznaky revizí cena není slučování. PRs se sloučení při splnění [kritéria pro sloučení](contributor-guide-pr-criteria.md). 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Zkontrolujte fronty požadavků vyžádané nefungoval lépe pro všechny uživatele

Existují dva základní tuto skutečnost ve frontě cena:

- Vložit požadavky, které jsou menší v rozsahu a, které obsahují velmi podobné změny trvat méně času ke kontrole. 
- Vložit požadavky, které jsou velké v rozsahu nebo, které obsahují různé, smíšených typy změn trvat delší dobu, kontroloval.

Můžete se dozvíte, jak vložit fronty požadavků nefungoval lépe tak, že řídit následujícími doporučenými postupy:

- Samostatné dílčí aktualizace stávající články z nových článků nebo hlavní přepíše. Pracujte na tyto změny v samostatné pobočkách pracovní. 

- Když odstraníte články nebo obrázky, Nemíchejte odstranění s novými obsahu doplňky nebo aktualizace. Úchyt pro změny/nový obsah v samostatném větvi pracovní

- Uvolnění nebo refaktoring obsahu: plánování rovnou s revidující cena. Možná budete muset přihlásil nápovědy vytvořit větev vydání nebo koordinaci sloučení časy s publikování časy tak, aby se váš obsah publikuje ve správný čas.

- Pokud se pokoušíte koordinaci aktualizace provedené ve repo ACOM (ie, se změní na levém navigačním panelu úvodní stránky, přesměruje nebo výukové mapy) se změnami, které vytváříte v úložišti azure obsah cena, musí se cena revidující koordinaci, které fungují předem. V opačném rizika máte spoustu přerušené odkazy.

## <a name="criteria-for-expedited-pull-requests"></a>Kritéria pro žádosti o urychlené vyžádané

- Obraťte se na azdocprs urychlení PRs pouze v případě, že je vytvářejte. Můžete požádat o spěšná cena zpracování zóny červené, ochrany osobních údajů, právnické a potíže se zabezpečením; pro opravdu nefunkční zákazníka prostředí; a pro vedení oddělení stupňování požadavků. 
- Obsah funkce verzích nevztahuje se na ně urychlené zpracování - obsah uvolnění funkce vyžaduje předchozí plánování nebo musí být zpracována ve frontě standardní priorita.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Rychle? Odeslání PRs, které se dají přijímat automaticky

Pomocí pravidel automatizaci PRMerger z vaší každodenní PRs sloučené automaticky.

PRMerger jsou přijatelné vaše cena automaticky, pokud:
* Ovlivňuje 10 soubory nebo méně.
* Obsahuje 15 potvrzení nebo méně.
* Nižší než 20 % změny textu.
* Voliče/switchers se neaktualizují.
* Žádné soubory se odstraní nebo přidat.
* Žádné obrázky začínáte, změnit nebo odstranit.

Pokud vaši žádost vyžádané nesplňuje tato kritéria, nápisem "PRmerger nelze sloučit" se automaticky přiřadí tak víte, že vyžaduje revize lidské recenzenta cena.

### <a name="need-to-make-a-lot-of-little-changes"></a>Potřebujete zdůraznit hodně málo změn?

Prohlédněte startovací z výše uvedených pravidel automatizaci PRMerger a postupujte takto:
* Odeslání články s light změnami společně cena 10 nebo méně soubory.
* Vytvoření samostatného cena články v obrázky nebo voliče změnu. Při této akci musí lidské revize.
* Vytvoření samostatného cena nové nebo odstraněné články. Při této akci musí lidské revize.

## <a name="related"></a>Související

- [Kritéria kvality vyžádané požadavku na zkontrolovat](contributor-guide-pr-criteria.md)

- [Vyžádat žádost o automatické komentář](contributor-guide-pull-request-comments.md)
