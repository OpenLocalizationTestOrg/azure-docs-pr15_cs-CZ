<properties
    pageTitle="Sdílení (CORS) podporu více Origin prostředků | Microsoft Azure"
    description="Zjistěte, jak povolit CORS podpora služby Microsoft Azure úložiště."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Sdílení (CORS) Podpora služby Azure úložiště Origin více zdrojů

Začínající verze 2013-08-15 služby Azure úložiště podporu sdílení zdrojů mezi Origin (CORS) služby objektů Blob, tabulky, fronty a soubor. CORS je funkce HTTP, který umožňuje webové aplikace spuštěné v části jednu doménu a získat informace v jiné doméně. Webové prohlížeče implementace omezení zabezpečení jmenoval [stejné origin zásad](http://www.w3.org/Security/wiki/Same_Origin_Policy) , které brání na webovou stránku, volající API v jiné doméně. CORS poskytuje zabezpečené tak a nechte jednu doménu (původní doméně) na volat rozhraní API v jiné doméně. [Specifikace CORS](http://www.w3.org/TR/cors/) podrobné informace najdete na webu CORS.

Můžete nastavit CORS pravidla jednotlivě pro každou z úložiště služby tak, že zavoláte [Nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [Nastavit vlastnosti služby fronty](https://msdn.microsoft.com/library/hh452232.aspx)a [Nastavení vlastnosti tabulky služby](https://msdn.microsoft.com/library/hh452240.aspx). Jakmile nastavíte CORS pravidla pro službu, bude správně ověřené žádost proti službu z jinou doménu vyhodnocen a zjistit, zda je povolen podle pravidel, která jste zadali.

>[AZURE.NOTE] Všimněte si, že CORS mechanismus ověřování. Všechny žádosti proti prostředek úložiště při zapnuté funkci CORS musí mít podpis správné ověřování nebo třeba proti veřejné zdroje.

## <a name="understanding-cors-requests"></a>Principy CORS požadavky

Žádost o CORS z původní doméně může obsahovat dva samostatné požadavky:

- Výstupem žádost, které dotazy CORS omezeními službou. Zadání výstupem není nutná, pokud metoda žádost o je [jednoduchý způsob](http://www.w3.org/TR/cors/), což znamená GET, vedoucí nebo příspěvek.

- Skutečné žádost, proti požadované zdroje.

### <a name="preflight-request"></a>Výstupem požadavek

Dotazy výstupem žádost CORS omezení, které byly pro službu úložiště vlastníkem účtu. Webový prohlížeč (nebo jiné uživatelského agenta) odešle možnosti požadavek, který obsahuje záhlaví žádost, metoda a origin domény. Služba úložiště vyhodnotí zamýšlené operace založené na předkonfigurovaná sady pravidel CORS určující, které origin domény metody požadavků a žádost o záhlaví zadánu skutečné žádosti proti prostředek úložiště.

Pokud CORS aktivované řešení služby a není CORS pravidla, která odpovídá výstupem žádost, službu odpoví stavový kód 200 (Ano) a obsahuje požadované záhlaví řízení přístupu v odpovědi.

Pokud CORS není povoleno pro službu nebo bez CORS pravidlo výstupem žádosti o službu odpoví stavový kód 403 (zakázáno).

Pokud žádost o možnosti neobsahuje požadované CORS záhlaví (záhlaví Origin a způsob řízení přístupu – žádost), službu odpoví stavový kód 400 (Chybný požadavek).

Všimněte si, že žádost o výstupem Vyhodnocená každá její položka proti službu (objektů Blob fronty a tabulky) a není požadovaný prostředek. Vlastník účtu musí povolili CORS jako součást vlastnosti účtu služby v pořadí žádost úspěšné.

### <a name="actual-request"></a>Skutečné požadavek

Jakmile výstupem žádost přijal a je vrácena odpověď, odešle prohlížeči skutečnou žádost proti úložiště zdroje. V prohlížeči se odepřít skutečné okamžitě požadovat, pokud je odmítnuto výstupem žádost.

Zadání skutečných zpracován jako normální požadavek na službu úložiště. Informace o stavu záhlaví Origin označuje, že žádost je žádost o CORS a službu kontroloval CORS pravidel. Pokud je nalezena shoda, záhlaví řízení přístupu jsou přidá odpověď a zpátky k desktopovému klientovi. Pokud není nalezena shoda, nejsou vrátí řízení přístupu CORS záhlaví.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Povolení CORS služby Azure úložiště

Pravidla CORS se nastaví na úrovni služby, je potřeba povolit nebo zakázat CORS pro každou službu (objektů Blob, fronty a tabulek) samostatně. Ve výchozím nastavení není k dispozici CORS pro každou službu. Povolit CORS, budete muset nastavit vlastnosti příslušné služby pomocí verze 2013-08-15 nebo novější a přidat pravidla CORS vlastností služby. Podrobné informace o tom, jak povolit nebo zakázat CORS pro službu a jak nastavit pravidla CORS získáte [Nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [Nastavit vlastnosti služby fronty](https://msdn.microsoft.com/library/hh452232.aspx)a [Nastavení vlastnosti tabulky služby](https://msdn.microsoft.com/library/hh452240.aspx).

Tady je ukázka jedno pravidlo CORS zadané prostřednictvím operace nastavit vlastnosti služby:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Každý prvek součástí pravidla CORS je popsáno níže:

- **AllowedOrigins**: origin domény, které jsou povoleny požádat proti úložiště služby prostřednictvím CORS. Původní doméně je doména, z něhož pochází žádost. Všimněte si, že původ musí být malá a velká písmena přesnou s počátek stáří uživatel odešle ke službě. Můžete taky použít zástupný znak "*" umožňuje všech domén origin aby žádostí o prostřednictvím CORS. Ve výše uvedeném příkladu domény [http://www.contoso.com](http://www.contoso.com) a [http://www.fabrikam.com](http://www.fabrikam.com) můžete požádat proti služby pomocí CORS.

- **AllowedMethods**: metody (slovesa žádost HTTP) používajících CORS požadavku na původní doméně. Ve výše uvedeném příkladu jsou povoleny pouze umístění a získat žádosti o.

- **AllowedHeaders**: záhlaví požadavků určující původní doméně může na žádost o CORS. Ve výše uvedeném příkladu jsou povoleny všechna záhlaví metadat počínaje x-ms metadata x ms meta cílové a x ms meta abc. Všimněte si, že zástupný znak "*" označuje, že jsou povoleny všechny záhlaví začínající na s předponou zadaný.

- **ExposedHeaders**: záhlaví odpověď, která může Odeslaná pošta v odpověď na požadavek CORS a zveřejněné příslušným prohlížeč Vystavitel žádost. Ve výše uvedeném příkladu prohlížeči pokyny zobrazíte všechny začínající záhlaví x-ms-meta.

- **MaxAgeInSeconds**: žádost o maximální velikost doby, že v prohlížeči by měl mezipaměti výstupem možnosti.

Služby Azure úložiště nepodporuje určení stanovené záhlaví **AllowedHeaders** a **ExposedHeaders** prvky. Pokud chcete, aby kategorie záhlaví, můžete určit běžné předpony kategorie. Například zadání *x-ms-meta** jako stanovené záhlaví naváže pravidlo, které se budou shodovat všechna záhlaví, které začínají x-ms-meta.

CORS pravidla platí následující omezení:

- Můžete určit, až na pět CORS pravidla za služba úložiště (objektů Blob tabulky a fronty).
- Maximální velikosti doručovaných všechna CORS pravidla nastavení v pozvánce na schůzku, bez značky XML, nesmí překročit 2 KB.
- Délka povolené záhlaví, vystaveného záhlaví nebo povolené origin neměli překročit 256 znaků.
- Povolené hlavičky a vystaveného může být buď:
  - Literál typu záhlaví, kde název přesné záhlaví je k dispozici, například **x-ms-meta zpracovat**. Maximálně 64 literál záhlaví může být zadán na žádost.
  - S předponou záhlaví, kde předponu záhlaví je k dispozici, například **x-ms metadata***. Určení předpony tímto způsobem umožňuje nebo zpřístupňuje záhlaví, začínající danou předponu. Než dvě stanovené záhlaví zadánu na žádost.
- Metody (nebo akce protokolu HTTP) podle **AllowedMethods** element musí odpovídat metody podporované službou Azure úložiště rozhraní API. Podporované metody jsou odstranit, GET, záhlaví, hromadné KORESPONDENCE, příspěvku, možnosti a umístění.

## <a name="understanding-cors-rule-evaluation-logic"></a>Principy CORS pravidlo hodnocení logiky

Služba úložiště obdrží žádost o výstupem či hodnotu skutečné, vyhodnotí žádosti na základě pravidel CORS, který jste vytvořili pro službu prostřednictvím operaci vhodné nastavit vlastnosti služby. V takovém pořadí, ve které byla nastavena v těle žádost o operace nastavit vlastnosti služby jsou vyhodnoceny CORS pravidla.

Pravidla CORS jsou vyhodnoceny následujícím způsobem:

1. Nejprve původní doméně žádosti o porovnáván domény uvedeny **AllowedOrigins** element. Pokud původní doméně zahrnuté v seznamu nebo všechny domény jsou povoleny s zástupný znak "*", pravidla výnosů hodnocení. Pokud není zahrnut původní doméně, je žádost se nezdaří.

2. Pak metoda (nebo akce HTTP) žádost porovnáván metody uvedené v elementu **AllowedMethods** . Je-li metodu zahrnuta v seznamu, pravidla hodnocení se vytvářejí; v opačném případě žádosti se nezdaří.

3. Pokud žádost vyhovuje pravidlu v jeho původní doméně a jeho způsobem, toto pravidlo opakovaným obrázku, které jsou vyhodnoceny žádosti a žádná další pravidla. Před můžete úspěšně žádost, ale záhlaví zadanou v žádosti jsou porovnáván záhlaví uvedené v elementu **AllowedHeaders** . Pokud záhlaví odeslané neodpovídají povolené záhlaví, žádost se nezdaří.

Protože pravidla se zpracovávají v pořadí, v jakém že se vyskytují v hlavním textu žádosti o, doporučeným zadat nejvíce omezující pravidla týkající se typů nejdřív v rozevíracím seznamu tak, aby tyto jsou vyhodnoceny nejdřív. Zadejte pravidla, které jsou menší omezující – například pravidlo umožňuje všech typů – na konci seznamu.

### <a name="example--cors-rules-evaluation"></a>Příklad – CORS pravidla hodnocení

Následující příklad ukazuje částečný požadavek textu pro operaci nastavit pravidla CORS služby úložiště. Informace o vytváření žádosti najdete v článku [Nastavení vlastností objektů Blob služeb](https://msdn.microsoft.com/library/hh452235.aspx), [Nastavit vlastnosti služby fronty](https://msdn.microsoft.com/library/hh452232.aspx)a [Nastavit vlastnosti tabulku služba](https://msdn.microsoft.com/library/hh452240.aspx) .

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Dále je třeba zvážit následující požadavky na CORS:

Žádost o||| Odpověď||
---|---|---|---|---
**Metoda** |**Origin** |**Žádost o záhlaví** |**Pravidlo** |**Výsledek**
**UMÍSTĚNÍ** | http://www.contoso.com |x-ms-objektů blob typu obsahu | První pravidlo |Úspěch
**ZÍSKÁNÍ** | http://www.contoso.com| x-ms-objektů blob typu obsahu | Druhé pravidlo |Úspěch
**ZÍSKAT** | http://www.contoso.com| x-ms-objektů blob typu obsahu | Druhé pravidlo | Chyba při

Požadavek na první odpovídá první pravidlo – původní doméně odpovídá povolené typů metodu odpovídá povolené metody a záhlaví odpovídá povolené záhlaví – a to se mu.

Druhá žádost neodpovídá první pravidlo, protože metodu neodpovídá povolené metody. To, ale neodpovídá druhé pravidlo tak, aby neproběhne úspěšně.

Požadavek na třetí odpovídá druhé pravidlo v jeho původní doméně a metody, jsou vyhodnoceny žádná další pravidla. Však *záhlaví x-ms--žádost – id klienta* není povolené na základě druhé pravidlo tak, aby žádost nepovede, bez ohledu na to, že sémantiku třetí pravidlo by povolili ho proběhla úspěšně.

>[AZURE.NOTE] I když tento příklad ukazuje méně omezující pravidla před některou přísnější, obecně doporučený postup je nejdřív seznam nejvíce omezující pravidel.

## <a name="understanding-how-the-vary-header-is-set"></a>Vysvětlení, jak nastavit záhlaví různé

Záhlaví *měnit* je standardní záhlaví protokolu HTTP/1.1 obsahujícího sadu polí v záhlaví žádost, které orientační agenta prohlížeče nebo uživatele o kritéria, která jste vybírali serverem zpracovat žádosti. Záhlaví *různé* se většinou používá pro ukládání do mezipaměti proxy servery, prohlížečů a CDN, které umožňuje určit, jak by měl cached odpověď. Podrobnosti najdete v tématu Specifikace pro [různé záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Pokud v prohlížeči nebo jiné uživatelského agenta uloží odpověď na žádost o CORS, původní doméně mezipaměti jako Povolené origin. Pokud druhý doménu stejného požadavku na zdroj úložiště když je aktivní mezipaměti, uživatelského agenta načte režim cached původní doméně. Druhý domény se neshoduje s režim cached domény tak, aby žádost selhává, když by jinak úspěšné. V některých případech úložišti Azure nastaví záhlaví různé **Origin** na pokyn uživatelského agenta následná CORS žádost odešlete službu při požadování doména se liší od počátku v mezipaměti.

Azure úložiště nastaví záhlaví *různé* **Origin** pro skutečné požadavky na získání/vedoucí v těchto případech:

- Při žádosti o origin přesně shoduje s povolené origin definována CORS pravidlo. Aby přesné shody pravidlo CORS nesmí zahrnovat zástupný znak "*" znak.

- Se žádné pravidlo odpovídající původ požadavku, ale CORS aktivované řešení služba úložiště.

V případě kde požadavek GET/vedoucí vyhovuje pravidlu CORS umožňující všech typů odpověď označuje, že jsou povoleny všechny typů a mezipaměť uživatelského agenta vám umožní další žádosti z libovolné domény origin, když je aktivní mezipaměti.

Všimněte si, že pro požadavky pomocí metody než GET/vedoucího, úložiště služby nenastaví záhlaví měnit, protože uživatelských agentů nejsou do mezipaměti odpovědi na těchto postupů.

Následující tabulka uvádí jak Azure úložiště bude odpovídat na získání/vedoucí požadavky podle výše uvedených případech:

Žádost o|Nastavení účtu a výsledek vyhodnocení pravidla|||Odpověď|||
---|---|---|---|---|---|---|---|---
**Origin záhlaví prezentovat na vyžádání** | **Pravidla CORS určeným pro tuto službu** | **Pravidlo odpovídající existuje umožňující všech typů (*)** | **Existuje odpovídající pravidlo pro origin přesné shody** | **Odpověď obsahuje záhlaví měnit nastavení na zdroj** | **Odpověď obsahuje Access – ovládací prvek-povolené-Origin: "*"** | **Odpověď zahrnuje Access – ovládací prvek-zveřejněným-záhlaví**
Ne|Ne|Ne|Ne|Ne|Ne|Ne
Ne|Ano|Ne|Ne|Ano|Ne|Ne
Ne|Ano|Ano|Ne|Ne|Ano|Ano
Ano|Ne|Ne|Ne|Ne|Ne|Ne
Ano|Ano|Ne|Ano|Ano|Ne|Ano
Ano|Ano|Ne|Ne|Ano|Ne|Ne
Ano|Ano|Ano|Ne|Ne|Ano|Ano


## <a name="billing-for-cors-requests"></a>Fakturace za CORS požadavky

Pokud jste povolili CORS některého z úložiště služby pro váš účet (tak, že zavoláte [Nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [Nastavit vlastnosti služby fronty](https://msdn.microsoft.com/library/hh452232.aspx)nebo [Nastavit vlastnosti tabulku služba](https://msdn.microsoft.com/library/hh452240.aspx)) je faktura úspěšných výstupem požadavků. Minimalizovat poplatky, zvažte nastavení **MaxAgeInSeconds** element v pravidlech CORS velké hodnoty tak, aby uživatelského agenta do mezipaměti žádost.

Neúspěšné výstupem požadavky se vám nebudou účtovat poplatky.

## <a name="next-steps"></a>Další kroky

[Nastavení vlastností služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx)

[Nastavení vlastností služby fronty](https://msdn.microsoft.com/library/hh452232.aspx)

[Nastavení vlastností tabulky služby](https://msdn.microsoft.com/library/hh452240.aspx)

[Sdílení specifikace W3C Origin více zdrojů](http://www.w3.org/TR/cors/)
