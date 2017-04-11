Tento článek shrnuje sadu osvědčené postupy pro spuštění Linux virtuálního počítače (OM) na Azure, přičemž pozornost škálovatelnost, dostupnost, správu a zabezpečení. Azure podporuje spuštění různých Oblíbené distribuce Linux, včetně CentOS Debian, červené klobouk organizace, se systémem Ubuntu a FreeBSD. Další informace najdete v tématu [Azure a Linux][azure-linux].

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento článek používá správce zdroje, který Microsoft doporučuje nové nasazení.

Nedoporučujeme pomocí jednoho OM pro výrobní úloh, protože je žádné dobu smlouva o úrovni služeb (SLA) pro jednoho VMs Azure. SLA získáte zaveďte více VMs [Nastavte dostupnost][availability-set]. Další informace najdete v tématu [spouštění více virtuálních na Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Diagram architektury

Zřízení OM v Azure zahrnuje více proměnné části než jenom OM samotné. Existuje výpočetní, sítě a úložiště prvky, které byste měli myslet.

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je na jedné OM "výpočetním -" stránky.

![[0]][0]

- **Pole Skupina zdroje.** [_Pole Skupina zdroje_] [ resource-manager-overview] je kontejner obsahující související materiály. Vytvoření skupiny prostředků k ukládání prostředků pro tento OM.

- **VM**. Můžete vytvořit OM ze seznamu publikované obrázky nebo ze souboru virtuálního pevného disku (virtuální pevný disk), která jste nahráli k úložišti objektů blob Azure.

- **Operační systém disk.** OS disk je virtuální pevný disk uložené v [úložišti Azure][azure-storage]. To znamená, že potrvají i v případě, že na hostitelském počítači havaruje. Disk OS `/dev/sda1`.

- **Dočasné disk.** OM se vytvoří pomocí dočasné disku. Disk je uložený na fyzické jednotky na hostitelském počítači. Je _uložen v Azure úložiště a může během restartování počítače a dalších událostí životního cyklu OM zmizí_ . Pomocí tohoto disku jenom pro dočasné dat, například stránky nebo vyměňovat soubory. Je dočasný disk `/dev/sdb1` a je připojen k `/mnt/resource` nebo `/mnt`.

- **Data discích.** [Datový disk] [ data-disk] je trvalý virtuálního pevného disku použít pro data aplikace. Disků dat uložených v Azure úložiště, jako je disk s operačním systémem.

- **Virtuální sítě (VNet) a podsítě.** Každý OM v Azure nasazení do (VNet), což je dál rozdělit podsítí.

- **Veřejnou IP adresu.** Veřejnou IP adresu je potřeba komunikovat s OM&mdash;třeba přes SSH.

- **Rozhraní sítě (NIC)**. NIC umožňuje OM komunikovat s virtuální sítě.

- **Skupina zabezpečení síti (NSG)**. [NSG] [ nsg] slouží k povolit nebo odepřít v síti do podsítě. NSG můžete propojit s jednotlivé NIC nebo podsítě. Pokud jste ji přidružit podsítě, NSG pravidla platí pro všechny VMs v této podsítě.
 
- **Diagnostika.** Protokolování diagnostiky je důležité pro správu a řešení potíží OM.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura Pokud požadujete konkrétní příliš velký doporučení by měl těmito doporučeními. 

### <a name="vm-recommendations"></a>Doporučení OM

Doporučujeme řady GS a Pošta, protože tyto počítače velikosti podporují [Premium úložiště][premium-storage]. Pokud nemáte specializované pracovní zátěž například výkonných výpočetních clusterů, vyberte jednu z těchto velikostí počítače. Podrobnosti najdete v tématu [virtuálního počítače velikosti][virtual-machine-sizes]. Při přesouvání existující pracovní zátěž do Azure, začněte s velikostí OM, který se nejvíc blíží hledanému místních serverů. Měření výkonu týkající se sloupcem procesor vytížení, paměti disku vstupní a výstupní operací za sekundu (procesorů) a v případě potřeby upravte velikost. Také pokud potřebujete více nic, mějte na paměti NIC omezení pro každou velikost.  

Když zřizujete OM a dalších zdrojů, je třeba určit umístění. Obecně vyberte umístění nejblíže k interním uživatelům nebo zákazníky. Ne všechny velikosti OM však může být k dispozici ve všech umístěních. Další informace najdete v tématu [služby podle regionů][services-by-region]. Seznam velikostí OM dostupných do zadaného umístění, spusťte tento příkaz Azure rozhraní příkazového řádku (rozhraní příkazového řádku):

```
    azure vm sizes --location <location>
```

Informace o volbě publikované obrázek OM, najdete v článku [obrázek a vyberte Azure virtuálního počítače obrázky][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Doporučení disk a úložiště

Nejlepších výsledků dosáhnete disku vstupu a výstupu, doporučujeme [Premium úložiště][premium-storage], které jsou uložená data na pevných látek jednotkách (disky SSD). Pole Náklady podle velikosti zřizování disku. Procesorů a výkon (to znamená přenosu dat sazba) také závisí na velikosti disku, tak když zřizujete disk, zvažte všechny tři faktory (kapacita procesorů a výkon). 

Úložiště účtů podporují VMs 1 až 20.

Přidání jednoho nebo více dat discích. Při vytváření virtuálního pevného disku neformátovaný. Přihlaste se k OM formátovat disk. Disků data zobrazit jako `/dev/sdc`, `/dev/sdd`a tak dál. Můžete spustit `lsblk` seznam blok zařízení, ať discích. Pomocí disk dat, vytvořit oddíl a soubor systému a připojení disku. Příklad:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Pokud máte spoustu disků dat, mějte na paměti celkové vstupu a výstupu limitů úložiště klienta. Další informace najdete v tématu [Limity disku virtuálního počítače][vm-disk-limits].

Po přidání disk dat (logické jednotky) Identifikačního čísla logické jednotky přiřazená disku. Pokud chcete, můžete zadat ID logické jednotky &mdash; například nahrazujete disk a chcete zachovat stejnou ID logické jednotky, případně pokud máte aplikaci, která vypadá pro konkrétní ID logické jednotky. Ale nezapomeňte, že logické jednotky ID musí být jedinečné pro každý disk.

Můžete chtít změnit plánovači vstupu a výstupu optimalizovat výkon pevných látek jednotky (disky SSD) (používané Premium úložiště). Společné doporučení je pro účely disky SSD plánovači nedostupný (NoOp), ale nástroj například [iostat] byste měli použít ke sledování disku výkon vstupu a výstupu pro konkrétní pracovní zátěž.

- Nejlepších výsledků dosáhnete vytvořte účet samostatný úložiště k ukládání diagnostických protokolů. Protokoly pro diagnostiku stačí standardní účet místně nadbytečné úložiště (LRS).


### <a name="network-recommendations"></a>Doporučení sítě

Na veřejnou IP adresu lze dynamické nebo statické. Výchozí hodnota je dynamické.

- Rezervovat [statické IP adresy] [ static-ip] potřebnosti pevné IP adresu, která se nemění &mdash; , pokud například potřebujete k vytvoření záznamu DNS, nebo potřebujete IP adresu být povolené.

- Můžete taky vytvořit plně kvalifikovaný název domény (FQDN) na IP adresu. Můžete pak registrovat [záznam CNAME] [ cname-record] v systému DNS, který odkazuje plně kvalifikovaný název domény. Další informace najdete v tématu [vytvoření plně kvalifikovaný název domény na portálu Azure][fqdn].

Všechny NSGs obsahují sady [pravidel výchozí][nsg-default-rules], včetně pravidlo, které blokuje všechny příchozí internetový provoz. Výchozí pravidla nelze odstranit, ale ostatní pravidla přepsat. Povolení internetové přenosy pro vytvoření pravidla, která povolí příchozí přenosy na konkrétní porty &mdash; například porty 80 pro HTTP.  

Aby SSH do NSG umožňující příchozích TCP port 22 přidáte pravidlo.

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

Je možné přizpůsobit virtuálního počítače nahoru nebo dolů změnou [velikosti OM][vm-resize]. 

Zobrazit si vodorovně, přepněte do dostupné nastavit za vyrovnávání zatížení dva nebo více VMs. Podrobnosti najdete v tématu [spouštění více virtuálních na Azure][multi-vm].

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Jak je uvedeno, je žádné SLA pro jeden OM. Pokud chcete dostat SLA, musíte nasadíte více VMs do sady dostupnosti.

Vaše OM možná ovlivněny [plánovaná údržba] [ planned-maintenance] nebo [neplánovanou údržbu][manage-vm-availability]. Můžete použít [OM restartujte protokoly] [ reboot-logs] a zjistit, zda byla restartování OM způsobená plánovanou údržbu.

VHD jsou podporovaným [Úložišti Azure][azure-storage], které je replikovat životnosti a k dispozici.

Chránit před ztrátou náhodné dat během normální operace (například z důvodu chyba uživatele), měli byste taky implementovat zálohy v okamžiku, použití [snímků objektů blob] [ blob-snapshot] nebo jiný nástroj.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

**Skupiny zdrojů.** Umístění pevně vázaný zdroje, které mají stejné životního cyklu do stejné [pole Skupina zdroje][resource-manager-overview]. Skupiny zdrojů umožňují nasazení a sledovat zdroje jako skupinu a shrnout fakturace nákladů podle skupiny zdrojů. Můžete taky odstranit zdroje jako sady, což je velmi užitečné pro zkušební nasazení. Zadejte smysluplné názvy zdrojů. Který usnadňuje vyhledání určitého zdroje a interpretaci svoji roli. V tématu [Doporučené konvence pro Azure zdroje][naming conventions].

**SSH**. Před vytvořením Linux OM vygenerovat 2048bitový RSA veřejný privátní klíč. Použití veřejných klíčové souboru při vytváření OM. Další informace najdete v tématu [jak používat SSH s Linux a Mac na Azure][ssh-linux].

**Diagnostika OM.** Povolit sledování a diagnostice, včetně základní stav metriky, protokolování diagnostiky infrastruktury a [diagnostice spouštěcí][boot-diagnostics]. Spuštění diagnostiky můžete Diagnostika Chyba při spuštění svého OM získá do-spouštěcí stavu. Další informace najdete v tématu [Povolení sledování a diagnostice][enable-monitoring].  

Následující příkaz rozhraní příkazového řádku povolí diagnostických nástrojů:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Zastavení virtuálního počítače.** Azure rozlišuje "Zastaveno" a "Deallocated" USA. Pokud je stav OM "zastaven" bude účtovaná. Které nejsou při OM odebrána.

Pomocí následujícího příkazu rozhraní příkazového řádku zrušit virtuálního počítače:

```
    azure vm deallocate <resource-group> <vm-name>
```

- Tlačítko **Ukončit** na portálu Azure taky zruší přidělení OM. Ale pokud vypnete prostřednictvím operační systém při přihlášení, se zastaví OM, ale _Ne_ uvolnit, tak je pořád strhne příslušná.

**Odstranění virtuálního počítače.** Pokud byste odstranili virtuálního počítače, VHD se neodstraní. To znamená, že že OM bez ztráty dat bezpečně odstranit. Však můžete pořád strhne příslušná úložiště. Pokud chcete odstranit virtuálního pevného disku, odstraňte soubor z [úložiště objektů blob][blob-storage].

Proti nechtěným odstraněním použít [Zámek prostředků] [ resource-lock] zamknout celý zdroj skupiny nebo zámek jednotlivých zdrojů, například OM. 

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Automatizace OS aktualizace pomocí rozšíření OM [OSPatching] . Když zřizujete OM nainstalujte tuto linku. Můžete určit, jak často se má instalace oprav a jestli se mají po opravy restartovat počítač.

Pomocí [řízení přístupu na základě rolí] [ rbac] (RBAC) pro řízení přístupu k Azure prostředky, které nasadíte. RBAC můžete přiřadit role se tak mohli ověřovat členům týmu DevOps. Například roli Čtenář nelze zobrazit Azure zdroje, ale vytvořit, spravovat nebo odstraňte je. Některé role jsou specifické pro typy konkrétní Azure prostředků. Například role přispěvatele virtuálního počítače můžete restartovat nebo zrušit virtuálního počítače, resetování hesla správce, vytvořit virtuálního počítače a atd. Jiné [předdefinované role RBAC] [ rbac-roles] , který může být užitečné pro tento odkaz architektura zahrnutí [DevTest Labs uživatele] [ rbac-devtest] a [Sítě Přispěvatel][rbac-network]. 

Uživatele můžete přidělovat více rolí a můžete vytvořit vlastní role pro ještě víc jemně odstupňovaná oprávnění.

> [AZURE.NOTE] RBAC není omezena akce, které můžete dělat uživatel přihlášení k lyncu angličtině. Oprávnění jsou určena typ účtu na hosta s operačním systémem.   

Použití [protokolů auditování] [ audit-logs] zobrazíte zřizování akce a dalších událostí OM.

- Zvažte [Šifrování disku Azure] [ disk-encryption] v případě potřeby šifrování disků OS a data. 

## <a name="solution-deployment"></a>Nasazení řešení

[Nasazení] [ github-folder] odkaz architektura, který ukazuje tyto doporučené postupy je k dispozici. Tento odkaz architektura obsahuje virtuální síti (VNet), skupiny zabezpečení síti (NSG) a do jednoho počítače virtuální (OM).

Existuje několik způsobů nasazení architektura tento odkaz. Nejjednodušší způsob je postupujte podle těchto kroků: 

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně".
[![Nasazení Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Jakmile otevře na odkaz v portálu Azure, musíte zadat hodnoty některá nastavení: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-single-vm-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Vyberte v rozevíracím seznamu, **linux** **Typ s operačním systémem** .
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

4. Parametr soubory obsahují pevně správce uživatelské jméno a heslo a důrazně doporučujeme okamžitě změnit obojí. Klikněte na OM s názvem `ra-single-vm0 `na portálu Azure. Klikněte na **Resetovat heslo** v části **Podpora + Poradce při potížích** . Vyberte **Resetovat heslo** do pole rozevíracího seznamu **režim** a potom vyberte nové **uživatelské jméno** a **heslo**. Klikněte na tlačítko **Aktualizovat** pro zachování nové uživatelské jméno a heslo.

Informace o dalších způsobech nasazení tento odkaz architektura najdete v tématu v souboru readme v [pokyny jednoduchým OM] [ github-folder] Github složky.

## <a name="customize-the-deployment"></a>Přizpůsobení nasazení

Pokud chcete změnit nasazení odpovídá vašim potřebám, postupujte podle pokynů [pokyny jednoduchým OM] [ github-folder] stránky.

## <a name="next-steps"></a>Další kroky

Aby [SLA virtuálních počítačích] [ vm-sla] použít, musíte nasadit dva nebo víc instancí v dostupnost nastavení. Další informace najdete v tématu [spouštění více virtuálních na Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Jeden Linux OM architektura v Azure"

