<properties
   pageTitle="Začínáme s SQL datový sklad hrozbou, že zjišťování"
   description="Jak začít s hrozbou, že zjišťování"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Začínáme s hrozbou, že rozpoznávání

> [AZURE.SELECTOR]
- [Sestavy auditování](sql-data-warehouse-auditing-overview.md)
- [Zjišťování hrozbou, že](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Základní informace

Zjišťování hrozbou, že rozpozná aktivity neobvyklých databáze označující potenciální hrozeb zabezpečení databáze. Zjišťování hrozbou, že je v náhledu a je podporovaný pro SQL datový sklad.

Zjišťování hrozbou, že obsahuje nové vrstvy cenného papíru, která uživatelům umožňuje zjišťování zpráv a odpovídání na potenciální hrozeb při jejich vzniku zadáním výstrah zabezpečení na neobvyklých aktivity. Uživatele můžete prozkoumat podezřelé události pomocí [Azure SQL datový sklad auditování](sql-data-warehouse-auditing-overview.md) a zjistit, pokud budou výsledkem pokus o přístup, porušení nebo využívat dat v datový sklad.
Zjišťování hrozbou, že zjednodušuje na adresu potenciální rizika na datový sklad bez nutnosti zabezpečení expert nebo spravovat pokročilým zabezpečením systémy sledování.

Například hrozbou, že zjišťování rozpozná určité činnosti neobvyklých databáze označující potenciální pokusy vkládání SQL. Vkládání příkazu SQL je taková běžné problémy zabezpečení aplikací Web na Internetu, slouží k útoku založených na datech aplikací. Útočníků využívat aplikace chyby která vloží škodlivým příkazy SQL do polí položku aplikace pro porušení nebo upravovat data v databázi.


## <a name="set-up-threat-detection-for-your-database"></a>Nastavení duplicit hrozbou, že databáze

1. Spusťte portálu Microsoft Azure na [https://portal.azure.com](https://portal.azure.com).

2. Přejděte na zásuvné konfigurace datový sklad SQL, který chcete sledovat. V nastavení zásuvné vyberte **auditování & hrozbou, že zjišťování**.

    ![Navigační podokno][1]

3. Ve skupině **Závislosti vzorců a zjišťování hrozbou, že** zapnutí konfigurace zásuvné **zapnuto** auditování, zobrazování hrozbou, že nastavení vyhledávání.

    ![Navigační podokno][2]

4. Zapnutí rozpoznávání hrozbou, že **zapnuto** .

5. Konfigurace seznamu e-mailů, které se zobrazí upozornění zabezpečení při zjišťování neobvyklých datový sklad činností.

6. Klikněte na **Uložit** v zásuvné konfigurace **auditování & hrozbou, že zjišťování** a uložit nový nebo aktualizovaný auditování ochrany proti novým ohrožením zjišťování zásad.

    ![Navigační podokno][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Prozkoumání neobvyklých datový sklad aktivity po zjištění událost nastavit jako podezřelé

1. Obdržíte e-mailové oznámení při zjišťování neobvyklých databáze činností. <br/>
E-mailu najdete informace o událost podezřelé zabezpečení včetně povahy neobvyklých aktivity, název databáze, název serveru a čas události. Kromě toho najdete informace o možných příčin a doporučené akce zkoumat a zmírnění potenciální hrozbou, že k databázi.<br/>

    ![Navigační podokno][4]

2. V e-mailu klikněte na odkaz **Protokol auditování SQL Azure** , který bude spusťte portálu klasické Azure a zobrazit relevantní záznamy auditování v době podezřelé události.

    ![Navigační podokno][5]

3. Klikněte na záznamy auditování, které chcete zobrazit další podrobnosti o činnostech podezřelé databáze například příkazu SQL IP selhání důvod, proč a klienta.

    ![Navigační podokno][6]

4. Sestavy auditování záznamy zásuvné klikněte na **Otevřít v aplikaci Excel** otevřete předem nakonfigurovaná šablona importovat a spouštění hlubší analýze protokol auditování v době podezřelé události aplikace excel.<br/>
**Poznámka:** V aplikaci Excel 2010 nebo novější je potřeba Power Query a nastavení **Rychle zkombinovat**

    ![Navigační podokno][7]

5. Konfigurace nastavení **Rychle zkombinovat** – na kartě **POWER QUERY** pásu karet, vyberte **Možnosti** zobrazíte dialogové okno Možnosti. Vyberte věnované ochraně osobních údajů a vyberte druhou možnost - "Ignorovat úrovně ochrany osobních údajů a zlepšit výkon":

    ![Navigační podokno][8]

6. Načtení protokolů auditování SQL, aby parametrů v nastavení karta jsou správně nastavené a klikněte na pásu karet "Data" a klikněte na tlačítko "Aktualizovat vše".

    ![Navigační podokno][9]

7. Výsledky se zobrazí v seznamu **Protokolů auditování SQL** , která umožňuje spuštění hlubší analýze neobvyklých aktivit, které nebyly zjištěny a zmírnit dopad událost zabezpečení v aplikaci.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
