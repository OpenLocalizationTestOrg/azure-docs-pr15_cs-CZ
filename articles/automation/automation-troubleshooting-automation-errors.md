<properties
   pageTitle="Zpracování chyb Azure automatizaci | Microsoft Azure"
   description="Tento článek obsahuje základní chyby zpracování kroky s řešeními problémů a oprava běžných chyb Azure automatizaci."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="Chyba automatizace zpracování chyb"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Tipy pro běžné chyby Azure automatické zpracování chyb ve vzorcích

Tento článek popisuje některé běžné chyby Azure automatizaci může dojít a návrhem možných zpracování chyb ve vzorcích postup, jak.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Poradce při potížích ověřování při práci s runbooks automatizaci Azure  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Scénář: Přihlaste se k účtu Azure se nezdařila.

**Chyby:** Při práci s přidat AzureAccount nebo přihlášení AzureRmAccount rutin dojde k chybě "Unknown_user_type: Neznámý typ uživatele".

**Důvod, proč chyby:** K této chybě dochází, pokud není platný název majetku pověření nebo uživatelské jméno a heslo, které jste použili k nastavení přihlašovacích údajů materiálů automatizaci nejsou platné.

**Tipy pro odstraňování potíží:** Pokud chcete zjistit, proč není správné snažit, Uděláte to takto:  

1. Ujistěte se, že nemáte žádné speciální znaky, včetně **@** znak v názvu automatizaci pověření materiálů, kterou používáte pro připojení k Azure.  

2. Zaškrtněte políčko přes uživatelské jméno a heslo, které jsou uložené v Azure automatizaci pověření místního prostředí PowerShell ISE editoru. Spuštěním těchto rutin v prostředí PowerShell ISE můžete udělat toto:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Když bude ověření se nepovede místně, znamená to, že jste si nenastavili přihlašovacích údajů Azure Active Directory správně. Podívejte se do příspěvku blogu [Authenticating Azure pomocí služby Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) pro zřízení účtu služby Azure Active Directory správně nastavená.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Scénář: Nelze najít Azure předplatného

**Chyby:** Zobrazí chyba "předplatné s názvem ``<subscription name>`` nebyl nalezen" při práci s vyberte AzureSubscription nebo vyberte AzureRmSubscription rutin.

**Důvod, proč chyby:** K této chybě dochází, pokud není platný název předplatného nebo Azure Active Directory uživatele, kterému je snaží dostat Podrobnosti předplatného není nastavena jako správce předplatného.

**Tipy pro odstraňování potíží:** Pokud chcete zjistit, zda správně ověřené Azure a mít přístup k předplatné, které chcete vybrat, Uděláte to takto:  

1. Ujistěte se, spusťte **Přidat AzureAccount** před spuštěním rutiny **Vyberte AzureSubscription** .  

2. Pokud se stále zobrazí tato chybová zpráva, upravit kód přidáním rutinu **Get-AzureSubscription** následující rutinu **AzureAccount přidat** a spustit kód.  Nyní ověřte, pokud výstup Get-AzureSubscription obsahuje podrobnosti předplatného.  
    * Pokud nevidíte žádné podrobnosti předplatného do výstupu, to znamená, že není zatím inicializován předplatné.  
    * Pokud se zobrazí podrobnosti předplatného do výstupu, potvrďte, že používáte správné předplatné jméno nebo ID s rutinu **Vyberte AzureSubscription** .   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Scénář: Ověření se nezdařilo, protože je povolený vícefaktorové ověřování Azure

**Chyby:** Zobrazí chyba "Přidat AzureAccount: AADSTS50079: požaduje zápisu silné ověřování (doklad nakreslenými)" při ověřování Azure pomocí Azure jména a hesla.

**Důvod, proč chyby:** Pokud máte na účtu Azure vícefaktorové ověřování, nelze použít uživatele služby Azure Active Directory ověření Azure.  Místo toho budete muset pomocí certifikátu nebo služby základní ověřování Azure.

