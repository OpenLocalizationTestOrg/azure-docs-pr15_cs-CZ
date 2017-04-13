<properties
   pageTitle="Červené klobouk aktualizace infrastruktury (RHUI) | Microsoft Azure"
   description="Další informace o červené klobouk aktualizace infrastruktury (RHUI) pro okamžitou červené klobouk Enterprise Linux instancí v Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Červené klobouk aktualizace infrastruktury (RHUI) pro červené klobouk na vyžádání Enterprise Linux VMs v Azure

Přístup k červené klobouk aktualizace infrastruktury (RHUI) nasazenou v Azure jsou registrovány virtuálních počítačích vytvořená z na vyžádání červené klobouk Enterprise Linux (RHEL) obrázky k dispozici v Azure Marketplace.  Na vyžádání RHEL instance mít přístup do místního yum úložiště a moct přijímat přírůstková aktualizace.

Seznam yum úložiště, který se spravuje RHUI, je nakonfigurovaný RHEL instance během vytváření. Není potřeba provést další konfiguraci – spuštění `yum update` po RHEL instance je připravená k získání nejnovější aktualizace.

> [AZURE.NOTE] Azure infrastruktury RHUI byl naposledy aktualizoval (září 2016) a vyžaduje změny v konfiguraci existující instancí RHEL pro nepřetržitého přístupu k Azure RHUI. Přečtěte si část aktualizace infrastruktury Azure RHUI podrobnosti.


