<properties 
    pageTitle="Základní informace o X12 a Enterprise integrace Pack | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Naučte se používat X12 smlouvami k vytváření aplikací pro použití logických operátorů" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Podnikové integrace s X12 

>[AZURE.NOTE]Tato funkce se vztahuje X12 stránky logiky aplikací. Další informace o EDIFACT klikněte [sem](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Vytvoření X12 smlouva 
Předtím, než si můžete vyměňovat X12 zprávy, potřebujete k vytvoření X12 smlouvy a uložte ho do svého účtu integrace. Podle těchto kroků vás provede jednotlivými proces vytváření X12 smlouvy.

### <a name="heres-what-you-need-before-you-get-started"></a>Tady je budete potřebovat než začnete
- [Integrace účet](./app-service-logic-enterprise-integration-accounts.md) podle předplatného Azure  
- Aspoň dva [partnery](./app-service-logic-enterprise-integration-partners.md) už určených ve vašem účtu integrace  

>[AZURE.NOTE]Při vytváření dohodu obsahu v souboru smlouvy se musí shodovat typ smlouvy.    


Poté, co jste [účet integrace vytvořené](./app-service-logic-enterprise-integration-accounts.md) a [přidané partnerů](./app-service-logic-enterprise-integration-partners.md), můžete vytvořit X12 smlouva provedením těchto kroků:  

### <a name="from-the-azure-portal-home-page"></a>Z Azure domovskou stránku portálu

Po přihlášení do [Azure portál](http://portal.azure.com "Azure portálu"):  
1. **Přejděte** v nabídce vyberte na levé straně.  

>[AZURE.TIP]Pokud nevidíte odkaz **Procházet** , budete muset nejdřív rozbalte nabídku. To udělat tak, že vyberete **Zobrazit nabídku** odkaz, který je umístěn v levém horním rohu sbalený nabídky.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integrace* zadejte do vyhledávacího pole Filtr a pak vyberte **Účty integrace** ze seznamu výsledků.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. V zásuvné **Integrace účty** , které se objeví vyberte integrace účtu, na kterém můžete vytvořit smlouvu. Pokud nevidíte veškerá integrace účty seznamy, [vytvořit první](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Vyberte dlaždici **smlouvami** . Pokud se nezobrazuje dlaždice smlouvami nejdřív přidat.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Klikněte na tlačítko **Přidat** v zásuvné smlouvami, která se otevře.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Zadejte **název** pro souhlas potom vyberte **Typ smlouvy**, **Hostitele partnera**, **Hostitele Identity**, **Hosta partnera**, **Hosta Identity**v zásuvné smlouvami, která se otevře.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Po nastavení příjem nastavení vlastnosti, klikněte na tlačítko **OK**  
Pojďme pokračovat:  
8. Vyberte **Nastavení přijímání** můžete nakonfigurovat, jak mají být zpracování zprávy přijmete prostřednictvím této smlouvy.  
9. Ovládací prvek nastavení přijímání je rozdělen v následujících částech, včetně identifikátory potvrzení, schémata, obálky, čísla ovládací prvek, ověření a interní nastavení. Nakonfigurujte tyto vlastnosti na základě vaší smlouvy u partnera, bude výměně zpráv s. Tady je přehled tyto možnosti řízení, nastavovat podle způsobu této smlouvy identifikovat a zpracovávání příchozích zpráv:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Klikněte na tlačítko **OK** uložte nastavení.  

### <a name="identifiers"></a>Identifikátory

|Vlastnost|Popis |
|---|---|
|ISA1 (se tak mohli ověřovat kvalifikátor)|V rozevíracím seznamu vyberte hodnotu kvalifikátor se tak mohli ověřovat.|
|ISA2|Volitelné. Zadejte informace se tak mohli ověřovat. Pokud je argument hodnota, kterou jste zadali jako ISA1 než 00, zadejte aspoň jeden alfanumerický znak a maximálně 10.|
|ISA3 (zabezpečení kvalifikátor)|V rozevíracím seznamu vyberte hodnotu kvalifikátor zabezpečení.|
|ISA4|Volitelné. Zadejte hodnotu zabezpečení informací. Pokud je argument hodnota, kterou jste zadali jako ISA3 než 00, zadejte aspoň jeden alfanumerický znak a maximálně 10.|

### <a name="acknowledgments"></a>Potvrzení 

|Vlastnost|Popis |
|----|----|
|TA1 očekávat|Zaškrtnutím tohoto políčka se vrátíte technické potvrzování (TA1) výměny odesílateli. Tato potvrzení jsou odesílateli výměny na základě nastavení Odeslat smlouvy.|
|IM očekávat|Zaškrtnutím tohoto políčka vrátíte funkční (IM) potvrzení odesílateli výměny. Vyberte, jestli chcete potvrzení 997 nebo 999 podle verzí schéma, které pracujete s. Tato potvrzení jsou odesílateli výměny na základě nastavení Odeslat smlouvy.|
|Zahrnout AK2/IK2 smyčky|Zaškrtnutím tohoto políčka Povolit generování AK2 smyčky funkční potvrzení pro přípustném transakce sady. Poznámka: Toto zaškrtávací políčko je povolený jenom v případě, že zaškrtnete políčko IM by měly.|

### <a name="schemas"></a>Schémata

Zvolte schématu pro každý typ transakce (ST1) a aplikace odesílatele (GS2). Přijímání kanálu provede zpětný překlad příchozí zprávy porovnáním hodnot pro ST1 a GS2 příchozí zprávy s hodnoty, které zde nastavíte a se schématem schématu příchozí zprávy, která že nastavíte tady.

|Vlastnost|Popis |
|----|----|
|Verze|Vyberte X12 verze|
|Typ transakce (ST01)|Vyberte typ transakce|
|Aplikace odesílatele (GS02)|Vyberte aplikaci odesílatele|
|Schéma|Vyberte soubor schématu, kterého chcete nám. Soubory schématu jsou umístěny ve vašem účtu integrace.|

### <a name="envelopes"></a>Obálky

|Vlastnost|Popis |
|----|----|
|Použití ISA11|Toto pole slouží k určení oddělovač v sadě transakce:</br></br>Vyberte standardní identifikátor používat desetinné zápis "." místo desetinné zápisu příchozí dokument v upravit dostávat kanálem k odesílání zpráv.</br></br>Vyberte oddělovač opakování můžete určit oddělovač u opakované výskyty jednoduchá data prvek nebo opakovaná datová struktura. (^) Se obvykle používá jako oddělovač opakování. Pro HIPAA schémata můžete použít jenom (^).|

### <a name="control-numbers"></a>Ovládací prvek čísel

|Vlastnost|Popis |
|----|----|
|Zakázání Interchange ovládací prvek číslo duplicitních položek|Zaškrtněte políčko Tento blokovat duplicitní křižovatky. -Li vybrán, portál služeb BizTalk zkontroluje číslo ovládací prvek interchange (ISA13) přijaté výměny neodpovídá požadovanou velikost ovládacího prvku výměny. Pokud je zjištěno shodu, kanálu přijímání nezpracuje výměnu.<br/>Pokud si Pokud chcete zakázat duplicitních výměny ovládací prvek čísel můžete určit počet dní, které kontrola provádí odeslání na odpovídající hodnotu pro vyhledat duplicitní ISA13 každých x dnů.|
|Zakázání skupiny ovládací prvek číslo duplicit|Zaškrtněte políčko Tento blokovat křižovatky reprezentovaný číslem od ovládacího prvku duplicitní skupiny.|
|Zakázání transakce sadu ovládací prvek číslo duplicit|Zaškrtněte políčko Tento blokovat křižovatky reprezentovaný číslem od ovládacího prvku nastavit duplicitní transakce.|

### <a name="validations"></a>Ověření

|Vlastnost|Popis |
|----|----|
|Typ zprávy|Typ upravit zprávy jako 850 nákupní objednávky nebo 999 implementaci potvrzení.|
|Upravit ověření|Ověřuje upravit na datové typy podle upravit vlastnosti schéma, omezení délky, prázdné údaje a koncových oddělovače.|
|Rozšířené ověření|Pokud datový typ je upravit, ověření na požadavek na prvek dat a povolené opakování, výčty a ověření dat prvek délka (min a max).|
|Povolit úvodní nebo koncové nuly|Další mezeru a nulové znaky, které jsou počáteční mezery a znaků se zachovají. Nebudou odebrány.|
|Koncové oddělovač zásad|Vygeneruje koncových oddělovače na výměny dostali. Možnosti zahrnují NotAllowed volitelné a povinné.|

### <a name="internal-settings"></a>Vnitřní nastavení

|Vlastnost|Popis |
|----|----|
|Převedení předpokládanou desetinným Nn založit 10 číselné hodnoty|Převede číslo upravit zadané ve formátu Nn základem 10 číselnou hodnotu v intermediate XML na portálu BizTalk služby.|
|Vytvoření prázdné značky XML, pokud jsou povoleny koncových oddělovače|Vyberte toto zaškrtávací políčko mít odesílatele výměny obsahovat prázdné značky XML pro koncové oddělovače.|
|Příchozí dávkování zpracování|Rozdělení výměny jako sady transakce - pozastavit transakci sady při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML použitím příslušných obálky do sady transakci. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak BizTalk služby pozastaví jenom ty transakce sady. </br></br>Rozdělení výměny jako sady transakce - pozastavení výměny při chybě: analyzuje každou transakci nastavení v křižovatky do samostatného dokumentu XML použitím příslušných obálky. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak BizTalk služby pozastaví celý výměny.</br></br>Zachovat výměny - pozastavení transakce sady při chybě: zůstane nedotčený, vytváření dokumentu XML pro celý dávkový výměnu výměnu. Vyberete tuto možnost pokud onAe nebo další transakce sad v výměnu ověření se nezdařilo, pak BizTalk služby pozastaví pouze ty transakce sady, a pokračujte v procesu všechny ostatní transakce sady.</br></br>Zachovat výměny - pozastavení výměny při chybě: zůstane nedotčený, vytváření dokumentu XML pro celý dávkový výměnu výměnu. Vyberete tuto možnost Pokud jeden nebo více transakce sad v výměnu ověření se nezdařilo, pak BizTalk služby pozastaví celý výměny.</br></br>|

Vaše smlouva je připravená k zpracovávání příchozích zpráv, které odpovídají schéma, které jste vybrali.

Konfigurace nastavení, které zpracovávají zprávy odešlete pro partnery společnosti:  
11. Vyberte **Nastavení pro odesílání** a nakonfigurovat, jak mají být zpracování zprávám odeslaným pomocí této smlouvy.  

Ovládací prvek odeslat nastavení je rozdělen v následujících částech, včetně identifikátory potvrzení, schémata, obálky, čísla ovládací prvek, znakové sady a oddělovače a ověření. 

Tady je přehled tyto možnosti řízení. Vyberte požadované nastavení podle způsobu zpracování zpráv, které budete posílat partnerům této smlouvy:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Klikněte na tlačítko **OK** uložte nastavení.  

### <a name="identifiers"></a>Identifikátory
|Vlastnost|Popis |
|----|----|
|Povolení kvalifikátor (ISA1)|V rozevíracím seznamu vyberte hodnotu kvalifikátor se tak mohli ověřovat.|
|ISA2|Zadejte informace se tak mohli ověřovat. Pokud je tato hodnota než 00, zadejte aspoň jeden alfanumerický znak a maximálně 10.|
|Zabezpečení kvalifikátor (ISA3)|V rozevíracím seznamu vyberte hodnotu kvalifikátor zabezpečení.|
|ISA4|Zadejte hodnotu zabezpečení informací. Pokud je tato hodnota než 00 textového pole hodnotu (ISA4) zadejte aspoň jeden alfanumerická hodnota a maximálně 10.|

### <a name="acknowledgment"></a>Potvrzení
|Vlastnost|Popis |
|----|----|
|TA1 očekávat|Zaškrtnutím tohoto políčka se vrátíte technické potvrzování (TA1) výměny odesílateli. Tohle nastavení určuje, že partnera Host (hostitel), která zprávu odesílá žádosti odpověď na odeslanou zprávu od partnera hosta smlouvy. Tato potvrzení očekává partnerem hostitele na základě nastavení přijmout smlouvu.|
|IM očekávat|Zaškrtnutím tohoto políčka vrátíte funkční (IM) potvrzení odesílateli výměny a pak vyberte, jestli chcete potvrzení 997 nebo 999 podle verzí schéma, které pracujete s. Tato potvrzení očekává partnerem hostitele na základě nastavení přijmout smlouvu.|
|IM verze|Vyberte požadovanou verzi IM|

### <a name="schemas"></a>Schémata
|Vlastnost|Popis |
|----|----|
|Verze|Vyberte X12 verze|
|Typ transakce (ST01)|Vyberte typ transakce|
|SCHÉMA|Vyberte schéma, používat. Schémata jsou umístěny ve vašem účtu integrace. Přístup k vaší schémata, nejprve propojte účtu integrace logiky aplikace.|

### <a name="envelopes"></a>Obálky
|Vlastnost|Popis |
|----|----|
|Použití ISA11|Toto pole slouží k určení oddělovač v sadě transakce:</br></br>Vyberte standardní identifikátor používat desetinné zápis "." místo desetinné zápisu příchozí dokument v upravit dostávat kanálem k odesílání zpráv.</br></br>Vyberte oddělovač opakování můžete určit oddělovač u opakované výskyty jednoduchá data prvek nebo opakovaná datová struktura. (^) Se obvykle používá jako oddělovač opakování. Pro HIPAA schémata můžete použít jenom (^).</br>|
|Oddělovač opakování|Zadejte oddělovač opakování|
|Číslo verze ovládacího prvku (ISA12)|Vyberte verzi standardní X12 používanou portálu BizTalk služby pro generování křižovatky odchozí.|
|Použití indikátor (ISA15)|Zadejte, zda místní křižovatky jsou informace, (I), výrobní data (P), nebo testovat data (T). Zobrazí upravit kanálu líbí, zvýší tuto vlastnost na kontextu.|
|Schéma|Můžete zadat, jak na portálu služby BizTalk vygeneruje segmenty GS a ST křižovatky kódovaný X12, která odešle kanálu odeslat.</br></br>Můžete přiřadit hodnoty spravuje organizace GS1 GS2, GS3, GS4, GS5, GS7 a GS8 údaje s hodnotami typu transakce a/verze datové prvky. Když portál služeb BizTalk zjistí, že zprávu XML obsahuje hodnoty nastavit u typů a prvků/verzi v řádku mřížky a potom naplní datové prvky spravuje organizace GS1 GS2, GS3, GS4, GS5, GS7 a GS8 v obálce odchozí výměny s hodnoty ze stejného řádku mřížky. Hodnoty typů a prvků/verze musí být jedinečná.</br></br>Volitelné. Spravuje organizace GS1 vyberte hodnotu pro kód funkční z rozevíracího seznamu.</br></br>Povinné. Pro GS2 zadejte hodnotu alfanumerický odesílatele aplikace s nejméně dvěma znaky a maximálně 15 znaků.</br></br>Povinné. Pro GS3 zadejte hodnotu alfanumerický u sluchátko aplikace s nejméně dvěma znaky a maximálně 15 znaků.</br></br>Volitelné. Pro GS4 vyberte CCYYMMDD rrmmdd.</br></br>Volitelné. Pro GS5 vyberte hh: mm, HHMMSS nebo HHMMSSdd.</br></br>Volitelné. Pro GS7 vyberte hodnotu pro příslušné subjekt z rozevíracího seznamu.</br></br>Volitelné. GS8 zadejte alfanumerická hodnota pro dokument označen aspoň jeden znak a maximálně 12 znaků.</br></br>**Poznámka**: Toto jsou hodnoty, které portál služeb BizTalk zadá v polích GS výměny sestavení Pokud zadejte transakce a/verze prvky ve stejném řádku jsou shodu můžou být přidružené výměnu.|

### <a name="control-numbers"></a>Ovládací prvek čísel
|Vlastnost|Popis |
|----|----|
|Interchange ovládací prvek číslo (ISA13)|Povinné. Zadejte rozsahu hodnot pro ovládací prvek číslo výměny používá portál služeb BizTalk při generování křižovatky odchozí. Zadejte číselnou hodnotu s minimálně 1 a maximálně 999999999.|
|Skupina ovládací prvek číslo (GS06)|Povinné. Zadejte oblast čísel, která portál služeb BizTalk by měl použít pro ovládací prvek číslo skupiny. Číselné hodnoty musí mít aspoň jeden znak a maximálně devět znaků.|
|Ovládací prvek číslo sady (ST02)|Kontrola nastavení transakce čísla (ST02) zadejte oblast číselné hodnoty pro střední povinná a alfanumerický hodnot pro volitelné Předpona a přípona. Maximální délka všemi čtyřmi poli je devět znaků.|
|Předpona|Pokud chcete určit rozsah čísel transakce sadu ovládací prvek používán odeslanou, zadejte hodnoty do polí ACK ovládací prvek číslo (ST02). Zadejte číselnou hodnotu Střední dvou polí a alfanumerická hodnota (v případě potřeby) do Předpona a přípona polí. Pole iniciály prostředního podporují a obsahují minimální a maximální hodnoty pro ovládací prvek číslo. Předpona a přípona jsou volitelné. Maximální délka pro všechny tři pole je devět znaků.|
|Přípona|Pokud chcete určit rozsah čísel transakce sadu ovládací prvek používán odeslanou, zadejte hodnoty do polí ACK ovládací prvek číslo (ST02). Zadejte číselnou hodnotu Střední dvou polí a alfanumerická hodnota (v případě potřeby) do Předpona a přípona polí. Pole iniciály prostředního podporují a obsahují minimální a maximální hodnoty pro ovládací prvek číslo. Předpona a přípona jsou volitelné. Maximální délka pro všechny tři pole je devět znaků.|

### <a name="character-sets-and-separators"></a>Znak sady a oddělovači
Kromě sada znaků, můžete zadat jinou sadu oddělovače pro použití u jednotlivých typů zprávy. Není-li znaková sada pro danou zprávu schéma, se používá znakové sady výchozí.

|Vlastnost|Popis |
|----|----|
|Znaková sada pro použití|Vyberte X12 znakovou sadu ověřuje vlastnosti, které zadáte smlouvy.</br></br>**Poznámka**: Toto nastavení portálu služby BizTalk pouze používá k ověření hodnoty zadané vlastností souvisejících smlouvy. Přijímání kanálem k odesílání zpráv nebo odeslat kanálem k odesílání zpráv ignoruje tato vlastnost znakové sady při provádění běhu zpracování.|
|Schéma|Symbol vyberte (+) a vyberte schéma z rozevíracího seznamu. U vybrané schéma vyberte oddělovače umožníte používat sadu:</br></br>Součásti prvek oddělovač – Enter znak k oddělení složené datové prvky.</br></br>Data prvek oddělovač – Enter znak jednotlivé prvky jednoduchá data v rámci složené datové prvky.</br></br></br></br>Nahrazení znak – zaškrtněte toto políčko Pokud datové, který obsahuje data znaky, které používají také jako data, segmentu nebo component oddělovače. Pak můžete zadat znak náhradní. Při generování výstupní X12 zprávu, všechny výskyty oddělovače v datové, které dat nahrazeny zadaný znak.</br></br>Segmentech ukončení – zadejte znak označte konec Upravit segment.</br></br>Přípona – vyberte znak, který se používá identifikátorem segment. Určuje příponu prvek data ukončení segmentu může být prázdný. Pokud ukončení segment prázdné, je třeba určit příponu.|

### <a name="validation"></a>Ověření
|Vlastnost|Popis |
|----|----|
|Typ zprávy|Tato možnost umožňuje ověření na výměny příjemce. Ověřování ověřuje upravit na transakce sadu údaje, ověřování datové typy, omezení délky a prázdné datové prvky a na konci oddělovače.|
|Upravit ověření||
|Rozšířené ověření|Tato možnost umožňuje rozšířené ověření křižovatky přijaté od odesílatele výměny. Jedná se o ověření délku pole, volitelnost a počet opakování kromě XSD datový typ ověření. Můžete povolit rozšíření ověření bez povolení ověření upravit, nebo naopak.|
|Povolit počáteční a koncové nuly|Tato možnost určuje, že křižovatky upravit dostali ze strany není ověření se nezdařilo: Pokud není v souladu s jeho délce požadavek z důvodu nebo koncových mezer údaj křižovatky upravit, ale dodržuje svůj požadavek délky, když jsou odebrány.|
|Koncové oddělovače|Tato možnost určuje křižovatky upravit dostali ze strany není ověření se nezdařilo: Pokud není v souladu s svůj požadavek délka kvůli začátku (nebo na konci) nuly nebo koncové mezery údaj křižovatky upravit, ale dodržuje svůj požadavek délky, když jsou odebrány.</br></br>Vyberte není povoleno, pokud nebudete chtít povolit koncových oddělovače a oddělovače ve křižovatky přijaté od odesílatele výměny. Obsahuje-li výměnu koncových oddělovače a oddělovače, je neplatný.</br></br>Vyberte volitelné přijmout křižovatky s a nebo bez koncových oddělovače tisíců.</br></br>Vyberte povinný, pokud přijaté výměny musí obsahovat koncových oddělovače a oddělovače.|

Po výběru možnosti **OK** na otevřené listy:  
13. Vyberte dlaždici **smlouvami** na zásuvné integrace účet a zobrazí se nově přidaný smlouva uvedené.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Víc se uč
- [Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack")  
