<properties
   pageTitle="Internet doporučené postupy pro zabezpečení věci | Microsoft Azure"
   description="Článek obsahuje seznam curated Microsoft Internet věci zabezpečení osvědčené postupy a obecné doporučení."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Internet věci zabezpečení doporučené postupy

Zabezpečení infrastruktury Internet věcí (IoT) je důležité podniku nikoho souvisejících se IoT řešení. Počet zařízení souvisejících a distribuované přírodní tato zařízení dopad událost nastavit jako zabezpečení týkající se k narušení milionů IoT zařízení je není rozsáhlé a může mít rozšířených dopad.

Z tohoto důvodu musí IoT zabezpečení zabezpečení v název hloubkové přístup. Data musí být zabezpečený v cloudu a pohyb v soukromé a veřejné sítích. Metody musejí být na místě bezpečně zřízení zařízení IoT sami. Každé vrstvy, na zařízení k síti na cloud back-end musí záruky silné zabezpečení.

Doporučené postupy IoT můžete rozdělit následujícím způsobem:

- Výrobce hardwaru IoT nebo integrátor
- Karta Vývojář v IoT řešení
- IoT řešení nasazovatel
- Operátor IoT řešení

Tento článek shrnuje [Z věci zabezpečení doporučené postupy pro Internet](../iot-suite/iot-security-best-practices.md). Naleznete v tomto článku podrobnější informace.

## <a name="iot-hardware-manufacturer-or-integrator"></a>Výrobce hardwaru IoT nebo integrátor

Pokud jste výrobce hardwaru IoT nebo integrátor hardwaru, postupujte podle následující doporučené postupy:

- **Obor hardware minimální požadavky**: návrh hardwaru by měl obsahovat minimální funkce nutné pro fungování hardware a nic Další. 
- **Zkontrolujte hardwaru takovéto manipulace doklad**: vytváříte v mechanismy zjišťování fyzické falšování hardwaru, například otevřením titulní zařízení odebrání součástí zařízení atd. 
- **Vytvoření kolem zabezpečené hardware**: Pokud [náklady na prodané zboží](https://en.wikipedia.org/wiki/Cost_of_goods_sold) povolit, vytvářet zabezpečení funkce, jako jsou šifrované a zabezpečené úložiště a spouštěcí Trusted Platform Module TPM funkce.
- **Upgrade zabezpečené**: upgradu firmwaru během života zařízení se tomu nelze vyhnout.

## <a name="iot-solution-developer"></a>Karta Vývojář v IoT řešení

Jestliže jste vývojáři IoT řešení, použijte následující doporučené postupy:

- **Sledovat zabezpečené software vývojové metodologii**: vývoj zabezpečené softwaru vyžaduje základu zdola nahoru přemýšlet o zabezpečení, od zahájení projektu úplně jeho implementaci testování a nasazení.
- **Zvolte software otevřít zdroj opatrně**: Otevřít zdroj software poskytuje možnost rychle vyvíjet řešení.
- **Integrace opatrně**: spoustu chyby zabezpečení software existují na hranici mezi knihoven a rozhraní API. 

## <a name="iot-solution-deployer"></a>IoT řešení nasazovatel

Pokud jste nasazovatel řešení IoT, postupujte podle následující doporučené postupy:

- **Nasazení hardwaru bezpečně**: nasazení IoT může vyžadovat hardwaru, který má být nasazené na nezabezpečené místech, například veřejné mezery nebo závadou národní prostředí.
- **Zabezpečit ověřovací klíče**: při nasazení každé zařízení vyžaduje ID zařízení a související ověřovací klíče generovaných cloudovou službu. Zabezpečit klávesy fyzicky i po nasazení. Libovolné hostují klávesy lze škodlivým zařízením jako stávající zařízení.

## <a name="iot-solution-operator"></a>Operátor IoT řešení

Pokud jste operátor IoT řešení, postupujte podle následující doporučené postupy:

- **Aktuálnost systémy**: zajistit, aby zařízení operačních systémů a všech ovladačů zařízení se aktualizují na nejnovější verze. 
- **Ochrana proti škodlivým aktivitám**: Pokud operační systém povolí, umístěte nejnovější funkce antivirový a proti malwaru v každém zařízení operačním systému. 
- **Často auditování**: sestavy auditování IoT infrastrukturu pro týkající se zabezpečení problémy při klíč reagovat na události zabezpečení.
- **Fyzicky chránit infrastruktury IoT**: nejhorší zabezpečení útoky IoT infrastruktury spouštěné pomocí fyzický přístup k zařízení.
- **Zamknout cloudu pověření**: cloudu přihlašovacími údaji používá ke konfiguraci a provozní nasazení IoT jsou pravděpodobně nejjednodušší způsob, jak získat přístup a ohrozit systému IoT. 
