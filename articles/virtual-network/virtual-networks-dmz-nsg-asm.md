<properties
   pageTitle="Příklad DMZ – vytvoření jednoduchého DMZ s NSGs | Microsoft Azure"
   description="Vytvoření DMZ se skupinami zabezpečení síti (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Příklad 1: vytvoření jednoduchého DMZ s NSGs

[Vraťte se na stránku osvědčené postupy zabezpečení okraj][HOME]

Tento příklad vytvoří jednoduchou DMZ čtyři servery systému windows a skupiny zabezpečení, sítě. Bude taky projděte si každý z příslušné příkazy k poskytování hlubší vysvětlení každého kroku. Je také oddíl přenosy scénář poskytnout hloubkové podrobné pokyny, jak pokračuje přenosy přes vrstvy obrana v DMZ. Nakonec do odkazů na oddíl je dokončeno kódem a pokyny k vytváření toto prostředí vyzkoušíte a experimentovat s různým scénáře. 

![Příchozí DMZ s NSG][1]

## <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje následující:

- Dvě cloudových služeb: "FrontEnd001" a "BackEnd001"
- Virtuální sítě "CorpNetwork" s dvěma podsítí; "FrontEnd" a "Back-end"
- Skupina zabezpečení sítě, která se použije pro obě podsítí
- Windows Server, který představuje serveru webových aplikací ("IIS01")
- Dvě okna servery, které představují aplikace zpět ukončit servery ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")

V části odkazy pod je skript Powershellu, který bude vytvořit maximálně prostředí jsme je popsali výše. Vytváření VMs a virtuální sítích, i když se dělají skriptem příklad nejsou píše v tomto dokumentu. 

Vytvoření prostředí;

  1.    Uložení souboru xml konfigurace sítě zahrnuté v části odkazy (aktualizován jména, umístění a IP adresy tak, aby odpovídaly daný scénář)
  2.    Aktualizovat uživatelské proměnné skriptu prostředí, které má být skript kontrolovat (předplatná, názvy služeb atd.)
  3.    Spouštět v prostředí PowerShell

**Poznámka**: oblast označený ve skript Powershellu se musí shodovat oblasti označený ve konfiguračního souboru xml sítě.

Po spuštění skriptu úspěšně další volitelné kroky úvahy, v části odkazy dva skripty k nastavení webu a aplikace server s jednoduché webové aplikace umožňuje testováním pomocí této konfiguraci DMZ.

V následujících částech najdete podrobný popis skupiny zabezpečení sítě a jak pracovat v tomto příkladu tak, že rozbor klíčové řádky skript Powershellu.

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení síti (NSG)
V tomto příkladu je skupinu NSG integrované a pak načte s šesti pravidla. 

>[AZURE.TIP] Obecně mají zvláštní pravidla "Povolit" nejprve vytvoříte a potom obecnější pravidla "Odepřít" naposledy. Vyžaduje přiřazené priority, které jsou pravidla Vyhodnocená první. Jakmile přenosy nachází použít u určitého pravidla, jsou vyhodnoceny žádná další pravidla. Pravidla NSG můžete použít buď směrem příchozí nebo odchozí (z hlediska podsítě).

Deklarativně jsou u příchozích vytvářeného následující pravidla:

1.  Povolené interní přenos DNS (port 53)
2.  Povolené RDP umožnění datových přenosů (port 3389) z Internetu do libovolné OM
3.  Jsou povoleny přenosy protokolu HTTP (port 80) z Internetu na webový server (IIS01)
4.  Jsou povoleny všechny přenosy (všechny porty) z IIS01 k AppVM1
5.  Všechny přenosy (všechny porty) z Internetu celé VNet (obě podsítí) byl odepřen.
6.  Veškerou odchozí komunikaci (všechny porty) služby podsítě Frontend podsítě back-end byl odepřen.

S následujícími pravidly vázaný na každé podsítě, pokud žádost HTTP příchozí z Internetu k webovému serveru, jak pravidla 3 (Povolit) a 5 (Odepřít) platit, ale od pravidlo 3 musí mít vyšší prioritu pouze byste měli použít a pravidlo 5 nebude možné uplatnit. Žádost HTTP bude mít takto možnost webový server. Pokud tento stejné přenos pokoušel kontaktovat DNS01 server, pravidla 5 (Odepřít) bude jako první použití a přenos nebude moct pořizovat předat na server. Pravidlo 6 (Odepřít) blokuje podsítě Frontend z spolu mluvit podsítě back-end (s výjimkou povolené přenosy v pravidlech 1 a 4), to chrání síť back-end v případě útočník kompromisům webové aplikace on Frontend mohl by mají omezený přístup k back-end "chráněné" síti (jenom pro zdroje vystaven na serveru AppVM01).

