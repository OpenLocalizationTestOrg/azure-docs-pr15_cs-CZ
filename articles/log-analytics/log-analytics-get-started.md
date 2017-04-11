<properties
    pageTitle="Začínáme s protokolu analýzy | Microsoft Azure"
    description="Nastavení a zprovoznění jde dosáhnout pomocí protokolu analýzy v Microsoft operace správy sady (OMS) v minutách."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Začínáme s protokolu analýzy

Nastavení a zprovoznění jde dosáhnout pomocí protokolu analýzy v Microsoft operace správy sady (OMS) v minutách. Při výběru postup vytvoření OMS pracovního prostoru, který je podobný účtu máte dvě možnosti:

- Web Microsoft operace správy Suite
- Předplatné Microsoft Azure

Můžete vytvořit bezplatné pracovního prostoru OMS pomocí OMS webu. Nebo můžete předplatné Microsoft Azure vytvoření OMS pracovního prostoru. Obě pracovní prostory jsou funkčně ekvivalentní, s tím rozdílem, že volného prostoru OMS můžete odeslat pouze 500 MB dat denně ke službě OMS. Pokud používáte Azure předplatného, můžete také, které předplatné pro přístup k další služby Azure. Bez ohledu na použitou k vytvoření pracovního prostoru metodu vytvoříte pracovního prostoru pomocí účtu Microsoft nebo účet organizace.

Tady je rychlý přehled toho procesu:

