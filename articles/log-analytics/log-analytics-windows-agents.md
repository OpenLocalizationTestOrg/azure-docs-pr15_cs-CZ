<properties
    pageTitle="Připojení k analýzy protokolu počítače se systémem Windows | Microsoft Azure"
    description="Tento článek popisuje postup v infrastrukturu místního počítače se systémem Windows přímé připojení k OMS pomocí upravenou verzi aplikace Microsoft sledování Agent (MMA)."
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
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Připojení k analýzy protokolu počítače se systémem Windows

Tento článek popisuje postup připojení počítače se systémem Windows ve vaší místní infrastruktury přímo do pracovních prostorů OMS pomocí upravenou verzi aplikace Microsoft sledování Agent (MMA). Budete muset nainstalovat a připojovat se k integrované OMS za účelem jejich odeslání dat do OMS a zobrazit a pracovat s dat na portálu OMS agentů u všech počítačů, které chcete. Každý agent můžete vykázat více pracovního prostoru.

Nainstalování agentů pomocí instalačního programu, přepínač příkazového řádku nebo s žádoucí stavu konfigurace (DSC) v Azure automatizaci.  

>[AZURE.NOTE] Pro virtuálních počítačích službou Azure instalaci zjednodušit pomocí [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).

Na počítačích s připojením k Internetu použije agent odeslání dat do OMS připojení k Internetu. U počítačů, které nemají připojení k Internetu můžete na proxy server nebo předávání OMS protokolu analýzy.

Připojení počítače Windows OMS je jednoduché, pomocí 3 jednoduchých krocích:

1. Stáhnout instalační soubor agent
2. Instalace agenta pomocí metody, kterou zvolíte
3. Konfigurace agenta nebo v případě potřeby přidat další pracovní prostory

Na následujícím obrázku vidíte vztah mezi počítače se systémem Windows a OMS jste nainstalovali a nakonfigurovali agentů.

