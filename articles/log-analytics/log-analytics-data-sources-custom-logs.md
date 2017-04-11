<properties 
   pageTitle="Vlastní protokoly do protokolu analýzy | Microsoft Azure"
   description="Protokol analýzy můžete shromažďovat soubory ve formátu RTF počítačích Windows a Linux události.  Tento článek popisuje, jak definovat nové vlastní protokolu a podrobné informace o záznamy, které by vytvářeli v úložišti OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-logs-in-log-analytics"></a>Vlastní protokoly do protokolu analýzy

Vlastní protokoly zdroje dat v protokolu analýzy umožňuje shromáždit události z soubory ve formátu RTF počítačích Windows a Linux. Řada aplikací zaznamenávat informace o soubory ve formátu RTF místo standardní protokolování služeb, jako je protokolu událostí systému Windows nebo Syslog.  Jakmile získány, můžete analyzovat každý záznam v protokolu na jednotlivá pole pomocí funkce [Vlastní pole](log-analytics-custom-fields.md) protokolu analýzy.

![Vlastní protokolu kolekce](media/log-analytics-data-sources-custom-logs/overview.png)

Protokoly shromažďované musí kritériím.

- Protokol musí mít jedna položka na řádek nebo používat časové razítko odpovídající jednu z následujících formátů na začátku každé položky.

    RRRR MM-DD HH: MM: <br>
  M/D/RRRR HH: MM: SS DOP. / ODP. <br>
  Pondělí DD, YYYY HH: mm:
    
- Soubor protokolu nesmí povolit cyklický aktualizace, kde je soubor přepíše nové položky. 

## <a name="defining-a-custom-log"></a>Definování vlastních protokol

Pomocí následujícího postupu definovat vlastní soubor protokolu.  Přejděte na konci tohoto článku návod ukázkového obrazovky pro přidání vlastní protokol.

### <a name="step-1-open-the-custom-log-wizard"></a>Krok 1. Otevřít Průvodce vlastní protokolu

Vlastní Průvodce protokolu běží na portálu OMS a umožňuje definovat nové vlastní protokol shromažďovat.

1.  Na portálu OMS přejděte na **Nastavení**.
2.  Klikněte na **Data** a potom **vlastní protokoly**.
3.  Ve výchozím nastavení se všechny změny konfigurace automaticky posunou všechny agentů.  Pro Linux agenti – konfiguračního souboru jsou odeslány Fluentd shromažďování dat  Pokud chcete upravit tento soubor ručně na každý Linux agent, potom zrušte zaškrtnutí políčka *použít pod konfigurace Moje počítačům Linux*.
4.  Klikněte na tlačítko **Přidat +** otevřete Průvodce vlastní protokolu.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Krok 2. Nahrávání a analyzovat protokolování ukázka

Začněte tím, že odešlete výběru vlastní protokolu.  Průvodce analyzovat a zobrazit položky v tomto souboru můžete ověřit.  Protokol analýzy budou používat na oddělovač, který zadáte k identifikaci jednotlivých záznamů.

**Nový řádek** je výchozím oddělovačem a používá pro soubory protokolů, které mají jednu položku na řádku.  Pokud řádku začíná datum a čas v jednom z dostupných formátů, můžete určit **časové razítko** oddělovač, který podporuje položky, které zahrnují více než jeden řádek. 

Pokud se používá časové razítko oddělovač, bude vlastnost TimeGenerated každý záznam uložený v OMS vyplněné s určeným pro danou položku do souboru protokolu datum a čas.  Pokud se používá oddělovač nový řádek, TimeGenerated vyplněné s datem a časem protokolu analýzy shromažďované položce. 

>[AZURE.NOTE]Protokol analýzy aktuálně zpracuje odebrané protokol pomocí oddělovače časové razítko jako UTC datum a čas.  To brzy změní se na zástupce používat časové pásmo. 
 
1.  Klikněte na **Procházet** a vyhledejte ukázkový soubor.  Všimněte si, že to může tlačítko může být označeno jako **Zvolte soubor, který** v některých prohlížečích.
2.  Klikněte na tlačítko **Další**. 
3.  Průvodce protokolu vlastní nahrání souboru a seznam záznamy, které označuje.
4.  Změňte oddělovač, který slouží k určení nový záznam a vyberte oddělovač, který nejlépe identifikovat záznamy v souboru protokolu.
5.  Klikněte na tlačítko **Další**.

