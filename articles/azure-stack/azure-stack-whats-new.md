<properties
    pageTitle="Co je nového ve vrstvě Azure | Microsoft Azure"
    description="Co je nového ve vrstvě Azure"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Co je nového v Azure zásobníku Technical Preview 2
Tato verze obsahuje nové funkce pro klienty a správce.

## <a name="network"></a>Sítě   
   - [iDNS](azure-stack-understanding-dns-in-tp2.md) nabízí registrace názvu vnitřní síti a řešení systému DNS (Domain Name) bez dalších infrastruktury služby DNS.
   - [Virtuální sítě bran](azure-stack-create-vpn-connection-one-node-tp2.md) poskytují možnosti připojení VPN Azure nebo místní zdroje.
   - Směruje definované uživatele můžete směrovat síťová komunikace přes brány firewall, zabezpečení, jiná zařízení a další služby.
   - Síť zdroje můžete vytvořit z Marketplace.   

## <a name="storage"></a>Úložiště
 - [Azure fronty](https://msdn.microsoft.com/library/dd179353.aspx) povolit spolehlivý a jestli ho trvalý služeb pro zasílání zpráv.
 - Data o výkonu úložiště zachycení [úložiště analýzy](https://msdn.microsoft.com/library/azure/hh343270.aspx) . Tato data umožňuje sledovat žádosti o analýze trendů použití a Diagnostika problémů s účtem úložiště.
 - Úložiště objektů BLOB podporuje [připojení blok operace](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Podpora účtu úložiště rozhraní API Premium.
 - Klient úložiště služby podpory pro běžné nástroje a SDK, například rozhraní příkazového řádku Azure Powershellu, .NET, Python a Java SDK. 
 - [Podpis přístup sdílené účtu úložiště](https://msdn.microsoft.com/library/azure/mt584140.aspx) povolit podrobného delegování přístupu ke službám úložiště aniž by bylo nutné sdílet svůj klíč účtu celou.  
 - Úložiště služby nyní používat [Účty služby spravovaných skupiny](https://technet.microsoft.com/library/hh831477.aspx) pro silných cenného papíru s nízkou správy režijních.
 - Můžete uvolnit nepoužitý klienta zdroje na vyžádání.
 - Délka uchovávání informací účtu odstraněné úložiště je, která dokáže nahradit.
 - Můžete obnovit odstraněné klienta úložiště účty.

## <a name="compute"></a>Výpočet
- Můžete zrušit virtuálních počítačích.
- Můžete nasadit rozšíření virtuálního počítače z důvodu řešení potíží a konfigurace správy.
- Změna velikosti disků virtuálního počítače.
- Virtuálních počítačích může obsahovat více síťových rozhraní.

### <a name="portal-experience"></a>Práce s portálem
 - Azure zásobníku regionech jsou logické jednotky měřítko a řízení v Azure vrstvě. V této verzi preview můžete zobrazit informace o služby, jako třeba výpočetním, sítě a úložiště podle oblasti.
 - Teď můžete zobrazit náhled rozhraní [azure updates.md zásobníku] (aktualizace).

## <a name="key-vault"></a>Klíčové trezoru
- [Klíč trezoru ve vrstvě Azure](azure-stack-kv-intro.md) poskytuje zabezpečené správy klíčů a hesla pro aplikace cloudu.
- Můžete auditování sledovat a použití klíče tak, že aplikace a VMs.

## <a name="billing-and-usage"></a>Fakturace a použití
 - [Fakturace a spotřeby rozhraní API](azure-stack-billing-and-chargeback.md) vystavit data na jak spotřebované svých služeb.  
 - Delegovaná poskytovatelů povolit prodejců nabízet služby Azure zásobníku svým zákazníkům.

## <a name="monitoring-and-health"></a>Sledování a stavu
 - Nové možnosti k dispozici v portálu a rozhraní API sledování umožňují včasným zobrazovat a spravovat upozornění na prostředí.  

## <a name="next-steps"></a>Další kroky
- [Princip Azure zásobníku Koncepce architektura](azure-stack-architecture.md)      
- [Princip zjistit předpoklady pro nasazení](azure-stack-deploy.md)
- [Nasazení Azure zásobníku](azure-stack-run-powershell-script.md)

  
