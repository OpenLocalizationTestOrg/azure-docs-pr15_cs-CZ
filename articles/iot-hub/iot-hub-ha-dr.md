<properties
 pageTitle="Rozbočovač IoT HA a DR | Microsoft Azure"
 description="Popisuje funkce, které umožňují vytvářet vysoce dostupné IoT řešení s havárie možnosti obnovení."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Obnovení vysoké dostupnost a havárie IoT centrální

Jako služby Azure IoT Centrum poskytuje dostupnost (HA) pomocí redundance na úrovni Azure oblast, bez jakékoli další práce vyžadované řešení. Kromě toho Azure nabízí celou řadu funkcí, které umožňují vytvářet řešení s možnosti obnovení (DR) havárie nebo více oblastí dostupnost případné potíže. Musíte návrhu a připravit řešení umožní využít výhod tyto funkce DR, pokud chcete zadat globální, více oblastí dostupnost zařízením nebo uživatelům. Článek [Azure firmy kontinuitu příručku](../resiliency/resiliency-technical-guidance.md) popisuje zabudovaných funkcí v Azure nepřerušený a DR. [Obnovení katastrofě a dostupnost pro Azure aplikace][] papír obsahuje architektura pokyny strategie pro Azure aplikací k dosažení HA a DR.

## <a name="azure-iot-hub-dr"></a>Azure DR IoT rozbočovači
Kromě uvnitř oblasti HA používá IoT centrální převzetí mechanismy havárie obnovení vyžadující zásah od uživatele. IoT centrální DR je vlastním zahájená a má cíle časového využití (RTO) 2 26 hodin a tyto cíle bodu obnovení (RPOs).

| Funkce | OPERACE RPO |
| ------------- | --- |
| Dostupnost služby pro operace registru a komunikace | Možné ztrátě CName |
| Data identity v registru identity zařízení | 0-5 minut ztrátu dat |
| Zprávy zařízení do cloudu | Dojde ke ztrátě všech nepřečtených zpráv |
| Operace sledování zpráv | Dojde ke ztrátě všech nepřečtených zpráv |
| Shluk zařízení zprávy | 0-5 minut ztrátou dat |
| Shluk zařízení názory fronty | Dojde ke ztrátě všech nepřečtených zpráv |

## <a name="regional-failover-with-iot-hub"></a>Místní převzetí s IoT centrální

Dokončení zpracování topologie nasazení řešení IoT je mimo rozsah v tomto článku, ale pro účely dostupnost a obnovení jsme zvážíme *Místní selhání* nasazení modelu.

V modelu místní převzetí řešení back-end běží především na jednom místě datacentru, ale další rozbočovač IoT a back-end používají v jiném umístění datacentra pro účely převzetí v případě centrem IoT v primární datacentra utrpí výpadku nebo nějakého důvodu přerušení připojení k síti ze zařízení pro primární datacentra. Zařízení pomocí koncový bod sekundární služby kdykoli primární brány není dostupný. S více oblastí převzetí funkce bylo možné za dostupnost jedné oblasti zlepšit dostupnost řešení.

Na vysoké úrovni implementovat modelu místního převzetí s IoT centrální, budete potřebovat takto.

* **Sekundární IoT centrální a zařízení směrování použití logických operátorů**: v případě přerušení služby ve vaší primární oblasti, musíte spuštění zařízení připojení k sekundární oblast. Možnost rozpoznání stavu přírodní většinou služeb souvisejících, je běžné řešení správce spustí proces překlopení mezi oblast. Nejlepší způsob, jak komunikovat nový koncový bod pro zařízení, a zachovat řízení procesu, je jejich pravidelně kontrolovat službu *concierge* aktuální aktivního koncového bodu. Služba concierge může být jednoduchý webovou aplikaci, která je replikovat a dostupný technik přesměrování DNS (například pomocí [Správce dopravy Azure][]).
* **Replikace registru identity** - za účelem použitelné, sekundární IoT centrální musí obsahovat všechny identity zařízení, které se mohou připojit k řešení. Řešení by měly zachovat replikovat geo zálohy identit zařízení a nahrajte je na vedlejší centrální IoT před změnou aktivní koncový bod pro zařízení. Funkce export identity zařízení IoT rozbočovače je velmi užitečné v tomto kontextu. Další informace najdete příručce [IoT centrální vývojář – identity registru][].
* **Sloučení logiky** - při oblasti primární znovu k dispozici, všechna data, která jste vytvořili v sekundární webu a vrátil musí k poštovním zpátky k oblasti primární. Většinou se vztahuje k zařízení identitami a aplikace metadata, která je nutno sloučit s primárním rozbočovači IoT a další úložiště specifické pro aplikaci v oblasti hlavní. Pro zjednodušení tento krok, obvykle doporučujeme používat idempotent operace. To minimalizuje důsledky nejen z případné konzistentní výskyt události, ale taky z duplicitní položky nebo doručení mimo pořadí událostí. Kromě toho logiku aplikace by měl být pro tolerovat potenciální nekonzistencí nebo "mírně" mimo datum stavu. To je kvůli další časové náročnosti systému "retušovat" na základě obnovení čárky cíle (operace RPO).

## <a name="next-steps"></a>Další kroky

Tyto odkazy vedou na další informace o Azure IoT centrální:

- [Začínáme s IoT rozbočovače (kurz)][lnk-get-started]
- [Co je Centrum IoT Azure?][]

[Obnovení havárie a dostupnost pro Azure aplikace]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure přenosy správce]: https://azure.microsoft.com/documentation/services/traffic-manager/
[Rozbočovač IoT Developer Guide – identity registru]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Co je Centrum IoT Azure?]: iot-hub-what-is-iot-hub.md
