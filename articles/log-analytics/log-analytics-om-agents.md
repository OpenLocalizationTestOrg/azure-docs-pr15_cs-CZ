<properties
    pageTitle="Připojení k protokolu analýzy Operations Manager | Microsoft Azure"
    description="Zachovat stávající investic do System Center Operations Manager a používat rozšířené možnosti s protokolu analýzy, můžete integrovat Operations Manager s OMS pracovního prostoru."
    services="log-analytics"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="magoedte"/>

# <a name="connect-operations-manager-to-log-analytics"></a>Připojení k protokolu analýzy Operations Manager

Zachovat stávající investic do System Center Operations Manager a používat rozšířené možnosti s protokolu analýzy, můžete integrovat Operations Manager s OMS pracovního prostoru.  Díky tomu, že využít možnosti OMS nadále používat Operations Manager do:

- Pokračujte, sledování stavu služeb IT s Operations Manager
- Udržovat integrace s ITSM řešení podpůrné problémů a vyřešení incidentu správy
- Správa životního cyklu agentů používaný pro místní organizaci a veřejné cloudu IaaS virtuálních počítačích sledující s Operations Manager

Integrace s System Center Operations Manager přidá hodnotu strategie operace služby využívá rychlosti a efektivitu OMS shromažďujete, ukládání a analýza dat z Operations Manager.  OMS pomáhá sladit a pracují identifikuje chyby problémy a zobrazení reoccurrences o existující proces správy problém.   Možnost vyhledávací web o průzkum výkonu, událostí a upozornění s formátovaným řídicích panelů a možnosti vytváření sestav vystavit tato data přehledně, ukazuje, které přináší OMS v complimenting Operations Manager zkontrolujte sílu.

Agentů vykazování do skupiny Správa Operations Manager shromáždit data od serverech podle protokolu analýzy zdrojů dat a řešení, které jste zapnuli automatický přístup ve vašem předplatném OMS.  V závislosti na řešení jste povolili, data z těchto řešení buď odeslány přímo ze serveru správy Operations Manager webové službě OMS nebo z důvodu objemu dat shromážděných systému agent Správa přístupových práv odeslány přímo z agenta OMS webové služby. Server pro správu přímo předá OMS data webové služby OMS, nikdy zapsané OperationsManager nebo OperationsManagerDW databázi.  Když na serveru správy dojde ke ztrátě připojení k webové službě OMS, ukládá data místně, dokud je znovu založení s OMS komunikace.  Pokud server management server je offline kvůli plánované údržby nebo neplánované výpadku, jiné server pro správu ve skupině Správa obnovit připojení protokolem OMS.  

Následující obrázek znázorňuje připojení mezi servery správy a ve skupině Správa System Center Operations Manager a OMS, včetně směru a porty.   

