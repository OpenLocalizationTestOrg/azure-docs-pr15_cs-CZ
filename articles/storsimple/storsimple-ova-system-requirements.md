<properties
   pageTitle="Požadavky na systém pro StorSimple virtuální matice"
   description="Další informace o software a sociální sítě požadavky pro své StorSimple virtuální pole"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Požadavky na systém pro StorSimple virtuální matice

## <a name="overview"></a>Základní informace

Tento článek popisuje důležité systémové požadavky pro vašeho webu virtuální pole pro Microsoft Azure StorSimple (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální) a pro klienty úložiště přístup k matice. Doporučujeme, abyste si prošli informace pečlivě před nasazení systému StorSimple a potom zpátky na ni odkázat podle potřeby při nasazení a další operace.

Systémové požadavky:

-   **Požadavky na software pro klienty úložiště** - popisuje virtualizace podporované platformy, webových prohlížečích, iniciátory iSCSI, SMB klienty, minimální virtuální zařízení požadavky a všech dalších věcí uvedených operačních systémech.

-   Informace o porty, které musí být otevřený v bráně firewall umožňující iSCSI, cloudu nebo přenosy Správa **sítě požadavky pro zařízení StorSimple** - obsahuje.

StorSimple systémové požadavky informace publikované v tomto článku platí pouze pro StorSimple virtuální matice.

- Pro zařízení 8000 řady přejděte na [Systémové požadavky pro zařízení StorSimple 8000 řady](storsimple-system-requirements.md).
 
