<properties 
    pageTitle="Podnikové integrace se službou EDIFACT | Microsoft Azure" 
    description="Naučte se používat EDIFACT smlouvami k vytváření aplikací pro použití logických operátorů" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Podnikové integrace se službou EDIFACT 

> [AZURE.NOTE] Tato stránka popisuje funkce EDIFACT logiky aplikací. Další informace o X12 klikněte [sem](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Vytvoření dohodu EDIFACT 
Předtím, než si můžete vyměňovat EDIFACT zprávy, budete muset vytvořit dohodu EDIFACT a uložte ho do svého účtu integrace. Podle těchto kroků vás provede jednotlivými proces vytváření dohodu EDIFACT.

### <a name="heres-what-you-need-before-you-get-started"></a>Tady je budete potřebovat než začnete
- [Integrace účet](./app-service-logic-enterprise-integration-accounts.md) podle předplatného Azure  
- Aspoň dva [partnery](./app-service-logic-enterprise-integration-partners.md) už určených ve vašem účtu integrace  

>[AZURE.NOTE]Při vytváření dohodu obsah zprávy se budou přijmout/odeslat do a od partnera se musí shodovat typ smlouvy.    


Poté, co jste [účet integrace vytvořené](./app-service-logic-enterprise-integration-accounts.md) a [přidané partnerů](./app-service-logic-enterprise-integration-partners.md), můžete vytvořit dohodu EDIFACT provedením těchto kroků:  

### <a name="from-the-azure-portal-home-page"></a>Z Azure domovskou stránku portálu

Po přihlášení do [Azure portál](http://portal.azure.com "Azure portálu"):  
1. **Přejděte** v nabídce vyberte na levé straně.  

>[AZURE.TIP]Pokud nevidíte odkaz **Procházet** , budete muset nejdřív rozbalte nabídku. To udělat tak, že vyberete **Zobrazit nabídku** odkaz, který je umístěn v levém horním rohu sbalené nabídky.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. *Integrace* zadejte do vyhledávacího pole Filtr a pak vyberte **Účty integrace** ze seznamu výsledků.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. V zásuvné **Integrace účty** , které se objeví vyberte integrace účtu, na kterém můžete vytvořit smlouvu. Pokud nevidíte veškerá integrace účty seznamy, [vytvořit první](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Vyberte dlaždici **smlouvami** . Pokud se nezobrazuje dlaždice smlouvami nejdřív přidat.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Klikněte na tlačítko **Přidat** v zásuvné smlouvami, která se otevře.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Zadejte **název** pro souhlas potom vyberte **Typ smlouvy** EDIFACT **Hostitele partnera**, **Identity Host (hostitel)**, **Hosta partnera**, **Hosta Identity**v zásuvné smlouvami, která se otevře.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Po nastavení vlastnosti smlouvu, vyberte **Zobrazit nastavení** můžete nakonfigurovat, jak mají být zpracování zprávy přijmete prostřednictvím této smlouvy.  
8. Ovládací prvek nastavení přijímání je rozdělen v následujících částech, včetně identifikátory, potvrzování, schémata, řízení čísel, ověřování, vnitřní nastavení a dávkové zpracování. Nakonfigurujte tyto vlastnosti na základě vaší smlouvy u partnera, bude výměně zpráv s. Tady je přehled tyto možnosti řízení, nastavovat podle způsobu této smlouvy identifikovat a zpracovávání příchozích zpráv:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Klikněte na tlačítko **OK** uložte nastavení.  

### <a name="identifiers"></a>Identifikátory

|Vlastnost|Popis |
|---|---|
|UNB6.1 (příjemců odkaz heslo)|Zadejte hodnotu alfanumerický rozsahu 1 až 14 znaků.|
|UNB6.2 (příjemců odkaz kvalifikátor)|Zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně dva znaky.|

### <a name="acknowledgments"></a>Potvrzení 

|Vlastnost|Popis |
|----|----|
|Přijetí zprávy (CONTRL)|Zaškrtnutím tohoto políčka se vrátíte technické potvrzování (CONTRL) výměny odesílateli. Potvrzení je odesílateli výměny na základě nastavení Odeslat smlouvy.|
|Potvrzení (CONTRL)|Zaškrtněte toto políčko vrátíte funkční (CONTRL) potvrzení odesílateli výměny potvrzení je odesílateli výměny na základě nastavení Odeslat smlouvy.|

### <a name="schemas"></a>Schémata

|Vlastnost|Popis |
|----|----|
|UNH2.1 (TYP)|Vyberte typ sady transakci.|
|UNH2.2 (VERZE)|Zadejte číslo verze zprávy. (Minimum, o jeden znak, maximum, tři znaky).|
|UNH2.3 (VYDANÁ VERZE)|Zadejte nové číslo verze zprávy. (Minimum, o jeden znak, maximum, tři znaky).|
|UNH2.5 (PŘIDRUŽENÝ PŘIŘAZENÉ KÓD)|Zadejte kód přiřazené. (Maximálně šest znaků. Musí být alfanumerický).|
|UNG2.1 (ID APLIKACE ODESÍLATELE)|Zadejte hodnotu alfanumerický s nejméně jeden znak a do velikosti 35 znaků.|
|UNG2.2 (KVALIFIKÁTOR KÓDU ODESÍLATELE APLIKACE)|Zadejte hodnotu alfanumerický s nejvýše čtyři znaky.|
|SCHÉMA|Vyberte dříve odeslaných schéma, které chcete používat z účtu přidružené integrace.|

### <a name="control-numbers"></a>Ovládací prvek čísel

|Vlastnost|Popis |
|----|----|
|Zakázání Interchange ovládací prvek číslo duplicitních položek|Zaškrtnutím tohoto políčka Blokovat duplicitní křižovatky. -Li vybrán, akce dekódovat EDIFACT vrátí číslo ovládací prvek interchange (UNB5) přijaté výměny neodpovídá dříve zpracovaných výměny ovládací prvek číslo. Pokud je zjištěno shodu, pak bude výměnu není zpracovat.
|Vyhledat duplicitní UNB5 každých (dní)|Pokud si Pokud chcete zakázat duplicitních výměny ovládací prvek čísel můžete zadat počet dní, které kontrola provádí odeslání na odpovídající hodnotu pro možnost **vyhledat duplicitní UNB5 každých (dní)** .|
|Zakázání skupiny ovládací prvek číslo duplicit|Zaškrtnutím tohoto políčka Blokovat křižovatky reprezentovaný číslem od ovládacího prvku duplicitní skupiny (UNG5).|
|Zakázání transakce sadu ovládací prvek číslo duplicit|Zaškrtnutím tohoto políčka Blokovat křižovatky reprezentovaný číslem od ovládacího prvku nastavit duplicitní transakce (UNH1).|
|Ovládací prvek číslo EDIFACT potvrzení|K určení transakce sadu referenční čísla se nemusí používat v potvrzení, zadejte hodnotu pro tuto předponu, oblasti referenční čísla a přípony.|

### <a name="validations"></a>Ověření

|Vlastnost|Popis |
|----|----|
|Typ zprávy|Zadejte zprávu. Po dokončení všech řádků ověření jiné se automaticky přidá. Pokud jsou zadána žádná pravidla, řádek označené jako výchozí se používá k ověření.|
|Upravit ověření|Zaškrtněte toto políčko ověření upravit na datové typy podle upravit vlastnosti schéma, omezení délky, prázdné údaje a koncových oddělovače.|
|Rozšířené ověření|Zaškrtněte toto políčko Povolit rozšířené ověřování (XSD) křižovatky přijaté od odesílatele výměny. Jedná se o ověření délku pole, volitelnost a počet opakování kromě XSD datový typ ověření.|
|Povolit úvodní nebo koncové nuly|Vyberte **Povolit** a povolit úvodní nebo koncové nuly; Program je zaměřen **NotAllowed** nepovolí úvodní nebo koncové nuly nebo **střih** chcete oříznout úvodní mezery a na konci.|
|Střih úvodní nebo koncové nuly|Zaškrtněte toto políčko oříznutí úvodních a koncových nuly|
|Koncové zásad oddělovače|Vyberte **Není povoleno,** Pokud nebudete chtít povolit koncových oddělovače a oddělovače ve křižovatky přijaté od odesílatele výměny. Obsahuje-li výměnu koncových oddělovače a oddělovače, je neplatný. Vyberte **Nepovinní** přijmout křižovatky s a nebo bez koncových oddělovače tisíců. Pokud přijaté výměny musí obsahovat koncových oddělovače a oddělovače zaškrtněte **povinná** .|

### <a name="internal-settings"></a>Vnitřní nastavení

|Vlastnost|Popis |
|----|----|
|Vytvoření prázdné značky XML, pokud jsou povoleny koncových oddělovače|Vyberte toto zaškrtávací políčko mít odesílatele výměny obsahovat prázdné značky XML pro koncové oddělovače.|
|Příchozí dávkování zpracování|Možnosti:</br></br>**Rozdělení výměny jako sady transakce - pozastavení transakce sady při chybě**: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML použitím příslušných obálku do sady transakce. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak pozastavené jenom ty transakce sady. Rozdělení výměny jako sady transakce - pozastavit výměny při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML použitím příslušných obálky. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak se pozastaví celý výměny.</br></br>**Zachovat výměny - pozastavení transakce sady při chybě**: zůstane nedotčený, vytváření dokumentu XML pro celý dávkový výměnu výměnu. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak jenom ty transakce sady pozastavené, během zpracování všech dalších sad transakce.</br></br>**Zachovat výměny - pozastavení výměny při chybě**: zůstane nedotčený, vytváření dokumentu XML pro celý dávkový výměnu výměnu. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak je pozastavené celý výměny.|

Vaše smlouva je připravená zpracovávání příchozích zpráv, které odpovídají nastavení, které jste vybrali.

Konfigurace nastavení, které zpracovávají zprávy odešlete pro partnery společnosti:  
10. Vyberte **Nastavení pro odesílání** a nakonfigurovat, jak mají být zpracování zprávám odeslaným pomocí této smlouvy.  

Ovládací prvek odeslat nastavení je rozdělen v následujících částech, včetně identifikátory potvrzení, schémat, obálky, znakové sady a oddělovače, čísla ovládací prvek a ověření. 

Tady je přehled tyto možnosti řízení. Vyberte požadované nastavení podle způsobu zpracování zpráv, které budete posílat partnerům této smlouvy:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Klikněte na tlačítko **OK** uložte nastavení.  

### <a name="identifiers"></a>Identifikátory
|Vlastnost|Popis |
|----|----|
|UNB1.2 (syntaxe verze)|Vyberte hodnotu **1** až **4**.|
|UNB2.3 (odesílatele obrácené směrovací adresy)|Zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně 14 znaků.|
|UNB3.3 (adresa příjemce obráceném směrování)|Zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně 14 znaků.|
|UNB6.1 (příjemců odkaz heslo)|Zadejte hodnotu alfanumerický s alespoň jednu a maximálně 14 znaků.|
|UNB6.2 (příjemců odkaz kvalifikátor)|Zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně dva znaky.|
|UNB7 (ID aplikace odkazu)|Zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně 14 znaků|

### <a name="acknowledgment"></a>Potvrzení
|Vlastnost|Popis |
|----|----|
|Přijetí zprávy (CONTRL)|Toto políčko zaškrtněte, pokud hostovanou partnera očekává přijímat technické potvrzování (CONTRL). Tohle nastavení určuje, že hostovanou partnera, která zprávu odesílá, požádá partnera hosta odpověď na odeslanou zprávu.|
|Potvrzení (CONTRL)|Toto políčko zaškrtněte, pokud hostovanou partnera očekává funkční potvrzování (CONTRL). Tohle nastavení určuje, že hostovanou partnera, která zprávu odesílá, požádá partnera hosta odpověď na odeslanou zprávu.|
|Vygenerování smyčka BS1/BS4 pro přípustném transakce sady|Pokud jste se rozhodli s žádostí o potvrzení funkčnosti, zaškrtnutím tohoto políčka lze vynutit generování BS1/BS4 smyčky funkční CONTRL potvrzení pro přípustném transakce sady.|

### <a name="schemas"></a>Schémata
|Vlastnost|Popis |
|----|----|
|UNH2.1 (TYP)|Vyberte typ sady transakci.|
|UNH2.2 (VERZE)|Zadejte číslo verze zprávy.|
|UNH2.3 (VYDANÁ VERZE)|Zadejte nové číslo verze zprávy.|
|SCHÉMA|Vyberte schéma, používat. Schémata jsou umístěny ve vašem účtu integrace. Přístup k vaší schémata, nejprve propojte účtu integrace logiky aplikace.|

### <a name="envelopes"></a>Obálky
|Vlastnost|Popis |
|----|----|
|UNB8 (zpracování kód Priority)|Zadejte hodnotu podle abecedy, který není více než jeden znak.|
|UNB10 (komunikace smlouva)|Zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně 40 znaků.|
|UNB11 (otestovat indikátor)|Zaškrtnutím tohoto políčka, který označuje, že výměny generovaného testovací data|
|Použití UNA Segment (rad řetězec služby)|Zaškrtnutím tohoto políčka Generovat segment UNA pro výměnu k odeslání.|
|Použití UNG segmentů (záhlaví skupiny funkce)|Zaškrtnutím tohoto políčka k vytvoření segmentů seskupení v záhlaví skupiny funkční na zprávy poslané na partnera Host. K vytvoření segmentů UNG se používají následující hodnoty:</br></br>**UNG1**zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně šest znaků.</br></br>**UNG2.1**zadejte hodnotu alfanumerický s nejméně jeden znak a do velikosti 35 znaků.</br></br>**UNG2.2**zadejte hodnotu alfanumerický s nejvýše čtyři znaky.</br></br>**UNG3.1**zadejte hodnotu alfanumerický s nejméně jeden znak a do velikosti 35 znaků.</br></br>**UNG3.2**zadejte hodnotu alfanumerický s nejvýše čtyři znaky.</br></br>**UNG6**zadejte hodnotu alfanumerický s alespoň jednu a maximálně tři znaky.</br></br>**UNG7.1**zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně tři znaky.</br></br>**UNG7.2**zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně tři znaky.</br></br>**UNG7.3**zadejte hodnotu alfanumerický s minimálně 1 znak a maximálně 6 znaků.</br></br>**UNG8**zadejte hodnotu alfanumerický s nejméně jeden znak a maximálně 14 znaků.|

### <a name="character-sets-and-separators"></a>Znak sady a oddělovači
Kromě sada znaků, můžete zadat jinou sadu oddělovače pro použití u jednotlivých typů zprávy. Není-li znaková sada pro danou zprávu schéma, se používá znakové sady výchozí.

|Vlastnost|Popis |
|----|----|
|UNB1.1 (systém Identifier)|Vyberte EDIFACT znakovou sadu, můžou být použité na odchozí výměny.|
|Schéma|Vyberte schéma z rozevíracího seznamu. Po dokončení každý řádek bude přidán nový řádek. U vybrané schéma vyberte oddělovače umožníte používat sadu:</br></br>**Součásti prvek oddělovač** – Enter znak k oddělení složené datové prvky.</br></br>**Oddělovač data prvek** – Enter znak jednotlivé prvky jednoduchá data v rámci složené datové prvky.</br></br></br></br>**Znak nahrazení** – zaškrtněte toto políčko Pokud datové, který obsahuje data znaky, které používají také jako data, segmentu nebo součást oddělovače. Pak můžete zadat znak náhradní. Při generování odchozí zprávy EDIFACT, jsou všechny výskyty oddělovače v datové části dat nahrazeny zadaný znak.</br></br>**Ukončení segmentu** – Enter znak označte konec Upravit segment.</br></br>**Přípona** – vyberte znak, který se používá identifikátorem segment. Určuje příponu prvek data ukončení segmentu může být prázdný. Pokud ukončení segment prázdné, je třeba určit příponu.|

### <a name="control-numbers"></a>Ovládací prvek čísel
|Vlastnost|Popis |
|----|----|
|UNB5 (Interchange ovládací prvek číslo)|Zadejte předponu rozsahu hodnot pro ovládací prvek číslo výměny a přípony. Tyto hodnoty byla použita pro vytvoření křižovatky odchozí. Předpona a přípona jsou volitelné. je nutné ovládací prvek číslo. Ovládací prvek číslo se zvyšuje pro každé nové zprávy. Předpona a přípona zůstávají stejné.|
|UNG5 (seskupení ovládací prvek číslo)|Zadejte předponu rozsahu hodnot pro ovládací prvek číslo výměny a přípony. Tyto hodnoty byla použita pro vytvoření požadovanou velikost ovládacího prvku skupiny. Předpona a přípona jsou volitelné. je nutné ovládací prvek číslo. Ovládací prvek číslo se zvyšuje pro každé nové zprávy, dokud je maximální hodnota; Předpona a přípona zůstávají stejné.|
|UNH1 (číslo odkaz záhlaví zprávy)|Zadejte předponu rozsahu hodnot pro ovládací prvek číslo výměny a přípony. Tyto hodnoty byla použita pro vytvoření číslo zprávy záhlaví. Předpona a přípona jsou volitelné. číslo je povinný. Číslo je zvýšen pro každé nové zprávy. Předpona a přípona zůstávají stejné.|

### <a name="validations"></a>Ověření
|Vlastnost|Popis |
|----|----|
|Typ zprávy|Tato možnost umožňuje ověření na výměny příjemce. Ověřování ověřuje upravit na transakce sadu údaje, ověřování datové typy, omezení délky a prázdné údaje a školení oddělovače.|
|Upravit ověření|Zaškrtněte toto políčko ověření upravit na datové typy podle upravit vlastnosti schéma, omezení délky, prázdné údaje a koncových oddělovače.|
|Rozšířené ověření|Tato možnost umožňuje rozšířené ověření křižovatky přijaté od odesílatele výměny. Jedná se o ověření délku pole, volitelnost a počet opakování kromě XSD datový typ ověření. Můžete povolit rozšíření ověření bez povolení ověření upravit, nebo naopak.|
|Povolit úvodní nebo koncové nuly|Tato možnost určuje, že křižovatky upravit dostali ze strany není ověření se nezdařilo: Pokud není v souladu s jeho délce požadavek z důvodu nebo koncových mezer údaj křižovatky upravit, ale dodržuje svůj požadavek délky, když jsou odebrány.|
|Střih úvodní nebo koncové nuly|Výběrem této možnosti se střih počáteční a koncové nuly.|
|Koncové oddělovače|Tato možnost určuje křižovatky upravit dostali ze strany není ověření se nezdařilo: Pokud není v souladu s svůj požadavek délka kvůli začátku (nebo na konci) nuly nebo koncové mezery údaj křižovatky upravit, ale dodržuje svůj požadavek délky, když jsou odebrány.</br></br>Vyberte **Není povoleno,** Pokud nebudete chtít povolit koncových oddělovače a oddělovače ve křižovatky přijaté od odesílatele výměny. Obsahuje-li výměnu koncových oddělovače a oddělovače, je neplatný.</br></br>Vyberte **Nepovinní** přijmout křižovatky s a nebo bez koncových oddělovače tisíců.</br></br>Pokud přijaté výměny musí obsahovat koncových oddělovače a oddělovače zaškrtněte **povinná** .|

Po výběru možnosti **OK** na otevřené zásuvné:  
12. Vyberte dlaždici **smlouvami** na zásuvné integrace účet a zobrazí se nově přidaný smlouva uvedené.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Víc se uč
- [Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack")  