## <a name="rhui-azure-infrastructure-update"></a>Aktualizace Azure infrastruktury RHUI
K září 2016 obsahuje Azure novou sadu serverů červené klobouk aktualizace infrastruktury (RHUI). Tyto servery jsou nasazeny s [Azure přenosy správce]( https://azure.microsoft.com/services/traffic-manager/) , aby jeden koncový bod (rhui 1.micrsoft.com) můžete použít libovolný OM bez ohledu na oblast. Jsou taky pomocí certifikátu SSL, který je zřetězen známý certifikační autorita (kořenové Baltimore). Tuto aktualizaci provádění automatické měl by nebezpečného pro některé zákazníky, kteří mají ACL nebo vlastní tabulkách RHUI aktualizace servery, aby tato aktualizace je "určovat, jestli se změnami." Ruční postup rychlého připojení k těmto nové serverům jsou dostupné na tuto stránku a celý skript pro rychlého připojení automatické způsobem (po ověření jednotlivých kroků). Nové obrázky RHEL srážek daně ze MZDY v Azure Marketplace (verze datem dne 2016 nebo novější) se automaticky přejděte na příkaz nové Azure RHUI servery a nevyžaduje žádnou další akci.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Nové osy rychlého připojení infrastruktury Azure RHUI

| Datum | Poznámka: |
| --- | --- |
|22 září 2016 | RHUI servery a nainstalovat pokyny k dispozici. VMs nasazených pomocí nových (s datem dne 2016) RHEL srážek daně ze MZDY marketplace obrázky automaticky použije nové RHUI servery, ale existující VMs jsou "určovat, jestli se změnami"
|1 dne 2016 | Starší verze obrázků RHEL srážek daně ze MZDY OM, (které použít starý servery Azure RHUI) budou odebrány z Galerie Azure Marketplace
|16 leden 2017 | Bude vyřadit z provozu starý Azure RHUI servery. Aktualizovat všechny vaše problémového RHEL VMs srážek daně ze MZDY tak, že tentokrát spravovat přístup k Azure RHUI

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Jsou IP adresy pro nové servery RHUI doručování obsahu

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Postup ruční aktualizace pomocí nových serverů Azure RHUI

Stáhněte (přes otočení) veřejný klíč

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Ověření stažený klíč

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Kontrola výstup, ověřte `keyid` a `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Instalace veřejný klíč

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Stáhněte si ověřte a nainstalujte klienta ot

Položka ke stažení: Pro RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Pro RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Ověření:

```
rpm -Kv azureclient.rpm
```

Kontrola do výstupu podpis balíčku je v pořádku

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Instalace OTÁČKY

```
sudo rpm -U azureclient.rpm
```

Po dokončení ověřte, jestli mají přístup k Azure RHUI formuláře OM

### <a name="all-in-one-script-for-automating-the-above-task"></a>Vše v jednom skriptu pro automatizaci výše uvedených úkolů
Podle potřeby použijte tento skript k automatizaci úkolů aktualizace problémového VMs nové Azure RHUI servery.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>Přehled RHUI
[Červené klobouk aktualizace infrastruktury](https://access.redhat.com/products/red-hat-update-infrastructure) nabízí vysoce scalable řešení pro správu obsahu úložiště yum červené klobouk Enterprise Linux mraků instancí hostované certifikované červené klobouk mraků poskytovatelů. Podle odesílání dat projektu buničiny RHUI umožňuje cloudu poskytovatelů místně odrážet hostované červené klobouk úložiště obsahu, vytvoříte vlastní úložištích s vlastním obsahem a zpřístupnit tyto úložištích pro velkou skupinu koncových uživatelů prostřednictvím systému Vyrovnávání zatížení obsahu doručení.

## <a name="regions-where-rhui-is-available"></a>Kde je k dispozici RHUI oblastí
RHUI je k dispozici ve všech oblastech, které jsou k dispozici RHEL na vyžádání obrázky. Nyní obsahuje všechny veřejné oblastí uvedená na stránku [řídicího panelu Azure stav](https://azure.microsoft.com/status/) a oblastí Azure nám pro státní správu. RHUI přístupu pro VMs zřízení z obrázků na vyžádání RHEL je součástí jejich cena. Jsme rozbalte položku dostupnost na vyžádání RHEL v budoucnu bude aktualizován dostupnost další místní/vnitrostátní cloudu.

> [AZURE.NOTE] Přístup k hostované Azure RHUI se omezí na VMs v rámci [rozsahy Microsoft Azure Datacentra IP adres](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Získávání aktualizací z jiného aktualizace úložiště

Pokud je třeba získávání aktualizací z různých aktualizace úložiště (místo hostované Azure RHUI) bude muset zrušení registrace instancí z RHUI a znovu zaregistrujte se požadované aktualizace infrastruktury (třeba červeným klobouk satelitní nebo červené klobouk zákazníka portál CDN). Budete potřebovat příslušné červené klobouk předplatné pro těchto služeb a registrace pro [Přístup ke cloudu klobouk červené v Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

Unregister RHUI a znovu zaregistrujte svůj sledovat infrastruktury aktualizace pod kroky.

1.  Upravte /etc/yum.repos.d/rh-cloud.repo a změňte všechny `enabled=1` k `enabled=0`. Příklad:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  Upravte /etc/yum/pluginconf.d/rhnplugin.conf a změňte `enabled=0` k `enabled=1`.
3.  Zaregistrujte se požadované infrastruktury, třeba červeným klobouk zákazníka portálu. Postupovat podle průvodce řešení červené klobouk o [tom, jak zaregistrovat a přihlášení k odběru systému do portálu červené klobouk zákazníka](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Přístup k RHUI hostované Azure je součástí cena obrázek přislíbený srážek daně (ze RHEL MZDY). Registrace OM RHEL srážek daně ze MZDY z Azure hostované RHUI nepřevádí virtuálního počítače do přenést svůj-vlastní-licence (BYOL) typ OM a proto se může být nabíhání dvojité poplatků za Pokud zaregistrovat stejné OM u jiného zdroje aktualizací. 
> 
> Pokud potřebujete konzistentní použít infrastrukturu aktualizace jiné než hostované Azure RHUI zvažte vytvoření a nasazení vlastní obrázky (BYOL typu) podle popisu v článku [Vytvoření a odeslání klobouk červené virtuální počítačem pro Azure](virtual-machines-linux-redhat-create-upload-vhd.md) .

## <a name="next-steps"></a>Další kroky
Vytvoření virtuálního červené klobouk Enterprise Linux počítače z Azure Marketplace přislíbený obrázek a využití hostované Azure RHUI nedokážete [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Budete moct použít `yum update` RHEL instance bez žádné další nastavení.