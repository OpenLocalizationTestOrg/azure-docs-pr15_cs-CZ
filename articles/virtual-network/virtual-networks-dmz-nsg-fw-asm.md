<properties
   pageTitle="Příklad DMZ – vytvoření DMZ chránit aplikací s bránu Firewall a NSGs | Microsoft Azure"
   description="Vytvoření DMZ s bránu Firewall a síťových skupin zabezpečení (NSG)"
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

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Příklad 2 – Vytvoření DMZ chránit aplikací s bránu Firewall a NSGs

[Vraťte se na stránku osvědčené postupy zabezpečení okraj][HOME]

Tento příklad vytvoří DMZ s bránu firewall, čtyři windows server a skupiny zabezpečení sítě. Bude taky projděte si každý z příslušné příkazy k poskytování hlubší vysvětlení každého kroku. Je také oddíl přenosy scénář poskytnout hloubkové podrobné pokyny, jak pokračuje přenosy přes vrstvy obrana v DMZ. Nakonec do odkazů na oddíl je dokončeno kódem a pokyny k vytváření toto prostředí vyzkoušíte a experimentovat s různým scénáře. 

![Příchozí DMZ s NVA a NSG][1]

## <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje následující:

- Dvě cloudových služeb: "FrontEnd001" a "BackEnd001"
- Virtuální sítě "CorpNetwork" s dvěma podsítí: "FrontEnd" a "Back-end"
- Jedinou skupinu zabezpečení sítě, která se použije pro obě podsítí
- Virtuální spotřebiče sítě, v tomto příkladu bránu NextGen Firewall Barracuda připojené k podsítě Frontend
- Windows Server, který představuje serveru webových aplikací ("IIS01")
- Dvě okna servery, které představují aplikace zpět ukončit servery ("AppVM01", "AppVM02")
- Windows server, který představuje server DNS ("DNS01")

>[AZURE.NOTE] I když tento příklad používá bránu NextGen Firewall Barracuda, v mnoha různých zařízeních virtuální sítě lze použít v tomto příkladu.

V části odkazy pod je skript Powershellu, který bude vytvořit maximálně prostředí jsme je popsali výše. Vytváření VMs a virtuální sítích, i když se dělají skriptem příklad nejsou píše v tomto dokumentu.
 
Vytvoření prostředí:

  1.    Uložení souboru xml konfigurace sítě zahrnuté v části odkazy (aktualizován jména, umístění a IP adresy tak, aby odpovídaly daný scénář)
  2.    Aktualizovat uživatelské proměnné skriptu prostředí, které má být skript kontrolovat (předplatná, názvy služeb atd.)
  3.    Spouštět v prostředí PowerShell

**Poznámka**: oblast označený ve skript Powershellu se musí shodovat oblasti označený ve konfiguračního souboru xml sítě.

Po spuštění skriptu úspěšně následující kroky po skriptu úvahy:

1.  Nastavení pravidel brány firewall, to je součástí následující oddíl: pravidla brány Firewall.
2.  Pokud chcete v části odkazy jsou dva skripty k nastavení webu a aplikace server s jednoduché webové aplikace umožňuje testováním pomocí této konfiguraci DMZ.

Další část vysvětluje většinu příkazů skriptů vzhledem k síti skupiny zabezpečení.

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení síti (NSG)
V tomto příkladu skupinu NSG vytvořené a potom načte s šesti pravidla. 

>[AZURE.TIP] Obecně mají zvláštní pravidla "Povolit" nejprve vytvoříte a potom obecnější pravidla "Odepřít" naposledy. Vyžaduje přiřazené priority, které jsou pravidla Vyhodnocená první. Jakmile přenosy nachází použít u určitého pravidla, jsou vyhodnoceny žádná další pravidla. Pravidla NSG můžete použít buď směrem příchozí nebo odchozí (z hlediska podsítě).

Deklarativně jsou u příchozích vytvářeného následující pravidla:

1.  Povolené interní přenos DNS (port 53)
2.  Povolené RDP umožnění datových přenosů (port 3389) z Internetu do libovolné OM
3.  Jsou povoleny přenosy protokolu HTTP (port 80) z Internetu na NVA (firewallem)
4.  Jsou povoleny všechny přenosy (všechny porty) z IIS01 k AppVM1
5.  Všechny přenosy (všechny porty) z Internetu celé VNet (obě podsítí) byl odepřen.
6.  Veškerou odchozí komunikaci (všechny porty) služby podsítě Frontend podsítě back-end byl odepřen.

