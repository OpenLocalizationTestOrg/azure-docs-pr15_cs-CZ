<properties
   pageTitle="Přehled zabezpečení Internet věci | Microsoft Azure"
   description=" Azure internetových služeb věcí (IoT) nabízí řadu možností. Tento článek pomůže pochopit, jak zajistit IoT řešení v Azure. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Přehled zabezpečení Internet věci

Azure internetových služeb věcí (IoT) nabízí řadu možností. Tyto služby testu enterprise umožňují:

- Shromáždit data od zařízení
- Analýza dat proudů v pohybu
- Ukládání a velkých datových sad dotazu
- Vizualizace dat v reálném čase a historie
- Integrace se systémy office zpět

Předvádění tyto možnosti, sadu IoT Azure balíčků společně více služby Azure pomocí vlastní rozšíření jako předkonfigurované řešení. Tato předkonfigurované řešení jsou základní implementace vzorů běžné IoT řešení, které pomáhají zkrátit, kterými doručovat IoT řešení. IoT software development Kit můžete přizpůsobit a rozšíření tato řešení vlastní požadavkům. Taky můžete tato řešení jako příklady nebo šablony při vytváření nových IoT řešení.

Sadu Azure IoT je výkonné řešení pro vaše potřeby IoT. Je ale upmost důležitost, že jsou určeny IoT řešení se zabezpečením nezapomeňte od začátku. Vzhledem k velkému počtu zařízení IoT všechny incidentu zabezpečení můžete rychle stát se jejím rozšířených akcí v významné důsledky.

Chcete-li vám pomůže pochopit, jak zajistit IoT řešení, máme následující informace.

## <a name="security-architecture"></a>Architektura zabezpečení

Při návrhu systému, je důležité pochopit potenciální hrozeb tohoto systému a podle toho, zadejte příslušné ochranu jako navržený a navržen systému. Je velmi důležitým navrhnout produkt od začátku s ohledem na zabezpečení, protože vysvětlení, jak se zlými úmysly pak můžou k narušení systém pomáhá Zkontrolujte odpovídající mitigations jsou na místě od začátku.

O Architektura zabezpečení IoT se dozvíte v tématu [Internet Architektura zabezpečení věci](../iot-suite/iot-security-architecture.md).

Tento článek popisuje v těchto tématech:

- [Zabezpečení začíná hrozbou, že modelem](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [Zabezpečení v IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Riziko modelování architektura Azure IoT odkaz](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Zabezpečení na základě nahoru

IoT představuje jedinečnou výzvy zabezpečení, ochrana osobních údajů a dodržování předpisů společnostem Celosvětová dostupnost. Na rozdíl od tradiční počítačové technologie kde těchto problémů základem software a jak je implementovaná IoT se týká, co se stane, když internetovým a fyzické světě konvergovat. Ochrana IoT řešení vyžaduje zajištění zabezpečené zřizování zařízeních a zabezpečené připojení mezi tato zařízení a cloudu a ochrana zabezpečené dat v cloudu během zpracování a úložiště. Práce proti tyto funkce, jsou však zařízení s omezenými zdroji, zeměpisnou distribuci nasazení a velkého počtu zařízení v rámci řešení.

Přečtěte si postup v případě zabezpečení v těchto oblastech o [zabezpečení Internet věci z základu nahoru](../iot-suite/securing-iot-ground-up.md).

Tento článek popisuje v těchto tématech:

- [Zabezpečené infrastruktury z důvodu nahoru](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure – zabezpečené infrastruktury IoT ve vašem podniku](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Doporučené postupy

Zabezpečení infrastrukturu IoT vyžaduje náročné strategie zabezpečení na hloubkové ose. Počínaje zabezpečení dat v cloudu, k ochraně integrity dat na cestě veřejné Internetu a poskytuje možnost bezpečně zřízení zařízení každé vrstvy vytvoří větší zabezpečení assurance v celé infrastruktury.

Se dozvíte o zabezpečení Internet věci osvědčené postupy v tématu [Doporučené postupy pro Internet věci zabezpečení](../iot-suite/iot-security-best-practices.md).

Tento článek popisuje v těchto tématech:

- [Výrobce hardwaru IoT/integrátor](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [Karta Vývojář v IoT řešení](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [IoT řešení nasazovatel](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [Operátor IoT řešení](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