Je výchozí odchozí pravidlo, které umožňuje přenosy se k Internetu. V tomto příkladu jsme jste povolení odchozí přenosy a není změna odchozího pravidla. Zamknutí přenosy v obou směrech definované směrování uživatele je potřeba, to je prozkoumat v "Příkladu 3" pod.

Každé pravidlo je podrobněji následujícím způsobem (**Poznámka**: libovolnou položku pod seznamem v začínající na s znak dolaru (například: $NSGName) je proměnná definované uživatelem z skriptu v části referenční tohoto dokumentu):

1. Nejdřív skupinu zabezpečení sítě musí být integrovaná podržte pravidla:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  První pravidlo v tomto příkladu vám umožní přenosů mezi všechny sítě interní server DNS na podsítě back-end. Některé důležité parametry má pravidlo:
  - "Zadejte" označuje, kterým směrem přenosy toto pravidlo se projeví; Toto je z hlediska podsítě nebo virtuálního počítače (podle toho, kde je vázaný tento NSG). Tedy pokud typ je "Vstupní" přenosy přechází podsítě, byste měli použít pravidlo a přenosů z podsítě nebudou ovlivněna toto pravidlo.
  - "Priorita" Nastaví pořadí, ve kterém bude vyhodnocen tok přenosu. Čím nižší číslo vyšší priorita. Jakmile pravidla platí pro konkrétní přenos, žádné další pravidla se zpracovávají. Tedy pokud pravidlo s prioritou 1 umožňuje přenos a pravidla s prioritou 2 zakazuje přenos a obě pravidla platí pro přenos a přenos bude mít možnost plovoucí dlaždice (protože pravidlo 1 mělo vyšší prioritu trvala efekt a byly použity žádné další pravidlo).
  - "Akce" označuje Pokud přenosy ovlivňují toto pravidlo Blokovat nebo povolit.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Pokud chcete toto pravidlo bude přenosy RDP tok z Internetu a RDP port serveru na některý z podsítě v VNET. Pokud chcete toto pravidlo používá dva typy zvláštní předpony adres; "VIRTUAL_NETWORK" a "INTERNET". Toto je snadný způsob, jak lze řešit větší kategorie předpony adres.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Pokud chcete toto pravidlo umožňuje příchozí internetový provoz narazí na webový server. Tím nezmění směrování chování; umožňuje pouze přenosy určené pro IIS01 předat. Tedy pokud komunikace z Internetu měli webového serveru jako cíle toto pravidlo by mohla a zastavit zpracování dalších pravidel. (V pravidlech prioritou 140 veškerý ostatní příchozí internetový provoz blokován). Pokud máte jenom zpracování přenosy protokolu HTTP, může toto pravidlo další omezení pouze umožňuje cílový Port 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Pokud chcete toto pravidlo umožňuje přenos předat ze serveru IIS01 AppVM01 serveru, pozdější pravidlo bloků všech Frontend do back-end přenosy. Je-li port známo, že má být přidán zlepšit toto pravidlo. Například pokud serveru IIS je zasažení pouze serveru SQL Server na AppVM01, oblasti cílový Port by měl být změnil z "*" (žádné) do 1433 (SQL port), takže menší příchozí napadení na AppVM01 by měl webové aplikace někdy být ohroženo.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Pokud chcete toto pravidlo zakazuje přenosy z Internetu na všechny servery v síti. V kombinaci s pravidlem prioritou 110 a 120 umožňuje pouze příchozí internetový provoz brány firewall a porty RDP na jiné servery a bloků všechno ostatní. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  Výsledný pravidlo zakazuje přenosy z podsítě Frontend podsítě back-end. Protože se jedná pravidlo pro příchozí připojení jenom obráceném přenosy povoleno (z back-end k Frontend).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Přenosy scénáře