### <a name="step-3-add-log-collection-paths"></a>Krok 3. Přidání dráhy kolekce protokolu

Na zástupce, kde můžete najít protokol vlastní je třeba definovat jedné nebo více cest.  Buď můžete zadat určitou cestu a název souboru protokolu nebo zadat cestu pomocí zástupných znaků pro název.  Tato možnost podporuje aplikace, které vytvoříte nový soubor každý den nebo dosáhne určitou velikost souboru.  Můžete také zadat více cest pro jednoho souboru protokolu.

Například aplikaci může vytvořit soubor protokolu každý den s datem zahrnuté v názvu jako log20100316.txt. Může být vzor pro tyto protokol *protokolu\*txt* která by se vztahují na libovolný soubor protokolu po aplikaci názvů prvku schéma.

Následující tabulka obsahuje příklady vzorů platné můžete určit jiné protokoly. 

| Popis | Cesta |
|:--|:--|
| Všechny soubory v *C:\Logs* s příponou na agent systému Windows | C:\Logs\\\*txt |
| Všechny soubory v *C:\Logs* název začíná protokolu a příponou na agent systému Windows | C:\Logs\log\*txt |
| Všechny soubory v */var/log/audit* s příponou Linux agenta | /var/log/audit/*.txt |
| Všechny soubory v */var/log/audit* název začíná protokolu a příponou Linux agenta | /var/log/audit/log\*txt |
  

1.  Vyberte Windows nebo Linux můžete určit, jaký formát cestu přidáváte.
2.  Zadejte cestu a klepněte **+** tlačítko.
3.  Opakujte postup pro všechny další cesty.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Krok 4. Zadejte název a popis protokolu

Název, který zadáte se použije pro typ protokolu ve výše uvedeném.  Bude vždy končit _CL odlište jako vlastní protokol.

1.  Zadejte název pro protokol.  ** \_CL** přípona je automaticky k dispozici.
2.  Přidání volitelný **Popis**.
3.  Klikněte na **Další** uložte definici vlastní protokolu.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Krok 5. Ověřte, že se právě shromažďují vlastní protokoly
Může trvat na hodinu pro počátečního data z nový vlastní protokol zobrazit v protokolu analýzy.  Spustí shromažďování položky z protokolů naleznete na cestě jste zadali od bodu definované vlastní protokolu.  Nebudou zachovány položky, které jste nahráli při vytváření vlastních protokol, ale shromáždí již existující položky vyhledá souborů protokolu.

Jakmile protokolu analýzy začne shromažďování vlastní protokoly systému, své záznamy budou k dispozici protokolu vyhledávání.  Použijte název udělili protokol vlastní **Typ** v dotazu.

>[AZURE.NOTE] Pokud je vlastnost prvotní_data chybí hledání, budete muset zavřít a znova otevřete prohlížeč.

### <a name="step-6-parse-the-custom-log-entries"></a>Krok 6. Analyzovat položky vlastní protokolu

Celý záznam se uloží do jedné vlastnosti s názvem **prvotní_data**.  Bude pravděpodobně chcete oddělit různá informací v každé položky do jednotlivých vlastností uložené v záznamu.  Udělat toto pomocí funkce [Vlastní pole](log-analytics-custom-fields.md) protokolu analýzy.

Podrobný postup pro analýzu vlastní záznam nezobrazují tady.  Získáte [Vlastní pole](log-analytics-custom-fields.md) si přečtěte následující dokumentaci pro tyto informace.

## <a name="disabling-a-custom-log"></a>Zakázání vlastní protokol

Definice vlastního protokolu nemůžete odebrat po už je vytvořená, ale můžete ho zakázat tím, že odeberete všechny své kolekce cesty.

1.  Na portálu OMS přejděte na **Nastavení**.
2.  Klikněte na **Data** a potom **vlastní protokoly**.
3.  Klikněte na **Podrobnosti** vedle definici vlastní protokolu zakázat.
4.  Odebrání všech kolekce cesty pro definici vlastní protokolu.


## <a name="data-collection"></a>Shromažďování dat

