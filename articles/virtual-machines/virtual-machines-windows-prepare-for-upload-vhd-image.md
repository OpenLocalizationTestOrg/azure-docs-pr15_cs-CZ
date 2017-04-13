<properties
    pageTitle="Příprava virtuálního pevného disku Windows Azure nahrajete | Microsoft Azure"
    description="Doporučené postupy pro přípravu virtuálního pevného disku Windows před nahráním na Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Příprava Windows virtuální pevný disk na Azure
Nahrajte Windows OM z místního na Azure, je třeba správně připravit virtuální pevný disk (virtuálního pevného disku). Existuje několik doporučené kroky, abyste mohli dokončit před odesláním virtuálního pevného disku Azure. V tomto článku se dozvíte, jak připravit Windows virtuální pevný disk na Microsoft Azure a také vysvětlení [kdy a jak používat Sysprep](#step23).

## <a name="prepare-the-virtual-disk"></a>Příprava virtuální disk

>[AZURE.NOTE] 
> Azure podporuje pouze [generování 1 virtuálních počítačích](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) , které jsou ve formátu virtuální pevný disk. Novější formát VHDX není podporován v Azure. 
>
> Virtuální pevný disk musí být pevnou velikost, nikoli dynamické. V případě potřeby tyto kroky udělat podrobností převod z VHDX nebo dynamické discích. Maximální velikost povolené virtuálního pevného disku je 1,023 GB.


Ujistěte se, že virtuální pevný disk Windows funguje zařízení správně místního serveru. Vyřešení chyby v OM samotné před pokusem převést nebo odeslání do Azure.

Pokud potřebujete virtuální disk převést na požadovaný formát pro Azure, použijte jeden z způsobů uvedeno v následujících částech. Před spuštěním proces převodu virtuálního disku nebo Sysprep obecnějším údajům OM.

### <a name="convert-using-hyper-v-manager"></a>Převedení správce pro Hyper-V
- Otevřete správce pro Hyper-V a vyberte místního počítače na levé straně. V nabídce nad ním, klikněte na **Akce** > **Upravit disku**.
    - V dialogovém okně **Vyhledejte virtuální pevný Disk** vyhledejte a vyberte virtuální disk.
    - Vyberte příkaz **Převést** na další obrazovce
        - Pokud budete muset převést z VHDX, vyberte **virtuální pevný disk** a klikněte na tlačítko **Další**
        - Pokud budete muset převést z dynamický, vyberte **pevnou velikost** a klikněte na tlačítko **Další**

    - Vyhledejte a vyberte **cestu k souboru nové virtuální pevný disk**.
    - Klikněte na tlačítko **Dokončit** zavřete.

### <a name="convert-using-powershell"></a>Převádět pomocí prostředí PowerShell
Můžete převést na virtuální disk pomocí [rutiny prostředí PowerShell převést virtuální pevný disk](http://technet.microsoft.com/library/hh848454.aspx). V následujícím příkladu jsme jsou převodu z VHDX do virtuálního pevného disku a převod z dynamického na pevné typ:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Převedení z VMware VMDK disku formátu
Pokud máte Windows OM obrázek ve [formátu souboru VMDK](https://en.wikipedia.org/wiki/VMDK)ji převeďte virtuálního pevného disku pomocí [Microsoft Virtual Machine převaděče](https://www.microsoft.com/download/details.aspx?id=42497). Přečtěte si blog [jak převést VMware VMDK do virtuálního pevného disku Hyper-V](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) Další informace.

## <a name="prepare-windows-configuration-for-upload"></a>Příprava konfigurace systému Windows pro nahrání

> [AZURE.NOTE] Spuštění všech následujících příkazů s [oprávněními správce](https://technet.microsoft.com/library/cc947813.aspx).

1. Odebrání všech statické trvalé trasy v tabulce směrování:

    - Pokud chcete zobrazit tabulku směrování, spusťte `route print`.
    - Zaškrtněte políčko v částech **Trvalé trasy** . Pokud tam je trvalý směrování, pomocí [postupu odstranění](https://technet.microsoft.com/library/cc739598.apx) ho odebrat.

2. Odebrání WinHTTP proxy:

    ```
    netsh winhttp reset proxy
    ```

3. Konfigurace diskové SAN zásadu [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Použití čas koordinovaný univerzální čas (UTC) pro Windows a nastavení typu spouštění služby Windows čas (w32time) na **automaticky**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Konfigurace služby systému Windows
5. Ujistěte se, že každá z těchto služeb Windows je nastavena na **Windows výchozí hodnoty**. Budou nakonfigurována nastavení spuštění uvedeno v následujícím seznamu. Můžete použít tyto příkazy k resetování nastavení spuštění:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Konfiguraci Vzdálená plocha
6. Pokud jsou všechny podepsaného certifikáty se posluchače vzdálené plochy RDP (Protocol) stejným, odstraňte je:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Další informace o konfiguraci certifikáty pro posluchače RDP najdete v tématu [Konfigurace certifikát posluchače v systému Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Konfigurace hodnoty [dat pro udržení spojení](https://technet.microsoft.com/library/cc957549.aspx) RDP služby:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Konfigurace režimu ověřování pro službu RDP:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Povolení služby RDP přidáním následující podklíče registru:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Konfigurace pravidel brány Windows Firewall
10. Povolit WinRM až tři profily brány firewall (doménu, soukromé a veřejné) a povolení služby vzdáleného Powershellu:

    ```
    Enable-PSRemoting -force
    ```

11. Ujistěte se, že se tato pravidla Host operační systém bránu firewall na místě:

    - Příchozí

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Příchozí a odchozí

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Odchozí

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Další kroky konfigurace systému Windows
12. Spuštění `winmgmt /verifyrepository` potvrďte, že je konzistentní úložišti správy přístrojového vybavení WMI (Windows). Pokud je poškozený úložiště, podívejte se na [Tento příspěvek blogu](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Ujistěte se, že nastavení spouštění konfigurace dat (BCD) odpovídá takto:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Odebere všechny filtry navíc Transport ovladač rozhraní, jako je software, který analyzuje TCP pakety.
15. Pokud chcete mít jistotu, disk správný a konzistentní, spusťte `CHKDSK /f` příkaz.
16. Odinstalujte další jiných výrobců software a ovladač související s fyzické součásti nebo jiných upgradovaný.
17. Ujistěte se, že aplikace jiných výrobců nepoužívá Port 3389. Tento port se používá pro službu RDP v Azure. Můžete použít `netstat -anob` příkaz Zkontrolovat porty, kteří používají aplikací.
18. Pokud Windows virtuálního pevného disku, který chcete nahrát řadiče domény, postupujte podle [kroků navíc](https://support.microsoft.com/kb/2904015) Příprava disku.
19. Restartujte počítač OM, abyste měli jistotu, že Windows je přesto funkční lze dosáhnout pomocí připojení RDP.
20. Resetujte aktuální heslo místního správce a ujistěte se, že můžete použít tento účet pro přihlášení k Windows přes připojení RDP.  Toto oprávnění řídí objekt zásad "Povolit přihlášení pomocí vzdálené plochy". Tento objekt je umístěn v části "Počítač konfigurace systému zabezpečení\Místní přiřazení uživatelských práv."


## <a name="install-windows-updates"></a>Instalace aktualizací systému Windows
22. Nainstalujte nejnovější aktualizace pro systém Windows. Pokud to není možné, ujistěte se, jestli jsou nainstalované tyto aktualizace:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs není obnovení z výpadku sítě a dojít k problémům s poškozením dat

    - [KB3115224](https://support.microsoft.com/kb/3115224) Vyšší spolehlivost pro VMs spuštěné v systému Windows Server 2012 R2 nebo Windows Server 2012 host

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Aktualizace zabezpečení systému Microsoft Windows adresu zvýšení oprávnění: 8 březen 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Mnoho událostí ID 129 přihlášení k lyncu při spuštění systému Windows Server 2012 R2 virtuálního počítače v Microsoft Azure

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs není přizpůsobuje výpadku sítě a vyskytnout problémy s poškozením dat

    - [KB3114025](https://support.microsoft.com/kb/3114025) Snížení výkonu při otevření Azure soubory úložiště z Windows 8.1 nebo serveru 2012 R2

    - [KB3033930](https://support.microsoft.com/kb/3033930) Hotfix zvyšuje limit 64 kB na RIO rezerv jednotlivé procesy služby Azure v systému Windows

    - [KB3004545](https://support.microsoft.com/kb/3004545) Nemáte přístup ke virtuálních počítačích, které jsou hostované na Azure hostitelské služby prostřednictvím připojení VPN v systému Windows

    - [KB3082343](https://support.microsoft.com/kb/3082343) Použijete-li Azure tunelů VPN k webu Windows serveru 2012 R2 RRAS se ztratí VPN křížově místní připojení

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Aktualizace zabezpečení systému Microsoft Windows adresu zvýšení oprávnění: 8 března 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Popis aktualizace zabezpečení pro CSRSS: 12 duben 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Systém ukotví příčky u během disku vstupu a výstupu v systému Windows<a id="step23"></a>
23. Pokud chcete vytvořit obrázek, který chcete nasadit více počítačů z nich, budete muset generalize obrázek spuštěním `sysprep` před do Azure nahrajete virtuální pevný disk. Není potřeba spustit `sysprep` pro používání specializované virtuálního pevného disku. Další informace o tom, jak vytvořit generalized obrázek najdete v následujících článcích:

    - [Vytvoření obrázku OM z existující OM Azure pomocí Správce prostředků nasazení modelu](virtual-machines-windows-create-vm-generalized.md)
    - [Vytvoření obrázku OM z existující OM Azure pomocí modemu nasazení klasické](virtual-machines-windows-classic-capture-image.md)
    - [Podpora Sysprep role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Konfigurace navrhované navíc

Následující nastavení nemají vliv na nahrávání virtuální pevný disk. Však důrazně doporučujeme je nakonfigurované máte.

- Nainstalujte [Agent Azure virtuálních počítačích](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Po nainstalování agent, můžete povolit OM rozšíření. Rozšíření OM implementovat většina kritické funkce, které chcete použít s VMs jako resetování hesel, konfiguraci RDP a mnoho jinými uživateli.

- Protokol výpis mohou být užitečné při odstraňování potíží selhání systému Windows. Povolení kolekce výpis protokolů:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Po vytvoření OM v Azure konfigurace definované velikost stránkovací na jednotce D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Další kroky

- [Nahrát obrázek OM Windows Azure správce prostředků nasazení](virtual-machines-windows-upload-image.md)
