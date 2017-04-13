<properties 
   pageTitle="Změna konfigurace zařízení StorSimple | Microsoft Azure" 
   description="Popisuje, jak používat službu StorSimple správce aby překonfigurovali StorSimple zařízení, které již byly nasazeny." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Pomocí službu StorSimple správce upravte konfiguraci StorSimple zařízení

## <a name="overview"></a>Základní informace 

Portál Azure klasické stránku **Konfigurovat** obsahuje všechny parametry zařízení, které lze překonfigurovat na StorSimple zařízení, které se spravuje StorSimple Správce služby. Tento kurz vysvětluje, jak můžete na stránce **Konfigurovat** provádět následující úkoly úroveň zařízení:

- Úprava nastavení zařízení 
- Úprava nastavení času 
- Změna nastavení DNS 
- Úprava síťová rozhraní
- Záměna nebo změňte jeho přiřazení IP adresy

## <a name="modify-device-settings"></a>Úprava nastavení zařízení

Nastavení zařízení obsahovat popisný název zařízení a popis zařízení.

StorSimple zařízení připojené k službu StorSimple správce se přiřadí výchozí název. Výchozí název většinou odpovídá pořadové číslo zařízení. Výchozí název zařízení, která je 15 znaků, například 8600-SHX0991003G44HT označuje, třeba takto:

- **8600** – označuje modelu zařízení.
- **TVX** – označuje místo výroby.
- **0991003** - označuje určitý produkt.
- **G44HT**- posledních 5 číslic se zvyšují vytvořit jedinečný pořadová čísla. Toto nemusí být sekvenční sadu.

Portál Azure klasické umožňuje změnit název zařízení a přiřadit ji jedinečný a popisný název podle svého výběru. Popisný název může obsahovat žádné znaky a může obsahovat maximálně 64 znaků.

Můžete také zadat popis zařízení. Popis zařízení obvykle pomůže identifikovat vlastník a fyzické umístění zařízení. Do pole Popis musí být menší než 256 znaků.
 
## <a name="modify-time-settings"></a>Úprava nastavení času

Zařízení musíte k synchronizaci času pro ověření u vašeho poskytovatele služeb cloudové úložiště. V rozevíracím seznamu vyberte časové pásmo a zadejte až dva servery čas NTP (Network Protocol). Primární NTP server je povinný a není zadán, kdy používat Windows PowerShell pro StorSimple pro nastavení vašeho zařízení. Můžete určit výchozí systému Windows Server **time.windows.com** jako NTP server. Můžete zobrazit primární konfigurace serveru NTP portálu Azure klasické, ale je nutné použít v prostředí Windows PowerShell rozhraní ho změnit.

Konfigurace sekundárního serveru NTP je nepovinný krok. Konfigurace sekundárního NTP serveru můžete portálu klasické. 

Při konfiguraci serveru NTP, dbejte na síti umožňuje přenosy NTP předat z vaší datacentra k Internetu. Při určování veřejné NTP serveru, musíte nakonfigurovány síťové brány firewall a jiných zařízeních zabezpečení umožňuje přenos NTP do a z externí sítě. Pokud není povoleno obousměrný NTP přenosy, je nutné použít interní server NTP (řadiče domény systému Windows obsahuje tato funkce). Pokud vaše zařízení nelze provést synchronizaci času, nemusí být komunikovat s poskytovatelem cloudové úložiště.