S následujícími pravidly vázaný na každé podsítě, pokud žádost HTTP příchozí z Internetu k webovému serveru, jak pravidla 3 (Povolit) a 5 (Odepřít) platit, ale od pravidlo 3 musí mít vyšší prioritu pouze byste měli použít a pravidlo 5 nebude možné uplatnit. Žádost HTTP bude mít takto možnost si bránu firewall. Pokud tento stejné přenos pokoušel kontaktovat DNS01 server, pravidla 5 (Odepřít) bude jako první použití a přenos nebude moct pořizovat předat na server. Pravidlo 6 (Odepřít) blokuje podsítě Frontend z spolu mluvit podsítě back-end (s výjimkou povolené přenosy v pravidlech 1 a 4), to chrání síť back-end v případě útočník kompromisům webové aplikace on Frontend mohl by mají omezený přístup k back-end "chráněné" síti (jenom pro zdroje vystaven na serveru AppVM01).

Je výchozí odchozí pravidlo, které umožňuje přenosy se k Internetu. V tomto příkladu jsme jste povolení odchozí přenosy a není změna odchozího pravidla. Uzamknout provozu v obou pokynů definované směrování uživatele je potřeba, to je prozkoumat v jiný příklad, který můžete najít v [hlavním zabezpečení hranice dokumentu][HOME].

Výše uvedené popsané NSG pravidla se hodně podobají NSG pravidla v [příkladu 1 - vytvořit jednoduchý DMZ s NSGs][Example1]. Zkontrolujte NSG popis v tomto dokumentu podrobné prohlédnout každé pravidlo NSG a jeho atributy.

## <a name="firewall-rules"></a>Pravidla brány firewall
Správa klienta muset nainstalovat na počítač spravovat bránu firewall a vytvořit konfigurace potřeby. Zobrazit si přečtěte následující dokumentaci z brány firewall (nebo jiných NVA) dodavatelů týkající se správy zařízení. Zbytek Tato část popisuje konfigurace brány sebe sama přes klienta správy dodavatelé (tedy ne Azure portál nebo Powershellu).

Pokyny pro stažení klienta a připojení k Barracuda v tomto příkladě použita najdete tady: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)

V bráně firewall pravidel přesměrování muset vytvořit. Protože v tomto příkladu pouze trasy internetový provoz v vazbou brány firewall a pak na webový server, není nutné pouze jeden přesměrování překladu síťových adres pravidlo. V bráně NextGen Firewall Barracuda v tomto příkladě použita pravidla bude pravidlo cílové překladu síťových adres ("cílová překladu síťových adres") tento přenášena.

Vytvořte následující pravidla (nebo ověření existující výchozí pravidla), počínaje řídicím panelu Správce NG Barracuda klientů, přejděte na kartu Konfigurace získáte v provozní konfiguraci oddíl sada pravidel pro. Síť s názvem "Hlavní pravidla" se zobrazí existující aktivní a deaktivovaný pravidla v bráně firewall. V pravém horním rohu tuto mřížku malá, zelené "a" tlačítka, klikněte v můžete vytvořit nové pravidlo (Poznámka: Brána firewall může být "uzamčené" změny, pokud se zobrazí tlačítko označené jako "Uzamknout" a nejde vytvořit nebo upravit existující pravidla, kliknutím na toto tlačítko "odemknout" Sada pravidel pro a povolit úpravy). Pokud chcete upravit existující pravidlo, zaškrtněte toto pravidlo, klikněte pravým tlačítkem myši a vyberte Upravit pravidlo.

Vytvoření nového pravidla a zadejte název, například "WebTraffic". 

Vzhled ikony pravidlo cílové překladu síťových adres: ![Ikonu překladu síťových adres cíl][2]

Pravidlo samotné vypadat nějak takto:

![Pravidlo brány firewall][3]

Tady žádné příchozí adresu, kterou narazí bránu Firewall snaží kontaktovat HTTP (port 80 a 443 HTTPS) bude odesláno rozhraní "DHCP1 místní IP" bránu Firewall a přesměrování na webový server s IP adresu 10.0.1.5. Vzhledem k tomu přenos chodit na portu 80 a přejdete na webový server na portu 80 bylo nutné stejně jako port. Však cílovém seznamu mohlo být 10.0.1.5:8080 pokud naše Webový Server data na portu 8080 tedy překladu příchozí port 80 v bráně firewall příchozí port 8080 na webový server.

Způsob připojení by měl taky být označeny, pravidla cílové z Internetu, "Dynamické SNAT" je nejvhodnější. 

Přestože vytvořil jenom jedno pravidlo je důležité, jestli je správně nastavený prioritu. Pokud v mřížce všech pravidel v bráně firewall toto nové pravidlo dole (pod pravidlo "BLOCKALL") se nikdy vyskytnou se. Zajistěte nově vytvořený pravidlo pro přenos web nad BLOCKALL pravidlo.

