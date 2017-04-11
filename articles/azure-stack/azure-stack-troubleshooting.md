<properties
    pageTitle="Poradce při potížích s Microsoft Azure zásobníku | Microsoft Azure"
    description="Azure zásobníku Poradce při potížích."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Poradce při potížích s Microsoft Azure zásobníku

V tomto dokumentu jsou běžné informace o řešení potíží pro Azure vrstvě.

Zaručuje nejlepší možnosti Ujistěte se, jestli prostředí pro nasazení splňuje [požadavky](azure-stack-deploy.md) a [přípravy](azure-stack-run-powershell-script.md) před instalací. 

Doporučení pro odstraňování potíží, které jsou popsané v této části jsou odvozeny z různých zdrojů a může nebo nemusí určitý problém vyřešit. Pokud jste zaznamenali problém není popsána, projděte [Azure zásobníku fórum MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) pro další technické podpory a dalších informací.

Příklady kódu jsou k dispozici jako je a nemůže být zaručena očekávané výsledky. V této části podléhá časté úpravy a aktualizace prováděn vylepšení dělený součinem jejich.

  

## <a name="known-issues"></a>Známé problémy

 - Může se objevit následující bez ukončení chyby během nasazení, které ovlivňují úspěch nasazení:
     - "Termín"C:\WinRM\Start-Logging.ps1"se bohužel nerozpoznal"
     - "Vyvolat EceAction: nelze indexovat do null pole" 
     - "InvokeEceAction: Nelze vytvořit vazbu argument parametr"Zpráva"protože se prázdný řetězec."
 - Můžete vidět nasazení nepovede v kroku 60.61.93 zobrazí se chybová zpráva "Aplikaci identifikátor URI nebyl nalezen." Toto chování se kvůli způsob, jakým aplikace jsou registrované v Azure Active Directory.  Pokud se zobrazí tato chyba, pokračujte [Spusťte instalační skript](azure-stack-rerun-deploy.md) od kroku 60.61.93 až do dokončení nasazení.
 - Uvidíte, **Nastavte dostupnost** zdroje v Tržiště se objeví ve skupinovém rámečku kategorie **virtualMachine ARM** – tento vzhled je symbolické problém.
 - Při vytváření nového virtuálního počítače na portálu v kroku **Základy** alternativy pro ukládání ve výchozím nastavení SSD.  Toto nastavení je třeba změnit na pevný disk nebo na **velikost** kroku nasazení OM, neuvidíte OM velikosti k dispozici pro výběr a pokračujte nasazení. 
 - Uvidíte AzureRM Powershellu moduly nainstalovaných už není ve výchozím nastavení na OM MAS CON01 (v TP1 to byl s názvem ClientVM). Toto chování se o záměr, protože je alternativní metody [nainstalovat moduly](azure-stack-connect-powershell.md)a připojit.  
 - Zobrazí se, že není zprostředkovatele prostředků **Microsoft.Insights** automaticky registrované pro klienta předplatná. Pokud chcete v tématu sledování dat pro OM nasazený jako klienta, spusťte tento příkaz z prostředí PowerShell (po můžete [nainstalovat a připojit se](azure-stack-connect-powershell.md) ke klientovi): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Zobrazí se však žádný text je zobrazen a k dispozici pro export exportovat funkce na portálu pro skupiny zdrojů.      
 - Můžete začít nasazení větší než k dispozici kvóta úložiště zdrojů.  Dochází k selhání tento nasazení a bude pozastavené zdroje účtu.  Dvěma způsoby remediation k dispozici:
     - Správce služby zvyšují kvóty, když změny se projeví okamžitě a běžně trvat až jednu hodinu se rozšíří.
     - Správce služby můžete vytvořit plán doplněk s další kvótu klienta poté můžete přidat k předplatnému.
 - Při vytváření VMs na Azure zásobníku prostředí s identity v "Azure - Čína" pomocí portálu, neuvidíte OM velikostí dostupných vyberte v **velikost** kroku nasazení OM a nebude možné pokračujte nasazení.
 - Může se objevit selhání nasazení na portálu OM má ve skutečnosti úspěšně nasazena.
 - Když odstraníte podle plánu, nabídky nebo předplatného, nemusí VMs odstraní.
 - Zobrazí se OM rozšíření tržiště.
 - Nelze nasadit OM z uložené OM obrázku.
 - Klienti možná uvidíte služeb, které nejsou součástí předplatné.  Když se student pokusí o nasazení tyto materiály, budou dojít k chybě.  Příklad: Klienta předplatné pouze zahrnuje úložiště zdroje.  Klienta uvidí možnost vytvořit dalších zdrojů, jako jsou VMs.  V tomto scénáři Pokud klienta pokusí nasazení OM, dostanou zprávu, že že OM nejde vytvořit. 
 - Při instalaci TP2, neměli aktivovat hostiteli brzy vyprší OS v virtuálního pevného disku, kde spuštění instalačního skriptu zásobníku Azure, nebo může dojít k chybě zpráv údaj Windows.


