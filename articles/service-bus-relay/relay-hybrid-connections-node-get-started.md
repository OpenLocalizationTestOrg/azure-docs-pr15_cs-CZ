<properties
    pageTitle="Začínáme s připojením k hybridní Relay | Microsoft Azure"
    description="Jak psát aplikace konzoly uzel pro hybridní připojení"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Začínáme s Relay hybridní připojení

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Co bude provést

Protože hybridní připojení vyžaduje klienta a serveru součásti, vytvoříme dvě aplikace konzoly v tomto kurzu. Tady je postup:

1. Vytvoření Relay názvů pomocí portálu Azure.

2. Vytvořte si připojení hybridní pomocí portálu Azure.

3. Napište serveru konzoly aplikaci přijímat zprávy.

4. Napište klient aplikace konzoly k odesílání zpráv.

## <a name="prerequisites"></a>Zjistit předpoklady pro

1. [Node.js](https://nodejs.org/en/) (v tomto příkladu uzel 7.0).

2. Předplatné Azure.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Vytvořte obor názvů pomocí portálu Azure

Pokud už máte obor Relay vytvořili přeskočíte do části [vytvoření připojení k hybridní na portálu Azure](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. připojení hybridní pomocí portálu Azure

Pokud už máte hybridní připojení vytvořili, přeskočíte na v části [Vytvoření serverové aplikace](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Vytvoření serverové aplikace (posluchače)

Poslech a přijímat zprávy z přenos, jsme napsali aplikace konzoly Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. vytvoření klientské aplikaci (odesílatele)

Zaslání zprávy předat jsme napsali aplikace konzoly Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. spuštění aplikace

1. Spusťte aplikaci serveru.

2. Spusťte aplikaci klienta a zadejte nějaký text.

3. Zajistit, aby aplikace konzoly serveru uloží text, který byl zadán v klientské aplikaci.

![spuštění aplikace](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gratulujeme, vytvoření aplikace hybridní připojení začátku do konce.

## <a name="next-steps"></a>Další kroky:

- [Předávací časté otázky](relay-faq.md)
- [Vytvořit obor názvů](relay-create-namespace-portal.md)
- [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Začínáme s uzel](relay-hybrid-connections-node-get-started.md)