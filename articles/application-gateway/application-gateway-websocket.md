<properties
   pageTitle="Podpora brány WebSocket aplikací | Microsoft Azure"
   description="Tato stránka obsahuje přehled podpory WebSocket brány aplikace."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Podpora brány WebSocket aplikací

Brána pro aplikace podporuje nativní WebSocket přes všech velikostí brány. Neexistuje žádné uživatel konfigurovat nastavení selektivně povolit nebo zakázat WebSocket podpory. Můžete nadále používat standardní HTTPListener na porty 80 a 443 pro příjem WebSocket vysílání. Přenosy WebSocket je pak směrovány WebSocket povolené back-end serveru pomocí fondu odpovídající back-end podle pravidel brány aplikací. Protokol WebSocket standardizovány v [RFC6455](https://tools.ietf.org/html/rfc6455) umožňuje plně duplexní komunikaci mezi serveru a klientských přes dlouhodobé TCP připojení. Tato funkce umožňuje více komunikativní komunikace mezi webový server a klienta, který může být obousměrný bez nutnosti hlasování jako povinný v založené na HTTP implementace.  WebSocket máte nízké nároky na rozdíl od HTTP a můžete znovu použít stejné připojení pomocí protokolu TCP pro více žádost/odpovědi výsledkem efektivnější využití prostředků. Protokoly WebSocket slouží k práci během tradiční HTTP porty 80 a 443.

Back-end serveru musí odpovídat sond brány aplikace, které jsou popsány v části [stav zkušební přehled](application-gateway-probe-overview.md) . Aplikace brány stavu sond protokolu HTTP/HTTPS pouze, to znamená, že každé back-end serveru musí odpovědět na HTTP sond brány aplikace, kterou chcete WebSocket provoz směrovat na serveru.

## <a name="listener-configuration-element"></a>Konfigurace prvek posluchače

Existující HTTPListener mohou sloužit k podpoře WebSocket. Toto je fragment prvku HttpListeners ze souboru ukázkové šablony. Měli byste HTTP a HTTPS posluchače podporují WebSocket a zabezpečit WebSocket přenosy. Podobně můžete používat [portál](application-gateway-create-gateway-portal.md) nebo [prostředí PowerShell](application-gateway-create-gateway-arm.md) vytvořit brány aplikační s posluchačů na porty 80 a 443 pro podporu WebSocket komunikace.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Konfigurace pravidla BackendAddressPool BackendHttpSetting a směrování

BackendAddressPool bude použito k definování fond back-end se servery WebSocket povolené. BackendHttpSetting by měl být definovány back-end porty 80 a 443 pouze. Vlastnosti na základě souborů cookie spřažení a requestTimeouts nejsou důležité pro přenos WebSocket. Je stejně jako v pravidla směrování vyžaduje. Směrování pravidlo: základní"nadále sloužit k nevážou odpovídající posluchače odpovídající fondu adres back-end. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket povolené back-end

Vaše back-end musí mít na webový server protokolu HTTP/HTTPS spuštěna nakonfigurované port (obvykle 80 a 443) pro WebSocket pracovat. Tento požadavek totiž WebSocket protokol vyžaduje počáteční signalizace být HTTP upgradu WebSocket protokolu jako záhlaví pole.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Jiný důvod, proč se to je že této aplikace brány back-end stavu zkušební podporuje pouze protokoly protokolu HTTP/HTTPS. Pokud server back-end nebudete reagovat protokolu HTTP/HTTPS sond, byste měli vzít z fondu back-end a bez žádosti o včetně požadavků WebSocket, by dosáhla tento back-end.

## <a name="next-steps"></a>Další kroky

Po získání informací o WebSocket podpory, přejděte na [Vytvoření brány aplikační](application-gateway-create-gateway.md) začít pracovat s webovou aplikací WebSocket povolené.
