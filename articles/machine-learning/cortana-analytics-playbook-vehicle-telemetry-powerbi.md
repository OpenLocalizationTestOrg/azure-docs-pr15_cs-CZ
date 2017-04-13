<properties 
    pageTitle="Pokyny k nastavení vozidla telemetrie analýzy řešení šablonu řídicího panelu PowerBI | Microsoft Azure" 
    description="Použití funkcí Cortana Intelligence získat v reálném čase a prediktivní Další informace o stavu vozidla a řízení zvyky." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Vozidla telemetrie analýzy řešení šablonu řídicího panelu PowerBI pokyny k nastavení

Této **nabídce** odkazy na kapitoly v tomto playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


Řešení vozidla Telemetrie analýzy hodnotí jak dealerům auta, výrobců automobilů a pojištění společnostmi můžete využít možnosti měřítka Cortana získat v reálném čase a prediktivní Další informace o stavu vozidla a řízení zvyky jednotka vylepšení v oblasti zákazníka vyzkoušet, R a D a marketingových kampaní. Tento dokument obsahuje podrobné informace o tom, jak můžete nakonfigurovat PowerBI sestavách a řídicích panelů po nasazení řešení ve vašem předplatném. 


## <a name="prerequisites"></a>Zjistit předpoklady pro
1.  Nasazení řešení vozidla Telemetrie analýzy tak, že přejdete do [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3)  
2.  [Instalace Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3.  [Azure předplatného](https://azure.microsoft.com/pricing/free-trial/). Pokud nemáte předplatné Azure, Začínáme s Azure bezplatného předplatného
4.  Účet Microsoft PowerBI
    

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence sadu součásti
Jako součást šablona řešení vozidla Telemetrie analýzy jsou ve vašem předplatném nasazeny následující Cortana Intelligence služby.

- **Událost rozbočovače** ingesting miliony vozidla telemetrie událostí do Azure.
- **Analytický toku**s pro získání v reálném čase Další informace o stavu vozidla a trvá tato data do dlouhodobé úložiště pro bohatší dávku analýzy.
- **Výukové počítače** ke zjištění odchylky v reálném čase a dávkové zpracování získat prediktivní přehledy.
- **HDInsight** využít k transformaci dat ve velkém měřítku
- **Data Factory** zpracovává průběhu, plánování, Správa zdrojů a sledování kanálu dávkové zpracování.

**Power BI** vám bude radit toto řešení bohaté řídicí panel pro data v reálném čase a prediktivní analýzy vizualizace. 

Řešení používá dvou různých zdrojů dat: **simulovaný vozidla signály a diagnostiky datovou sadu** a **vozidla katalogu**.

Simulator telematika vozidla je součástí tohoto řešení. Posílá diagnostické informace a signalizuje odpovídající stav vozidla a řízení vzorek v daném okamžiku v čase. 

Katalog vozidla je obsahující VIN odkaz datovou sadu mapování modelu


## <a name="powerbi-dashboard-preparation"></a>Příprava PowerBI řídicího panelu

### <a name="deployment"></a>Nasazení

Po dokončení nasazení byste měli vidět v následujícím diagramu spolu s ostatními tyto součásti označené zeleně. 

- Přejděte do příslušné služby k ověření, zda všechny tyto nasadili úspěšně, klikněte na šipku v pravém horním rohu zelené uzlů.
- Pokud si Pokud chcete stáhnout balíček simulator dat, klikněte na šipku u horní doprava na uzel **Vozidla telematika Simulator** . Uložte a extrahujte soubory místně ve vašem počítači. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Teď jste připraveni na řídicím panelu PowerBI nakonfigurovat propracovaných vizualizacích získat v reálném čase a zvyky prediktivní Další informace o stavu vozidla a řízení. Trvá asi 45 minut za hodinu můžete vytvořit v sestavách a nakonfigurovat řídicího panelu. 


### <a name="setup-power-bi-real-time-dashboard"></a>Nastavení data v reálném řídicího panelu Power BI

**Generování simulovaný dat**

1. V místním počítači přejděte do složky, které jste extrahovali balíček Simulator telematika vozidla![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Spusťte aplikaci ***CarEventGenerator.exe***.
3.  Posílá diagnostické informace a signalizuje odpovídající stav vozidla a řízení vzorek v daném okamžiku v čase. To je publikován k centrální události Azure instanci nakonfigurovaný jako součást nasazení.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Spuštění aplikace v reálném čase řídicího panelu**

Řešení obsahuje aplikace generovaný řídicího panelu v reálném čase v PowerBI. Tato aplikace okolním instance centrální události, odkud toku analýzy publikuje události nepřetržitě. Pro každý událost, která bude tato aplikace zpracuje dat pomocí počítače výukové žádost o odpovědi na bodování koncového bodu. Výsledné datovou sadu je publikován na PowerBI push rozhraní API pro vizualizaci. 

Stažení aplikace:

1.  Klikněte na uzel PowerBI v zobrazení diagramu a klikněte na **Stáhnout aplikaci řídicího panelu v reálném čase**"odkaz v podokně Vlastnosti.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Extrahování a uložte aplikaci místně![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Spuštění aplikace **RealtimeDashboardApp.exe**
4.  Poskytnutí platných přihlašovacích údajů Power BI, přihlaste se a klikněte na **přijmout**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>Konfigurace PowerBI sestav
Data v reálném sestavy a řídicí panel trvat dokončete asi 30 45 minut. Přejděte na [http://powerbi.com](http://powerbi.com) a přihlášení.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

V Power BI je generováno nové datové sady. Klikněte na datovou sadu **ConnectedCarsRealtime** .

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Uložení prázdná sestava stisknutím **kláves Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Zadejte název sestavy *vozidla Telemetrie analýzy v reálném čase - sestavy*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Data v reálném sestavy
Existují tři sestavy v reálném čase v tomto řešení:

1.  Vozidla operace
2.  Vozidla údržby
3.  Statistiky stavu vozidla

Můžete nakonfigurovat tři v reálném čase sestavy nebo zastavení po všechny fáze a pokračujte v další části Konfigurace dávku sestav. Doporučujeme, abyste je možné vytvořit všechny tři sestavy vizualizovat úplné přehledy v reálném čase cestu řešení.  

### <a name="1-vehicles-in-operation"></a>1. operace vozidla
  
Poklikejte na **stránce 1** a přejmenujte ho na "Vozidla operace"  
    ![Připojení auta – vozidla operace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Vyberte pole **vin** z **polí** a zvolte typ vizualizace jako **"Karta"**.  

Jak ukazuje obrázek, vytvoří se vizualizace na kartách.  
    ![Připojení auta – vyberte vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci.  

Vyberte pole **Město** a **vin** z polí. Změna vizualizace **"Mapu"**. Přetáhněte **vin** v oblasti hodnot. Přetáhněte **města** z polí do oblasti **legendy** .   
    ![Připojení auta – vizualizace na kartách](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Vyberte **Formát** část **vizualizace**, klikněte na **název** a změnit **Text** na **"Vozidla operace podle města"**.  
    ![Připojení auta – vozidla operace podle města](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Poslední vizualizace vypadá, jak je vidět na obrázku.    
    ![Připojení auta – konečný vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci.  

Vyberte pole **Město** a **vin**, změňte typ vizualizace na **Skupinový sloupcový graf**. Zajistit, aby pole **Město** v **oblasti osa** a **vin** v **oblasti hodnoty**  

Řazení grafu tak, že **"Počet vin"**  
    ![Připojení auta – počet vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Změnit **název** grafu na **"Vozidla operace podle města"**  

Klikněte v části **Formát** a potom vyberte **Barvy dat**, klikněte možnosti **"Na"** **Zobrazit vše**  
    ![Připojení auta – zobrazit všechna Data barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Změna barvy jednotlivých město kliknutím na ikonu barvy.  
    ![Připojení auta – Změna barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klikněte do prázdné oblasti přidáte novou vizualizaci.  

Vyberte vizualizaci **Skupinový sloupcový graf** z vizualizace, přetáhněte pole **Město** v oblasti **Osa** **modelu** v oblasti **legendy** a **vin** v oblasti **hodnoty** .  
    ![Připojení auta – skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Připojení auta – vykreslování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Změna uspořádání všechny vizualizace na této stránce, jak je vidět na obrázku.  
    ![Připojení auta – vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Úspěšně jste nakonfigurovali v reálném čase sestava "Vozidla operace". Můžete přejít k vytvoření další data v reálném sestavy nebo ukončit a konfigurace řídicího panelu. 

### <a name="2-vehicles-requiring-maintenance"></a>2. vozidla údržby
  
Klikněte na ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) přidat nové sestavy, přejmenujte ho na **"vozidla vyžadující Údržba"**

![Připojení auta – vozidla údržby](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Vyberte pole **vin** a změňte typ vizualizace na **karty**.  
    ![Připojení auta – vizualizace na kartách Vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Máme pole s názvem "MaintenanceLabel" v datové sadě. Toto pole může mít hodnotu "0" nebo "1"." Je nastaven modelem Azure počítače výukové zřízení jako součást řešení a integrovaný s v reálném čase cestu. Hodnotu "1" označuje, že vozidla vyžaduje údržbu. 

Přidání filtr **Úrovni stránky** zobrazující vozidla data, která jsou vyžadující údržbu: 

1. Přetáhněte pole **"MaintenanceLabel"** do **Stránky úroveň filtry**.  
![Připojení auta – úrovni stránky filtry](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. V nabídce **Základní filtrování** prezentovat v dolní části MaintenanceLabel stránky úroveň filtr.  
![Připojení auta – základní filtrování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Nastavte jeho hodnotu filtr na **"1"**    
![Připojení automobilů - hodnota filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Klikněte do prázdné oblasti přidáte novou vizualizaci.  

**Skupinový sloupcový graf** vybírat vizualizace  
![Připojení auta – vizualizace na kartách Vind](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Připojení auta – skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Pole **modelu** přetáhněte do oblasti **Osa** , **Vin** do oblasti **hodnoty** . Seřaďte vizualizace podle **počtu vin**.  Změnit **název** grafu na **"Vozidla údržby modelem"**  

Přetáhněte pole **vin** do **Sytost** prezentovat na **pole** ![pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) část **vizualizace** karta  
![Připojení auta – sytostí barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Změna **Barev Data** ve vizualizacích z oddílu **Formát**  
Změnit barvu Minimum, kterou chcete: **F2C812**  
Změna velikosti barvu, kterou chcete: **FF6300**  
![Připojení auta – změny barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Připojení auta – nové barvy vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klikněte do prázdné oblasti přidáte novou vizualizaci.  

**Skupinový sloupcový graf** vybírat vizualizace, přetáhněte **vin** pole do oblasti **hodnoty** , přetáhněte pole **Město** do oblasti **Osa** . Řazení grafu tak, že **"Počet vin"**. Změnit **název** grafu na **"Vozidla údržby podle města"**   
![Připojení auta – vozidla údržby podle města](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klikněte do prázdné oblasti přidáte novou vizualizaci.  

Vyberte vizualizace na **Kartách více řádků** z vizualizace, přetáhněte do oblasti **pole** **modelu** a **vin** .  
![Připojení auta – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Změna uspořádání všechny vizualizace, závěrečná zpráva vypadá takto:  
![Připojení auta – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidla vyžadující Údržba". Můžete přejít k vytvoření další data v reálném sestavy nebo ukončit a konfigurace řídicího panelu. 

### <a name="3-vehicles-health-statistics"></a>3. statistiky stavu vozidla
  
Klikněte na ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) přidat nové sestavy, přejmenujte ho na **"vozidla stavu** statistiky"  

Vyberte vizualizaci **Obrys** vizualizace a pak **rychlost** pole přetáhnout do oblasti **hodnoty, minimální hodnota, maximální hodnota** .  
![Připojení auta – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Změna výchozí agregované hodnoty v **rychlosti** v **oblasti hodnot** na **průměr** 

Změna výchozí agregované hodnoty v **rychlosti** v **oblasti minimální** **Minimum**

Změna výchozí agregované hodnoty v oblasti **maximální** **rychlost** na **Maximum**

![Připojení auta – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Přejmenování **Sloupce název** **"Průměrná rychlost"** 
 
![Připojení auta – sloupce](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klikněte do prázdné oblasti přidáte novou vizualizaci.  

Podobně přidáte **Obrys** **oil průměr engine**, **průměr palivu**a **Průměrná engine mírného**.  

Změna výchozí agregované hodnoty v polí v jednotlivých obrys jako za výše uvedených kroků v **"Průměrná rychlost"** odhadnout.

![Připojení auta – měřítka](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci.

Vyberte **spojnicový a skupinový sloupcový graf** z vizualizace a pak přetáhněte pole **Město** do **Sdílené osy**, přetáhněte **rychlost** **tirepressure a engineoil pole** do oblasti **Hodnoty sloupců** , změnit jeho typ agregace na **průměr**. 

Přetáhněte pole **engineTemperature** do oblasti **Hodnoty čáry** , změňte typ agregace **průměr**. 

![Připojení auta – vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Změňte **název** grafu na **"Průměrná rychlost, můžete zadat tlaku, engine oil a engine teplotní"**.  

![Připojení auta – vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci.

Vyberte vizualizaci **Stromová mapa** vizualizace, přetáhněte pole **modelu** do oblasti **skupiny** a přetáhněte pole **MaintenanceProbability** do oblasti **hodnoty** .

Změňte **název** grafu na **"Vozidla modely údržby"**.

![Připojení auta – Změna názvu grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci.

Vyberte vizualizaci **100 % skládaný pruhový graf** , přetáhněte pole **Město** do oblasti **Osa** a přetáhněte **MaintenanceProbability** **RecallProbability** pole do oblasti **hodnoty** .

![Připojení auta – přidat novou vizualizaci](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klikněte na **Formát**, vyberte **Data barvy**a nastavte barvu **MaintenanceProbability** hodnotu **"F2C80F"**.

Změna **názvu** grafu na **"pravděpodobnosti z vozidla údržbu & odvolat podle města"**.

![Připojení auta – přidat novou vizualizaci](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci.

Vyberte vizualizaci z vizualizace **Plošný graf** , přetáhněte pole **modelu** do oblasti **OS** a přetáhněte pole **engineOil tirepressure, rychlosti a MaintenanceProbability** do oblasti **hodnoty** . Změňte jejich typ agregace na **"Průměr"**. 

![Připojení auta – změna agregace typu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Změna názvu grafu **"Průměrná engine oil**, přestanou tlaku a rychlosti údržbu pravděpodobnost modelem".

![Připojení auta – Změna názvu grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klikněte do prázdné oblasti přidáte novou vizualizaci:

1. Vyberte vizualizaci **Bodovém grafu** vizualizace.
2. Přetáhněte pole **modelu** do oblasti **Podrobnosti** a **legendy** .
3. Přetáhněte pole **paliva** do oblasti **osy x** , změňte agregace **průměr**.
4. Přetáhněte **engineTemparature** do **oblasti y**, přejděte agregace **průměr**
5. Přetáhněte pole **vin** do oblasti **velikost** .


![Připojení auta – přidat novou vizualizaci](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Změňte **název** grafu na **"Průměry paliva, Engine teplota modelem"**.

![Připojení auta – Změna názvu grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Konečnou sestavu bude vypadat takto:

![Připojení automobilů výsledné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Vizualizace PIN kód ze zprávy do řídicího panelu v reálném čase
  
Vytvoření řídicího panelu prázdné kliknutím na ikonu plus vedle řídicí panely. Pojmenujte ho "Telemetrie analýzy přístrojový panel vozidla"

![Řídicí panel připojeného auta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Připnete vizualizaci z výše uvedených sestav na řídicím panelu. 
 
![Řídicí panel připojeného auta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Řídicí panel by měl vypadat takto při vytváření tři sestavy a odpovídající vizualizace jsou připnuté na řídicí panel. Pokud jste nevytvořili všechny sestavy, může vypadat jinak řídicího panelu. 

![Řídicí panel připojeného auta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Blahopřejeme! Úspěšně jste vytvořili v reálném čase řídicího panelu. Při dalším provést CarEventGenerator.exe a RealtimeDashboardApp.exe byste měli vidět živé aktualizace na řídicím panelu. By měl trvá asi 10 až 15 minut proveďte následující kroky.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Nastavení řídicího panelu Power BI dávkové zpracování

>[AZURE.NOTE] Konce dávkové zpracování profilace dokončíte spuštění a zpracovat rok jmění vygenerovaných dat trvá asi dvě hodiny (z úspěšné dokončení nasazení). Takže čekající na zpracování dokončíte před pokračováním dalším krokům. 

**Stáhněte si soubor návrháře PowerBI**

-   Soubor návrháře předkonfigurovaná PowerBI je součástí nasazení
-   Klikněte na uzel PowerBI v zobrazení diagramu a klikněte na **Stáhnout soubor návrháře PowerBI** odkaz v podokně Vlastnosti![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Uložit místně

**Konfigurace PowerBI sestav**

-   Otevřete soubor návrháře "VehicleTelemetryAnalytics - Desktop Report.pbix" PowerBI desktopové aplikaci. Pokud už nemáte, nainstalujte plochy PowerBI z [plochy PowerBI nainstalovat](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Klikněte na **Upravit dotazů**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Poklepejte na **zdroje**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Aktualizujte Azure SQL serveru, na kterém máte zřízení jako součást nasazení serveru připojovací řetězec. Klikněte na uzel Azure SQL v diagramu a zobrazit název serveru v podokně Vlastnosti.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Nechte **databáze** jako *connectedcar*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Klikněte na **OK**.
- Zobrazí se **přihlašovací údaje Windows** vybrána karta tak, že výchozí, změňte do **databáze pověření** kliknutím na kartu **databáze** vpravo.
- Zadejte **uživatelské jméno** a **heslo** zadané při jeho vzhled nasazení databáze SQL Azure.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Klikněte na tlačítko **Připojit**
- Opakujte výše uvedené kroky pro každý tři zbývající dotaz prezentace v pravém podokně a potom aktualizujte podrobnosti připojení zdroje dat.
- Klikněte na **Zavřít a načíst**. Power BI Desktop soubor datové sady připojeni k tabulky databáze SQL Azure.
- **Zavřít** Power BI Desktop soubor.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Klikněte na tlačítko **Uložit** uložte změny. 
 
Teď jste nakonfigurovali všechny sestavy odpovídající cestu dávkové zpracování v řešení. 


## <a name="upload-to-powerbicom"></a>Odeslání do *powerbi.com*
 
1.  Přejděte na portál PowerBI web na http://powerbi.com a přihlásíte.
2.  Klikněte na **načíst Data**  
3.  Nahrání souboru Power BI Desktop.  
4.  Pokud chcete odeslat, klikněte na **-načíst Data > načíst soubory -> místní soubor**  
5.  Přejděte na **"VehicleTelemetryAnalytics – plochy Report.pbix"**  
6.  Po nahrání souboru budete přesměrováni zpět na váš pracovní prostor Power BI.  

Datovou sadu, sestavy a prázdné řídicích panelů se vytvoří za vás.  
 

Připnutí grafy na existující řídicí panel **Vozidla Telemetrie analýzy řídicího panelu** v **Power BI**. Klikněte na řídicím panelu prázdné vytvořili výše a potom kliknutím na oddíl **sestavy** nově nahraný sestavy.  

![PowerBI.com Telemetrie vozidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Poznámka: Sestava obsahuje šest stránek:**  
Stránka 1: Vozidla hustoty  
Stránka 2: V reálném čase vozidla stavu  
Stránka 3: Řízeného agresivní vozidla   
Stránka 4: Připomenout vozidla  
5 stránky: Efektivní řízený úsilím vozidla paliva  
6 stránky: Contoso Logo  

![PowerBI.com připojeného auta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Z stránka 3**pin následující:  

1.  Počet VIN  
    ![PowerBI.com připojeného auta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Agresivní řízený úsilím vozidla modelem – kaskádového grafu  
    ![Vozidla Telemetrie - Pin grafy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Ze stránky 5**pin následující: 
 
1.  Počet vin    
    ![Vozidla Telemetrie - Pin grafy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Paliva efektivně vozidla modelem: Skupinový sloupcový graf  
    ![Vozidla Telemetrie - Pin grafy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Ze stránky 4**pin následující:  

1.  Počet vin  
    ![Vozidla Telemetrie - Pin grafy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Vytváření modelů stažených vozidla podle města,: stromová mapa  
    ![Vozidla Telemetrie - Pin grafy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Ze stránky 6**pin následující:  

1.  Logo contoso motory  
    ![Vozidla Telemetrie - Pin grafy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Uspořádání řídicího panelu**  

1.  Přejděte do řídicího panelu
2.  Najeďte myší na každý graf a přejmenovat, který ho podle názvů podle na následujícím obrázku dokončení řídicího panelu. Také pohyb grafy vypadat na řídicím panelu.  
    ![Vozidla Telemetrie - uspořádání řídicího panelu 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![PowerBI.com Telemetrie vozidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Pokud jste vytvořili v sestavách uvedené v tomto dokumentu, by měl vypadat konečný dokončeným řídícím panelem jako na následujícím obrázku. 

![Vozidla Telemetrie - uspořádání řídicího panelu 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Blahopřejeme! Úspěšně jste vytvořili v sestavách a řídicích panelů můžete získat v reálném čase, prediktivní a dávkové Další informace o stavu vozidla a řízení zvyky.  