![OMS operace – správce –-schéma integrace](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## <a name="system-requirements"></a>Požadavky na systém
Než začnete, zkontrolujte následující podrobnosti ověřil, jestli že splňujete potřebné předpoklady.

- OMS podporuje pouze Operations Manager 2012 SP1 UR6 a novějším a Operations Manager 2012 R2 UR2 a vyšší.  Podpora proxy jsme přidali Operations Manager 2012 SP1 UR7 a Operations Manager 2012 R2 UR3.
- Všechny agentů Operations Manager musí vyhovovat požadavkům minimální podpory. Zajistěte, aby agenti jsou až minimální aktualizace, jinak se nepovede přenosy agent Windows a mnoho chyb mohou vyplnit v protokolu událostí Operations Manager.
- Předplatné OMS.  Další informace prohlédněte si [začít pracovat s protokolu analýzy](log-analytics-get-started.md).

## <a name="connecting-operations-manager-to-oms"></a>Připojení k OMS Operations Manager
Proveďte následující řadu kroků pro nastavení vaší skupině Operations Manager Správa připojení k jedné z vašich pracovních prostorů OMS.

1. V konzole Operations Manager vyberte **Správa** pracovního prostoru.
2. Rozbalte položku operace správy sadu a klikněte na **připojení**.
3. Klikněte na odkaz **Registrace operace správy sadě** .
4. Na **operace správy sadu rychlého připojení průvodce: ověřování** stránky, zadejte e-mailovou adresu nebo telefonní číslo a heslo účtu správce, který je spojený s vaším předplatným OMS a klikněte na **přihlásit**.
5. Když máte úspěšně oprávnění, na **operace správy sadu rychlého připojení průvodce: Vyberte pracovního prostoru** stránka, zobrazí se výzva k výběru OMS pracovního prostoru.  Pokud máte víc než jeden pracovní prostor, vyberte pracovní prostor, který chcete zaregistrovat ke skupině Operations Manager správy z rozevíracího seznamu a klikněte na tlačítko **Další**.

    >[AZURE.NOTE] Operations Manager podporuje pouze jeden pracovní prostor OMS najednou. Připojení a počítačů, které byly do OMS registrovaný u předchozí pracovního prostoru budou odebrány z OMS.

6. Na **operace správy sadu rychlého připojení průvodce: shrnutí** stránky, zkontrolujte si nastavení a pokud jsou správné, klikněte na **vytvořit**.
7. Na **operace správy sadu rychlého připojení průvodce: dokončení** stránky, klikněte na tlačítko **Zavřít**.

### <a name="add-agent-managed-computers"></a>Přidání agent Správa přístupových práv počítačů
Po konfiguraci integrace s OMS pracovního prostoru, to jenom naváže připojení k OMS, se údaje z agentů vykazování do skupiny správy. K tomu nebude dojít až po dokončení konfigurace, které konkrétní počítače agent Správa přístupových práv shromáždí data pro analýzu protokolu. Můžete vybrat objekty počítače jednotlivě nebo vyberte skupinu, která obsahuje objekty počítače Windows. Nelze vybrat skupinu, která obsahuje instance jiné třídy, například logické disků nebo SQL databáze.

1. Spusťte konzolu Operations Manager a vyberte příslušný pracovní prostor **Správa** .
2. Rozbalte položku operace správy sadu a klikněte na **připojení**.
3. V části Akce klikněte na odkaz **Přidat/skupinu počítačů** nadpis v pravé části podokna.
4. V dialogovém okně **Počítač hledání** můžete vyhledat počítače nebo skupiny kontroloval Operations Manager. Vybrat počítače nebo skupiny, které integrovaný OMS, klikněte na tlačítko **Přidat**a potom klikněte na **OK**.

Můžete zobrazit počítačů a skupin nakonfigurované tak, aby shromáždit data od uzel spravovaných počítače ve skupinovém rámečku operace správy sady v pracovním prostoru **správy** konzoly operace.  Tady můžete přidat nebo odebrat počítačů a skupin podle potřeby.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Konfigurace nastavení proxy OMS v konzole operace
Pokud je vnitřní proxy serveru mezi Správa skupiny a OMS webové služby, proveďte následující kroky.  Tato nastavení jsou centrálně spravovaných ze skupiny správy a úměrně agent Správa přístupových práv systémů, které jsou součástí obor sběr dat pro OMS.  Toto je užitečné pro při určitých řešení vyhnout server pro správu a odešlete data přímo OMS webové služby.

1. Spusťte konzolu Operations Manager a vyberte příslušný pracovní prostor **Správa** .
2. Rozbalte sadu správy operace a potom klikněte na **připojení**.
3. V zobrazení OMS připojení klikněte na **Nastavení Proxy serveru**.
4. Na **operace správy sadu průvodce: Proxy Server** stránek, vyberte **použít proxy server pro přístup k sadě správy operace**, pak zadejte adresu URL s číslo portu, například http://corpproxy:80 a klikněte na tlačítko **Dokončit**.

Pokud proxy server vyžaduje ověření, proveďte následující kroky konfigurace nastavení, které je potřeba šířit na spravovaných počítačů, které ohlásí OMS ve skupině Správa a přihlašovací údaje.

1. Spusťte konzolu Operations Manager a vyberte příslušný pracovní prostor **Správa** .
2. V části **RunAs konfigurace**vyberte **profily**.
3. Otevření profilu **Systém Centrum Advisor spustit jako profil proxy serveru** .
4. Spustit jako Průvodce profily klikněte na Přidat používat účet Spustit jako. Můžete vytvořit nový [účet Spustit jako](https://technet.microsoft.com/library/hh321655.aspx) nebo použít existující účet. Tento účet musí mít dostatečná oprávnění projít proxy serveru.
5. Pokud chcete nastavit účet, který chcete spravovat, zvolte **vybrané třídy, skupině nebo objekt**, klikněte na **příkaz Select...** a potom klikněte na **Skupina...** Chcete-li otevřít **Skupiny vyhledávacího** pole.
6. Vyhledejte a vyberte **Microsoft systém Centrum Advisor sledování serveru skupinu**.  Po výběru skupiny zavřete **Skupiny vyhledávacím** polem klikněte na tlačítko **OK** .
7.  Klikněte na **OK** zavřete dialogové okno **Přidat účet Spustit jako** .
8.  Klikněte na **Uložit** uložte provedené změny a ukončete průvodce.

Po vytvoření připojení a konfigurace které agentů bude shromažďování a vykazování dat OMS, následující konfigurace se použije ve skupině Správa ne nutně v pořadí:

- Vytvoří se účet Spustit **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** .  Je přidružená k spustit jako profilu **Microsoft systém Centrum Advisor spustit jako profil objektů Blob** a zacílení dvě třídy - **Server shromažďování** a **Operace správce správy skupiny**.
- Dva konektory vzniká.  První jmenuje **Microsoft.SystemCenter.Advisor.DataConnector** a automaticky nakonfigurována předplatné, které bude předávat všechna upozornění generovaných instance všechny tříd ve skupině Správa na OMS protokolu analýzy. Druhé spojnice je **Advisor spojnice**, což je zodpovědný za komunikaci s OMS webové služby a sdílení dat.
- Agentů a skupiny, které jste vybrali sběr dat ve skupině Správa se přidá do **Microsoft systém Centrum Advisor sledování skupiny pro Server**.

## <a name="management-pack-updates"></a>Aktualizace Management pack
Po dokončení konfigurace skupiny Správa Operations Manager naváže připojení ke službě OMS.  Server pro správu synchronizovat síť se službou webové služby a dostávat informace o konfiguraci aktualizované ve formě management Pack pro řešení, které jste zapnuli automatický přístup integrovaných s Operations Manager.   Operations Manager bude kontrola aktualizací pro tyto management Pack automaticky stahovaly a naimportovat, pokud jsou k dispozici.  Existují dvě pravidla zejména kterého ovládacího prvku toto chování:

- **Microsoft.SystemCenter.Advisor.MPUpdate** - aktualizuje základní management Pack OMS. Ve výchozím nastavení se spustí každých hodin Dvanáctihodinové (12).
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** – aktualizace řešení management Pack povolené v pracovním prostoru. Ve výchozím nastavení se spouští každých minut pěti (5).

Je možné přepsat tyto dvě pravidla zabránit automatické stahování zakázáním je, nebo změnit frekvenci pro intervalu server pro správu synchronizuje s OMS zjistit, zda nové management pack je k dispozici a mají stahovat.  Postupujte podle pokynů [jak přepsat pravidla nebo Monitor](https://technet.microsoft.com/library/hh212869.aspx) změnit **frekvenci** parametr s hodnotou v sekundách změnit plán synchronizace nebo změnit parametr **povoleno** zakázat pravidla.  Cíl přepsání všech objektů třídy operace správce správy skupiny.

Pokud chcete pokračovat po stávajícího procesu změnit ovládací prvek pro řízení management pack vydáních ve skupině Správa výrobních, můžete zakázat pravidla a povolit v určité době, kdy jsou povoleny aktualizace. Pokud máte ve vašem prostředí vývoj nebo q & a Správa skupiny a má připojení k Internetu, můžete nakonfigurovat danou skupinu správy s OMS pracovní prostor pro podporu tento scénář.  To vám umožní dělat revize vyhodnocení iterativních verzích management Pack OMS před uvolněním do skupiny Správa výrobních.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Přepněte do nového pracovního prostoru OMS skupině pro Operations Manager
1. Přihlaste se k svému předplatnému OMS a vytvoření nového pracovního prostoru v [Sadě Microsoft operace správy počítače](http://oms.microsoft.com/).
2. Spusťte konzolu Operations Manager pomocí účtu, který je členem role správců operace správce a vyberte příslušný pracovní prostor **Správa** .
3. Rozbalte sadu správy operace a vyberte **připojení**.
4. Vyberte odkaz **Znovu konfigurovat sadu správy operace** na straně prostřední části podokna.
5. Postupujte podle pokynů **Průvodce operace správy sadu rychlého připojení** a zadejte e-mailovou adresu nebo telefonní číslo a heslo účtu správce, který souvisí s nový pracovní prostor OMS.

    > [AZURE.NOTE] **Operace správy sadu rychlého připojení průvodce: Vyberte pracovního prostoru** stránky bude prezentovat existující pracovní prostor, který používá.


## <a name="validate-operations-manager-integration-with-oms"></a>Ověření Operations Manager integrace s OMS
Existuje několik způsobů, jak můžete ověřit, že OMS pro Operations Manager integrace byla úspěšně dokončena.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Potvrďte integrace z portálu OMS

1.  Na portálu OMS klikněte na dlaždici **Nastavení**
2.  Vyberte **připojení zdroje**.
3.  V tabulce v části systém Center Operations Manager byste měli vidět na název skupiny správy společně s počet agentů a stav po poslední přijetí data.

    ![OMS nastavení connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

4.  Poznamenejte si **ID pracovního prostoru** hodnotu v levé části na stránce nastavení.  Bude jeho ověřením proti skupiny Správa Operations Manager dole.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Potvrďte integrace z konzoly operace

1.  Spusťte konzolu Operations Manager a vyberte příslušný pracovní prostor **Správa** .
2.  Vyberte **Balíčky správy** a v **vyhledejte:** textového pole zadejte **Advisor** nebo **měřítka**.
3.  V závislosti na řešení, které jste zapnuli automatický přístup zobrazí se odpovídající management pack uvedené v seznamu výsledků hledání.  Například pokud jste povolili řešení upozornění správy, management pack Microsoft System Center Advisor upozornění Management bude v seznamu.
4.  V zobrazení **Sledování** přejděte do zobrazení **Stav Suite\Health správy operace** .  Vyberte na serveru správy v podokně **Stav serveru správy** a v podokně **Podrobností** potvrďte, že hodnota vlastnost **ověřování služby URI** odpovídá ID OMS pracovního prostoru.

    ![OMS-OpsMgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)


## <a name="remove-integration-with-oms"></a>Odebrání integrace se službou OMS
Pokud už vyžadují integrace mezi Operations Manager Správa skupiny a pracovním prostorem OMS, existuje několik kroky potřebné k správně odebrat připojení a konfigurace ve skupině Správa. Následující postup bude mít můžete aktualizovat pracovního prostoru OMS tak, že odstraníte odkaz skupiny správy, odstraňte OMS spojnic a potom odstraňte management Pack podpůrné OMS.   

1.  Otevřete Operations Manager příkazového prostředí pomocí účtu, který je členem role správců operace správce.

    >[AZURE.WARNING] Ověřte nemáte žádné vlastní správy sady slovem Advisor nebo IntelligencePack v názvu před pokračováním, jinak podle těchto kroků se odstraňte je ze skupiny správy.

2.  Z prostředí příkazovém řádku zadejte`Get-SCOMManagementPack -name "*advisor*" | Remove-SCOMManagementPack`

3.  Další typ`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack`

4.  Spusťte konzolu operací správce operace s účtem, který je členem role správců operace správce.
5.  V části **Správa**vyberte uzel **Management Pack** a v **vyhledejte:** zadejte **Advisor** a ověřte následující management Pack naimportují stále ve skupině Správa:

    - Konference Advisor Microsoft System Center
    - Microsoft System Center Advisor interní

6. Na portálu OMS klikněte na dlaždici **Nastavení** .
7.  Vyberte **připojení zdroje**.
8.  V tabulce v části systém Center Operations Manager byste měli vidět na název skupiny správy, které chcete odebrat z pracovního prostoru.  V části sloupec **Poslední Data**klikněte na **Odebrat**.  

    >[AZURE.NOTE] **Odebrat** odkaz nebudou dostupné až po 14 dní, pokud neaktivní zjistit z připojeného Správa skupiny.  
   
9.  Otevře se okno s žádostí o potvrzení, že chcete pokračovat v odebrání.  Kliknutím na tlačítko **Ano** pokračovat. 

Odstranění dva konektory - Microsoft.SystemCenter.Advisor.DataConnector Advisor spojnice skript Powershellu pod uložit do počítače a provést následující příklady použití.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

>[AZURE.NOTE] Počítač běží tento skript z, pokud nejsou na serveru správy by měl mít nainstalovaný v závislosti na verzi skupiny Správa příkazového prostředí Operations Manager 2012 SP1 nebo R2.

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

V budoucnu Pokud nebudete chtít opětovné připojení skupina Správa na OMS schůzek, musíte se znovu importovat `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` management pack souboru z poslední použité do skupiny správy kumulativní aktualizace.  Můžete najít tento soubor v `%ProgramFiles%\Microsoft System Center 2012` nebo `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` složky.

## <a name="next-steps"></a>Další kroky

- [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md) shromažďování dat a přidejte funkce.
- [Konfigurace nastavení proxy a brány firewall v protokolu analýzy](log-analytics-proxy-firewall.md) Pokud vaše organizace používá proxy server a bránu firewall, aby agentů komunikovat pomocí služby protokolu analýzy.