Po jeho vytvoření, musí být posune bránu firewall a pak aktivovat, pokud není to pravidlo změny se projeví. V následující části je popsán postup nabízených a aktivace.

## <a name="rule-activation"></a>Aktivace pravidla
S sada pravidel pro upravit tak, aby přidat toto pravidlo musí být sada pravidel pro nahrané na bránu firewall a aktivovali.

![Aktivace pravidlo brány firewall][4]

V pravém horním rohu klient management jsou clusteru tlačítek. Klikněte na tlačítko "Odeslat změn" změněné pravidla odešlete bránu firewall a potom klikněte na tlačítko "Aktivovat".

S aktivací sada pravidel brány firewall pro tento příklad prostředí sestavení dokončení. Pokud chcete, můžete spustit skripty vytvořit příspěvek v části odkazy přidání aplikace do tohoto prostředí otestovat pod přenosy scénáře.

>[AZURE.IMPORTANT] Je důležité si uvědomit, že nebude klikněte na webový server přímo. Požaduje-li prohlížeč stránky HTTP z FrontEnd001.CloudApp.Net, endpoint HTTP (port 80) předá tyto přenosy brány není webový server. Brána firewall, podle pravidlo vytvoří výše uvedené dále, které vyžadují webový server.

## <a name="traffic-scenarios"></a>Přenosy scénáře

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Povolené) Na webový Server Web průchod bránou Firewall
1.  Stránka s uživatelem žádosti o HTTP Internet z FrontEnd001.CloudApp.Net (Internet protilehlé cloudové služby)
2.  Cloudové služby průchodů přenosů otevřít koncového bodu na portu 80 místní rozhraní brány firewall 10.0.1.4:80
3.  Frontend podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    Použití NSG pravidla 3 (Internet kvůli brány Firewall), je povolené, zastavte pravidlo zpracování
4.  Přenosy narazí interní IP adresu brány firewall (10.0.1.4)
5.  To je přenos portu 80, přesměrování na webový server IIS01 najdete v článku pravidlo předávání brány firewall
6.  IIS01 je sleduje web přenos, obdrží tohoto požadavku a spustí zpracování požadavku
7.  IIS01 se zeptá SQL Server na AppVM01 informace
8.  Žádná odchozího pravidla na Frontend podsítě jsou povoleny přenosy
9.  Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    NSG pravidla 3 (Internet kvůli brány Firewall) není použití, přesuňte do další pravidla
  4.    4 NSG pravidlo použít (IIS01 k AppVM01) jsou povoleny přenosy, zastavit zpracování pravidel
10. AppVM01 obdrží dotaz SQL zobrazený a odpoví
11. Protože neexistují žádná odchozího pravidla na podsítě back-end jsou povolené odpověď
12. Frontend podsítě začíná zpracování příchozí pravidla:
  1.    Neexistuje žádný NSG pravidlo, které se vztahuje na vstupní adres podsítí back-end Frontend podsítě, takže žádné NSG pravidla použít
  2.    Výchozí systém pravidlo povolení přenosu mezi podsítí by přenosy tento tak přenos je povolen.
13. Serveru IIS obdrží odpověď SQL a dokončí odpověď HTTP a odešle žadateli předán
14. Vzhledem k tomu, že je relace překladu síťových adres z bránu firewall, (počátečním) je cílová odpověď pro bránu Firewall.
15. Brána firewall obdrží odpovědi z webového serveru a předá Internet uživatele
16. Protože jsou povolené žádná odchozího pravidla na podsítě Frontend odpověď a Internet uživatel obdrží požadavku na webovou stránku.

#### <a name="allowed-rdp-to-backend"></a>(Povolené) RDP do back-end
1.  Správce serveru na internet požádá o RDP relace AppVM01 na BackEnd001.CloudApp.Net:xxxxx xxxxx-li číslo portu náhodně přiřazeného pro RDP AppVM01 (přiřazený port najdete na portálu Azure nebo pomocí Powershellu)
2.  Protože bránu Firewall je přijímá jenom na adrese FrontEnd001.CloudApp.Net, se nezabývá s tento tok
3.  Back-end podsítě začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Použití NSG pravidlo 2 RDP (), je povolené, zastavte pravidlo zpracování
4.  Žádná odchozího pravidla platí výchozí pravidla a jsou povoleny zpáteční přenosy
5.  Relace RDP je povolena
6.  AppVM01 zobrazí dotaz na uživatelské jméno heslo

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Povolené) Vyhledání Server DNS webu na serveru DNS
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Povolené) Soubor aplikace access webového serveru na AppVM01
1.  IIS01 zeptá souboru na AppVM01
2.  Žádná odchozího pravidla na Frontend podsítě jsou povoleny přenosy
3.  Podsítě back-end začíná zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    NSG pravidla 3 (Internet kvůli brány Firewall) není použití, přesuňte do další pravidla
  4.    4 NSG pravidlo použít (IIS01 k AppVM01) jsou povoleny přenosy, zastavit zpracování pravidel
