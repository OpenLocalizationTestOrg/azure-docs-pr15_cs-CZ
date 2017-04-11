<properties
  pageTitle="Vlastní sond Vyrovnávání zatížení a sledování stavu | Microsoft Azure"
  description="Naučte se používat vlastní sond pro vyrovnávání zatížení Azure sledování instancí za vyrovnávání zatížení"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Princip sond Vyrovnávání zatížení

Azure Vyrovnávání zatížení nabízí možnost sledování stavu instance serveru pomocí sond. Když zkušební přestane odpovídat, Vyrovnávání zatížení ukončí odesílání nových připojení chybná instanci. Nemá vliv existující připojení a nový připojení se odešle správný instance.

Cloudové služby role (pracovní rolí a webu) použijte agenta guest ke sledování zkušební. Pokud použijete virtuálních počítačích za vyrovnávání zatížení, musí být nakonfigurované TCP a HTTP vlastní sond.

## <a name="understand-probe-count-and-timeout"></a>Princip zkušební počet a časových limitů

Zkušební postup závisí na:

- Počet úspěšných sond, které umožňují instanci popisky jako nahoru.
- Počet neúspěšných sond, které způsobují instanci popisky jako dolů.

Časový limit a četnost nastavenou v SuccessFailCount zjistit, zda je instance potvrzeno být běží nebo ne. Na portálu Azure časový limit nastavenou dvakrát hodnotu četnosti.

Konfigurace zkušební všechny instance Vyrovnávání zatížení pro koncový bod (to znamená Vyrovnávání zatížení sady) musí být stejný. To znamená, že ve stejném hostovanou službu pro určitý koncový bod kombinaci nesmí obsahovat různé zkušební konfigurace pro každou roli instance nebo virtuálního počítače. Pokaždé, například, musí mít stejné místní porty a protokoly časové limity.

>[AZURE.IMPORTANT] Vyrovnávání zatížení zkušební používá IP adresu 168.63.129.16. Tento veřejnou IP adresu usnadňuje komunikaci na vnitřní platformy materiály k přenést svůj vlastní IP Azure virtuální scénáři. Virtuální veřejnou IP adresu 168.63.129.16 se používá ve všech oblastech a se nezmění. Doporučujeme, abyste tento IP adresu povolit ve všech zásad místní brány firewall. Neměli používat rizika zabezpečení protože pouze interní Azure platformu můžete zdroje na zprávu od, které budou řešit. Pokud není to uděláte, bude neočekávané chování v různých scénářích jako konfigurace na stejnou oblast IP adresu 168.63.129.16 a s duplicitní IP adres.

## <a name="learn-about-the-types-of-probes"></a>Další informace o typech sond

### <a name="guest-agent-probe"></a>Zkušební agent hosta

Tento zkušební neexistuje cloudové služby Azure pouze. Vyrovnávání zatížení využívá hosta agenta do virtuálního počítače, sleduje a odpoví na HTTP 200 OK odpověď pouze v případě instance připravena (tedy ne v jiném uveďte například zaneprázdněn(a), recyklace nebo zastavení).

