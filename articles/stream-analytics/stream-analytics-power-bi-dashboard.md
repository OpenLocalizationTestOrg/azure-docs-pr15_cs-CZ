<properties
    pageTitle="Řídicího panelu Power BI na toku analýzy | Microsoft Azure"
    description="Shromáždění business intelligence a analýza velkých objemů dat z úlohy toku analýzy pomocí v reálném čase streamování řídicího panelu Power BI."
    keywords="řídicí panel analýzy, v reálném čase řídicího panelu"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Technologie pro analýzu a Power BI můžete vysílat datovými proudy: řídicího panelu v reálném čase analytics pro přenos dat

Azure toku analýzy můžete využít jednu z úvodní analytických nástrojů, Microsoft Power BI. Naučte se používat Azure toku analýzy analyzovat velký objem, datových proudů data a získat přehled v řídicím panelu v reálném čase analýzy Power BI.

Pomocí [Microsoft Power BI](https://powerbi.com/) můžete rychle vytvořit živou řídicího panelu. [Podívejte se na video, které ukazují scénáře](https://www.youtube.com/watch?v=SGUpT-a99MA).

V tomto článku se dozvíte, jak vytvořit vlastní nástroje vlastní business intelligence pomocí Power BI jako výstup pro analýzy toku Azure úlohy a využít řídicího panelu v reálném čase.

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Účet Microsoft Azure
* Vstupní pro danou úlohu toku analýzy zpracovávat streamování data z. Technologie pro analýzu toku zadávaná z Azure události rozbočovače nebo objektů Blob Azure úložiště.  
* Pracovní nebo školní účet pro Power BI

## <a name="create-azure-stream-analytics-job"></a>Vytvoření projektu Azure toku analýzy

Z [Portálu klasické Azure](https://manage.windowsazure.com)klikněte na **Nový datových služeb, analýzy toku, vytvořit**.

Zadejte následující hodnoty, klikněte na **vytvořit analýzy toku úlohy**:

* **Název úlohy** - zadejte název projektu. Například **DeviceTemperatures**.
* **Oblast** – vyberte oblast místo, kam chcete projekt nachází. Zvažte umístění úkoly a události centrální ve stejné oblasti, aby se zajistila optimální výkon a účtovány náklady přenos dat mezi oblastmi.
* **Úložiště účet** – zvolit úložiště účet, který chcete používat k ukládání sledování data pro všechny úlohami toku analýzy v této oblasti. Máte možnost zvolit existujícího účtu úložiště nebo vytvořte nový účet.

Klikněte na **Analýza toku** v levém podokně seznamu úloh toku analýzy.

![graphic1][graphic1]

> [AZURE.TIP] Se stavem **Nezahájeno**se zobrazí nový projekt. Všimněte si, že je zakázané v dolní části stránky na tlačítko **Start** . Toto je očekávaná jako musíte nakonfigurovat úlohu vstupní, výstup dotazu, a tak dále před zahájením projektu.

## <a name="specify-job-input"></a>Určení úlohu vstupní

Pro účely tohoto návodu jsme jsou za předpokladu, že používáte rozbočovač událost jako vstup s serializaci JSON a kódování UTF-8.

* Klikněte na název projektu.
* Klikněte na **vstupy** v horní části stránky a potom klikněte na **Přidat vstupní**. Dialogové okno, které otevře vás provede procesem počet návodu pro nastavení vašeho zadání vstupních hodnot.
*   Vyberte **toku dat**a klikněte na tlačítko vpravo.
*   Vyberte **Událost centrální**a klikněte pravým tlačítkem.
*   Zadejte nebo vyberte následující hodnoty na třetí stránce:
  * **Zadávání Alias** – zadejte popisný název pro tuto úlohu vstupní. Všimněte si, že použijete tento název v dotazu později.
  * **Centrální události** - Pokud události rozbočovač jste vytvořili ve stejném předplatném jako toku analýzy úlohu, vyberte obor názvů, který je v centru události.
*   Pokud rozbočovače události se v jiné předplatné, vyberte **Použití centrální událost z jiné předplatné** a ruční zadání informací o ** **Namespace Bus služby**, **Centrální název události**, centrální zásad název události**, **Klíč zásad centrální události**a **Počet oddílů centrální události**.

> [AZURE.NOTE]  V tomto příkladu výchozí počet oddílů, tedy 16.

* **Název události centrální** - vyberte název události centrální Azure máte.
* **Název zásady centrální události** - vyberte událost centrální zásad pro používáte rozbočovač události. Ujistěte se, že má tuto zásadu spravovat oprávnění.
*   **Událost centrální spotř skupiny** – necháte prázdné nebo zadejte skupinu spotř máte na vaše Centrum události. Všimněte si, že každá skupina spotř události rozbočovače může mít jenom 5 čteček najednou. Ano rozhodněte, vpravo spotř skupiny pro danou úlohu příslušným způsobem. Pokud pole necháte prázdné, bude používat jako výchozí skupina příjemce.

*   Klikněte pravým tlačítkem.
*   Zadejte následující hodnoty:
  * **Formát Serializer události** - JSON
  * **Kódování** - UTF8
*   Klikněte na tlačítko Kontrola přidat tento zdroj a pro ověření, že toku analýzy můžete úspěšně se připojit k centru události.

## <a name="add-power-bi-output"></a>Přidání výstupu Power BI

1.  Klikněte na **výstup** v horní části stránky a potom klikněte na **Přidat výstup**. Zobrazí se že Power BI uvedené jako výstup možnost.

    ![graphic2][graphic2]  

2.  Vyberte **Power BI** a potom klikněte na tlačítko vpravo.
3.  Zobrazí se obrazovka takto:

    ![graphic3][graphic3]  

4.  V tomto kroku zadejte pracovní nebo školní účet pro výstup projektu toku analýzy. Pokud už máte účet Power BI, vyberte **Povolit spustit**. V opačném případě vyberte **Přihlásit se**. [Tady je dobré blogu procházení podrobností Power BI registrace](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Další uvidíte obrazovku takto:

    ![graphic4][graphic4]  

Obsahují hodnoty jako níže:

* **Výstup Alias** – vložíte všechny výstup alias snadno můžete odkázat na. Výstup alias je užitečné, pokud se rozhodnete mít víc výstupy pro danou úlohu. V takovém případě budete muset v nápovědě k tento výstup dotazu. Například použijeme výstupní alias hodnota = "OutPbi".
* **Název sady dat** – zadejte název sady dat, která se má Power BI výstup mít. Použijeme například "pbidemo".
*   **Název tabulky** : Zadejte název tabulky v části datovou sadu výstup Power BI. Řekněme, že se název "pbidemo". V současné době výstup Power BI z toku analýzy úlohy může obsahovat pouze jednu tabulku v datovou sadu.
*   **Pracovní prostor** – vyberte pracovního prostoru do vašeho tenanta Power BI, pod kterým se vytvoří datové sady.

>   [AZURE.NOTE] Byste neměli vytvářet explicitně této sadě dat a tabulky ve vašem účtu Power BI. Budou se automaticky vytvoří při zahájení práce toku technologie pro analýzu a zahájení úlohy čerpacího výstup do Power BI. Pokud váš dotaz úlohy nevrací žádné výsledky, datovou sadu a tabulky nevytvoří.

*   Klikněte na **OK**, **Testovat připojení** a teď exportujete konfiguraci dokončení.

>   [AZURE.WARNING] Také mějte na paměti, že pokud Power BI už měli datovou sadu a tabulky se stejným názvem jako jiný, který jste zadali v této úlohy toku analýzy, existující data přepíše.


## <a name="write-query"></a>Zápis dotazu

Přejděte na kartu **dotazu** práce. Napište svůj dotaz výstup má v Power BI. Například může být něco například následující dotaz SQL:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Zahájení práce. Ověření, že vaše Centrum události dostává informace o události a vygeneruje očekávané výsledky dotazu. Pokud váš dotaz výstupy 0 řádky, datovou sadu Power BI a tabulek nevytvoří se automaticky.

## <a name="create-the-dashboard-in-power-bi"></a>Vytvoření řídicího panelu v Power BI

Přejděte na [Powerbi.com](https://powerbi.com) a přihlaste pomocí svého pracovního nebo školního účtu. Pokud úlohy dotazu toku analýzy vypíše výsledek, zobrazí se již vytvořené vaší datovou sadu:

![graphic5][graphic5]

Při vytváření řídicího panelu, přejděte na možnost řídicí panely a vytvoření nové řídicího panelu.

![graphic6][graphic6]

V tomto příkladu jsme budete popisek ho "Ukázka řídicího panelu".

Teď, klikněte na datovou sadu vytvořil práce toku analýzy (pbidemo v našem příkladu aktuální). Přejdete na stránku chcete vytvořit graf horní části tohoto datovou sadu. Je ale příkladem zpráv, které můžete vytvořit:

Vyberte ∑ temp a načasovat pole. Automaticky přejde na hodnotu a os grafu:

![graphic7][graphic7]

Díky tomu automaticky pošle grafu jako níže:

![graphic8][graphic8]

V části hodnota klikněte v seznamu dolů pro temp a zvolte **průměrné** teploty a v diagramu, klepněte na **vizualizaci** a zvolte **spojnicový graf**:

![graphic9][graphic9]

Teď zobrazí se spojnicovým grafem průměrných určitou dobu.  Pomocí možnosti Připnout jako níže, můžete připnout to do řídicího panelu, který jste vytvořili:

![graphic10][graphic10]

Teď kódu řídicího panelu pomocí této připnuté sestavy, zobrazí se sestava aktualizace v reálném čase. Zkuste změnit data v událostech – zásobníku temp nebo něco jako je například a se projeví v reálném čase, který se projeví v řídicím panelu.

Poznámka: Tento kurz prokázáno postup vytvoření ale jeden typ grafu pro datovou sadu. Power BI vám pomůže vytvářet dalších nástrojů business intelligence zákazníka pro vaši organizaci. Jiný příklad řídicího panelu Power BI podívejte se na video [Začínáme s Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Další informace o konfiguraci výstup Power BI a používat Power BI skupiny prohlédněte si [Power BI oddílu](stream-analytics-define-outputs.md#power-bi) [že principy analýzy toku výstupy](stream-analytics-define-outputs.md "výstupy Principy analýzy proudu"). Další užitečné zdroje zobrazíte další informace o vytváření řídicích panelů s Power BI je [řídicího panelu Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Omezení a doporučené postupy

Power BI využívá souběžné a výkon omezení podle níže uvedeného popisu: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Ceny Power BI")

Protože ty Power BI výkopu samotné nejčastěji mají přirozený případů, kde Azure toku analýzy znamená snížení zatížení významné data.
Doporučujeme používat TumblingWindow nebo HoppingWindow ověřit, že data nabízených by být maximálně 1 nabízených/druhé a dotaz výkopu v rámci požadavky na výkon – můžete použít následující rovnice pro výpočet hodnoty a udělte okno aplikace v sekundách:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Jako příklad – Pokud jste vytvořili 1 000 zařízení odesílání dat každý druhý, pracujete Power BI Pro skladové jednotky, který podporuje 1,000,000 řádků/hodiny a chcete získat průměr data podle zařízení na Power BI můžete udělat maximálně push každé 4 sekundy na zařízení (viz níže):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Což znamená, že chcete změnit původní dotazu:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>Aktualizace view PowerBI

Běžné otázky je "Proč není na řídicím panelu Automatické aktualizace v PowerBI?".

K dosažení tohoto v PowerBI využít Q & A a položit otázku například "Maximální hodnoty temp, kde je časové razítko dnes" a připnout této dlaždici do řídicího panelu.

### <a name="renew-authorization"></a>Obnovení se tak mohli ověřovat

Musíte se znovu ověření účtu Power BI, pokud jeho heslo změnila vaše úloha byla vytvořená nebo poslední ověření. Pokud Multi-Factor Authentication (MFA) je nakonfigurovaný na vašeho klienta Azure Active Directory (AAD), budete taky muset obnovovat každých 2 týdnů tak mohli ověřovat Power BI. Příznakem tohoto problému je žádné výstupu projektu a "ověřit uživatele chyba" v protokolech operace:

![graphic12][graphic12]

Podobně pokud úlohy pokusu o spuštění během tokenu vypršela, dojde k chybě a zahájení práce se nezdaří. Chyba bude vypadat podobně jako níže:

![Chyba ověřování PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Tento problém můžete vyřešit, zastavte spuštěná úloha a přejděte do výstupu Power BI.  Klikněte na odkaz "Prodloužit se tak mohli ověřovat" a restartujte práce z zastavit naposledy Chcete-li předejít ztrátě dat.

![Obnovení ověření PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Po povolení aktualizaci s Power BI uvidíte Zelená výstraha v oblasti se tak mohli ověřovat:

![Obnovení ověření PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
