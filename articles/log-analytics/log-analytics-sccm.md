<properties
    pageTitle="Správce konfigurace připojení k protokolu analýzy | Microsoft Azure"
    description="Tento článek popisuje postup připojení Správce konfigurace k protokolu technologie pro analýzu a začněte analýza dat."
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
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Správce konfigurace připojení k protokolu analýzy

Správce konfigurace pro System Center můžete připojit k protokolu analýzy v OMS k datům kolekce zařízení synchronizovat. Díky data z vašeho správce konfigurace nasazení k dispozici v OMS.

Existuje celá řada kroky potřebné k připojení Správce konfigurace k OMS, ke které tady najdete rychlý rundown celkového procesu:

1. Na portálu Správa Azure zaregistrovat Správce konfigurace jako webové aplikace a/nebo rozhraní API webových aplikací a ujistěte se, že máte ID klienta a tajné klíč klienta od registrace ze služby Azure Active Directory. Viz [portál použít k vytvoření aplikace služby Active Directory a služby základní, které mají přístup k prostředkům](../resource-group-create-service-principal-portal.md) podrobné informace o provádění tohoto kroku.
2. V Azure portálu pro správu, [poskytnout přístup OMS Správce konfigurace (registrované web appu)](#provide-configuration-manager-with-permissions-to-oms).
3. Ve Správci konfigurace, [Přidání připojení pomocí Průvodce přidáním OMS připojení](#add-an-oms-connection-to-configuration-manager).
4. Ve Správci konfigurace můžete [Aktualizovat vlastnosti připojení](#update-oms-connection-properties) Pokud tajné klávesu heslo nebo klienta někdy vyprší jejich platnost nebo je ztracené.
5. S informacemi z portálu OMS, [Stáhněte si a nainstalujte Microsoft Agent sledování](#download-and-install-the-agent) v počítači se systémem Správce konfigurace služeb připojení odkazovat role systému webu. Agent odešle data Správce konfigurace OMS.
6. V OMS [import kolekce pomocí Správce konfigurace](#import-collections) jako skupiny počítačů.
7. V OMS zobrazte data ze Správce konfigurace jako [skupiny počítačů](log-analytics-computer-groups.md).

Další informace o připojení k OMS na [synchronizovat data ze Správce konfigurace pro sadu Microsoft operace správy](https://technet.microsoft.com/library/mt757374.aspx)Správce konfigurace.



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Správce konfigurace poskytnout oprávnění k OMS

Tento postup vám portálu pro správu Azure s oprávněními pro přístup k OMS. *Role přispěvatele* konkrétně musí udělit uživatelům ve skupině prostředků. Zároveň, který umožňuje portálu pro správu Azure Správce konfigurace připojení k OMS.

>[AZURE.NOTE] Je nutné zadat oprávnění k OMS pro správce konfigurace. Chybová zpráva v opačném dostanete při použití Průvodce konfigurací ve Správci konfigurace.


1. Otevřete [portál Azure](https://portal.azure.com/) a klikněte na tlačítko **Procházet** > **Protokolu analýzy (OMS)** otevřete zásuvné protokolu analýzy (OMS).  
2. Na zásuvné **Protokolu analýzy (OMS)** klikněte na **Přidat** otevřete zásuvné **OMS pracovního prostoru** .  
  ![OMS zásuvné](./media/log-analytics-sccm/sccm-azure01.png)
3. Na zásuvné **Pracovního prostoru OMS** zadejte tyto informace a klikněte na tlačítko **OK**.
  - **Pracovní prostor OMS**
  - **Předplatné**
  - **Pole Skupina zdroje**
  - **Umístění**
  - **Ceny osy**  
    ![OMS zásuvné](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Výše uvedený příklad vytvoří nové skupiny prostředků. Skupina zdroje se používá jenom poskytnout správce konfigurace oprávnění k pracovnímu prostoru OMS v tomto příkladu.

4. Klikněte na tlačítko **Procházet** > **skupiny zdrojů** otevřete zásuvné **skupiny zdrojů** .
5. V zásuvné **skupiny zdrojů** klikněte na skupina zdroje, který jste vytvořili nad otevřete &lt;název skupiny prostředků&gt; zásuvné nastavení.  
  ![nastavení zásuvné skupina zdroje](./media/log-analytics-sccm/sccm-azure03.png)
6. V &lt;název skupiny prostředků&gt; zásuvné nastavení, klikněte na řízení přístupu (IAM) otevřete &lt;název skupiny prostředků&gt; zásuvné uživatelů.  
  ![zdroje skupiny uživatelů zásuvné](./media/log-analytics-sccm/sccm-azure04.png)  
7. V &lt;název skupiny prostředků&gt; zásuvné uživatele, klikněte na tlačítko **Přidat** otevřete zásuvné **Přidat přístup** .
8. V zásuvné **přístup přidat** klikněte na tlačítko **Vybrat roli**a vyberte role **přispěvatele** .  
  ![Vyberte roli](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klikněte na **Přidat uživatele**, vyberte Správce konfigurace uživatele, klikněte na **Výběr**a klikněte na tlačítko **OK**.  
  ![Přidání uživatelů](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Přidání připojení k OMS Správci konfigurace

K přidání připojení k OMS, musíte mít prostředí Správce konfigurace [služby spojovací bod](https://technet.microsoft.com/library/mt627781.aspx) nakonfigurován pro online režimu.

1. V pracovním prostoru **správy** Správce konfigurace vyberte **OMS spojnice**. Otevře se **Průvodce přidáním OMS připojení**. Vyberte **Další**.

2. Na **hlavní** obrazovce potvrďte, že jste provedli následujících akcí a podrobnosti pro každou položku a potom vyberte **Další**.
  1. Na portálu Správa Azure jste registrované Správce konfigurace jako webové aplikace a/nebo rozhraní API webových aplikací a jestli máte [Kód klienta z zápisu](../active-directory/active-directory-integrating-applications.md).
  2. Na portálu Správa Azure jste si vytvořili tajná klávesu se aplikace pro aplikaci registrovaných v Azure Active Directory.  
  3. Na portálu Správa Azure jste uvedli registrovaných web appu s oprávněními pro přístup k OMS.  
  ![Připojení k OMS průvodce Obecné stránky](./media/log-analytics-sccm/sccm-console-general01.png)

3. Na obrazovce **Azure Active Directory** nakonfiguroval nastavení připojení k OMS poskytnutím **klienta** , **ID klienta** a **Klíč tajná klienta** a potom vyberte **Další**.  
  ![Připojení ke stránce OMS Průvodce Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Pokud úspěšně provést všechny další postupy pak informace na obrazovce **Konfigurace připojení OMS** se automaticky zobrazí na této stránce. Informace o nastavení připojení by se měly pro **Azure předplatného** , **Skupina Azure zdroje** a **Pracovní prostor Správa sadu operace**.  
  ![Připojení ke stránce OMS Průvodce OMS připojení](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. Průvodce připojí ke službě OMS pomocí informace, které jste zadávání. Vyberte kolekce zařízení, které chcete synchronizovat s OMS a potom klikněte na **Přidat**.  
  ![Vyberte kolekce](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Ověřte nastavení připojení na obrazovce **Souhrn** a potom vyberte **Další**. Okno **průběhu** se zobrazí stav připojení a potom by měl **Dokončeno**.

>[AZURE.NOTE] Musíte se připojit OMS na web nejvyšší úroveň v hierarchii vašeho. Pokud připojení OMS samostatného primární web a potom přidáte webu Centrální správa vašeho prostředí, budete muset odstranit a znovu vytvořit připojení OMS nové hierarchie.

Po Správce konfigurace jste připojili ke OMS, můžete přidat nebo odebrat kolekce a zobrazte vlastnosti připojení OMS.

## <a name="update-oms-connection-properties"></a>Aktualizovat vlastnosti připojení OMS

Pokud heslo nebo klient tajná klíč někdy platnost nebo se ztratí, musíte aktualizovat ručně OMS vlastnosti připojení.

1. Ve Správci konfigurace přejděte ke **Cloudovým službám** a potom vyberte **OMS spojnici** a otevře se stránka **OMS vlastnosti připojení** .
2. Na této stránce klikněte na kartu **Azure Active Directory** zobrazíte **klienta** **ID klienta** **klienta tajné klíčové vypršení platnosti**. **Ověřit** **tajná klíč klienta** Pokud vypršela.


## <a name="download-and-install-the-agent"></a>Stažení a instalace agenta

1. Na portálu OMS [stáhnout instalační soubor agent z OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Instalace a konfigurace agenta v počítači se systémem role Správce konfigurace služeb připojení čárky webu systému pomocí jedné z těchto způsobů:
  - [Instalace pomocí instalačního programu agent](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Instalace agenta pomocí příkazového řádku](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Instalace agenta pomocí DSC v Azure automatizaci](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Import kolekce

Když jste přidali do Správce konfigurace připojení k OMS a nainstalovaný agent v počítači se systémem Správce konfigurace služeb připojení bodu role systému webu, dalším krokem je k importu kolekce ze Správce konfigurace v OMS jako skupiny počítačů.

Po povolení použití informací o členství kolekce se načte každé 3 hodiny na aktuálnost kolekce členství. Můžete zakázat použití kdykoli.

1. Na portálu OMS klikněte na **Nastavení**.
2. Klikněte na kartu **Skupiny počítače** a pak klikněte na kartu **SCCM** .
3. Vyberte **Správce konfigurace Import kolekce členství** a klikněte na **Uložit**.  
  ![Skupiny počítačů - SCCM karta](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Zobrazení dat pomocí Správce konfigurace

Po přidání připojení k OMS Správci konfigurace a agent nainstalované v počítači se systémem role Správce konfigurace služeb připojení čárky webu systému, z agenta odesílaly OMS. Správce konfigurace kolekce se v OMS, zobrazují jako [skupiny počítačů](log-analytics-computer-groups.md). Zobrazení skupin na stránce **Správce konfigurace systému** ve skupinovém rámečku **Počítač skupiny** v **dialogovém okně Nastavení**.

Po dokončení importu kolekce, najdete v článku zjištění kolik počítačích s kolekce členství. Můžete taky zobrazit počet kolekce, které jste importovali.

![Počítač skupiny – karta SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Po kliknutí na některý z, otevře se hledání zobrazí všechny importovaného skupin nebo všech počítačů, které patří do jednotlivých skupin. Pomocí [Protokolu vyhledávání](log-analytics-log-searches.md), můžete začít hloubkovou analýzu dat Správce konfigurace systému.

## <a name="next-steps"></a>Další kroky

- Pomocí [Protokolu hledání](log-analytics-log-searches.md) zobrazíte podrobné informace o datech Správce konfigurace systému.
