<properties
   pageTitle="Hybridní pracovního postupu Runbook: Úlohy postupu runbook ukončí se stavem Suspended | Microsoft Azure"
   description="Příznaky příčiny a řešení chyby ukončení pracovního postupu Runbook hybridní projektu."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybridní pracovního postupu Runbook: Úlohy postupu runbook ukončí se stavem Suspended

## <a name="summary"></a>Souhrn

Vaše postupu runbook se pozastaví brzy po pokusu o spuštění třikrát. Jsou podmínky, které může přerušit postupu runbook úspěšnému dokončení a související chybová zpráva neobsahuje žádné další informace označující důvod, proč. Tento článek obsahuje kroky pro řešení potíží při problémech selhání spuštění pracovního postupu Runbook hybridní postupu runbook.

Pokud Azure problém není určená v tomto článku, navštivte fóra Azure na [webu MSDN a přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete publikovat problém na tyto fóra nebo na [ @AzureSupport na Twitter](https://twitter.com/AzureSupport). Žádost o Azure podporu můžete poslat taky, tak, že vyberete, **využijte možnost získání podpory** na webu [Azure podpory](https://azure.microsoft.com/support/options/) .

## <a name="symptom"></a>Příznaku

Spuštění postupu Runbook selhání a vrácena chyba, "úlohy akci, která"Aktivovat"nedá spustit, protože proces zastavení. Akce úlohy pokusu o 3krát."


## <a name="cause"></a>Příčina

Existuje několik možných příčin chyby: 

  1. Hybridní pracovník je za proxy nebo brány firewall
  2. V počítači spuštěné pracovní hybridní na je nižší než minimální hardwarové [požadavky](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. Runbooks nemůže ověřit s místními prostředky


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>: 1 pracovního postupu Runbook hybridní vznikne za proxy nebo brány firewall

Počítač, který je spuštěna pracovního postupu Runbook hybridní je za bránu firewall nebo proxy server a přístup k síti odchozí nemusí povoleno nebo správně nakonfigurované.

### <a name="solution"></a>Řešení

Zkontrolujte počítač má přístup pro odchozí připojení na *. cloudapp.net na porty 443 9354 a 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Příčina 2: Počítač má nainstalovanou starší než minimální požadavky na hardware

Počítače se systémem pracovního postupu Runbook hybridní by splňovat minimální požadavky na hardware před jmenování ho hostovat tuto funkci. V závislosti na využití prostředků jiných procesy na pozadí a konflikty způsobená runbooks během provádění jinak počítač bude stát se jejím delší než využít a způsobovat zpoždění úlohy postupu runbook nebo vypršení časového limitu. 

### <a name="solution"></a>Řešení 

Nejdřív potvrďte, že určenou ke spuštění pracovního postupu Runbook hybridní funkci počítač splňuje minimální požadavky na hardware.  Pokud ano, sledujte využití procesoru a paměti k určení všechny korelace výkonu pracovního postupu Runbook hybridní procesy a Windows.  Pokud se sloupcem procesor tlaku či paměti může to znamenat potřebu upgradu nebo přidat další procesory nebo zvýšit paměti adresy kritický zdroje a vyřešení chyby. Můžete taky vyberte zdroj různých výpočetním, který podporuje minimální požadavky a měřítko při vytížení označuje, že je potřeba zvýšení.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Příčina 3: Runbooks nemůže ověřit s místními prostředky

### <a name="solution"></a>Řešení

Vyhledejte v protokolu událostí **Microsoft SMA** pro odpovídající událost s popisem *Win32 k ukončení procesu s kódem [4294967295]*.  Příčiny této chyby je ještě konfigurovat ověření vaší runbooks nebo uvedené spustit jako přihlašovací údaje pro hybridní pracovní skupina.  Zkontrolujte [postupu Runbook oprávnění](automation-hybrid-runbook-worker.md#runbook-permissions) k potvrzení, že jste správně nakonfigurovali ověřování pro váš runbooks.  


 