## <a name="deployment"></a>Nasazení

### <a name="deployment-failure"></a>Chyba při zavedení
Pokud dochází k selhání při instalaci, instalační služby Azure zásobníku umožňuje pokračovat se nepodařilo nainstalovat pomocí následujících [kroků nasazení znovu spustit](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Na konci nasazení relaci Powershellu pořád, otevřený a nezobrazují žádné výstup

Toto chování se pravděpodobně jenom výsledek výchozí chování aplikace okna příkazového prostředí PowerShell má vybrán. Nasazení Koncepce skutečně proběhlo ale skript bylo pozastaveno při výběru okna. Můžete ověřit, že jde o případ vyhledáním slovo "select" v záhlaví okna příkazového.  Stisknutím klávesy ESC ho zrušit a za ni se má zobrazit zpráva dokončení.

## <a name="templates"></a>Šablony

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure šablony nebude nasadit do zásobníku Azure

Ujistěte se, že:

- Šablona musíte používat službu Microsoft Azure, která už je k dispozici nebo v náhledu v Azure vrstvě.
- Rozhraní API pro konkrétní zdroje jsou podporované místní instancí Azure zásobníku a směrujete platné umístění ("místní" v Azure zásobníku Technical Preview (podokna nástrojů) 2, a "Východoasijských USA" nebo "Jižní Indie" Azure).
- Zkontrolujte [v tomto článku](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) o rutinách Test AzureRmResourceGroupDeployment, která zachytí malé rozdíly v syntaxi správce prostředků Azure.

Můžete taky můžete Azure zásobníku šablony, abyste předtím zadali [GitHub úložiště](http://aka.ms/AzureStackGitHub/) vám pomůžou začít.

## <a name="virtual-machines"></a>Virtuálních počítačích

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Po spuštění hostitel osobních Microsoft Azure zásobníku Koncepce, jsou všechny mé klienti VMs pryč ze správce pro Hyper-V a vraťte automatické po uplynutí trochu?

Jak systém nadcházející zpět podsystém úložiště a RPs potřebujete zjistit konzistence. Čas potřebný závisí na hardware a XPS Specification použit, ale bude určitou dobu, po restartování hostiteli pro klienta VMs vrátit a rozpoznán.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Můžu odstranili některé virtuálních počítačích, ale dál vidět soubory virtuální pevný disk na disku. Očekává se toto chování?

Ano, je to chování by měly. Protože byl navržen tímto způsobem:

- Když odstraníte virtuálního počítače, VHD se neodstraní. Disků je samostatná zdrojů ve skupině prostředků.
- Při odstranění účtu úložiště získá odstranění zobrazený okamžitě prostřednictvím Azure správce prostředků (portál, Powershellu), ale disků, které mohou obsahovat stále nacházejí v úložišti dokud běží uvolnění paměti.

Pokud se zobrazí VHD "řádky", je důležité vědět, pokud jsou součástí složky pro ukládání účet, který byl odstraněn. Pokud účet úložiště nebyl odstraněn, bude normální, že jsou tu.

Další informace o konfiguraci prahové uchovávání informací v [Správa úložiště účtů](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Postup instalace
Následující informace o Azure zásobníku instalačních pokynů mohou být užitečné pro řešení potíží:  

| Index | Jméno | Popis|
| ----- | ----- | -----|
|0.11 | (FUNKCI ZABRÁNĚNÍ SPUŠTĚNÍ DAT) Ověření fyzických počítačů | Ověření hardware a konfigurace OS na fyzické uzlů. |
| 0.12 | (FUNKCI ZABRÁNĚNÍ SPUŠTĚNÍ DAT) Konfigurace sítě fyzických počítačů pro Koncepce | Konfigurace přepínače virtuální sítě a rozhraní. |
| 0.14 | (FUNKCI ZABRÁNĚNÍ SPUŠTĚNÍ DAT) Nasazení domény | Nasazení služby Active Directory v virtuálního počítače. |
| 0,15 | (FUNKCI ZABRÁNĚNÍ SPUŠTĚNÍ DAT) Konfigurace serveru domény | Konfigurace serveru domény se skupinami zabezpečení atd. |
| číslo 0,16 | (FUNKCI ZABRÁNĚNÍ SPUŠTĚNÍ DAT) Konfigurace fyzické počítače | Konfigurace sítě, připojení k doméně a správci místní nastavení. |
| 0,18 | (STO) Konfigurace clusteru úložiště | Vytvoření úložiště clusteru, vytvoření úložiště fondu a soubor serveru. |
| 0.19 | (CPI) Nastavení struktury infrastruktury | Nastavte si požadavky pro nasazení struktury. |
| 0.21 | (SÍTĚ) Nastavení BGP a překladu síťových adres | Nainstaluje BGP a překladu síťových adres – třeba pouze pro jeden uzel. |
| 0.22 | (SÍTĚ) Konfigurace překladu síťových adres a časového serveru | Synchronizuje časového serveru a nakonfiguruje překladu síťových adres položky. |
| 40.41 | (CPI) Vytvoření VMs hosta | Vytvoření správy VMs. |
| 40.42 | (FBI) Nastavení prostředí PowerShell JEA | Nastavení prostředí PowerShell JEA pro všechny role. |
| 40.43 | (FBI) Nastavení Azure zásobníku certifikační autorita | Nainstaluje Azure zásobníku certifikační autorita. |
| 40.44 | (FBI) Konfigurace Azure zásobníku certifikační autority | Konfiguruje Azure zásobníku certifikační autorita. |
| 40.45 | (SÍTĚ) Nastavení NC na VMs | Nainstaluje NC VMs hosta |
| 40.46 | (SÍTĚ) Konfigurace NC na VMs | Konfigurace NC na VMs hosta |
| 40.47 | (SÍTĚ) Konfigurace VMs hosta | Konfigurace správy VMs s NC ACL. |
| 60.61.81 | (FBI) Nasazení Azure zásobníku struktury vyzvánění technické – FabricRing předpoklad | Vytvoří virtuální FabricRing |
| 60.61.82 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění – nasazení struktury vyzvánění obrázku | Instalace a konfiguruje Azure zásobníku struktury vyzvánění obrázku. |
| 60.61.83 | (FBI) Nasaďte Správce rozšíření poskytovatelů zdroje | Instalace rozšíření správce pro zdroje poskytovatele |
| 60.61.84 | (ACS) Nastavte konzistentní Azure úložiště na úrovni uzlů. | Nainstaluje a nakonfiguruje Azure konzistentní úložiště na úrovni uzlů. |
| 60.61.85 | (ACS) Nastavte konzistentní Azure úložiště na úrovni clusteru. | Nainstaluje a nakonfiguruje Azure konzistentní úložiště na úrovni clusteru. |
| 60.61.86 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - podmínkou | Požadavky pro InfraServiceController |
| 60.61.87 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - podmínkou | Požadavky pro CPI |
| 60.61.88 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - podmínkou | Požadavky pro ASAppGateway |
| 60.61.89 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - předpoklad | Požadavky na úložiště řadiče |
| 60.61.90 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - předpoklad | Požadavky pro HealthMonitoring |
| 60.61.91 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - předpoklad | Požadavky pro ECE |
| 60.61.92 | (FBI) Nasazení služby Azure zásobníku struktury vyzvánění řadiče - předpoklad | Požadavky pro PMM |
| 60.61.93 | (Katal) Vytvoření AzureStack služby objektů | Vytvoření Azure grafu aplikace a služby objekty v AAD. |
| 60.61.94 | (SÍTĚ) Nastavení GW VMs | Nainstaluje GW VMs Host. |
| 60.61.95 | (SÍTĚ) Konfigurace GW VMs | Konfiguruje GW VMs Host. |
| 60.61.96 | (SÍTĚ) Nasazení iDNS tabulkami hosts | Nasazení iDNS na infrastrukturu hosts |
| 60.61.97 | (SÍTĚ) Konfigurace iDNS | Konfigurace iDNS rolí |
| 60.61.98 | (FBI) Nastavení WSUS VMs | Nainstaluje WSUS server VMs Host. |
| 60.61.99 | (FBI) Konfigurace WSUS VMs | WSUS server konfiguruje VMs hosta. |
| 60.61.100 | (FBI) Nastavení VMs Azure SQL | Nainstaluje server Azure SQL VMs hosta |
| 60.61.101 | (Katal) Instalační program požadavcích na byl VMs. | Nastaví požadavcích na Microsoft Azure zásobníku na VMs hosta. |
| 60.61.102 | (Katal) Instalačnímu programu se VMs | Nainstaluje Microsoft Azure zásobníku VMs Host. |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení zprostředkovatelé zdroje a řadiče | Nainstaluje zprostředkovatelé zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.121 | (FBI) Nasazení poskytovatelů zdroje a řadiče | Instalace zprostředkovatele zdroje a řadiče |
| 60.120.122 | (FBI) Konfigurace řadiče | Konfiguruje řadiče |
| 60.120.123 | (Katal) Konfigurace WAS VMs | Microsoft Azure zásobníku konfiguruje VMs Host. |
| 60.120.124 | (Katal) Konfigurace AAD Azure zásobníku. | Konfiguruje Azure zásobníku Azure AD. |
| 60.120.125 | (Katal) Instalace služby AD FS | Instalace služby Active Directory Federation Services (služby AD FS) |
| 60.120.126 | (Katal) Instalace služby AD FS/grafu | Nainstaluje Azure zásobníku grafu |
| 60.120.127 | (Katal) Konfigurace služby AD FS | Konfiguruje Active Directory Federation Services (služby AD FS) |
| 60.140.141 | (FBI) Konfigurace SRP | Konfiguruje zprostředkovatele prostředků úložiště |
| 60.140.142 | (ACS) Konfigurace Azure konzistentní úložiště. | Konfiguruje Azure konzistentní úložiště. |
| 60.140.143 | (FBI) Vytvoření účtů úložiště | Vytvořte všechny účty úložiště pro použití v různých poskytovatelů. |
| 60.140.144 | (FBI) Použití zaregistrujte SRP | Použití zaregistrujte úložiště poskytovatele. |
| 60.140.145 | (CPI) Migrace vytvořeného VMs, tabulkami Hosts a obrázku do CPI | Migruje objekty vytvořené VMs tabulkami Hosts a obrázku do CPI |
| 60.140.146 | (FBI) Konfigurace programu Windows Defender | Konfiguruje programu Windows Defender |
| 60.160.161 | (PONDĚLÍ) Konfigurace sledování agenta | Konfiguruje sledování agenta |
| 60.160.162 | (FBI) Předpoklady NRP | Zjistit předpoklady pro instalaci NRP |
| 60.160.163 | (FBI) NRP nasazení | Instalace NRP  |
| 60.160.164 | (FBI) Konfigurace NRP | Konfiguruje NRP |
| 60.160.165 | (FBI) Předpoklady CRP | Instalace plán.KAPACIT požadavky |
| 60.160.166 | (FBI) Plán.kapacit nasazení | Instalace CRP |
| 60.160.167 | (FBI) Konfigurace CRP | Konfiguruje CRP |
| 60.160.168 | (FBI) Předpoklady FRP | Zjistit předpoklady pro instalaci FRP |
| 60.160.169 | (FBI) FRP nasazení | Instalace FRP  |
| 60.160.170 | (FBI) Konfigurace FRP | Konfiguruje FRP |
| 60.160.174 | (FBI) URP základní | Nainstaluje URP požadavky |
| 60.160.175 | (FBI) URP nasazení | Instalace URP  |
| 60.160.176 | (FBI) Konfigurace URP | Konfiguruje URP |
| 60.160.171 | (FBI) HRP podmínkou | Zjistit předpoklady pro instalaci HRP |
| 60.160.172 | (FBI) HRP nasazení | Instalace HRP  |
| 60.160.173 | (FBI) Konfigurace HRP | Konfiguruje HRP |
| 60.160.177 | (KV) KeyVault podmínkou | Zjistit předpoklady pro instalaci KeyVault |
| 60.160.178 | (KV) KeyVault nasazení | Instalace KeyVault  |
| 60.160.179 | (KV) Konfigurace KeyVault | Konfiguruje KeyVault | 
| 60.190.191 | (FBI) Konfigurace Galerie | Konfigurace Galerie |
| 60.190.192 | (FBI) Konfigurace služeb vyzvánění struktury | Konfigurace služeb vyzvánění struktury |
| 60.221 | (FBI) Nastavení konzoly VMs | Nainstaluje konzoly serveru VMs Host. |
| 60.222 | (FBI) Nastavení konzoly VMs | Přesuňte obsah DVM ke konzole OM. |
| 251 | Připravte pro budoucí hostitele restartování | Nastavení zásad restartujte počítač |


## <a name="next-steps"></a>Další kroky

[Nejčastější dotazy](azure-stack-FAQ.md)