Zobrazíte seznam veřejných serverů NTP přejděte na [Web servery NTP](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Co se stane, pokud je zařízení nasazený v různých časové pásmo?

Pokud je zařízení nasazený v různých časové pásmo, se změní časové pásmo zařízení. Předpokladu, že zálohování zásady použití časové pásmo zařízení, záložní zásady automaticky upraví podle nové časové pásmo. Požaduje bez zásahu uživatele.

## <a name="modify-dns-settings"></a>Změna nastavení DNS

DNS server se používá při zařízení pokusí komunikace s poskytovateli služeb cloudové úložiště. Pro dostupnost musíte nakonfigurovat primární a sekundární servery DNS během počáteční zařízení nasazení. Aby překonfigurovali primárního serveru DNS, musíte používat na zařízení StorSimple rozhraní prostředí Windows PowerShell.

Chcete-li změnit sekundární server DNS, můžete používat portál Azure klasické.



## <a name="modify-network-interfaces"></a>Úprava síťová rozhraní

Šest zařízení Síťová rozhraní čtyři z nich jsou 1 GbE a dvě z nich jsou 10 GbE má vaše zařízení. Tato rozhraní jsou označeny jako DATA 0 – DATA 5. DATA 0, 1 dat, dat 4 a 5 dat jsou 1 GbE, zatímco 10 rozhraní sítě GbE DATA 2 a DATA 3.

Nastavení **sítě rozhraní** pro jednotlivá pole rozhraní se nemusí používat. Aby dostupnost, doporučujeme mít aspoň dva iSCSI rozhraní a dvě cloudu s podporou na vašem zařízení. Jsme doporučujeme ale nevyžadují zakázán nepoužitý rozhraní.

Při konfiguraci jednotlivých rozhraní sítě, musíte nakonfigurovat virtuální IP (VIP).

DATA 0 zapnuté cloudu ve výchozím nastavení. Při konfiguraci DATA 0, taky musíte nakonfigurovat dvou pevné IP adres, jedno pro každé řadiče domény. Tyto pevné IP adresy mohou sloužit k přímý přístup k řadiče zařízení a hodí se po instalaci aktualizací na zařízení nebo Pokud přistupujete řadiče za účelem řešení problémů.

StorSimple 8000 řady Update 1 je nastavený metriky dat 0 nejnižší; Proto pokud vaše zařízení běží StorSimple 8000 řady Update 1, všechny přenosy cloudu směrován regresi datových 0. Poznamenejte si tohoto Pokud máte víc než jeden cloudu s podporou síťové na zařízení s StorSimple.

>[AZURE.NOTE] Pevné IP adresy řadiče se používají pro přípravu aktualizace zařízení. Pevné IP adresy proto, musí být směrovatelné a připojit k Internetu.

Pro každé rozhraní sítě se zobrazí následující parametry:

- **Rychlost** – ne konfigurovatelné uživatelem parametru. DATA 0, 1 dat, dat 4 a DATA 5 jsou vždy 1 GbE, zatímco 10 GbE rozhraní DATA 2 a DATA 3.

     >[AZURE.NOTE] Rychlost a duplexní jsou vždy automaticky vyjednávání. Typu Jumbo nejsou podporované.
 
- **Stav rozhraní** – rozhraní můžete povolit nebo zakázat. Pokud je povoleno, zařízení se pokusí pomocí rozhraní. Doporučujeme povolit pouze rozhraní, které jsou připojené k síti a používat. Zakážete všechna rozhraní, které nepoužíváte.

- **Typ rozhraní** – tento parametr umožňuje izolovat iSCSI adres přenosy cloudové úložiště. Tento parametr může být jeden z těchto věcí:

    - **Cloudu povolené** – pokud povoleno, zařízení se pomocí tohoto rozhraní komunikovat s cloudu.
    - **iSCSI povolené** – pokud povoleno, zařízení pomocí rozhraní komunikovat s hostiteli iSCSI.

    Doporučujeme izolovat iSCSI adres přenosy cloudové úložiště. Všimněte si také, že pokud hostitele je v rámci stejné podsítě jako zařízení, nemusíte přiřazení brány; ale pokud hostitele je v různých podsítě než zařízení, musíte se přiřazení brány.

- **IP adresa** – to může být IPv4 nebo IPv6 nebo obojí. Rodiny adres protokolu IPv4 a IPv6 jsou podporovány pro síťová rozhraní zařízení. Při použití IPv4, zadejte IP adresu 32-bit (*xxx.xxx.xxx.xxx*) v tečka desítkové soustavě. Když používáte IPv6, stačí zadat předponu 4místný a adresu 128bitové vygeneruje se automaticky pro rozhraní sítě zařízení na základě této předpony.

- **Podsítě** – to odkazuje na masku podsítě a jsou nakonfigurované prostřednictvím rozhraní prostředí Windows PowerShell.

- **Brána** – to je výchozí brány, který bude použito toto rozhraní při pokusu o komunikaci s uzly, které nejsou ve stejné místo IP adresy (podsítě). Výchozí brány musí být ve stejné adresní prostor (podsítě) jako rozhraní IP adresa stanovené maskou podsítě.

- **Pevné IP adresa** – toto pole je k dispozici pouze při konfiguraci dat 0 rozhraní. Pro operace ATP aktualizace Poradce při potížích s zařízení budete muset připojit přímo do Správce zařízení. Pevné IP adresu lze použít pro přístup k aktivní i pasivní řadiče na vašem zařízení.

Portálu Azure klasické překonfigurovat řadiče 0 a 1 řadiče domény.

>[AZURE.NOTE] 
>
>- Pro zajištění správné funkce, ověřte rychlost rozhraní a oboustranný tisk na přepínač každé rozhraní zařízení připojené k. Přepnout rozhraní by měl buď vyjednávání s nebo nakonfigurován pro Ethernetu (1 000 MB /) a být plně duplexní. Rozhraní operační pomaleji rychlostí nebo polovina duplexní povede problémy s výkonem.
>
>- Minimalizovat přerušení a výpadek služeb, doporučujeme vám povolit portfast jednotlivých hodnot porty přepnout rozhraní iSCSI sítě zařízení se připojují k. Zajistíte, že v případě selhání můžete rychle vytvořit připojení k síti.
 
## <a name="swap-or-reassign-ips"></a>Záměna nebo změňte jeho přiřazení IP adresy

V současné době Pokud libovolné sítě rozhraní řadiče přiřadí VIP, který používá (ve stejném zařízení nebo jiné zařízení v síti), potom správce se nepovede myší. Proto budete muset postup správné Pokud jsou výměna virtuální pro rozhraní zařízení, protože vytvoříte duplicitní situace IP.

Postupujte podle těchto kroků zaměnit nebo změňte jeho přiřazení virtuální některého z rozhraní sítě:

#### <a name="to-reassign-ips"></a>Chcete-li přiřadit IP adresy

1. Zrušte zaškrtnutí políčka IP adresa obě rozhraní.

2. Po IP adresách, které jsou prázdná, přiřaďte nové IP adresy odpovídajících rozhraní.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [nakonfigurovat MPIO StorSimple zařízení](storsimple-configure-mpio-windows-server.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
     