![diagram rychlého připojení](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Předpoklady protokolu technologie pro analýzu a nasazení kritéria

- Potřebujete na placené předplatné Microsoft Azure plně použití protokolu analýzy. Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/) , který umožňuje přístup k jiné služby Azure. Nebo můžete vytvořit bezplatný účet OMS na [Sadu operace správy](http://microsoft.com/oms) webu a klikněte na **akci zdarma**.
- Pracovní prostor OMS
- Každý počítač Windows, který chcete shromáždit data od spuštěním Windows Server 2008 SP1 nebo vyšší
- [Brána firewall](log-analytics-proxy-firewall.md) přístup k OMS webové adresy služby
- Server [OMS protokolu analýzy předávání](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (brány) předat přenosy z servery OMS, pokud není k dispozici v počítači přístup k Internetu
- Pokud používáte Operations Manager podporuje protokol analýzy Operations Manager 2012 SP1 UR6 hodnoty i všech a Operations Manager 2012 R2 UR2 a vyšší. Podpora proxy jsme přidali Operations Manager 2012 SP1 UR7 a Operations Manager 2012 R2 UR3. Zjistěte, jak bude integrovaný s OMS.
- Určení, zda počítače přímý přístup k Internetu. Pokud ne, vyžadují server brány přístup k serverům OMS webové služby. Všechny přístup, spočívá ve využití HTTPS.
- Zjistěte, jaký technologií a servery odešle OMS data. Například řadiče domény, SQL Server, atd.
- Udělit oprávnění uživatelům v OMS a Azure.
- Pokud jste starali o využití dat, jednotlivě nasazení všech řešení a otestujte dopad na výkon před přidáním další řešení.
- : Kontrolujte potvrzení použití dat a výkonu při přidávání řešení a funkcí analýzy protokolu. Platí to i kolekce událost protokolu kolekce, shromažďování dat výkonu, atd. Je lepší začínat minimální kolekce do použití dat nebo byl zjištěn vliv na výkon.
- Ověřte, zda Windows agentů nejsou také spravovány Operations Manager, v opačném případě bude duplicitních dat dojít. To se týká rovněž k Azure na základě agenti, které mají Azure diagnostiky povolené.
- Po instalaci agentů Ověřte správnou agent. Pokud ne, zkontrolujte, zkontrolujte, že je kryptografický rozhraní API: Next generování (CNG) klíč izolace není zakázané prostřednictvím zásad skupiny.
- Řešení některých protokolu analýzy mají další požadavky



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Registrace 3 kroků pomocí sadu operace správy

1. Přejděte na web [Operace správy sadu](http://microsoft.com/oms) a klikněte na **akci zdarma**. Přihlaste se pomocí účtu Microsoft, třeba Outlook.com nebo pomocí účtu organizace poskytovanou vaší společnosti nebo vzdělávací instituce pomocí služby Office 365 nebo další služby od Microsoftu.
2. Zadejte název jedinečný pracovního prostoru. Pracovní prostor je kontejner logické kam se ukládají data správy. Poskytuje způsob, jak data oddílu mezi různé týmy ve vaší organizaci, jako jsou data výhradním jeho pracovního prostoru. Zadejte e-mailovou adresu a oblasti, kde chcete mít data uložená.  
    ![Vytvořit pracovní prostor a propojit předplatného](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Můžete pak vytvořit nové předplatné Azure nebo odkaz k existujícímu předplatnému Azure. Pokud chcete pokračovat s bezplatnou zkušební verzi, klikněte na **Ne**.  
  ![Vytvořit pracovní prostor a propojit předplatného](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Jste připravení začít pracovat s portálu operace správy sadu.

Se dozvíte více o nastavení pracovního prostoru a propojení existující Azure účty pracovního prostoru vytvořená pomocí sadu operace správy na [Spravovat přístup k protokolu analýzy](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Registrace rychle pomocí Microsoft Azure

1. Přejděte na [portál Azure](https://portal.azure.com) přihlásit, projděte si seznam služeb a vyberte **Protokolu analýzy (OMS)**.  
    ![Azure portálu](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klikněte na tlačítko **Přidat**a potom vyberte možnosti pro následující položky:
    - Název **OMS pracovního prostoru**
    - **Předplatné** – Pokud máte víc předplatných, vyberte si jednu, který chcete přidružit k nového pracovního prostoru.
    - **Pole Skupina zdroje**
    - **Umístění**
    - **Ceny osy**  
        ![rychlé vytváření](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klikněte na **vytvořit** a zobrazí se podrobnosti pracovního prostoru na portálu Azure.       
    ![Podrobnosti o pracovního prostoru](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Klikněte na odkaz **OMS portál** otevřete webu sadu správy operace s nového pracovního prostoru.

Jste připraveni začít používat portál operace správy sadu.

Je další informace o nastavení pracovního prostoru a propojení existujících pracovních prostorů, které jste vytvořili pomocí sadu operace správy k Azure předplatným na [Spravovat přístup k protokolu analýzy](log-analytics-manage-access.md).

## <a name="get-started-with-the-operations-management-suite-portal"></a>Začínáme s portálem operace správy sady
Zvolte řešení a spojit se servery, které chcete spravovat, klikněte na dlaždici **Nastavení** a postupujte podle kroků v této části.  

![Začínáme](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Přidání řešení** – zobrazení nainstalovaných řešení.  
    ![řešení](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klikněte na Přidat další řešení **najdete v galerii** .  
    ![řešení](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Vyberte řešení a potom klikněte na **Přidat**.
2. **Připojit zdroj** - určit, jak by se připojit k serveru prostředí pro shromažďování dat:
    - Windows Server nebo klienta připojte přímo – nainstalujte agent.
    - Připojte Linux servery s agentem OMS Linux.
    - Použijte účet Azure úložiště s Windows nebo Linux Azure diagnostiky OM koncovku nakonfigurovanou.
    - Umožňuje připojit Správa skupin nebo celé nasazení Operations Manager System Center Operations Manager.
    - Povolte Windows Telemetrie můžete upgradovat analýzy.
        ![propojených zdrojů](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Shromažďování dat** Nakonfigurujte alespoň jeden zdroj dat k načtení dat do pracovního prostoru. Po dokončení klikněte na **Uložit**.    

    ![shromažďování dat](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>V případě potřeby servery přímé připojení k sadě operace Správa instalací agent

Následující příklad ukazuje, jak nainstalovat agenta Windows.

1. Klikněte na dlaždici **Nastavení** , klikněte na kartu **Připojení zdroje** , klikněte na kartu pro typ zdroje, které chcete přidat a buď stažení agent nebo Další informace o tom, jak povolit agent. Například, klepněte na **Stáhnout Agent systému Windows (64bitová verze)**. Pro Windows agenti – můžete nainstalovat pouze zástupce v systému Windows Server 2008 SP 1 nebo novější nebo Windows 7 s aktualizací SP1 nebo novější.
2. Agent nainstalujte na jeden nebo více serverů. Můžete nainstalovat agentů po jednom nebo více automatické metoda s [vlastní skript](log-analytics-windows-agents.md), nebo pomocí existujícího řešení distribuce softwaru, který bude pravděpodobně nutné.
3. Po souhlasíte, podmínky licenční smlouvy a zvolte instalační složky, zaškrtněte políčko **Připojit agenta do Azure protokolu analýzy (OMS)**.   
    ![instalace Agent](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Na další stránce zobrazí se výzva pro pracovní prostor ID a klíčem pracovního prostoru. Pracovní prostor ID a klíčem jsou zobrazeny na obrazovce, které jste stáhli soubor agent.  
    ![Agent klíče](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![připojení serverů](./media/log-analytics-get-started/oms-onboard-key.png)
5. Během instalace můžete klepnutím na tlačítko **Upřesnit** volitelně s nastavením proxy serveru a zadat ověřovací informace. Kliknutím na tlačítko **Další** vrátit na obrazovku informace pracovního prostoru.
6. Klikněte na **Další** ověřte svoje ID pracovní prostor a klíče. Pokud nástroj nalezne chyby, můžete kliknout **zpět** a opravy. Po ověření pracovního prostoru ID a klávesy klikněte na **nainstalovat** a dokončete instalaci agent.
7. V Ovládacích panelech klikněte na Microsoft Agent sledování > karta Azure protokolu analýzy (OMS). Zelená značka zaškrtnutí ikona se zobrazí při agentů komunikovat se službou operace správy sadu. Standardně trvá přibližně 5 až 10 minut.

>[AZURE.NOTE] Řešení kapacita Správa a konfigurace hodnocení aktuálně nepodporuje servery připojené přímo k sadě operace správy.


Taky můžete připojit agent pro System Center operace správce 2012 SP1 nebo novější. Vyberte **Připojit agent pro System Center Operations Manager**. Když zvolíte, že možnost, budete posílat dat ve službě bez nutnosti další hardware a zatížení Správa skupin.

Je další informace o připojení agentů sadě správy operace ve [počítače se systémem Windows připojit k protokolu analýzy](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Volitelně můžete připojte serverů pomocí System Center Operations Manager

1. V konzole Operations Manager vyberte **Správa**.
2. Rozbalte položku **Provozní přehledy** a vyberte **Provozní přehledy připojení**.

  >[AZURE.NOTE] Podle toho, jaké aktualizace zahrnutí části SCOM používáte může se zobrazit uzel pro *System Center Advisor*, *Provozní přehledy*nebo *Operace správy sadu*.

3. Klikněte na odkaz **Registrace provozní interpretace** směrem vpravo nahoře a postupujte podle pokynů.
4. Po dokončení Průvodce registrací, klikněte na odkaz **Přidat/skupinu počítačů** .
5. V dialogovém okně **Počítač hledání** můžete vyhledat počítače nebo skupiny kontroloval Operations Manager. Vyberte počítače nebo skupiny, které integrovaný je do protokolu analýzy, klikněte na **Přidat**a klikněte na tlačítko **OK**. Můžete ověřit, zda službu OMS získává data tak, že přejdete na dlaždici **použití** portálu operace správy sadu. Data by se měly v přibližně 5 až 10 minut.

Další informace o připojení Operations Manager k sadě operace správy při [Připojení Operations Manager do protokolu analýzy](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>V případě potřeby analýze dat ze služby cloudu v Microsoft Azure

Se sadou správy operací můžete rychle najít IIS protokoly pro cloudovými službami a virtuálních počítačích událostí a povolením diagnostiky cloudové služby Azure. Můžete taky zobrazí další přehledy pro Azure virtuálních počítačích když nainstalujete Microsoft Agent sledování. Je další informace o tom, jak nakonfigurovat Azure prostředí pro pomocí sadu správy operace v [úložišti Azure připojit k protokolu analýzy](log-analytics-azure-storage.md).


## <a name="next-steps"></a>Další kroky

- [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md) shromažďování dat a přidejte funkce.
- Seznámení s [protokolu hledání](log-analytics-log-searches.md) zobrazíte podrobné informace shromážděné řešení.
- Uložení a zobrazení vlastní hledání pomocí [řídicích panelů](log-analytics-dashboards.md) .
