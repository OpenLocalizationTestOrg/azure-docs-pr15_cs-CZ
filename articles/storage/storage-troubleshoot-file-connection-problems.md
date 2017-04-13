<properties
    pageTitle="Řešení potíží úložiště souborů Azure | Microsoft Azure"
    description="Odstraňování potíží úložiště souborů Azure"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Poradce při potížích úložiště souborů Azure

Tento článek obsahuje seznam běžných problémů, které se vztahují k ukládání souborů Microsoft Azure při připojení klientů Windows a Linux. Umožňuje také možné příčiny a řešení těchto problémů.

**Všeobecné problémy (dojít v systému Windows a Linux klientech)**

- [Chyby kvóty při pokusu o otevření souboru](#quotaerror)

- [Snížení výkonu z Windows nebo Linux přistupujete úložiště souborů Azure](#slowboth)

**Problémy klientů systému Windows**

- [Snížení výkonu při otevření souboru Azure úložiště z Windows 8.1 nebo Windows Server 2012 R2](#windowsslow)

- [Chyba 53 pokoušející se připojit Azure sdílené složky](#error53)

- [Čistý použití byl úspěšný, ale není vidět sdílení připojenou v programu Průzkumník Windows Azure souboru](#netuse)

- [Můj účet úložiště obsahuje "/" a sítě použít příkaz dojde k chybě](#slashfails)

- [Moje aplikace a služby nemáte přístup ke připojené soubory Azure jednotky.](#accessfiledrive)

- [Další doporučení pro optimalizaci výkonu](#additional)

**Problémy klientů Linux**

- [Chyba "Je kopírování souboru do cíle, který nepodporuje šifrování" při nahrávání nebo kopírování souborů k souborům Azure](#encryption)

- [Sdílení "Host (hostitel) je dolů" Chyba na existující soubor nebo prostředí zablokování při provádění seznam příkazů na bodu připojení](#errorhold)

- [Připojit chyby 115 při pokusu o připojení Azure souborů na Linux OM](#error15)

- [Linux OM dochází k náhodné zpožděním při příkazům typu "ls"](#delayproblem)

## <a name="general-problems"></a>Všeobecné problémy
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Chyby kvóty při pokusu o otevření souboru

Ve Windows dostáváte chybové zprávy o vypadat takto:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Nedostatečná kvóta je k dispozici k provedení tohoto příkazu**

**Úchyt pro neplatná hodnota došlo: 53**

Na Linux dostáváte chybové zprávy o vypadat takto:

**<filename>[oprávnění odepřen]**

**Překročená kvóta disku**

#### <a name="cause"></a>Příčina

K problému, protože jste dosáhli horní mez souběžné otevřené úchytů, které jsou povoleny k souboru.

#### <a name="solution"></a>Řešení

Snižte počet souběžné otevřít úchytů zavřením některé úchytů a opakujte. Další informace najdete v tématu [Microsoft Azure úložiště výkon a škálovatelnost kontrolního seznamu](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Snížení výkonu při přístupu k ukládání souborů systému Windows nebo Linux

- Pokud nemáte konkrétní požadavek minimální velikost vstupu a výstupu, doporučujeme použít 1 MB jako velikost vstupu a výstupu pro optimální výkon.

- Pokud znáte konečný velikost souboru, který bude rozšiřovat s zápisy a software nemá problémy s kompatibilitou při ještě písemné zadní na soubor, ve kterém nuly, nastavte velikost souboru předem místo každé zápisu právě rozšíření zápisu.

## <a name="windows-client-problems"></a>Problémy klientů systému Windows
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Snížení výkonu při přístupu k ukládání souborů z Windows 8.1 nebo Windows Server 2012 R2

Pro klienty, kteří se systémem Windows 8.1 nebo Windows Server 2012 R2 Ujistěte se, že je nainstalovaný hotfix [KB3114025](https://support.microsoft.com/kb/3114025) . Opravy zlepšuje vytvořit a zavřete úchyt výkonu.

Spuštěním následujícího skriptu zjistit, zda byla hotfix nainstalované v:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Pokud je nainstalovaná hotfix, se zobrazí následující výstup:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Hotfix KB3114025 ve výchozím nastavení spuštění v 2015 prosinec nainstalovaný obsahovat Windows serveru 2012 R2 obrázky v Azure Marketplace.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Další doporučení pro optimalizaci výkonu

Nikdy vytvořte nebo otevřete soubor pro režim cached vstup / požaduje přístup pro zápis, ale ne přístup pro čtení. To znamená, když volání **Funkce CreateFile()**, nikdy zadat jenom **všeobecné_zápis**, ale vždy určit **všeobecné_čtení | Všeobecné_zápis**. Zápis jen ty nelze ukládat do mezipaměti Malé zápisy místně, i když máte otevřený úchyt souboru. Toto Malé zápisy ukládá snížení špatných výkonu. Všimněte si, že "na" dopravy CRT **fopen()** otevře ty jen pro zápis.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Chyba 53" při pokusu o připojení nebo odpojení sdílené složky Azure

Tento problém můžou způsobovat následující podmínky:

#### <a name="cause-1"></a>Příčina 1

"Systém 53 došlo k chybě. Přístup odepřen." Z bezpečnostních důvodů připojení ke sdíleným složkám Azure soubory blokovány, pokud není zašifrovaných komunikační kanál a pokus o připojení není vytvořený na stejné datovém centru, ve kterém jsou umístěny Azure sdílené složky. Šifrování kanálu komunikace není k dispozici klienta uživatele OS, které nejsou podporovány SMB šifrování. To je označen "systém 53 došlo k chybě. Přístup odepřen"při pokusu o připojení sdílené složky z místního nebo z jiné datovém centru uživatele zobrazí chybová zpráva. Windows 8, Windows Server 2012 a novějších verzích každý požadavek potvrdit, který obsahuje SMB 3.0, který podporuje šifrování.

#### <a name="solution-for-cause-1"></a>Řešení pro příčinu 1

Připojení z klienta, který splňuje požadavky na systém Windows 8, Windows Server 2012 nebo novější verze nebo, která připojení z počítače virtuální, který je v centru dat jako účet Azure úložiště, který se používá pro sdílení souborů Azure.

#### <a name="cause-2"></a>Příčina 2

"Chyba systému 53" při připojování Azure sdílené složky může dojít, pokud je blokován Port 445 odchozí komunikaci datacentrem Azure soubory. Klepnutím [sem](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) zobrazíte souhrn poskytovatelé, kteří povolit nebo zakázat přístup z portu 445.

Společnosti Comcast a některých organizací IT blokovat tento port. Pokud chcete zjistit, zda je důvod, proč za zpráva "Chyba systému 53", můžete k vytvoření dotazu na koncový bod TCP:445 Portqry. Pokud koncový bod TCP:445 se zobrazí jako filtrována, je blokován TCP port. Tady je příklad dotazu:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Pokud TCP 445 blokován pravidlem cestě sítě, zobrazí se následující výstup:

**Port TCP 445 (služba microsoft ds): FILTROVÁNÍ**

Další informace o použití Portqry najdete v článku [Popis nástroje Portqry.exe příkazového řádku](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Řešení pro příčina 2

Práce s vaší organizací IT otevřete Port 445 odchozích [Azure IP](https://www.microsoft.com/download/details.aspx?id=41653)oblastí.

#### <a name="cause-3"></a>Příčina 3

Pokud je povoleno NTLMv1 komunikace na straně klienta také stavu "Chyba systému 53". S NTLMv1 povolené vytvoří zabezpečené méně klienta. Proto komunikace budou blokované soubory Azure. Pokud chcete ověřit, zda je příčinou chyby do schránky, ověřte, že na tento podklíč registru je nastavený na hodnotu 3:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Další informace naleznete v tématu [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) na TechNetu.

#### <a name="solution-for-cause-3"></a>Řešení pro příčina 3

Tento problém vyřešíte vrátit hodnotu LmCompatibilityLevel v klíči registru HKLM\SYSTEM\CurrentControlSet\Control\Lsa výchozí hodnotu 3.

Azure soubory podporuje pouze ověřování NTLM verze 2. Ujistěte se, že je zásada skupiny klientům. To bude zabránit tato chyba. To je také považuje z bezpečnostních důvodů. Další informace najdete v tématu [jak nakonfigurovat NTLM 2 prostřednictvím zásad skupiny](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Doporučené zásadu je **jenom odpovědi odesílat NTLM verze 2**. Odpovídá registru hodnotu 3. Klienti ověřování pouze NTLM 2 a používají zabezpečení relace NTLM verze 2, pokud je server podporuje. Řadiče domény přijímají ověřování LM, NTLM a NTLM 2.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Čistý použití byl úspěšný, ale není vidět sdílení připojenou v programu Průzkumník Windows Azure souboru

#### <a name="cause"></a>Příčina

Ve výchozím nastavení Průzkumník nejde spustit jako správce. Pokud dostanete **čisté použití** z příkazového řádku správce mapujete síťové jednotce "Jako správce." Protože jsou namapované zaměřené na uživatele, není zobrazena uživatelský účet, který je přihlášena jednotky, pokud připojený pod jiný uživatelský účet.

#### <a name="solution"></a>Řešení

Příkaz sdílet připojte z příkazového řádku bez oprávnění správce. Můžete taky můžete postupovat podle [tohoto tématu TechNetu](https://technet.microsoft.com/library/ee844140.aspx) konfigurace hodnotu registru **EnableLinkedConnections** .

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Můj účet úložiště obsahuje "/" a sítě použít příkaz dojde k chybě

#### <a name="cause"></a>Příčina

Po spuštění příkazu **čisté use** v části příkazového řádku (cmd.exe) je analyzovat přidáním "/" mezi možnostmi příkazového řádku. To způsobí selhání mapování jednotky.

#### <a name="solution"></a>Řešení

Použijte některý z následujících kroků pro tento problém vyřešit:

• Pomocí následujícího příkazu Powershellu:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Ze souboru dávku lze to jako

`Echo new-smbMapping ... | powershell -command –`

• Umístění dvojitých uvozovkách klíč k alternativním řešením tohoto problému – Pokud "/" je první znak. Pokud ano, použijte interaktivní režimu a zadejte svoje heslo samostatně nebo obnovovat klíče k získání kódu Product key nespouští se znakem lomítko (/).

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Moje aplikace a služby nemáte přístup ke připojené soubory Azure jednotky

#### <a name="cause"></a>Příčina

Jednotky jsou připojeny za uživatele. Pokud je v části jiný uživatelský účet aplikace nebo služby, nezobrazí uživatelé jednotku.

#### <a name="solution"></a>Řešení

Jednotka připojení ze stejného uživatelského účtu, pod kterým je aplikace. Lze to provést pomocí nástrojů, jako jsou psexec.

Můžete taky můžete vytvořit nový uživatel, který má stejná oprávnění jako účet služby nebo systém sítě a spusťte **čistý použijte** v části tohoto účtu a **cmdkey** . Název účtu úložiště by měl být uživatelské jméno a heslo by měla být klíč účtu úložiště. Další možností pro **čisté použití** je předat název účtu úložiště a zadejte uživatelské jméno a heslo parametry příkazu **čisté use** .

Po provedení těchto pokynů se může zobrazit tato chybová zpráva: "systém 1312 došlo k chybě. Zadaná přihlašovací relace neexistuje. Ji může již byly ukončeny"při spuštění **čisté použití** účtu služby systému nebo síti. Pokud k tomu dojde, zkontrolujte, že uživatelské jméno, které je **čistý** použití zahrnuje informace domény (třeba: "[název účtu úložiště]. file.core.windows .net").

## <a name="linux-client-problems"></a>Problémy klientů Linux

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Chyba "Je kopírování souboru do cíle, který nepodporuje šifrování"

#### <a name="cause"></a>Příčina

Nástroje BitLocker zašifrovaných souborů můžete zkopírovat Azure souborů. Ukládání souborů nepodporuje NTFS EFS. Proto že pravděpodobně používáte EFS v tomto případě. Pokud máte soubory, které jsou šifrované EFS, může docházet k chybám kopírování k základnímu úložišti soubor Pokud příkaz Kopírovat dešifrování zkopírovaného souboru.

#### <a name="workaround"></a>Řešení:

Kopírování souboru do úložiště souborů, můžete ho nejdřív dešifrovat. Můžete to udělat jedním z těchto způsobů:

• Použijte **možnost/d. kopírovat**.

• Nastavit následujícímu klíči registru:

- Cesta = HKLM\Software\Policies\Microsoft\Windows\System
- Typ hodnoty = DWORD
- Název = CopyFileAllowDecryptedRemoteDestination
- Hodnota = 1

Všimněte si, že nastavení klíče registru ovlivní všechny operace Kopírovat do sítě však sdílí.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Sdílení "Host (hostitel) je dolů" Chyba na existující soubor nebo prostředí přestane reagovat, když spustíte seznam příkazů na bodu připojení

#### <a name="cause"></a>Příčina

Tento dojde k chybě v klientském počítači Linux klienta je nečinný delší dobu. Když k této chybě dochází, odpojení klienta a připojení klienta vyprší její časový limit.

#### <a name="solution"></a>Řešení

Tento problém se teď podařilo odstranit jádra Linux jako součást [změnit nastavení](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), na zjištění stavu úkolů backport do rozdělení Linux.

Works řešením tohoto problému udržet připojení a vyhněte se dál pustíme do stavu nečinnosti, takže souboru v Azure sdílené složky, který napíšete pravidelně. Je třeba operace zápisu, například přepisování datum vytvoření a změny v souboru. Jinak může se zobrazit výsledky uložené v mezipaměti a operaci nemusí aktivovat připojení.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Připojit chyby 115" při pokusu o připojení souborů Azure na Linux OM

#### <a name="cause"></a>Příčina

Distribuce Linux ještě nepodporují funkce šifrování v SMB 3.0. V některých distribuce uživatel může "115" chybová zpráva Pokud se pokusíte Azure připojení souborů pomocí SMB 3.0 z důvodu chybějící funkce.

#### <a name="solution"></a>Řešení

Pokud Linux SMB klienta, který se používá nepodporuje šifrování, Azure připojení souborů pomocí SMB 2.1 z Linux OM v centru dat jako účet úložiště souborů.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux OM dochází k náhodné zpožděním při příkazům typu "ls"

#### <a name="cause"></a>Příčina

Tato situace může nastat, pokud tento příkaz neobsahuje možnost **serverino** . Bez **serverino**spustí příkaz ls **stat** na každý soubor.

#### <a name="solution"></a>Řešení

Kontrola **serverino** "/ atd/fstab" položky:

azureuser.File.Core.Windows.NET/WMS/Comer na/Domů/sampledir typ cifs (rw, nodev, relatime, ver = 2.1, sec = ntlmssp mezipaměti = striktně, uživatelské jméno = xxx, domény = X, file_mode = 0755, dir_mode = 0755, serverino, parametru rsize = 65536 wsize = 65536 actimeo = 1)

Pokud možnost **serverino** neobsahuje data, odpojte a připojit znovu soubory Azure tak, že s vybranou možností **serverino** .

## <a name="learn-more"></a>Víc se uč

- [Začínáme s úložiště souborů Azure v systému Windows](storage-dotnet-how-to-use-files.md)

- [Začínáme s úložiště souborů Azure na Linux](storage-how-to-use-files-linux.md)
