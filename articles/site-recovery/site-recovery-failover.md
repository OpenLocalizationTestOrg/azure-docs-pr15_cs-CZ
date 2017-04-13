<properties
    pageTitle="Selhání obnovení webu | Microsoft Azure" 
    description="Obnovení Azure webu souřadnic replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Informace o převzetí Azure nebo vedlejší datacentra." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Selhání obnovení webu

Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md)

## <a name="overview"></a>Základní informace

Tento článek obsahuje informace a pokyny pro obnovení (nejsou-li více než a selhání zpět) virtuálních počítačích a fyzické servery, které jsou chráněny obnovení webu. 

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.


## <a name="types-of-failover"></a>Typy překlopení

Po ochranu aktivované řešení virtuálních počítačích a fyzické servery a máte replikovat spuštěním převzetí služeb při selhání podle potřeby. Obnovení webu podporuje několik typů překlopení.

**Překlopení** | **Kdy je vhodné spustit** | **Podrobnosti** | **Proces**
---|---|---|---
**Převzetí test** | Spuštění ověřit strategie replikace nebo provedení přechod obnovení havárie | Bez ztráty dat nebo výpadek služeb.<br/><br/>Žádný vliv na replikace<br/><br/>Žádný vliv na provozním prostředí | Zahájení záložní<br/><br/>Určení, jak bude připojen zkušební počítače k sítí po překlopení<br/><br/>Sledování průběhu na kartě **projekty** . Test počítačích se vytvářejí a začněte v sekundární umístění<br/><br/>Azure - připojit k počítači, na portálu Azure<br/><br/>Sekundární webu – přistupovat k počítači na stejnou Host (hostitel) a cloudu<br/><br/>Dokončete testování a automaticky vyčistit test převzetí nastavení.
**Plánované překlopení** | Spustit a dodržování předpisů<br/><br/>Spuštění pro plánovanou údržbu<br/><br/>Spuštění překlopení data, která chcete zachovat pracovní vytížení pro známé výpadků – například očekávané výpadku nebo špatných počasí<br/><br/>Po převzetí dostanete k navrácení z primárního na vedlejší | Bez ztráty dat<br/><br/>Prostoj při vzniká během časové náročnosti vypnutí virtuálního počítače na primárním a vyvoláte sekundární místa.<br/><br/>Virtuálních počítačích jsou dopad cílových počítačů bude zdroj počítačích po překlopení. | Zahájení záložní<br/><br/>Sledování průběhu na kartě **projekty** . Vypnout zdroj počítačích<br/><br/>Zahájení počítačích otevřené v sekundární umístění<br/><br/>Azure - připojit k počítači otevřené v portálu Azure<br/><br/>Sekundární webu – přistupovat k počítači ve stejném hostiteli a ve stejné cloudu<br/><br/>Potvrzení záložní
**Neplánované překlopení** | Spustit tento převzetí informace způsobem při primární webu nedostupné vzhledem k neočekávané incidentu, například power výpadku nebo virový útok <br/><br/> Můžete spustit neplánované převzetí se teď dá i v případě, že hlavní web není dostupný. | Závisí na nastavení frekvence replikace ztrátu dat<br/><br/>Data budou aktuální v souladu s posledního byl synchronizován | Zahájení záložní<br/><br/>Sledování průběhu na kartě **projekty** . Pokud chcete akci vypnutí virtuálních počítačích a synchronizovat nejnovější data<br/><br/>Zahájení počítačích otevřené v sekundární umístění<br/><br/>Azure - připojit k počítači otevřené v portálu Azure<br/><br/>Sekundární webu přistupovat k počítači ve stejném hostiteli a ve stejné cloudu<br/><br/>Potvrzení záložní


Typy převzetí služeb při selhání, které jsou podporovány, závisí na vaší situaci nasazení.