#### <a name="allowed-web-to-web-server"></a>(*Povolené*) Na webový Server Web
1.  Stránka s uživatelem žádosti o HTTP Internet z FrontEnd001.CloudApp.Net (Internet protilehlé cloudové služby)
2.  Cloudových služeb průchodů přenosů otevřít koncového bodu na portu 80 směrem IIS01 (webový server)
3.  Frontend podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    Použití NSG pravidla 3 (Internet kvůli IIS01), je povolené, zastavte pravidlo zpracování
4.  Přenosy narazí interní IP adresu webového serveru IIS01 (10.0.1.5)
5.  IIS01 web přenosy přijímá, přijímá tento žádosti a spustí zpracování požadavku
6.  IIS01 se zeptá SQL Server na AppVM01 informace
7.  Žádná odchozího pravidla na Frontend podsítě jsou povoleny přenosy
8.  Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    NSG pravidla 3 (Internet kvůli brány Firewall) není použití, přesuňte do další pravidla
  4.    4 NSG pravidlo použít (IIS01 k AppVM01) jsou povoleny přenosy, zastavit zpracování pravidel
9.  AppVM01 obdrží dotaz SQL zobrazený a odpoví
10. Protože neexistují žádná odchozího pravidla na podsítě back-end jsou povolené odpověď
11. Frontend podsítě začíná zpracování příchozí pravidla:
  1.    Neexistuje žádný NSG pravidlo, které se vztahuje na vstupní adres podsítí back-end Frontend podsítě, takže žádné NSG pravidla použít
  2.    Výchozí systém pravidlo povolení přenosu mezi podsítí by přenosy tento tak přenos je povolen.
12. Serveru IIS obdrží odpověď SQL a dokončí odpověď HTTP a odešle žadateli předán
13. Protože jsou povolené žádná odchozího pravidla na podsítě Frontend odpověď a Internet uživatel obdrží požadavku na webovou stránku.

#### <a name="allowed-rdp-to-backend"></a>(*Povolené*) RDP do back-end
1.  Správce serveru na internet požádá o RDP relace AppVM01 na BackEnd001.CloudApp.Net:xxxxx xxxxx-li číslo portu náhodně přiřazeného pro RDP AppVM01 (přiřazený port najdete na portálu Azure nebo pomocí Powershellu)
2.  Back-end podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Použití NSG pravidlo 2 RDP (), je povolené, zastavte pravidlo zpracování
3.  Žádná odchozího pravidla platí výchozí pravidla a jsou povoleny zpáteční přenosy
4.  Relace RDP je povolena
5.  AppVM01 zobrazí dotaz na uživatelské jméno heslo

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Povolené*) Vyhledání Server DNS webu na serveru DNS
1.  Webový Server, IIS01, potřeb datového kanálu na www.data.gov, ale je potřeba přeložit adresu.
2.  Konfigurace sítě pro seznamy VNet DNS01 (10.0.2.4 podsítě back-end) jako primární server DNS, IIS01 odešle žádost o DNS DNS01
3.  Žádná odchozího pravidla na Frontend podsítě jsou povoleny přenosy
4.  Back-end podsítě začíná zpracování příchozí pravidla:
  1.     Použití NSG pravidlo 1 (DNS), je povolené, zastavte pravidlo zpracování
5.  DNS server obdrží žádost
6.  DNS server nemá adresu mezipaměti a dotazem kořenový server DNS na Internetu
7.  Žádná odchozího pravidla na back-end podsítě jsou povoleny přenosy
8.  Server Internet DNS odpovídá, protože pro tuto relaci vznikla interně, jsou povolené odpověď
9.  Uloží odpověď na DNS server a odpoví na původní žádost zpátky na IIS01
10. Žádná odchozího pravidla na back-end podsítě jsou povoleny přenosy
11. Frontend podsítě začíná zpracování příchozí pravidla:
  1.    Neexistuje žádný NSG pravidlo, které se vztahuje na vstupní adres podsítí back-end Frontend podsítě, takže žádné NSG pravidla použít
  2.    Výchozí systém pravidlo povolení přenosu mezi podsítí by přenosy tento tak přenos je povolen
12. IIS01 obdrží odpovědi od DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Povolené*) Soubor aplikace access webového serveru na AppVM01
1.  IIS01 zeptá souboru na AppVM01
2.  Žádná odchozího pravidla na Frontend podsítě jsou povoleny přenosy
3.  Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    NSG pravidla 3 (Internet kvůli IIS01) není použití, přesuňte do další pravidla
  4.    4 NSG pravidlo použít (IIS01 k AppVM01) jsou povoleny přenosy, zastavit zpracování pravidel