![OMS přímé agent diagramu](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Systémové požadavky a požadovaná konfigurace
Před instalací nebo nasadit agentů, zkontrolujte následující podrobnosti zajistit, že splníte požadavky na potřeby.

- OMS MMA můžete nainstalovat jenom na počítači se systémem Windows Server 2008 SP 1 nebo novější nebo Windows 7 s aktualizací SP1 nebo novější.
- Musíte mít předplatné OMS.  Další informace najdete v tématu [Začínáme s protokolu analýzy](log-analytics-get-started.md).
- Každý počítač Windows musíte mít pro připojení k Internetu pomocí HTTPS. Připojení může být přímé prostřednictvím proxy nebo prostřednictvím technologie pro analýzu předávání OMS protokolu.
- OMS MMA můžete nainstalovat na samostatných počítačů, servery a virtuálních počítačích. Pokud se chcete připojit hostované Azure virtuálních počítačích OMS, najdete v článku [připojení Azure virtuálních počítačích k protokolu analýzy](log-analytics-azure-vm-extension.md).
- Agent musí používat TCP port 443 různých zdrojů. Další informace najdete v tématu [Konfigurace brány firewall nastavení serveru proxy a v protokolu analýzy](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Agent nastavení budou moct soubor stáhnout z OMS
1. Na portálu OMS na stránce **Přehled** klikněte na dlaždici **Nastavení** .  Klikněte na kartu **Připojení zdroje** nahoře.  
    ![Karta připojení zdroje](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. V části **Připojit přímo počítače**klikněte na **Stáhnout Agent systému Windows** pro typ procesoru počítače stáhnout instalační soubor.
3. Na pravé straně **ID pracovního prostoru**klikněte na ikonu kopírovat a vložit ID do programu Poznámkový blok.
4. Na pravé straně **Primárního klíče**klikněte na ikonu kopírovat a vložit klávesu do Poznámkový blok.     
    ![kopírování ID pracovní prostor a primárního klíče](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Instalace pomocí instalačního programu agent
1. Spusťte instalační program agent nainstalovat na počítač, na kterém chcete spravovat.
2. Na stránce Vítejte klikněte na **Další**.
3. Na stránce Licenční podmínky pro další licence a potom klepněte na tlačítko **souhlasím**.
4. Na stránce cílovou složku změnit nebo použít výchozí instalace složku a klikněte na tlačítko **Další**.
5. Na stránce Možnosti nastavení Agent můžete připojit k Azure protokolu analýzy (OMS), Operations Manager agent nebo můžou být možností prázdné Pokud chcete nakonfigurovat agent později. Klikněte na tlačítko **Další**.   
    - Pokud jste se rozhodli pro připojení k Azure protokolu analýzy (OMS), vložte **ID pracovní prostor** a **Pracovní prostor klíč (primární)** , kterou jste zkopírovali do programu Poznámkový blok v předchozím postupu a klikněte na tlačítko **Další**.  
        ![Vložte ID pracovní prostor a primárního klíče](./media/log-analytics-windows-agents/connect-workspace.png)
    - Pokud jste se rozhodli pro připojení k Operations Manager, zadejte **Název skupiny správy**, název **Serveru pro správu** a **Port serveru pro správu**a potom na tlačítko **Další**. Na stránce účtu akce agenta místní systémovým účtem nebo účtem místní domény a potom na tlačítko **Další**.  
        ![Konfigurace správy skupiny](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent akce účtu](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Na stránce Připraveno k instalaci zkontrolujte své volby a potom klikněte na **nainstalovat**.
7. Ke konfiguraci byla úspěšně dokončena stránky, klikněte na tlačítko **Dokončit**.
8. Až budete hotovi, zobrazí se v **Ovládacích panelech** **Microsoft Agent sledování** . Můžete zkontrolovat vaší konfigurace a ověřte, zda je agent připojen k provozní přehledy (OMS). Pokud připojení k OMS, agent, zobrazí se zpráva s oznámením: **Microsoft Agent sledování má úspěšném připojení ke službě Microsoft operace správy Suite.**

## <a name="install-the-agent-using-the-command-line"></a>Instalace agenta pomocí příkazového řádku
- Upravit a potom použijte následující příklad nainstalovat agenta pomocí příkazového řádku.

    >[AZURE.NOTE] Pokud chcete upgradovat agent, musíte použít protokol analýzy skriptování rozhraní API. Naleznete v části Další upgradovat agent.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Upgrade agent a přidání pracovního prostoru pomocí skriptu
Můžete upgradovat agent a přidání pracovního prostoru pomocí protokolu analýzy skriptování rozhraní API v následujícím příkladu Powershellu.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Pokud jste vypotřebovali příkazového řádku nebo skriptu dříve k instalaci nebo konfiguraci agenta `EnableAzureOperationalInsights` nahradil `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Instalace agenta pomocí DSC v Azure automatizaci

>[AZURE.NOTE] Tento postup a skript příklad nejde upgradovat existující agent.

1. Naimportujte xPSDesiredStateConfiguration DSC modulu z [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure automatizaci.  
2.  Vytvoření Azure automatizaci proměnné prostředky pro *OPSINSIGHTS_WS_ID* a *OPSINSIGHTS_WS_KEY*. Nastavte *OPSINSIGHTS_WS_ID* svoje ID pracovního prostoru OMS protokolu technologie pro analýzu a *OPSINSIGHTS_WS_KEY* primární klíč pracovního prostoru.
3.  Pomocí níže uvedených skriptu a uložit ho jako MMAgent.ps1
4.  Upravit a potom použijte následující příklad nainstalovat agenta pomocí DSC v Azure automatizaci. Importujte MMAgent.ps1 do automatizaci Azure pomocí rozhraní pro automatizaci Azure nebo rutiny.
5.  Přiřaďte uzel konfiguraci. Během 15 minut uzel zkontrolujte její konfiguraci a MMA se posune uzel.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Ruční konfigurace agent nebo přidejte další pracovní prostory
Pokud jste si nainstalovali agentů nenakonfigurovali, ale nebo pokud chcete agent vykazování více pracovního prostoru, můžete tyto informace povolí agent nebo ji nakonfigurovat. Po konfiguraci agent zaregistruje s agentem a pošle potřebné informace o konfiguraci a balíčky správy, které obsahují informace o řešení.

1. Po instalaci Microsoft Agent sledování, otevřete **Ovládací panely**.
2. Otevřete **Microsoft Agent sledování** a pak klikněte na kartu **Azure protokolu analýzy (OMS)** .   
3. Klikněte na **Přidat** otevřete dialogové okno **Přidat pracovní prostor protokolu analýzy** .
4. Vloží **ID pracovní prostor** a **Pracovní prostor klíč (primární)** , kterou jste zkopírovali do programu Poznámkový blok v předchozím postupu pro pracovní prostor, který chcete přidat a potom klikněte na **OK**.  
    ![Konfigurace provozní přehledy](./media/log-analytics-windows-agents/add-workspace.png)

Po údaje z počítače kontroloval agent, počet kontroloval OMS se zobrazí na portálu OMS na kartě **Připojení zdroje** v **dialogovém okně Nastavení** jako **Servery připojení**.


## <a name="to-disable-an-agent"></a>Jak zakázat agent
1. Po instalaci je agent, otevřete **Ovládací panely**.
2. Otevřete Microsoft Agent sledování a pak klikněte na kartu **Azure protokolu analýzy (OMS)** .
3. Vyberte pracovní prostor a pak klikněte na **Odebrat**. Opakujte tento krok pro všechny ostatní pracovní prostory.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Volitelně nakonfigurujte agentů a nahlaste do skupiny správy Operations Manager

Pokud používáte Operations Manager v infrastrukturu, můžete také MMA agent jako agent Operations Manager.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Konfigurace MMA agentů a nahlaste do skupiny správy Operations Manager
1.  Na počítači nainstalovanou agent otevřete **Ovládací panely**.
2.  Otevřete **Microsoft Agent sledování** a pak klikněte na kartu **Operations Manager** .
    ![Karta Microsoft sledování Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Pokud serverech Operations Manager integrace se službou Active Directory, klikněte na **automaticky aktualizovat přiřazení skupina správy ze služby AD DS**.
4.  Klikněte na tlačítko **Přidat** pro otevření dialogového okna **Přidat skupiny správy** .  
    ![Sledování programu Microsoft Agent přidat skupinu správy](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  Do pole **název skupiny správy** zadejte název skupiny správy.
6.  Do pole **primární management server** zadejte název počítače serveru primární správy.
7.  V dialogovém okně **Správa port serveru** zadejte číslo portu TCP.
8.  V části **Akce účtu agenta**zvolte místní systémovým účtem nebo účtem místní domény.
9.  Klikněte na **OK** zavřete dialogové okno **Přidat skupinu správu** a pak klikněte na **OK** zavřete dialogové okno **Vlastnosti Agent sledování Microsoft** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Volitelně nakonfigurujte agentů předávání OMS protokolu analýzy

Pokud máte servery nebo klienti, které nemají připojení k Internetu, máte pořád ještě jejich odeslání dat do OMS pomocí technologie pro analýzu předávání OMS protokolu.  Při použití předávání všech dat ze agentů prochází jediný server, který má přístup k Internetu. Předávání převede data z agentů OMS přímo bez analýze všech dat, která jsou přenášena.

V tématu [OMS protokolu analýzy předávání](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) zobrazíte další informace o předávání, včetně nastavení a konfigurace.

Informace o tom, jak konfigurovat agentů proxy server, který je v tomto případě předávání OMS, najdete v článku [Nastavení proxy server a bránu firewall v protokolu analýzy](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Volitelně nakonfigurujte nastavení proxy serveru a brány firewall
Pokud máte servery proxy nebo brány firewall ve vašem prostředí, které omezit přístup k Internetu funguje, najdete v článku [Konfigurace brány firewall nastavení serveru proxy a v protokolu analýzy](log-analytics-proxy-firewall.md) povolit agentů komunikovat ke službě OMS.

## <a name="next-steps"></a>Další kroky

- [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md) shromažďování dat a přidejte funkce.
- [Konfigurace nastavení proxy a brány firewall v protokolu analýzy](log-analytics-proxy-firewall.md) Pokud vaše organizace používá proxy server a bránu firewall, aby agentů komunikovat pomocí služby protokolu analýzy.
