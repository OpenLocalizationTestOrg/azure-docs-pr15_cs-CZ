<properties
   pageTitle="Integrace Azure protokolu nejčastější dotazy týkající se | Microsoft Azure"
   description="V tomto článku najdete odpovědi na otázky týkající se integrace Azure protokolu."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Integrace Azure protokolu nejčastější dotazy

V tomto článku najdete odpovědi na otázky týkající se Azure protokolu integraci, služby, který umožňuje integraci neformátovaných protokolů z Azure zdrojů do místního informací o zabezpečení a události Management (SIEM) systémy. Tato integrace poskytuje jednotné řídicí panel pro všechny vaše majetek místní nebo v cloudu, tak, aby se daly agregovat, sladit, analyzovat a výstrahy zabezpečení události spojené s aplikací.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Jak lze zobrazit úložiště účty, ze kterých táhne integrace Azure protokolu protokolů Azure OM z?

Spusťte příkaz **azlog zdrojovém seznamu**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Jak mám aktualizovat konfigurací proxy?

Pokud vaše nastavení serveru proxy přímo neumožňuje Azure úložiště přístup, otevřete **AZLOG. EXE. Konfigurace** souboru v **c:\Program Files\Microsoft Azure protokolu integrace**. Aktualizujte soubor zahrnout **defaultProxy** část s proxy adresy vaší organizace. Po dokončení aktualizace ukončit a znovu spusťte službu pomocí příkazů **azlog čisté zastavit** a **azlog čisté start**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Jak lze zobrazit informace o předplatném v událostí systému Windows?

Při přidávání zdroji přidejte **subscriptionid** popisný název.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Událost XML má metadata, jak je ukázáno v následujícím příkladu, včetně id předplatného.

![Událost XML][1]

## <a name="error-messages"></a>Chybové zprávy

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Při spuštění příkazu **azlog createazureid**, proč se zobrazí následující chyba?

Chyba:

  *Nepodařilo se vytvořit AAD aplikace – klient 72f988bf-86f1-41af-91ab-2d7cd011db37-důvod, proč = "Zakázáno" - zpráva = "Dostatečná oprávnění ke dokončit."*

**Azlog createazureid** pokusí vytvořit hlavní název služby v všechny klienty Azure AD pro předplatné, na kterých Azure má přístup k. Pokud Azure přihlašovací jméno je pouze hosta uživatel v tomto klientovi Azure AD, příkaz nezdaří s "Dostatečná oprávnění ke dokončit." Požádat o správce klienta a přidejte svůj účet uživatele v klientovi.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Při spuštění příkazu **azlog povolit**, proč se zobrazí následující chyba?

Chyba:

  *Upozornění: vytváření přiřazování rolí - AuthorizationFailed: klient janedo@microsoft.com' s objektem id "fe9e03e4-4dad-4328-910f-fd24a9660bd2" nemá oprávnění k provedení akce "Microsoft.Authorization/roleAssignments/write" v oboru "/ předplatná/70 d 95299-d689-4 při instalaci c 97-b971-0d8ff0000000".*

**Povolte Azlog** příkaz přiřadí roli Reader pro službu Azure AD hlavní (vytvořená pomocí **Azlog createazureid**) k předplatným k dispozici. Nejsou-li Azure přihlaste vedlejšího správce nebo vlastník předplatného, dojde k chybová zpráva se nezdařila se tak mohli ověřovat. Řízení přístupu na základě rolí Azure (RBAC) spolu správce nebo vlastník je potřeba k provedení této akce.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Kde najdu definici vlastností v protokolu auditování

Najdete tady:

- [Auditování operace s správce prostředků](../resource-group-audit.md)
- [Seznam správy událostí v předplatné v Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Kde můžu podrobné informace najdete v Centru zabezpečení Azure upozornění?

V tématu [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Jak lze změnit co se shromažďují pomocí OM diagnostiky?

Najdete v článku [pomocí povolit Azure Diagnostika v virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) podrobné informace o tom, jak získat, změnit a nastavte Azure Diagnostika v systému Windows *(WAD)* konfigurace. Následuje výběru:

### <a name="get-the-wad-config"></a>Získání konfigurace WAD

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Změna konfigurace WAD

V následujícím příkladu je konfigurace, kde jsou pouze EventID 4624 a EventId 4625 odebrané protokol událostí. Microsoft Antimalware události se shromažďují z protokolu událostí systému. Zobrazí se [jinými události] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) podrobné informace o použití výrazů výrazu XPath.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Nastavení WAD

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Po provedení změn, zkontrolujte účtu úložiště zajistit, aby byly shromážděny správné události.

Pokud máte nějaké otázky o Integration protokolu Azure, pošlete nám prosím e-mailu pro [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