4.  AppVM01 obdrží žádost a odpoví souboru (za předpokladu, že přístup je povoleno)
5.  Protože neexistují žádná odchozího pravidla na podsítě back-end jsou povolené odpověď
6.  Frontend podsítě začíná zpracování příchozí pravidla:
  1.    Neexistuje žádný NSG pravidlo, které se vztahuje na vstupní adres podsítí back-end Frontend podsítě, takže žádné NSG pravidla použít
  2.    Výchozí systém pravidlo povolení přenosu mezi podsítí by přenosy tento tak přenos je povolen.
7.  Serveru IIS obdrží soubor

#### <a name="denied-web-to-backend-server"></a>(*Odepřen*) Webový server back-end
1.  Uživatel Internet pokusí o přístup k souboru na AppVM01 prostřednictvím služby BackEnd001.CloudApp.Net
2.  Od otevřený pro sdílení souborů nejsou koncové body, to by předat cloudové služby a nebudete kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, tento provoz by blokovat NSG pravidlo 5 (Internet kvůli VNet)

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Odepřen*) Vyhledání DNS web na serveru DNS
1.  Internet při pokusu o vyhledání interní DNS záznamu na DNS01 prostřednictvím služby BackEnd001.CloudApp.Net
2.  Když nejsou otevřené DNS pro koncové body, to by předat Cloudovou službu a by kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, NSG pravidlo 5 (Internet kvůli VNet) budou blokovány tento provoz (Poznámka: tohoto pravidla 1 (DNS) by použití ze dvou důvodů, nejdřív adresy zdrojového je internet, pokud chcete toto pravidlo pouze u místní VNet jako zdroj, taky toto pravidlo povolení je tak by nikdy zakázat přenosů)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Odepřen*) Web pro přístup k SQL průchod bránou Firewall
1.  Uživatel Internet požádá SQL data z FrontEnd001.CloudApp.Net (Internet protilehlé cloudové služby)
2.  Protože neexistují žádné koncové body otevřenou pro SQL, to by předat Cloudovou službu a by dosáhla brány firewall
3.  Kdyby koncové body otevřít z nějakého důvodu, začíná podsítě Frontend zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    Použití NSG pravidla 3 (Internet kvůli IIS01), je povolené, zastavte pravidlo zpracování
4.  Přenosy narazí interní IP adresu IIS01 (10.0.1.5)
5.  IIS01 nesleduje na port 1433, takže žádné odpověď na požadavek

## <a name="conclusion"></a>Uzavření
Toto je relativně jednoduchý přímo dopředu způsob izolace podsítě back-end z příchozích.

Další příklady a přehled o hranice zabezpečení sítě najdete [tady][HOME].

## <a name="references"></a>Odkazy
### <a name="main-script-and-network-config"></a>Hlavní skript a konfigurace sítě
Uložte celou skript skript Powershellu. Konfigurace sítě uložte do souboru s názvem "NetworkConf1.xml".
Podle potřeby upravte proměnné definované uživatelem. Skript spustit a pak postupujte podle pokynů instalačního programu pravidlo brány Firewall obsažené v části příkladu 1.

#### <a name="full-script"></a>Úplné skriptu
Bude tento skript, na základě uživatelem definované proměnných;

1.  Připojení k předplatné Azure
2.  Vytvoření nového účtu úložiště
3.  Vytvoření nového VNet a dvě podsítí definované v souboru konfigurace sítě
4.  Vytvoření 4 windows server VMs
5.  Konfigurace, včetně NSG:
  - Vytváření NSG
  - Vyplnění s pravidly
  - Vazba NSG na příslušný podsítí

Tento skript Powershellu by měla běžet místně na internetový připojen PC nebo serveru.

>[AZURE.IMPORTANT] Když tento skript spustit, může se upozornění a další informační zprávy, které se objeví v prostředí PowerShell. Pouze chybové zprávy v červené barvě jsou příčinu.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #   

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
      

#### <a name="network-config-file"></a>Konfigurační soubor sítě
Uložte tento soubor xml aktualizované umístění a přidat odkaz na tento soubor proměnné $NetworkConfigFile ve výše uvedené skriptu.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Ukázky skriptů aplikace
Pokud chcete nainstalovat aplikace vzorku, i další příklady DMZ, jednu poskytuje následující odkaz: [Ukázka skriptu aplikace][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Příchozí DMZ s NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

