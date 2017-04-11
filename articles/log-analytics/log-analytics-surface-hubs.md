<properties
    pageTitle="Sledování povrchový rozbočovače s protokolu analýzy | Microsoft Azure"
    description="Sledování stavu povrch rozbočovače a pochopit, jak se používají pomocí povrch centrální řešení."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Sledování povrchový rozbočovače s protokolu analýzy

Tento článek popisuje, jak můžete řešení povrch centrální v protokolu analýzy sledování Microsoft Surface centrální zařízeních s Microsoft operace správy sady (OMS). Technologie pro analýzu protokolu vám pomáhá sledovat stav povrch rozbočovače stejně jako pochopit, jak se používají.

Každý povrch centrální má Microsoft Agent sledování nainstalovaný. Jeho pomocí agenta, můžete poslat data z rozbočovače povrch OMS. Protokoly z rozbočovače povrch a jsou se budou číst a pak se odesílají službě OMS. Problémy, jako je offline, servery kalendář nesynchronizují, nebo pokud nemůžete se přihlásit do zařízení účet Skypu jsou zobrazeny v OMS v řídicím panelu plochy centrální. Pomocí data v řídicím panelu poznáte zařízení, která se nepoužívá nebo, která jsou jiné problémy a potenciálně použít opravy zjištěny problémy.


## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení

Instalace a konfigurace řešení použijte následující informace. Abyste mohli spravovat rozbočovače povrch z Microsoft operace správy sady (OMS), musíte mít takto:

- Platné předplatné [OMS](http://www.microsoft.com/oms).
- Úroveň [OMS předplatného](https://azure.microsoft.com/pricing/details/log-analytics/) , která bude podporovat počtu zařízení, které chcete sledovat. Ceny OMS se liší podle toho, kolik zařízení jsou zápisu a množství zpracovávaných dat ho procesů. Je vhodné toto vzít v úvahu při plánování zavedení povrch centrální.

Potom můžete buď přidá předplatné OMS do stávajícího předplatného Microsoft Azure nebo vytvoření nového pracovního prostoru přímo prostřednictvím portálu OMS. Podrobné pokyny k oběma způsoby je [začít s protokolu analýzy](log-analytics-get-started.md). Po nastavení předplatného OMS si zapsat zařízeních Surface centrální dvěma způsoby:

- Automaticky pomocí InTune
- Ručně pomocí **Nastavení** na povrch Centrum zařízení.

## <a name="set-up-monitoring"></a>Nastavení sledování

Můžete sledovat stav a aktivity rozbočovače povrch pomocí protokolu analýzy v OMS. Centrální povrch v OMS můžete zápis pomocí InTune nebo místně pomocí **Nastavení** v centru plocha.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Připojení povrchový rozbočovače OMS prostřednictvím InTune

Musíte mít ID pracovní prostor a pracovní prostor klíč pro OMS pracovní prostor, který bude spravovat rozbočovače plocha. Můžete získat z portálu OMS.

InTune je produktu společnosti Microsoft, který umožňuje centrální správu OMS nastavení, které se použijí na jednu nebo víc zařízení. Tyto kroky pro nastavení vašeho zařízení prostřednictvím InTune:

1. Přihlaste se k InTune.
2. Přejděte na **Nastavení** > **připojení zdroje**.
3. Vytvořit nebo upravit zásadu založené na šabloně povrch centrální.
4. Přejděte do části OMS (Azure provozní přehledy) zásady a přidejte *ID pracovní prostor* a *Pracovní prostor klíč* zásad.
5. Uložení zásadu.
6. Zásady přidružit příslušné skupiny zařízení.

  ![Zásady InTune](./media/log-analytics-surface-hubs/intune.png)

Nastavení OMS InTune potom synchronizuje se zařízení v cílové skupině, zápis v pracovním prostoru OMS.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Připojení k OMS pomocí aplikace nastavení plochy rozbočovače

Musíte mít ID pracovní prostor a pracovní prostor klíč pro OMS pracovní prostor, který bude spravovat rozbočovače plocha. Můžete získat z portálu OMS.

Pokud nepoužíváte InTune ke správě prostředí, můžete zápisu zařízení ručně pomocí **Nastavení** na každého rozbočovače povrchový:

1. Z rozbočovače povrch otevřete **Nastavení**.
2. Zadejte přihlašovací údaje k Správce zařízení po zobrazení výzvy.
3. Klikněte na **Toto zařízení**a v části **Sledování**klikněte na **Konfigurovat nastavení OMS**.
4. Zaškrtněte políčko **Povolit sledování**.
6. V dialogovém okně Nastavení OMS zadejte **Pracovního prostoru ID** a zadejte **Klíč pracovního prostoru**.  
  ![nastavení](./media/log-analytics-surface-hubs/settings.png)
7. Klikněte na **OK** dokončete konfiguraci.

Zobrazí se potvrzení o tom, zda byla úspěšně použita konfigurace OMS zařízení. Pokud byl, zobrazí se zpráva oznamující, že agent úspěšném připojení ke službě OMS. Zařízení potom začne odesílání dat OMS, kde můžete zobrazit a pracovat.

## <a name="monitor-surface-hubs"></a>Vystavení rozbočovače monitoru

Sledování povrch rozbočovače pomocí OMS vypadá podobně sledování jiných zapsaný zařízení.

1. Přihlaste se k portálu OMS.
2. Přejděte do řídicího panelu plochy centrální řešení pack.
3. Zobrazí se vaše zařízení stavu.

  ![Vystavení centrální řídicího panelu](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Můžete vytvořit [upozornění](log-analytics-alerts.md) založené na existující nebo vlastní protokolu vyhledávání. Použití data, která OMS shromažďuje z rozbočovače povrchový, můžete hledat problémy a upozornění na podmínky, které definujete zařízení.


## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobná data povrch centrální.
- Vytvořte [upozornění](log-analytics-alerts.md) na oznámí, že objeví potíže s rozbočovače plocha.
