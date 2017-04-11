<properties
    pageTitle="Konfigurace nastavení proxy serveru a brány firewall v protokolu analýzy | Microsoft Azure"
    description="Konfigurace nastavení proxy serveru a brány firewall Pokud agentů nebo OMS služby nepotřebujete používat zvláštní porty."
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
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Konfigurace nastavení proxy serveru a brány firewall v protokolu analýzy

Akce potřebné k nastavení proxy serveru a nastavení brány firewall pro analýzu protokolu v OMS lišit při použití Operations Manager a jeho agentů a sledování agentů Microsoft, který připojit přímo k serverům. Přečtěte si následující oddíly typu agent, který používáte.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Konfigurace nastavení proxy serveru a brány firewall s Microsoft Agent sledování

Microsoft sledování Agent pro připojení k a zaregistrovat ke službě OMS musí mít přístup k číslo portu svojí domény a adresy URL. Pokud používáte proxy server pro komunikaci mezi agent a služba OMS, musíte zajistit, aby byly přístupné vhodné prostředky. Pokud používáte bránu firewall omezit přístup k Internetu, budete muset konfigurace brány firewall pro povolení přístupu k OMS. Následující tabulka uvádí, které potřebuje OMS porty.

|**Agent zdroje**|**Porty**|**Přemostění HTTPS kontroly**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Ano|
|\*. oms.opinsights.azure.com|443|Ano|
|\*. blob.core.windows.net|443|Ano|
|Ods.systemcenteradvisor.com|443| |

Následující postup slouží ke konfiguraci nastavení serveru proxy pro Microsoft Agent sledování pomocí ovládacích panelů. Musíte použít postup pro každý server. Pokud máte hodně servery, které potřebujete ke konfiguraci, může jednodušší použít tento proces zautomatizovat pomocí skriptu. Pokud ano, najdete v článku [Nastavení proxy serveru Microsoft Agent sledování pomocí skriptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script)pomocí následujícího postupu.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Konfigurace nastavení proxy serveru pro Microsoft Agent sledování pomocí ovládacích panelů

1. Otevřete **Ovládací panely**.

2. Otevřete **Microsoft Agent sledování**.