4.  AppVM01 obdrží žádost a odpoví souboru (za předpokladu, že přístup je povoleno)
5.  Protože neexistují žádná odchozího pravidla na podsítě back-end jsou povolené odpověď
6.  Frontend podsítě začíná zpracování příchozí pravidla:
  1.    Neexistuje žádný NSG pravidlo, které se vztahuje na vstupní adres podsítí back-end Frontend podsítě, takže žádné NSG pravidla použít
  2.    Výchozí systém pravidlo povolení přenosu mezi podsítí by přenosy tento tak přenos je povolen.
7.  Serveru IIS obdrží soubor

#### <a name="denied-web-direct-to-web-server"></a>(Odepřen) Web přímo na webový Server
Protože webový Server, IIS01 a bránu Firewall jsou ve stejném cloudové služby mají být sdíleny stejné veřejné vystaveného IP adresu. Všechny přenosy protokolu HTTP by tedy přesměrováni do brány firewall. Při žádosti by být úspěšně podávané množství, nelze přejít přímo na webový Server, ho předaný, jak navržený, přes bránu Firewall nejdřív. Zobrazit první scénář v této části toku přenosu.

#### <a name="denied-web-to-backend-server"></a>(Odepřen) Webový server back-end
1.  Uživatel Internet pokusí o přístup k souboru na AppVM01 prostřednictvím služby BackEnd001.CloudApp.Net
2.  Od otevřený pro sdílení souborů nejsou koncové body, to by předat cloudové služby a nebudete kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, tento provoz by blokovat NSG pravidlo 5 (Internet kvůli VNet)

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Odepřen) Vyhledání DNS web na serveru DNS
1.  Internet při pokusu o vyhledání interní DNS záznamu na DNS01 prostřednictvím služby BackEnd001.CloudApp.Net
2.  Když nejsou otevřené DNS pro koncové body, to by předat Cloudovou službu a by kontaktovat server
3.  Kdyby koncové body otevřít z nějakého důvodu, NSG pravidlo 5 (Internet kvůli VNet) budou blokovány tento provoz (Poznámka: tohoto pravidla 1 (DNS) by použití ze dvou důvodů, nejdřív adresy zdrojového je internet, pokud chcete toto pravidlo pouze u místní VNet jako zdroj, taky toto pravidlo povolení je tak by nikdy zakázat přenosů)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Odepřen) Web pro přístup k SQL průchod bránou Firewall
1.  Uživatel Internet požádá SQL data z FrontEnd001.CloudApp.Net (Internet protilehlé cloudové služby)
2.  Vzhledem k tomu, že je otevřeno žádné koncové body pro SQL, to by předat Cloudovou službu a by dosáhla bránu firewall.
3.  Kdyby koncové body otevřít z nějakého důvodu, začíná podsítě Frontend zpracování příchozí pravidla:
  1.    NSG pravidlo 1 (DNS) není použití, přesuňte do další pravidla
  2.    Není použití, přesuňte do další pravidlo NSG pravidlo 2 RDP)
  3.    Použití NSG pravidla 2 (Internet kvůli brány Firewall), je povolené, zastavte pravidlo zpracování
4.  Přenosy narazí interní IP adresu brány firewall (10.0.1.4)
5.  Brána firewall má žádné pravidla pro předávání pro SQL a vynechává provoz

## <a name="conclusion"></a>Uzavření
Tímto způsobem relativně přímo dopředu ochrany aplikace s bránu firewall a izolace podsítě back-end z příchozích.

Další příklady a přehled o hranice zabezpečení sítě najdete [tady][HOME].

## <a name="references"></a>Odkazy
### <a name="main-script-and-network-config"></a>Hlavní skript a konfigurace sítě
Uložte celou skript skript Powershellu. Konfigurace sítě uložte do souboru s názvem "NetworkConf2.xml".
Podle potřeby upravte proměnné definované uživatelem. Skript spustit a pak postupujte podle pokynů instalačního programu pravidlo brány Firewall výše.

#### <a name="full-script"></a>Úplné skriptu
Tento skript budou na základě proměnných definované uživatelem:

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
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
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
       myFirewall - 10.0.1.4
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
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
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
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
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
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Příchozí DMZ s NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ikona překladu síťových adres cíl"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Pravidlo brány firewall"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Aktivace pravidlo brány firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
