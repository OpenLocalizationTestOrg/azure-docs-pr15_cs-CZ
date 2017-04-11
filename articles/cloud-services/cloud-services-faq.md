<properties
    pageTitle="Cloud Services nejčastější dotazy týkající se | Microsoft Azure"
    description="Nejčastější dotazy k Cloudovým službám."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloudových služeb časté otázky
Tento článek odpovídá na některé časté otázky k Microsoft Azure Cloud Services. Můžete také navštívit [Azure podporují nejčastější dotazy týkající se](http://go.microsoft.com/fwlink/?LinkID=185083) pro obecné Azure ceny a informace o podpoře. Můžete taky použijte [Cloud Services OM velikosti stránky](cloud-services-sizes-specs.md) pro informace o velikosti.

## <a name="certificates"></a>Certifikáty

### <a name="where-should-i-install-my-certificate"></a>Kde můžu nainstalovat certifikát?

- **Moje**  
Aplikace certifikátu s privátním klíčem (\*.pfx, \*p12).

- **CERTIFIKAČNÍ AUTORITA**  
Přechodné certifikáty přejděte v obchodě (zásady a Sub čas).

- **KOŘENOVÉ**  
Uložte kořenové CA, aby vaše hlavní kořenové CA certifikátu by měl přejděte sem.

### <a name="i-cant-remove-expired-certificate"></a>Nemůžete odebrat vypršela platnost certifikátu

Azure zabrání odebrání certifikátu, dokud je v použít. Budete muset odstranit nasazení, která používá certifikát nebo aktualizovat nasazení pomocí jiných nebo obnovených certifikátu.

### <a name="delete-an-expired-certificate"></a>Odstranění certifikátu vypršela platnost

Dokud není používán certifikát, můžete použít rutinu Powershellu [Odebrat AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) odebrat certifikát.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Vypršela platnost certifikáty pro rozšíření s názvem Správa služby Windows Azure

Tato poukázky vzniká kdykoli rozšíření se přidá do cloudové služby, třeba koncovku Vzdálená plocha. Tato poukázky slouží jenom pro šifrování a dešifrování soukromé konfigurace rozšíření. Pokud tato poukázky platnost nezáleží. Datum vypršení platnosti není zaškrtnuté políčko.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Certifikáty, které jste odstranili znovu zobrazuje

Tyto znovu zobrazuje nejspíš kvůli nástroj, který používáte, například Visual Studio. Pokaždé, když se znovu připojíte s nástroj, který používá certifikát, bude znova nahrát Azure.

### <a name="my-certificates-keep-disappearing"></a>Zachovat zmizení certifikáty

Když recykluje instance virtuálního počítače všechny místní změny budou ztraceny. [Spuštění úlohy](cloud-services-startup-tasks.md) použijte nainstalovat certifikáty do virtuálního počítače každém spuštění roli.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Nemůžu najít správy certifikáty na portálu

[Osvědčení o řízení](..\azure-api-management-certs.md) jsou dostupné jenom v portálu klasické Azure. Portál aktuální Azure nepoužívá osvědčení o řízení. 

### <a name="how-can-i-disable-a-management-certificate"></a>Jak můžu vypnout Správa certifikátů

[Osvědčení o řízení](..\azure-api-management-certs.md) se nedá zakázat. Odstraníte je portálu klasické Azure nechcete, aby se už používá.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Jak vytvořit certifikát SSL pro konkrétní IP adresu?

Postupujte podle pokynů [vytvořte kurz certifikát](cloud-services-certs-create.md). Použijte jako název DNS na IP adresu.

## <a name="security"></a>Zabezpečení

### <a name="disable-ssl-30"></a>Zakázání SSL 3.0

Zakažte SSL 3.0 a TLS zabezpečení, vytvořte spuštění úkol, který je popsána na tento příspěvek blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Změna velikosti do cloudové služby

### <a name="i-cannot-scale-beyond-x-instances"></a>Můžu nelze měřítko za X instance

Vaše předplatné Azure má omezení počtu jádra, které můžete použít. Změna měřítka nebudou fungovat, pokud jste použili k dispozici jádra. Například pokud máte maximálně 100 jádra, to znamená, může mít 100 instancí virtuálního počítače A1 velikosti cloudové službě nebo 50 A2 velikosti instance virtuálního počítače.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Nelze rezervovat IP v více VIP cloudové službě

Nejprve zkontrolujte, zda je zapnutá virtuálního počítače instanci, kterou se snažíte rezervovat IP pro. Druhé Ujistěte se, že používáte rezervovaná IP adresy pro to vůbec měl nasazení pracovní a výrobních. **Co nedělat** nastavením při upgradu nasazení.