Další informace najdete v tématu [Konfigurace soubor definice služby (csdef) pro stav sond](https://msdn.microsoft.com/library/azure/ee758710.aspx) nebo [začít vytvářet Vyrovnávání zatížení internetové služby cloudu](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Co dělá zkušební agent guest označit instance jako chybné?

Pokud agent hosta přestane odpovídat HTTP 200 OK, Vyrovnávání zatížení označí instance jako odpovídat a ukončí odesílání přenosy na tuto instanci. Vyrovnávání zatížení i nadále ping instance. Pokud agent hosta odpoví HTTP 200, Vyrovnávání zatížení odešle přenosy na tuto instanci znovu.

Při použití roli web kód webu obvykle běží ve w3wp.exe, který není kontroloval Azure agent struktury nebo Host. To znamená, že k chybám v w3wp.exe (například HTTP 500 odpovědi) nebude nahlášený agenta hosta a vyrovnávání zatížení nevezme tuto instanci mimo otočení.

### <a name="http-custom-probe"></a>Vlastní zkušební HTTP

Vlastní zkušební Vyrovnávání zatížení HTTP přepíše výchozí zkušební agent Host, což znamená, že můžete vytvořit vlastní logiky stav instanci rolí. Vyrovnávání zatížení sond koncový bod každých 15 sekund, ve výchozím nastavení. Instanci je považován za v otočení Vyrovnávání zatížení Pokud odpoví HTTP 200 v rámci doby vypršení časového limitu (31 sekund, než ve výchozím nastavení).

To může být užitečné, pokud chcete implementovat vlastní logiku odeberete otočení Vyrovnávání zatížení instance. Například může rozhodnete odebrat instance, pokud je nad 90 % procesoru a vrátí-200 stav. Pokud máte web role, které používají w3wp.exe, znamená to, taky můžete použít automatické sledování webu, protože k chybám v kódu webu vrátí-200 stav zkušební Vyrovnávání zatížení.

>[AZURE.NOTE] Vlastní zkušební HTTP podporuje pouze protokolu HTTP a relativní cest. Protokol HTTPS nepodporuje.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Co dělá vlastní zkušební HTTP označit instance jako chybné?

- Žádost HTTP vrátí kód odpovědi HTTP než 200 (například 403 404 a 500). Toto je kladné potvrzení, instance aplikace vzít mimo služby hned.
- Nastavit informace HTTP server nereaguje vůbec po časový limit. V závislosti na hodnotu časového limitu, která je nastavená, může to znamenat, které více zkušební žádosti přejít nezodpovězené předtím, než zkušební získá označená jako neběží (to znamená před SuccessFailCount sond posílají).
- Server slouží k zavření připojení prostřednictvím protokolu TCP obnovit.

### <a name="tcp-custom-probe"></a>Vlastní zkušební TCP

TCP sond zahájit připojení provedením třícestné s definované číslo portu.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Co dělá TCP vlastní zkušební označit instance jako chybné?

- TCP server nereaguje vůbec po časový limit. Když zkušební označen jako neběží závisí na počet neúspěšných zkušební požadavky nakonfigurované přejdete nezodpovězené před označením zkušební jako není spuštěný.
- Zkušební obdrží TCP v instanci rolí obnovit.

Další informace o konfiguraci zkušební stavu HTTP nebo TCP zkušební najdete v článku [Začínáme vytváření Vyrovnávání zatížení internetového v správce prostředků pomocí Powershellu](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Přidání správný instancí zpátky do otočení Vyrovnávání zatížení

TCP a HTTP sondy jsou považovány za správný a označení instanci rolí za správný, pokud:

- Vyrovnávání zatížení získá kladné zkušební při prvním spuštění OM.
- Číslo SuccessFailCount (viz výše) určuje hodnotu úspěšné sond, která jsou potřebná k označení instanci rolí jako správný. Pokud byla odebrána instanci rolí, musíte počet úspěšné, po sobě jdoucí sond rovnat nebo vyšší než hodnota SuccessFailCount označíte instanci rolí jako spuštění.

>[AZURE.NOTE] Pokud kolísající stavu instanci rolí čeká Vyrovnávání zatížení již před přepnutím instanci rolí zpět v pořádku stavu. Důvodem je prostřednictvím zásad můžete chránit uživatele a infrastrukturu.

## <a name="use-log-analytics-for-load-balancer"></a>Použití protokolu analýz pro vyrovnávání zatížení

Informace o stavu zkušební a počtu zkušební můžete [protokolu analýz pro vyrovnávání zatížení](load-balancer-monitor-log.md) . Protokolování lze s Power BI nebo provozní přehledy Azure zadejte statistických údajů o vyrovnávání zatížení stavu.