**Tipy pro odstraňování potíží:** Použijte certifikát s rutiny pro správu služby Azure, najdete pod odkazy [Vytvoření a přidání certifikát pro správu služby Azure.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Hlavní název služby pomocí rutiny správce prostředků Azure, najdete pod odkazy pro [Vytvoření jistinu služby Azure portálu](./resource-group-create-service-principal-portal.md) a [ověřování služby jistinu pomocí Správce prostředků Azure.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Odstranění běžných chyb při práci s runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scénář: Postupu Runbook selže kvůli rekonstruované objektu

**Chyby:** Vaše postupu runbook selhává, zobrazí se chyba "nelze vytvořit vazbu parametr ``<ParameterName>``. Nelze převést ``<ParameterType>`` hodnota typu Deserialized ``<ParameterType>`` k zadání ``<ParameterType>``".

**Důvod, proč chyby:** Pokud vaše postupu runbook je pracovní postup Powershellu, uloží složité objekty ve formátu rekonstruované s cílem zachovat postupu runbook stavu, pokud je pozastavené pracovního postupu.  

**Tipy pro odstraňování potíží:**  
Některé z následujících tří postupů bude tento problém:

1. Pokud jsou potrubí složité objekty z jednoho rutina do jiného, zalomení těchto rutin v InlineScript.  
2. Předáte jméno nebo hodnotu, kterou potřebujete z složité objektu místo předávání celý objekt.  

3. Pomocí prostředí PowerShell postupu runbook místo prostředí PowerShell pracovního postupu runbook.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Scénář: Postupu Runbook úlohy nezdařilo překročení kvóty přidělené

**Chyby:** Práce postupu runbook se nezdaří s chybou "kvóty pro měsíční celkové pracovní doba běhu překročení pro toto předplatné".

**Důvod, proč chyby:** K této chybě dochází při spuštění úlohy větší než 500 minutu kvótu pro váš účet. Tato kvóta platí pro všechny typy provádění úloh například testování úlohy, zahájení práce na portálu, spuštění úlohy pomocí webhooks a plánování úlohy provést pomocí portálu Azure nebo ve vaší datacentra. Další informace o ceny pro automatizaci najdete v článku [Automatické ceny](https://azure.microsoft.com/pricing/details/automation/).

**Tipy pro odstraňování potíží:** Pokud chcete používat víc než 500 minut zpracování měsíčně je třeba změnit svoje předplatné z bezplatné vrstvě základní osy. Můžete provést upgrade základní osy provedením následujících kroků:  

1. Přihlaste se k předplatnému Azure  
2. Vyberte automatizaci účet, který chcete upgradovat  
3. Klikněte na možnost **Nastavení** > **ceny osy a použití** > **ceny osy**  
4. Na zásuvné **Zvolit ceny vrstvy** zaškrtněte **základní**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scénář: Rutina nebyl rozpoznán při provádění postupu runbook

**Chyby:** Práce postupu runbook selhává, zobrazí se chyba "``<cmdlet name>``: termín ``<cmdlet name>`` se bohužel nerozpoznal jako název rutiny, funkce, soubor skriptu nebo funkční programu."

**Důvod, proč chyby:** K této chybě dojde, pokud modul prostředí PowerShell nenašli rutinu, který používáte ve vaší postupu runbook.  Může to být způsobeno modul obsahující rutinu chybí z účtu, dojde ke konfliktu název pod názvem postupu runbook nebo rutiny existuje také v jiném modulu a automatizaci nerozpoznává název.

**Tipy pro odstraňování potíží:** Některé z následujících způsobů bude tento problém můžete:  

- Zkontrolujte, že zadání názvu rutina správně.  

- Zkontrolujte, jestli rutinu existuje ve vašem účtu automatizaci a že nejsou žádné konflikty. Abyste ověřili, pokud je k dispozici rutinu, otevřete postupu runbook v režimu úprav a vyhledejte rutinu hledáte v knihovně nebo spustit **příkaz Get ``<CommandName>`` **.  Po ověření, které rutinu neexistuje k tomuto účtu a že nejsou žádné konflikty jméno s jinými rutiny nebo runbooks, přidejte ho na plátno a ujistěte se, že používáte platný parametr nastavení ve vaší postupu runbook.  

- Pokud máte konfliktu jmen a rutinu je k dispozici ve dvou různých modulů kontroly, můžete to vyřešit pomocí plně kvalifikovaný název rutiny. Můžete **ModuleName\CmdletName**.  

- Spouštěné postupu runbook místní ve skupině pracovní hybridní a nakonec Ujistěte se, že modul/rutinu nainstalovaná v počítači, který je hostitelem hybridní kolegy.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Scénář: Dlouho pracovního postupu runbook konzistentní selhává, s výjimkou: "úlohy nemůže pokračovat systémem, protože byl opakovaně odstraněn ze stejného kontrolní bod".

**Důvod, proč chyby:** Jedná se o chování kvůli "Podílet" sledování procesů v rámci automatizaci Azure, což automaticky pozastaví postupu runbook, pokud se provede déle než 3 hodiny. Chybová zpráva však neposkytuje "co dále" možnosti. Pro několik důvodů, proč můžete pozastaví postupu runbook. Pozastaví objevil převážně důsledku chyb. Například výjimku nezachycené postupu runbook, selhání sítě nebo selhání na pracovního postupu Runbook systém postupu runbook všechny způsobí postupu runbook pozastaví a začněte od jeho posledního kontrolní bod při obnovení.

**Tipy pro odstraňování potíží:** Dokumentovaného řešení chcete tomuhle problému předejít je použití kontroly v pracovním postupu.  Další informace v nápovědě k [Výukové prostředí PowerShell pracovní postupy](automation-powershell-workflow.md#Checkpoints).  Podrobnější informace o "Podílet" a kontrolní bod najdete v tomto článku blogu [Pomocí kontroly v Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Odstranění běžných chyb při importu moduly

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Scénář: Modul selhání importu nebo rutiny nelze provést po importu

**Chyby:** Modul selhání importu nebo úspěšný import, ale bez rutin extrahování.

**Důvod, proč chyby:** Několik běžných příčin, které modul nemusí úspěšně naimportovat Azure automatizace jsou:  

- Struktura neodpovídá strukturu, který automatizaci, musí být v.  

- Modul jsou závislé na jiné moduly, které nebyla nasazena ke svému účtu automatizaci.  

- Modul chybí jeho závislosti ve složce.  

- Rutinu **New-AzureRmAutomationModule** používá k nahrání modulu a nebyly vyřízeny úložiště úplnou cestu nebo nebyly načíst modulu pomocí veřejně přístupný adresy URL.  

**Tipy pro odstraňování potíží:**  
Některé z následujících způsobů bude tento problém můžete:  

- Ujistěte se, že modul spočívá v tomto formátu:  
ModuleName.Zip **->** Název_modulu nebo číslo verze **->** (ModuleName.psm1, ModuleName.psd1)

- Otevřete soubor .psd1 a jestli modulu nemá nějaké závislosti.  Pokud ano, nahrajte moduly k automatizaci účtu.  

- Ujistěte se, zda jsou k dispozici ve složce modul všechny odkazované knihoven DLL.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Odstranění běžných chyb při práci s žádoucí konfigurace stavu (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scénář: Uzel je ve stavu selhání se chyba "Nebyl nalezen"

**Chyby:** Sestavy s stav **Vadný** a obsahující chybu má uzel "pokus o načtení akci z serveru https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction se nezdařila platnou konfiguraci ``<guid>`` nebyl nalezen."

**Důvod, proč chyby:** Tento obvykle dojde k chybě uzel přiřazen název konfigurace (například ABC) místo názvu konfigurace uzel (například ABC. WebServer).  

**Tipy pro odstraňování potíží:**  

- Ujistěte se, že přiřazujete uzel s "název konfigurace uzel" a ne "název konfigurace".  

- Můžete přiřadit uzel konfigurace uzel portálu Azure pomocí rutiny Powershellu.
    - Chcete přiřadit uzel konfigurace uzel Azure portálu, otevřete zásuvné **DSC uzly** a pak vyberte uzel a klikněte na tlačítko **přiřadit uzel konfigurace** .  
    - Chcete-li přiřadit uzel konfigurace uzlu pomocí rutiny prostředí PowerShell, použijte rutinu **Set-AzureRmAutomationDscNode**


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scénář: Žádné uzel konfigurace (soubory MOF) byly vytvořeny při konfiguraci

**Chyby:** Práce kompilace DSC pozastaví zobrazí se chyba: "byla úspěšně dokončena kompilace, ale bez .mofs konfigurace uzel byly vytvořeny".

**Důvod, proč chyby:** Pokud se vydává následující výraz, který se vyhodnotí **uzel** klíčových slov v konfiguraci DSC $null pak žádné uzel konfigurace.    

**Tipy pro odstraňování potíží:**  
Některé z následujících způsobů bude tento problém můžete:  

- Ujistěte se, že není k $null vyhodnocení výrazu vedle **uzlů** klíčových slov v definici konfigurace.  
- Pokud předáváte ConfigurationData při kompilaci konfiguraci, ujistěte se, že předáváte očekávané hodnoty, které vyžaduje konfiguraci z [ConfigurationData](automation-dsc-compile.md#configurationdata).


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Scénář: Sestava uzel DSC změní zasekne "Probíhá" stavu

**Chyby:** DSC Agent výstupy "Nalezena žádná instance s ohledem nemovitostí s hodnotou."

**Důvod, proč chyby:** Jste aktualizovali WMF verze a mít poškodil WMI.  

**Tipy pro odstraňování potíží:** Postupujte podle pokynů v příspěvku na blogu [DSC známé problémy a omezení](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) vyřešit problém.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Scénář: Nelze použít pověření v konfiguraci DSC

**Chyby:** DSC kompilace práce bylo pozastavené zobrazí se chyba: "výjimka System.InvalidOperationException chybové vlastnost ověření typ zpracování ``<some resource name>``: převod a uložení šifrované heslo jako prostý text je může jenom v případě, že je nastaven PSDscAllowPlainTextPassword true (pravda)".

**Důvod, proč chyby:** Jste používali pověření v konfiguraci ale neměli možnost velkými **ConfigurationData** na **PSDscAllowPlainTextPassword** nastaveno true pro každou uzel konfiguraci.  

**Tipy pro odstraňování potíží:**  
- Zkontrolujte, že předat velkými **ConfigurationData** na **PSDscAllowPlainTextPassword** nastaveno true pro každou uzel konfiguraci uvedené v konfiguraci. Další informace najdete v příručce [vybavení DSC automatizaci Azure](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Další kroky

Pokud jste postupovali výše uvedené kroky pro řešení potíží a potřebujete další pomoc kdykoli v tomto článku, můžete:

- Pokud potřebujete pomoc od odborníků na Azure. Odeslání váš problém [MSDN Azure nebo přetečení zásobníku fóra.](https://azure.microsoft.com/support/forums/)

- Soubor případ Azure podpory. Přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klepněte na **získat pomoc** **technické a fakturační podpory**.

- Pokládat dotazy k požadavek skriptu na [Centrum skriptů](https://azure.microsoft.com/documentation/scripts/) Pokud hledáte řešení postupu runbook automatizaci Azure nebo modul integrace.

- Pro automatizaci Azure na [Uživatele hlasovou](https://feedback.azure.com/forums/34192--general-feedback)publikovat žádostí o názor nebo funkce.
