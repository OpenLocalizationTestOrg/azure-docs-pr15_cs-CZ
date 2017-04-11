<properties
   pageTitle="Jak se připojit ke zdrojům dat | Microsoft Azure"
   description="Článek s postupy zvýraznění postup připojení ke zdrojům dat s katalogem dat Azure."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Jak se připojit ke zdrojům dat

## <a name="introduction"></a>Úvod
**Katalog dat Microsoft Azure** je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. **Katalog dat Azure** jinými slovy, je celé o tom pomáhá lidé zjišťovat, pochopit a získání další hodnoty z existujících dat pomocí zdrojů dat a nápovědu organizace. Klíčové poměr tento scénář je používat data – po uživatel zjistí zdroje dat a rozumí jeho účel, dalším krokem je připojení ke zdroji dat jeho data můžete umístit.

## <a name="data-source-locations"></a>Umístění zdroje dat
Při registraci zdroj dat obdrží **Katalog dat Azure** metadata o zdroji dat. Tato metadata obsahuje podrobné informace o zdroji dat umístění. Podrobnosti o umístění lišit podle typu zdroje dat ke zdroji dat, ale bude vždy obsahovat informace potřebné k připojení. Umístění tabulce SQL serveru obsahuje například název serveru, název databáze, schématu název a název tabulky, zatímco místu pro sestavu SQL Server Reporting Services obsahuje název serveru a cesta k sestavě. Další typy zdrojů dat bude mít umístění odrážející strukturu a funkce systému zdroje.

## <a name="integrated-client-tools"></a>Integrované klientských nástrojích
Nejjednodušší způsob, jak připojit ke zdroji dat, je použít "otevřít ve..." v nabídce na portálu **Katalog dat Azure** . V této nabídce zobrazí seznam možností pro připojení k materiálů vybraná data.
Pokud chcete použít jako výchozí zobrazení vedle sebe, této nabídce k dispozici na jednotlivé dlaždice.

 ![Otevření tabulky SQL serveru v aplikaci Excel z dlaždici materiálů dat](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Při použití zobrazení seznamu, nabídce je dostupná v panel hledání v horní části okna portálu.

 ![Otevření sestavy služby SQL Server Reporting Services v správce sestav na panelu hledání](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Podporované klientské aplikace
Při použití "otevřít ve..." Nabídka pro zdroje dat na portálu katalog dat Azure správné klientské aplikaci musí být nainstalovaný v klientském počítači.

| Otevřít v aplikaci | Přípony souboru / protokol | Verze podporovaných aplikací |
| --- | --- | --- |
| Aplikace Excel | ODC | Excel 2010 nebo novější |
| Excel (nahoře 1 000) | ODC | Excel 2010 nebo novější |
| Power Query | XLSX | Excel 2016 nebo Excelu 2010 nebo Excelu 2013 s Power Query pro Excel doplňku nainstalovaný
| Power BI Desktop | .pbix | Power BI Desktop červenec 2016 nebo novější |
| SQL Server Data Tools | vsweb: / / | Visual Studio 2013 aktualizace 4 nebo novější se nainstalovaný SQL Server nástrojů |
| Správce sestav | http:// | V tématu [požadavky na prohlížeč pro SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Data, nástrojů
Možnosti dostupné v nabídce závisí na typu dat materiálů aktuálně vybrané. Samozřejmě, ne všechny možné nástroje bude obsahovat "otevřít ve..." nabídka, ale je stále snadno připojit ke zdroji dat pomocí nástrojů klienta. Vybrán materiálů dat na portálu **Katalog dat Azure** úplný umístění zobrazí v podokně Vlastnosti.

 ![Informace o připojení k tabulce SQL serveru](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Podrobné informace o připojení se liší podle typu zdroje dat pro zdroj dat, ale informace obsažené v portálu vám nabídne všechno, co potřebujete pro připojení ke zdroji dat pomocí nástroje pro všechny klienta. Uživatele můžete zkopírovat podrobnosti o připojení pro zdroje dat, které budou mít zjištěny pomocí **Katalogu dat Azure**, mohli pracovat s daty v jejich nástroj podle výběru.

## <a name="connecting-and-data-source-permissions"></a>Připojení a datové zdroje oprávnění
Ale **Katalog dat Azure** udělá zjistitelný zdroje dat, přístup k datům samotné zůstane pod kontrolou datový zdroj vlastník nebo správce. Objevují ostatní zdroje dat v **Katalogu dat Azure** neposkytuje uživatele všechna oprávnění pro přístup ke zdroji dat, vlastní.

Můžete usnadnit její pro uživatele, kteří zjišťovat zdroje dat, ale nemáte oprávnění pro přístup k jeho data, můžete uživatelům zadejte informace do vlastnost požádat o přístup při přidávání poznámek zdroje dat. Informací, které tady – včetně odkazů na obrázku nebo kontaktní místo k získání přístupu zdroj dat – jsou uvedeny vedle umístění informací o zdroji dat na portálu.

 ![Informace o připojení s žádosti o přístup postupujte podle pokynů](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Souhrn
Registrace zdroje dat s **Katalogem dat Azure** zajišťuje tato data zjistitelný zkopírováním strukturální a popisný metadata ze zdroje dat do služby katalogu. Po zdroje dat obsahuje registrované a zjištění, uživatelé mohli připojovat ke zdroji dat na portálu **Katalog dat Azure** "otevřít ve..." " nabídka nebo nástroje pro jejich data podle výběru.

## <a name="see-also"></a>Viz taky
- [Začínáme s katalogem dat Azure](data-catalog-get-started.md) kurz podrobné informace o připojení ke zdrojům dat
