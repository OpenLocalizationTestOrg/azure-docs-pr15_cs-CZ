<properties
    pageTitle="Azure postupů upgradu Mobile zapojení Web SDK | Microsoft Azure"
    description="Nejnovější aktualizace a postupy pro Web SDK pro zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure postupů upgradu Mobile zapojení Web SDK

Pokud už máte starší verzi Azure Mobile zapojení Web SDK integrovaný webové aplikace, budete muset po upgradu SDK zvažte následující možnosti.

Přeskočilo více verzí SDK Mobile zapojení Web může potřebnosti k provedení několika postupů během upgradu. Například pokud migrace z 1.4.0 do 1.6.0 nejdřív postupujte upgradovat z 1.4.0 1.5.0. Pak postupujte podle upgradovat z 1.5.0 1.6.0.

Bez ohledu upgradujete z verze nahradit starší verzi souboru azure-engagement.js nejnovější verzi souboru.

## <a name="upgrade-from-121-to-200"></a>Upgrade z 1.2.1 na 2.0.0

Tato část popisuje jak migrovat Mobile zapojení Web SDK integrace služby Capptain zaměstnanecké přidružení Capptain zabezpečení, které do aplikace pro zapojení Mobile Azure. Při migraci ze starší verze, obraťte se na webu Capptain nejdřív migrovat do 1.2.1 a pak použijte následující postupy.

Tato verze SDK Web zapojení Mobile, které nejsou podporovány inteligentní televizního Samsung, Opera Televizi, webOS nebo funkci Reach.

>[AZURE.IMPORTANT] Capptain a využití Mobile Azure nejsou stejné služby. Následující postup zvýrazní jenom jak migrovat aplikaci klienta. Migrace SDK Mobile zapojení Web v aplikaci se nemigrují dat ze serveru Capptain k serveru Mobile Engagement.

### <a name="javascript-files"></a>Soubory JavaScriptu

Soubor capptain-sdk.js nahraďte soubor azure-engagement.js a aktualizujte skript importy příslušným způsobem.

### <a name="remove-capptain-reach"></a>Odebrání Capptain Reach

Tato verze SDK Mobile zapojení Web nepodporuje funkci Reach. Pokud Capptain Reach integrována do aplikace, musíte ho odebrat.

Odebrat dosáhla CSS import ze stránky a odstraňte soubor související .css (capptain-reach.css, ve výchozím nastavení).

Odstranění v následujících zdrojích Reach: Zavřít obrázek (capptain-close.png, ve výchozím nastavení) a ikonu značky (capptain ikona oznámení, ve výchozím nastavení).

Odebrání uživatelské rozhraní dostanete oznámení ve aplikace. Výchozí rozložení vypadat takto:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Odebrání uživatelské rozhraní dosáhla textu a web oznámení a hlasování. Výchozí rozložení vypadat takto:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Odebrání `reach` objektu z konfigurace, pokud existuje. Vypadá takto:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Odeberte všechny ostatní Reach vlastní nastavení, například kategorie.

### <a name="remove-deprecated-apis"></a>Odebrání nepoužívaných rozhraní API

Některé rozhraní API Capptain jsou změněny v SDK Mobile zapojení Web.

Odebrání volání následující rozhraní API: `agent.connect`, `agent.disconnect`, `agent.pause`, a `agent.sendMessageToDevice`.

Odebrat všechny instance tohoto následující zpětná z konfigurace Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, a `onPushMessageReceived`.

### <a name="configuration"></a>Konfigurace

Zapojení mobilní používá ke konfiguraci SDK identifikátory, například aplikaci identifikátor připojovací řetězec.

Nahraďte ID aplikace připojovací řetězec. Globální objektu pro konfiguraci SDK se nezměnilo z `capptain` k `azureEngagement`.

Před migrací:

    window.capptain = {
      appId: ...,
      [...]
    };

Po migraci:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Připojovací řetězec pro aplikaci se zobrazí v portálu Azure.

### <a name="javascript-apis"></a>Rozhraní API JavaScript

Globální objekt JavaScript `window.capptain` přejmenování se `window.azureEngagement` , ale můžete použít `window.engagement` alias pro rozhraní API volání. Stanovit konfigurační SDK nelze použít alias.

Například `capptain.deviceId` změní `engagement.deviceId`, `capptain.agent.startActivity` změní `engagement.agent.startActivity`a tak dál.