**Převzetí směru** | **Převzetí test** | **Plánované překlopení** | **Neplánované překlopení**
---|---|---|---
Hlavní web VMM sekundární VMM Web | Podporované | Podporované | Podporované 
Sekundární webu VMM primární VMM Web | Podporované | Podporované | Podporované
Shluk do cloudu (jednoho VMM server) |  Podporované | Podporované | Podporované
VMM webu Azure | Podporované | Podporované | Podporované 
Azure VMM Web | Nepodporované | Podporované | Nepodporované 
Web pro Hyper-V Azure | Podporované | Podporované | Podporované
Azure web Hyper-V | Nepodporované | Podporované | Nepodporované
VMware webu Azure | Podporované (rozšířené scénář)<br/><br/> Nepodporované (starší verze scénář) |Nepodporované | Podporované
Pole fyzicky server Azure | Podporované (rozšířené scénář)<br/><br/> Nepodporované (starší verze scénář) | Nepodporované | Podporované

## <a name="failover-and-failback"></a>Převzetí a navrácení

Převzetí virtuálních počítačích s webem sekundární místního nebo Azure, v závislosti na nasazení. Počítač, který selhání Azure se vytvoří jako Azure virtuálního počítače. Můžete převzít jednoho virtuálního počítače nebo pole fyzicky serveru nebo obnovení plán. Obnovení plány se skládá z jedné nebo více objednali skupiny, které obsahují chráněné virtuálních počítačích nebo pole fyzicky servery. Slouží k organizovat převzetí více počítačů, které selhání podle skupiny jsou. [Přečtěte si další](site-recovery-create-recovery-plans.md) informace o obnovení plány. 

Po dokončení převzetí a počítače probíhá zařídit i sekundární umístění Všimněte si, že:

- Pokud jste se nepodařilo přes Azure, po počítačích převzetí netvoří chráněný nebo replikace primární a sekundární umístění. V Azure virtuálních počítačích ukládají do úložiště geo replikovat, která umožňuje odolnost proti chybám, ale ne replikace.
- Pokud jste neplánované převzetí sekundární webu po nejsou zamknutí počítačích převzetí sekundární umístění nebo replikace.
- Pokud jste plánované převzetí s webem sekundární po počítačích převzetí sekundární umístění není zamčený.
 

### <a name="failback-from-secondary-site"></a>Navrácení z sekundární webu

Navrácení z sekundární webu primární používá stejným způsobem jako přepnutí z primárního sekundární. Po přepnutí z primární sekundární datacentra potvrzené a úplné, můžete ji spustit obráceném replikace až primární webu k dispozici. Synchronizace dat delta spustí obráceném replikace replikace mezi primární a sekundární webu. Zpětné replikace spojuje virtuálních počítačích do přísnější ale sekundární datacentra je stále aktivní umístění. Před uskutečněním primární webu do aktivní umístění aktivujete plánované přepojení z sekundární na primární, následuje další obráceném replikace.

### <a name="failback-from-azure"></a>Navrácení z Azure

Pokud jste selhání na Azure virtuálních počítačích jsou chráněny Azure odolnost proti chybám funkcí pro virtuálních počítačích. Pokud chcete vytvořit původního webu primární aktivní umístění spustíte plánované přepojení. Pokud není k dispozici původního webu může selhat zpátky do původního umístění nebo do jiného umístění. Spusťte replikace po navrácení primární umístění znovu spusťte obráceném replikace.

### <a name="failover-considerations"></a>Důležité informace o převzetí

- **IP adresy za převzetí**– ve výchozím nastavení nezdařeného přes počítač bude mít jinou IP adresu, než zdrojového počítače. Pokud chcete zachovat stejnou viz IP adresy: 
    - **Sekundární webu**– Pokud jste nedaří myši na vedlejší web a chcete zachovat IP adresu, [Přečtěte si](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) Tento článek. Poznámka: Pokud váš poskytovatel služeb Internetu podporuje, si zachovat veřejnou IP adresu.
    - **Azure**– Pokud jste nedaří myši na Azure můžete určit IP adresu, kterou chcete přiřadit na kartě **Konfigurovat** vlastnosti virtuálního počítače. Po přepnutí Azure nelze zachovat veřejnou IP adresu. Můžete zachovat RFC 1918 adresu mezery, které se používají jako vnitřní adresy.