3. Klikněte na kartu **Nastavení Proxy** .<br>  
  ![Karta nastavení proxy serveru](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Vyberte **použít serveru proxy** a zadejte adresu URL a port číslo, pokud je požadována, podobně jako v příkladu znázorněném. Pokud proxy server vyžaduje ověření, zadejte uživatelské jméno a heslo pro přístup k proxy serveru.

Pomocí následujícího postupu vytvořit skript Powershellu, jehož spuštěním nastavení proxy serveru pro každý agent, ke kterému je připojen přímo servery.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Konfigurace nastavení proxy serveru pro Microsoft Agent sledování pomocí skriptu

Zkopírujte následující ukázková aktualizovat informace specifické pro prostředí, uložte s příponou souboru PS1 a znovu spusťte skript na každém počítači, ke kterému je připojen přímo službu OMS.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Konfigurace nastavení proxy serveru a brány firewall s Operations Manager

Pro skupinu Operations Manager Správa připojení k a zaregistrovat ke službě OMS musí mít přístup k čísla portů domény a adresy URL. Pokud používáte proxy server pro komunikaci mezi Operations Manager management server a OMS služby, musíte zajistit, aby byly přístupné vhodné prostředky. Pokud používáte bránu firewall omezit přístup k Internetu, budete muset konfigurace brány firewall pro povolení přístupu k OMS. I když server management Operations Manager není za proxy server, může být jeho zástupci. V tomto případě proxy serveru by měl být nakonfigurované stejným způsobem jako agenti jsou k umožňuje a zabezpečení a data řešení správy protokolu získat zasílat OMS webová služba.

Aby Operations Manager agentů komunikovat s službu OMS musí mít infrastrukturu Operations Manager (včetně agentů) správné nastavení serveru proxy a verze. Nastavení pro agentů proxy jsou zadány v konzole Operations Manager. Vaše verze by měl být jeden z těchto věcí:

- Operace správce 2012 SP1 kumulativní aktualizace 7 nebo novější
- Operace správce 2012 R2 kumulativní aktualizace 3 nebo novější


Následující tabulka uvádí porty související k těmto úkolům.

>[AZURE.NOTE] Některé z následujících materiálů zmínit Advisor a provozní přehledech, jak byly předchozí verze OMS. Však bude v budoucnosti změnit uvedených zdrojů.

Tady je seznam zdrojů agent a porty:<br>

|**Agent zdroje**|**Porty**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.BLOB.Core.Windows.NET/\*|443|
|Ods.systemcenteradvisor.com|443|
<br>
Tady je seznam správy prostředků serveru a porty:<br>

|**Správa serveru zdroje**|**Porty**|**Přemostění HTTPS kontroly**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Ano| 
|data.systemcenteradvisor.com|443| | 
|Ods.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Ano| 
<br>
Tady je seznam OMS a Operations Manager konzoly zdrojů a porty.<br>

|**OMS Operations Manager konzoly zdroj**|**Porty**|
|----|----|
|Service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Port 80 a 443|
|\*. microsoft.com|Port 80 a 443|
|\*. microsoftonline.com|Port 80 a 443|
|\*. mms.microsoft.com|Port 80 a 443|
|Login.Windows.NET|Port 80 a 443|
<br>

Použijte následující postupy k registraci skupiny Správa Operations Manager se službou OMS. Pokud máte problémy s komunikací mezi skupině správy a služba OMS, umožňuje ověřovací postup řešení potíží s přenosem dat ve službě OMS.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Požádat o výjimky pro koncové body OMS služby

1. Informace z první tabulky prezentovány dříve zajistit prostředky potřebné pro server pro správu Operations Manager přístupných osobám s postižením všechny bran, bude pravděpodobně nutné použijte.
2. Použití informacemi z druhé tabulky prezentovány dříve zajistit prostředky potřebné pro operace konzola v Operations Manager a OMS přístupných osobám s postižením všechny bran, bude pravděpodobně nutné.
3. Pokud používáte proxy server s Internet Exploreru, ujistěte se, že je nakonfigurované a funguje správně. Pokud chcete ověřit, můžete otevřít zabezpečeného webového připojení (HTTPS), například [https://bing.com](https://bing.com). Pokud zabezpečeného webového připojení nefunguje v prohlížeči, pravděpodobně fungovat v konzole Správa Operations Manager s webovým službám v cloudu.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Konfigurace proxy serveru v konzole pro Operations Manager

1. Spusťte konzolu Operations Manager a vyberte příslušný pracovní prostor **Správa** .

2. Rozbalte **Provozní přehledy**a vyberte **Provozní přehledy připojení**.<br>  
    ![Operace připojení OMS správce](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. V zobrazení OMS připojení klikněte na **Nastavení Proxy serveru**.<br>  
    ![Operace připojení OMS Správce konfigurace Proxy serveru](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. V operačních přehledy Průvodce nastavením: Proxy Server, vyberte **použití proxy serveru pro přístup k webové službě provozní přehledy**a pak zadejte adresu URL s port číslo, například **http://myproxy:80**.<br>  
    ![Operace správce OMS proxy adresy](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Chcete-li zadat přihlašovací údaje, pokud proxy server vyžaduje ověření
 Nutné šířit na spravovaných počítačů, které ohlásí OMS pověření proxy serveru a nastavení. Těchto serverech musí být v *Microsoft systém Centrum Advisor sledování skupiny pro Server*. Přihlašovací údaje jsou šifrovány v registru každého serveru ve skupině.

1. Spusťte konzolu Operations Manager a vyberte příslušný pracovní prostor **Správa** .
2. V části **RunAs konfigurace**vyberte **profily**.
3. Otevření profilu **Systém Centrum Advisor spustit jako profil proxy serveru** .  
    ![Obrázek profilu systém Centrum Advisor spustit jako proxy serveru](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Spustit jako Průvodce profily klikněte na **Přidat** používat účet Spustit jako. Můžete vytvořit nový účet Spustit jako nebo použít existující účet. Tento účet musí mít dostatečná oprávnění projít proxy serveru.  
    ![Obrázek průvodce spustit jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Pokud chcete nastavit účet, který chcete spravovat, zvolte **vybrané třídy, skupině nebo objektu** objekt vyhledávacího pole.  
    ![Obrázek průvodce spustit jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Vyhledejte a zvolte **Microsoft systém Centrum Advisor sledování skupiny pro Server**.  
    ![Obrázek pole Hledat objekt](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klikněte na **OK** zavřete pole přidat spustit jako účet.  
    ![Obrázek průvodce spustit jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Dokončete průvodce a uložte požadované změny.  
    ![Obrázek průvodce spustit jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Potvrďte tuto OMS správu sad stáhnou

Pokud jste přidali řešení OMS, můžete tyto informace zobrazit v konzole pro Operations Manager jako management Pack klikněte v části **Správa**. Vyhledejte *System Center Advisor* můžete rychle najít.  
    ![Správa sad stáhli](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) nebo OMS management Pack taky můžete vyhledat pomocí následujícího příkazu prostředí Windows PowerShell na serveru správy Operations Manager:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Ověřte této Operations Manager je odesílání dat ve službě OMS

1. Do pole server management Operations Manager otevřete sledování výkonu (perfmon.exe) a vyberte **Sledování výkonu**.
2. Klikněte na **Přidat**a potom vyberte **Stav služby Správa skupiny**.
3. Přidejte peaks začínající na **HTTP**.  
    ![Přidání čítačů](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Pokud konfigurace Operations Manager je dobré, že uvidíte aktivitou pro čítače stavu služby správy pro události a jiných dat položek, podle balíčky správy, které jste přidali v OMS a zásady kolekce nakonfigurované protokolu.  
    ![Výkon sledování znázorňující činnosti](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Další kroky

- [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md) shromažďování dat a přidejte funkce.
- Seznámení s [protokolu hledání](log-analytics-log-searches.md) zobrazíte podrobné informace shromážděné řešení.