Protokol analýzy shromáždí nové položky z každé vlastní protokolu zhruba každých 5 minut.  Agent zaznamená jeho umístění v jednotlivých souborů protokolu, který shromáždí z.  Pokud agent přejde do režimu offline pro určitého časového období, bude položky od posledního místa vypnutí, shromažďovat protokolu analýzy i v případě ty položky, které byly vytvořené v době, kdy agent offline.

Aby došlo k jedné vlastnosti s názvem **prvotní_data**jsou zápisu celý obsah položky protokolu.  Můžete to analyzovat do více vlastností, které lze analyzovat a vyhledat samostatně definování [Vlastních polí](log-analytics-custom-fields.md) po vytvoření vlastní protokolu.


## <a name="custom-log-record-properties"></a>Vlastní protokolu dialogovém okně Vlastnosti záznamu

Vlastní protokolu záznamy obsahují typu s protokolu název, který zadáte a vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| TimeGenerated | Datum a čas, shromážděné záznam podle protokolu analýzy.  Pokud protokol používá oddělovač času na základě Toto je údaj o čase shromážděné z položky. |
| SourceSystem  | Typ agent záznam byla vybrána z. <br> OpsManager – agent systému Windows, buď přímé připojení nebo SCOM <br> Linux – všechny Linux agentů  |
| Prvotní_data             | Celý text shromážděných položky. |
| ManagementGroupName | Název skupiny správy SCOM agentů.  U jiných agenti – jedná AOI -\<ID pracovního prostoru\> |


## <a name="log-searches-with-custom-log-records"></a>Protokol vyhledávání záznamů vlastní protokolu

Záznamy z vlastních protokolů jsou uložené v úložišti OMS stejně jako záznamy z jiného zdroje dat.  Budou muset typu odpovídající název, který zadáte při definování protokol, abyste mohli používat vlastnost typ v hledání načítat záznamy odebrané konkrétní protokol.

Následující tabulka obsahuje jiné příklady protokolu vyhledávání, které načítat záznamy ze vlastní protokoly.

| Dotaz | Popis |
|:--|:--|
| Typ = MyApp_CL | Všechny události z vlastní protokolu pojmenované MyApp_CL. |
| Typ = MyApp_CL Severity_CF = chyba | Všechny události z vlastní protokol pojmenovaných MyApp_CL s hodnotou *chyby* ve vlastní pole s názvem *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Ukázka návodu obrazovky pro přidání vlastní protokol

V následující části provede příklad vytvoření vlastní protokol.  Ukázkový protokol odebraná má jedna položka v každém řádku začínající datum a čas a potom čárka pole oddělená oddělovači kód, stavu a zpráv.  Několik položek příkladu jsou uvedeny níže.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Nahrávání a analyzovat protokolování ukázka

Zadáte souborů protokolu jsme vidí události, které bude shromažďování.  Nový řádek v tomto případě je dostatečně oddělovač.  Pokud jedna položka v protokolu může zahrnovat více řádků přes, oddělovače časové razítko by potřeba použít.

![Nahrávání a analyzovat protokolování ukázka](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Přidání dráhy kolekce protokolu

Soubory protokolu bude nacházet v *C:\MyApp\Logs*.  Nový soubor se vytvoří každý den s názvem, který obsahuje datum ve vzorku *appYYYYMMDD.log*.  Dostatečné vzor pro tento protokol bude *C:\MyApp\Logs\\\*.log*.

![Cesta kolekce protokolu](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Zadejte název a popis protokolu

Použijeme název *MyApp_CL* a zadejte do pole **Popis**.

![Název protokolu](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Ověřte, že se právě shromažďují vlastní protokoly

Používáme dotazu nebo *Typ = MyApp_CL* k vrácení všech záznamů shromážděny protokoly systému.

![Dotaz protokolu bez jakýchkoli vlastních polí](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Analyzovat položky vlastní protokolu

Vlastní pole používáme k definování *EventTime*, *kód*, *Stav*a pole *zprávu* a uvidíte rozdíl v záznamy, které jsou vracená dotazem.

![Dotaz protokolu ve vlastních polích](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Další kroky

- Použití [Vlastních polí](log-analytics-custom-fields.md) analyzovat položky vlastní protokolu na jednotlivá pole.
- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení. 