- **Částečné převzetí**– Pokud chcete převzít součástí webu spíše než celý web Všimněte si, že: 
    - **Sekundární webu**– Pokud se chcete připojit kliknutím na tlačítko primární selhání součástí primárního webu sekundární web, pomocí připojení VPN k webu připojit přes aplikací na vedlejší webu součástí infrastruktury spuštěná na primární webu selhalo. Pokud celá podsítě selhání zachovat IP adresu virtuálního počítače. Pokud selhání částečné podsítě nelze zachovat IP adresu počítače virtuální protože podsítí nelze rozdělit mezi weby.
    - **Azure**– pokud převzetí částečného web Azure a se chcete připojit primární webu, můžete VPN k webu připojit nezdařeného přes aplikaci Azure infrastruktury komponent spuštěná na primární webu. Poznámka: Pokud dojde k chybě podsítě celý myší můžete lépe IP adresu virtuálního počítače. Pokud selhání částečné podsítě nelze zachovat IP adresu počítače virtuální protože podsítí nelze rozdělit mezi weby.
 
- **Písmeno**– Pokud chcete zachovat písmeno na virtuálních počítačích po převzetí zásady SAN virtuálního počítače můžete nastavit na **zapnuto**. Virtuální počítač disků přechodu do online automaticky. [Přečtěte si další](https://technet.microsoft.com/library/gg252636.aspx).
- **Požadavků klientů směrování**– obnovení webu funguje s Azure přenosy správce ke směrování požadavků na klientské aplikaci po překlopení.




## <a name="run-a-test-failover"></a>Spusťte test selhání

Když spustíte test přepojení budete požádáni vyberte nastavení sítě pro test otevřené počítače. Máte několik možností.  

**Možnost převzetí test** | **Popis** | **Převzetí zaškrtnutí** | **Podrobnosti**
---|---|---|---
**Převzetí Azure – bez sítě** | Nevybírejte cílové Azure sítě | Převzetí kontroly, které testování virtuálního počítače spustí podle očekávání v Azure | Všechny test virtuálních počítačích v plánu obnovit se přidají do jednoho Cloudová služba a můžete připojit k sobě<br/><br/>Počítačích nejste připojení k síti Azure po překlopení.<br/><br/>Uživatelé mohli připojovat ke zkušební počítače s veřejnou IP adresu
**Převzetí Azure – k síti** | Vyberte cíl Azure sítě | Převzetí ověří, že test počítačích jsou připojené k síti | Vytvoření Azure síti, která obsahuje samostatný ze sítě Azure a nastavení infrastrukturu pro replikovanou virtuální počítač očekávaným způsobem.<br/><br/>Podsítě test virtuálního počítače je založená na podsítě dnem selhalo přes virtuální počítač očekává se připojit k Pokud dojde k selhání plánované nebo neplánované.
**Převzetí sekundárním web VMM – bez sítě** | Nevybírejte síti OM | Převzetí zkontroluje vytvořené zkušební počítače.<br/><br/>Test virtuálního počítače se vytvoří ve stejném hostiteli jako host, na kterém se nachází otevřené virtuálního počítače. Není přidána do cloudu, ve kterém se nachází otevřené virtuálního počítače. | <p>Selhalo přes počítače nebudete připojení k jakákoli síť.<br/><br/>Počítači můžete připojení k síti OM, když už vytvořená
**Převzetí sekundárním web VMM – k síti** | Vyberte stávající sítě OM | Převzetí zkontroluje, zda se vytvářejí virtuálních počítačích | Test virtuálního počítače se vytvoří ve stejném hostiteli jako host, na kterém se nachází otevřené virtuálního počítače. Není přidána do cloudu, ve kterém se nachází otevřené virtuálního počítače.<br/><br/>Vytvoření OM síti, která obsahuje samostatný ze sítě<br/><br/>Pokud používáte VLAN síť doporučujeme že vytvoříte samostatný logická síť (není použitý v výrobní) v VMM k tomuto účelu. Logická síť slouží k vytvoření OM sítě pro účely test překlopení.<br/><br/>Logická síť by měl být přidružené k alespoň jedno z síťové adaptéry všech serverů Hyper-V hostování virtuálních počítačích.<br/><br/>Logické sítích VLAN by měl být izolace sítě weby, které přidáte do logické sítě.<br/><br/>Pokud používáte založené na Windows sítě virtualizace logická síť, obnovení webu Azure automaticky vytvoří izolace OM sítě.
**Převzetí sekundární web VMM – vytvoření sítě** | Dočasné testovací sítě vytvoří se automaticky v závislosti na nastavení, které určíte v **Logických sítí** a weby související sítě | Převzetí zkontroluje, zda se vytvářejí virtuálních počítačích | Tuto možnost použijte, pokud plánu obnovy využívá víc než jedné sítě OM. Pokud používáte Windows sítě virtualizace sítí, můžete tuto možnost automaticky vytvořit v síti virtuálního počítače otevřené OM sítě se stejným nastavením (podsítí a fondy IP adresa). Tyto OM sítě se automaticky vyčistí po dokončení testu překlopení.</p><p>Test virtuálního počítače se vytvoří ve stejném hostiteli jako host, na kterém se nachází otevřené virtuálního počítače. Není přidána do cloudu, ve kterém se nachází otevřené virtuálního počítače.

>[AZURE.NOTE] Stejný jako IP adresu, kdy dostal IP adresu věnovat virtuálního počítače při test selhání dělá plánované nebo neplánované selhání (za předpokladu, že je k dispozici v testovací převzetí sítě IP adresu. Pokud není dostupná v síti převzetí test stejnou IP adresu virtuálního počítače dostanou jiné IP adresy k dispozici v síti převzetí test.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Spusťte test přepojení z místního Azure

Tento postup popisuje, jak provádět test přepojení pro plán obnovení. Převzetí jednoho počítače můžete taky můžete spustit na kartě **virtuálních počítačích** .

1. **Obnovení plány jednotného zasílání zpráv**vyberte > *recoveryplan_name*. Klikněte na **převzetí** > **převzetí testu**.
2. Na stránce **Potvrďte převzetí Test** určete, jak bude připojený otevřené počítačích k síti Azure po překlopení.
3. Pokud jste nedaří myši na Azure a šifrování aktivované řešení v cloudu, v **Šifrovací klíč** vyberte certifikát, který byl vydán po povolení šifrování dat během instalace zprostředkovatele. 
4. Sledování průběhu převzetí na kartě **projekty** . Je třeba neuvidíte počítači otevřené test na portálu Azure.
5. Dostanete se k počítači otevřené v Azure z místního webu zahájení připojení ke vzdálené ploše virtuálního počítače. port 3389 se musí být otevřený na koncový bod pro virtuální počítač.
5. Až budete mít Hotovo, dosáhne záložní fáze **testování dokončeno** , klikněte na **Úplný zkušební** dokončete.
5. V **poznámkách** zaznamenat a uložit všechny připomínky přidružený k převzetí testu.
8. Klikněte na **dokončení testu převzetí** automaticky vyčištění testovacím prostředí. Po dokončení to převzetí test se zobrazí stav**omplete** C.

> [AZURE.NOTE] Pokud test přepojení dál pokračující víc než dva týdny budete dokončit silou. Všechny prvky nebo virtuálních počítačích vytvářeny automaticky při selhání test se odeberou.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Spuštění selhání test z webu primární místního sekundární místního Web

Musíte udělat několik věcí spusťte test selhání, včetně kopii řadiče domény a umístění test DHCP a DNS serverů v testovacím prostředí. Můžete provést několika způsoby:

- Pokud chcete spustit test přepojení pomocí existující síť, připravte Active Directory, DHCP a DNS v síti.
- Pokud chcete spustit test přepojení pomocí možnosti vytváření OM sítí automaticky, přidáte ruční krok před skupiny-1 obnovení plán, který budete chtít použít pro překlopení test a potom přidáte zdroje infrastruktury automaticky vytvořené sítě před spuštěním testovací překlopení.

#### <a name="things-to-note"></a>Co je potřeba pamatovat

- Když replikace sekundární web typ síti používá počítači otevřené nevyžaduje podle typu logické sítě použít pro překlopení test, ale některé kombinace nemusí fungovat. Pokud otevřené používá DHCP a na základě VLAN izolace, v síti OM otevřené nevyžaduje statický fond IP adres. Aby pomocí Windows sítě virtualizace pro překlopení test nebude fungovat, protože jsou k dispozici žádné fondy adres. Kromě toho test převzetí nebudou fungovat, pokud není otevřené network izolace bez a testovací sítě je virtualizace sítě Windows. Je to proto síť bez izolace nemá podsítí muset vytvořit síť virtualizace sítě Windows.
- Způsob, ve které otevřené virtuálních počítačích připojeni k namapované OM sítě po převzetí závisí na konfiguraci sítě OM v konzole VMM:
    - **OM síti nakonfigurován bez izolace nebo VLAN izolace**– Pokud DHCP definované pro síť OM, otevřené virtuální počítač bude připojen k VLAN ID, pomocí nastavení, které jsou určeny pro web sítě přidružené logické sítě. Virtuální počítač dostanou adresy IP z dostupných DHCP serveru. Nemusíte statický fond IP adres definované pro cílové OM síti. Pokud fond statické IP adresy se používá k síti OM virtuálního počítače otevřené připojení k VLAN ID, pomocí nastavení, které jsou určeny pro web sítě přidružené logické sítě. Virtuální počítač dostanou adresy IP z fondu definované pro OM síť. Pokud statický fond IP adres není definované v cílové OM síti, se nepovede přidělování IP adres. Na zdrojovém a cílovém serverech VMM, které budete používat pro ochranu a obnovení se vytvoří fondu IP adres.
    - **OM síti se systémem Windows sítě virtualizace**– jestli se toto nastavení používají statického fondu stanovit pro cílové OM síti nakonfigurování sítě OM, bez ohledu na to, zda nakonfigurování sítě OM zdroj používat DHCP nebo statické IP adres fondu. Když definujete DHCP VMM cílový představovat serveru DHCP a zadejte IP adresu z fondu, který je definován pro cílové OM síti. Pokud je definován použití statického fondu IP adres pro zdrojový server, VMM cílový přidělit k IP adrese z fondu. V obou případech přidělování adres IP selže, pokud není definované statický fond IP adres.

#### <a name="run-test"></a>Spusťte test

Tento postup popisuje, jak provádět test přepojení pro plán obnovení. Převzetí na jednom počítači virtuální nebo pole fyzicky serveru můžete taky můžete spustit na kartě **virtuálních počítačích** .

1. **Obnovení plány jednotného zasílání zpráv**vyberte > *recoveryplan_name*. Klikněte na **převzetí** > **převzetí testu**.
2. Na stránce **Potvrďte testování převzetí** zadejte virtuálních počítačích by měl být připojení k sítí po převzetí test.
3. Sledování průběhu převzetí na kartě **projekty** . Fáze** testování dokončeno** dosáhne záložní kliknutím na tlačítko skončí zkušební převzetí **Dokončení otestovat** .
4. Kliknutím na **poznámky** můžete zaznamenat a uložit všechny připomínky přidružený k převzetí testu.
4. Po dokončení ověřte, že virtuálních počítačích aplikace úspěšně spuštěna.
5. Po ověření, že virtuálních počítačích aplikace úspěšně spuštěna, proveďte převzetí test vyčištění izolovaném prostředí. Pokud jste se rozhodli automaticky vytvořit OM sítě, vyčištění odstraní všechny virtuálních počítačích test a otestujte sítě.

> [AZURE.NOTE] Pokud test přepojení dál pokračující víc než dva týdny budete dokončit silou. Všechny prvky nebo virtuálních počítačích vytvářeny automaticky při selhání test se odeberou.


#### <a name="prepare-dhcp"></a>Příprava DHCP

Pokud virtuálních počítačích účastní test převzetí používejte DHCP, serveru DHCP test by měl být vytvořen v rámci izolace sítě, která se vytvoří pro účely test překlopení.


### <a name="prepare-active-directory"></a>Příprava služby Active Directory
Spusťte test selhání aplikace testování, musíte mít kopii provozním prostředí služby Active Directory v testovacím prostředí. Absolvovat [testování důležité informace o převzetí služby active directory](site-recovery-active-directory.md#considerations-for-test-failover) v části Další podrobnosti. 


### <a name="prepare-dns"></a>Příprava DNS

Příprava server DNS převzetí test následujícím způsobem:

- **DHCP**– DHCP používáte virtuálních počítačích IP adresu test DNS u testovací DHCP server aktualizovat. Pokud používáte typu sítě virtualizace sítě systému Windows VMM server funguje jako serveru DHCP. Proto třeba aktualizovat IP adresu DNS v síti převzetí testu. V tomto případě virtuálních počítačích zaregistruje sami relevantní server DNS.
- **Adresa**– používáte virtuálních počítačích statickou IP adresu, IP adresu serveru DNS test mají být aktualizovány v testovací převzetí sítě. Možná budete muset aktualizace DNS u IP adresu test virtuálních počítačích. Následující ukázkový skript můžete použít k tomuto účelu: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Spuštění plánované selhání (primární sekundární)

 Tento postup spustit plánované přepojení pro plán obnovení. Převzetí jednoho virtuálního počítače můžete taky můžete spustit na kartě **virtuálních počítačích** .

1. Než začnete, zkontrolujte, že všechny virtuálních počítačích požadované převzetí dokončili počáteční replikace.
2. **Obnovení plány jednotného zasílání zpráv**vyberte > *recoveryplan_name*. Klikněte na **převzetí** > **plánované překlopení**. 
3. Na stránce **Potvrďte plánované převzetí **zvolte zdrojové a cílové umístění. Poznámka: směru překlopení.

    - Pokud předchozí převzetí služeb při selhání odpracovaných podle očekávání a všech serverech virtuálního počítače umístěných na zdrojového nebo cílového umístění, podrobnosti o převzetí směr jsou pouze informace. 
    - Pokud virtuálních počítačích jsou aktivní pro zdrojový a cílové umístění, zobrazí se tlačítko **Změnit směr** . Tlačítko změnit a určit směr, ve kterém by měla proběhnout záložní.

5. Pokud jste nedaří myši na Azure a šifrování aktivované řešení v cloudu, v **Šifrovací klíč** vyberte certifikát, který byl vydán po povolení šifrování dat během instalace zprostředkovatele na serveru VMM. 
6. Plánované selhání, nebude zahájen prvním krokem při vypnutí virtuálních počítačích zajistit beze ztráty dat. Na kartě **úlohy** můžete sledovat průběh překlopení. Pokud dojde k chybě v převzetí (nebo na počítač virtuální v skript, který je součástí plánu obnovy), přestane plánované převzetí plán obnovení. Přenesení můžete zahájit znovu.
8. Po vytvoření otevřené virtuálních počítačích jsou v potvrdit nevyřízená. Klikněte na tlačítko **Potvrdit** potvrďte záložní. 
9. Po replikace dokončete virtuálních počítačích při spuštění v sekundární umístění. 

## <a name="run-an-unplanned-failover"></a>Spuštění neplánované překlopení

Tento postup spustit neplánované převzetí pro plán obnovení. Převzetí na jednom počítači virtuální nebo pole fyzicky serveru můžete taky můžete spustit na kartě **virtuálních počítačích** .

1. **Obnovení plány jednotného zasílání zpráv**vyberte > *recoveryplan_name*. Klikněte na **převzetí** > **neplánované převzetí**. 
3. Na stránce **Potvrďte neplánované převzetí **zvolte zdrojové a cílové umístění. Poznámka: směru překlopení.

    - Pokud předchozí převzetí služeb při selhání odpracovaných podle očekávání a všech serverech virtuálního počítače umístěných na zdrojového nebo cílového umístění, podrobnosti o převzetí směr jsou pouze informace. 
    - Pokud virtuálních počítačích jsou aktivní pro zdrojový a cílové umístění, zobrazí se tlačítko **Změnit směr** . Tlačítko změnit a určit směr, ve kterém by měla proběhnout záložní.

4. Pokud jste nedaří myši na Azure a šifrování aktivované řešení v cloudu, v **Šifrovací klíč** vyberte certifikát, který byl vydán po povolení šifrování dat během instalace zprostředkovatele na serveru VMM. 
5. Vyberte **vypnout virtuálních počítačích a synchronizovat nejnovější data** můžete určit, že obnovení webu pokuste ukončit chráněné virtuálních počítačích a synchronizovat data tak, aby se nezdaří nejnovější verzi data nad. Pokud nevyberete možnost nebo pokus nemá úspěšné záložní bude od nejnovějších bod k dispozici obnovení virtuálního počítače.
6. Na kartě **úlohy** můžete sledovat průběh překlopení. Všimněte si, že i když dojde k chybám při neplánované selhání, plánu obnovy běží až po dokončení.
7. Převzetí virtuálních počítačích po ve stavu **Potvrdit čeká na vyřízení** . Klikněte na tlačítko **Potvrdit** potvrďte záložní.
8. Pokud nastavíte replikace používat více obnovení bodů v bodě obnovení změnit můžete vybrat obnovení bod, který není nejnovější (nejnovější je použit ve výchozím nastavení). Po potvrzení další obnovení body se odeberou.
9. Po dokončení replikace virtuálních počítačích otevřou a pracují sekundární umístění. Ale nejsou chráněný nebo replikace. Až primární webu k dispozici znovu se stejnou základní infrastruktury, klikněte na **Obráceném replikovat** zahájíte obráceném replikace. Zajistíte, že všechna data replikovat zpět na primární webu a virtuálního počítače je připraven k převzetí znovu. Po neplánované převzetí náročnější v přenos dat zpětná replikace. Převod použije stejným způsobem, který je nakonfigurovaný pro nastavení počáteční replikace v cloudu.

## <a name="failback-from-secondary-to-primary"></a>Navrácení z sekundární na primární

 Po přepnutí z primárního sekundární umístění replikovanou virtuálních počítačích nejsou chráněny obnovení webu a vedlejší umístění teď budou sloužit jako primární. Následující postup ukazuje selhání zpátky na původní primární webu. Tento postup spustit plánované přepojení pro plán obnovení. Převzetí jednoho virtuálního počítače můžete taky můžete spustit na kartě **virtuálních počítačích** .

1. **Obnovení plány jednotného zasílání zpráv**vyberte > *recoveryplan_name*. Klikněte na **převzetí** > **plánované překlopení**.
2. Na stránce **Potvrďte plánované převzetí **zvolte zdrojové a cílové umístění. Poznámka: směru překlopení. Pokud se nepovedlo převzetí z primárního jako očekávat a všechny virtuálních počítačích jsou sekundární umístění, do kterého toto se týká jenom informace.
3. Pokud jste selhání zpět z Azure vyberte nastavení v **Synchronizace dat**:

    - **Synchronizovat data před převzetí (pouze synchronizuje delta změny)**– tato možnost slouží k minimalizaci výpadek virtuálních počítačích jako synchronizuje vypnutím je. Provede následující akce:
        - Fáze 1: Zabírá snímek virtuálního počítače v Azure a zkopíruje k hostiteli Hyper-V místním. Počítači pokračovat v Azure.
        - Fáze 2: Virtuálního počítače v Azure vypne, tak, že nejsou žádné nové změny dojít tam. Poslední sada změny převádějí na místního serveru a virtuálního místního počítače je spuštěn.
    

    - **Synchronizovat data při selhání pouze (úplné ke stažení)**– tuto možnost použijte, pokud jste už na Azure poměrně dlouho. Tato možnost je rychlejší, protože Očekáváme, že většina disku změnil a jsme nechcete, aby se vám při výpočtu kontrolního. Slouží k provedení stažení disku. Je vhodné také po odstranění místní virtuálního počítače.
    
    > [AZURE.NOTE] Doporučujeme tuto možnost použijte, pokud jste už používáte Azure určitou dobu (za měsíc nebo více) nebo odstranil OM místní. Tato možnost není provádět výpočty kontrolního.
    
5. Pokud jste nedaří myši na Azure a šifrování aktivované řešení v cloudu, v **Šifrovací klíč** vyberte certifikát, který byl vydán po povolení šifrování dat během instalace zprostředkovatele na serveru VMM. 
5. Ve výchozím nastavení se používá na místo posledního obnovení, ale v **Bodě obnovení změnit** můžete zadat bod různých obnovení. 
6. Klikněte na políčko Spustit navrácení.  Na kartě **úlohy** můžete sledovat průběh překlopení. 
7. f jste vybrali možnost Synchronizovat data před překlopení, po dokončení synchronizace počátečního data a budete chtít vypnout virtuálních počítačích v Azure, klikněte na tlačítko **projekty**  >  <planned failover job name> **Dokončení překlopení**. To vypne Azure počítače přepojuje poslední změny do virtuálního místního počítače a jej spustil.
8. Můžete si teď se přihlásit k virtuální počítač jeho ověřením neexistuje podle očekávání. 
9. Virtuální počítač je v potvrdit nevyřízená. Klikněte na tlačítko **Potvrdit** potvrďte záložní.
10. Teď k dokončení navrácení klikněte na **Zpětná replikovat** a začněte chránit virtuálního počítače primární webu.



## <a name="failback-to-an-alternate-location"></a>Překlopení zpět do jiného umístění

Pokud jste nainstalovali ochranu mezi [webem Hyper-V a Azure](site-recovery-hyper-v-site-to-azure.md) máte možnost navrácení z Azure na alternativní místního umístění. To je užitečné v případě potřeby nastavení nového místní hardwaru. Tady je, můžete to udělat takto.

1. Pokud nastavujete nový hardware instalace systému Windows Server 2012 R2 a role pro Hyper-V na serveru.
2. Vytvoření přepínače virtuální sítě se stejným názvem, který jste měli na původní serveru.
3. Vyberte **Chráněné položky** -> **Ochranu skupiny**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> chcete navrácení a vyberte **Plánované překlopení**.
4. **Potvrďte plánované převzetí** vyberte **vytvořit místní virtuální počítač Pokud není k dispozici**. 
5. V **Poli Název hostitele** vyberte nové server Host (hostitel) Hyper-V, podle kterého chcete umístit virtuální počítač.
6. Synchronizace dat doporučujeme, že abyste vybrali možnost **synchronizovat data před záložní**. To slouží k minimalizaci výpadek virtuálních počítačích jako synchronizuje vypnutím je. Provede následující akce:

    - Fáze 1: Zabírá snímek virtuálního počítače v Azure a zkopíruje k hostiteli Hyper-V místním. Počítači pokračovat v Azure.
    - Fáze 2: Virtuálního počítače v Azure vypne, tak, že nejsou žádné nové změny dojít tam. Poslední sada změny převádějí na místního serveru a virtuálního místního počítače je spuštěn.
    
7. Klikněte na zaškrtnutí zahájíte převzetí (navrácení).
8. Po dokončení počáteční synchronizace a budete chtít vypnout virtuálního počítače v Azure, klikněte na tlačítko **projekty** > <planned failover job> > **Dokončení překlopení**. To vypne Azure počítače přepojuje poslední změny do virtuálního místního počítače a jej spustil.
9. Můžou přihlásit virtuální počítač místní ověřte, jestli že všechno funguje očekávaným způsobem. Klikněte na Dokončit záložní **potvrzení** .
10. Klepněte na položku **Otočení replikovat** a začněte chránit virtuálního místního počítače.

    >[AZURE.NOTE] Pokud zrušíte navrácení dokud je v kroku Data synchronizace, OM místní bude ve stavu poškozená. Důvodem je kopií synchronizace dat nejnovějšími daty z Azure OM disků disků místní data a jako volání před verzí synchronizace dokončí, disku, který nemusí být data do konzistentního stavu. Pokud OM zapnuto, pokud soubor je spuštěn po synchronizace dat je zrušená, nemusí spustit. Znovu spustit převzetí dokončete synchronizace Data.
 