- 7000 řady zařízení najdete [Systémové požadavky pro zařízení StorSimple 5000 7000 řady](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Požadavky na software

Požadavky na software obsahují informace o podporované webové prohlížeče, SMB verze, virtualizace platformy a požadavky minimální virtuální zařízení.

### <a name="supported-virtualization-platforms"></a>Podporované virtualizace platformy


| **Hypervisoru** | **Verze**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 nebo novější |
| VMware ESXi    | 5,5 a novější                        |

### <a name="virtual-device-requirements"></a>Požadavky na virtuální zařízení

| **Součásti**                                | **Požadavek**            |
|----------------------------------------------|----------------------------|
| Minimální počet procesorů virtuální (jádra) | 4                          |
| Minimální paměť (RAM)                         | 8 GB                       |
| Disk místo<sup>1</sup>                       | Operační systém disku - 80 GB <br></br>Data disku - 500 GB 8 TB|
| Minimální počet sítě složka       | 1                          |
| Minimální šířky pásma Internetu<sup>2</sup>       | 5 MB /                     |

<sup>1</sup> – tenká zřízení

<sup>2</sup> – požadavky na síť může lišit v závislosti denní kurz změnit data. Například pokud zařízení potřebuje k obecnějším údajům 10 GB nebo více změn během dne, pak denní zálohování přes 5 připojení MB / může trvat až 4,25 hodin (je-li data nelze komprimovaná nebo deduplicated).

### <a name="supported-web-browsers"></a>Podporované webové prohlížeče

| **Součásti**     | **Verze** | **Další požadavky/poznámky** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Nejnovější verze  |                                   |
| Internet Explorer | Nejnovější verze  | Testováno aplikaci Internet Explorer 11  |
| Google Chrome     | Nejnovější verze  | Testovat s Chrome 46             |

### <a name="supported-storage-clients"></a>Podporované úložiště klientů 

Jsou následující požadavky na software pro iniciátory iSCSI, které přístup k vaší StorSimple virtuální (nakonfigurované pole jako iSCSI server).

| **Podporované operační systémy** | **Verze požadovaná** | **Další požadavky/poznámky** |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008 R2 SP1, 2012, 2012R2 |StorSimple můžete vytvořit tence zřizování a plně zřizování svazky. Částečně zřizování svazky je možné vytvořit. StorSimple iSCSI svazky jsou podporované jenom pro: <ul><li>Jednoduché svazky na Windows základní discích.</li><li>Windows NTFS pro formátování svazku.</li>|


Jsou následující požadavky na software pro klienty SMB, které přístup k vaší StorSimple virtuální (nakonfigurované pole jako souborový server).

| **Verze SMB** |
|-------------|
| Plány SMB 2.x     |
| PLÁNY SMB 3.0     |
| PLÁNY SMB 3.02    |

> [AZURE.IMPORTANT] Kopírování nebo ukládat soubory chráněné pomocí Windows soubor systému souborů EFS souborový server StorSimple virtuální matice; Výsledkem bude Nepodporovaná konfigurace. 

## <a name="networking-requirements"></a>Požadavky na sítě 

V následující tabulce jsou uvedeny porty, které je potřeba otevřít v bráně firewall umožňující iSCSI SMB, cloudu, Správa přenosy dat nebo. V této tabulce *v* příchozích a *odchozích* odkazuje na směr, odkud příchozí žádosti klienta o přístup k zařízení. *Nebo *odchozí* * odkazuje na směr, ve kterém zařízení StorSimple odešle data externě, za nasazení: například odchozí k Internetu.

| **Číslo portu<sup>1</sup>** | **Či oddálení** | **Obor port** | **Povinné**              | **Poznámky**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Rezervovat           | WAN            | Ne                        | Port pro odchozí spojení se používá pro přístup k Internetu vyhledat aktualizace. <br></br>Webový odchozí proxy server je uživatel, která dokáže nahradit. |
| TCP 443 (HTTPS)          | Rezervovat           | WAN            | Ano                       | Port pro odchozí spojení se používá pro přístup k datům v cloudu. <br></br>Webový odchozí proxy server je uživatel, která dokáže nahradit. |
| UDP 53 (DNS)             | Rezervovat           | WAN            | V některých případech; v části poznámky. | Tento port jenom v případě, že používáte internetové DNS server požaduje. <br></br> **Poznámka**: Pokud nasazení souborovém serveru, doporučujeme vám použít místního serveru DNS.|
| UDP 123 (NTP)            | Rezervovat           | WAN            | V některých případech; v části poznámky. | Tento port je požadován jenom v případě, že používáte server Internetové NTP.<br></br> **Poznámka:** Pokud nasazení souborovém serveru, doporučujeme synchronizace času s řadiče domény Active Directory.  |
| TCP 80 (HTTP)           | V            | MÍSTNÍ SÍTĚ            | Ano                       | Jedná se o příchozí port pro místní uživatelské rozhraní na zařízení StorSimple místní správy. <br></br> **Poznámka**: přístup k rozhraní místní přes HTTP bude automaticky přesměrovávat na HTTPS.|
| TCP 443 (HTTPS)          | V            | MÍSTNÍ SÍTĚ            | Ano                       | Jedná se o příchozí port pro místní uživatelské rozhraní na zařízení StorSimple místní správy.|
| TCP 3260 (iSCSI)         | V            | MÍSTNÍ SÍTĚ            | Ne                        | Tento port slouží k přístupu k datům prostřednictvím iSCSI.|

<sup>1</sup> žádné příchozí porty muset otevřít na veřejné Internetu.

> [AZURE.IMPORTANT] Zajistit, aby bránu firewall upravit nebo dešifrování všechny přenosy SSL mezi StorSimple zařízení a Azure.


### <a name="url-patterns-for-firewall-rules"></a>Adresa URL vzory pravidla brány firewall 

Správci sítě můžete nakonfigurovat často Upřesnit firewall pravidla založená na adresu URL vzory k filtrování příchozí a odchozí přenosy. Virtuální matice a služba StorSimple správce, závisí na jiné aplikace Microsoft například Bus služby Azure, Azure Active Directory, řízení přístupu, úložiště účty a servery služby Microsoft Update. Adresa URL vzory související s těmito aplikacemi mohou sloužit k konfigurace pravidel brány firewall. Je důležité pochopit, že můžete změnit adresu URL vzory související s těmito aplikacemi. To zase vyžaduje správce sítě ke sledování a aktualizace pravidel brány firewall pro StorSimple jako a v případě potřeby. 

Doporučujeme nastavit pravidla brány firewall pro odchozí přenosy dat podle StorSimple liberally pevnou IP adres ve většině případů. Však níže uvedených informací můžete nastavit pravidla Upřesnit brány firewall, které jsou potřeba k vytvoření zabezpečené prostředí.

> [AZURE.NOTE] 
> 
> - Zařízení (zdrojový) IP adresy by měl nastavit vždy všechny rozhraní povolena cloudu sítě. 
> - Cíl IP adresy by měl nastavit [Azure datacentra rozsahy IP adres](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| Vzor adresy URL                                                      | Součásti nebo funkce                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Služba StorSimple správce<br>Služba řízení přístupu<br>Bus služby Azure|
|`http://*.backup.windowsazure.com`|Registrace zařízení|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Odvolání certifikátů |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Účty Azure úložiště a sledování |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Servery služby Microsoft Update<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Podpora balíčku                                                  |
| `http://*.data.microsoft.com `                   | Služba telemetrie v systému Windows, najdete v článku [aktualizace pro zkušeností zákazníků a diagnostiky telemetrie](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Další krok

-   [Příprava na portálu pro nasazení své StorSimple virtuální pole](storsimple-ova-deploy1-portal-prep.md)
