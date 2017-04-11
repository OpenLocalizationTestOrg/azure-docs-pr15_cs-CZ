<properties
    pageTitle="Vlastní nastavení pro aplikaci služby prostředí"
    description="Vlastní nastavení pro aplikaci služby prostředí"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Vlastní nastavení pro aplikaci služby prostředí

## <a name="overview"></a>Základní informace ##
Protože aplikace služby prostředí jsou izolace jednoho zákazníkovi, existují některé nastavení, která se dají použít pouze pro aplikaci služby prostředí. Tento článek dokumenty různých konkrétní kustomizace, které jsou dostupné pro aplikaci služby prostředí.

Pokud nemáte prostředí aplikace služby, najdete v článku [jak vytvářet prostředí aplikace služby](app-service-web-how-to-create-an-app-service-environment.md).

Pomocí matice v novém atributu **clusterSettings** mohou být uloženy vlastní nastavení prostředí aplikace služeb. Tenhle atribut nachází ve slovníku "Vlastnosti" *hostingEnvironments* správce prostředků Azure entity.

Následující zkrácený správce prostředků šabloně fragment kódu ukazuje atribut **clusterSettings** :


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Atribut **clusterSettings** mohou být součástí šablony správce prostředků aktualizovat prostředí aplikace služeb.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Použití Průzkumníka Azure zdroje aktualizovat prostředí aplikace služby
Můžete taky můžete aktualizovat prostředí služeb aplikací pomocí [Průzkumník Windows Azure zdroje](https://resources.azure.com).  

1. V Průzkumníku zdroje, přejděte na stránku na uzel prostředí aplikace služeb (**předplatná** > **resourceGroups** > **poskytovatelů** > **Micrososft.Web** > **hostingEnvironments**). Klikněte na konkrétní prostředí služby aplikace, kterou chcete aktualizovat.

2. V pravém podokně klikněte v horním rohu nástrojů umožňuje interaktivní úpravy v Průzkumníku zdroje **Pro čtení i zápis** .  

3. Klepněte na modrou **Upravit** tlačítko Chcete-li upravit šablonu správce prostředků.

4. Přejděte do dolní části pravého podokna. Atribut **clusterSettings** je dole v místě, kde můžete zadat nebo aktualizovat jeho hodnotu.

5. Zadejte (nebo zkopírujte a vložte) pole Konfigurace hodnoty, které chcete v atributu **clusterSettings** .  

6. Klikněte zelenou **umístění** tlačítka, která obsahuje umístěné nahoře v podokně vpravo potvrďte změny prostředí aplikace služeb.

Však odešlete změnu trvá zhruba 30 minut vynásobené počet front-end v prostředí služby aplikace aby se změna projevila.
Například pokud prostředí aplikace služby obsahuje čtyři front-end, bude trvat zhruba dvou dobu dokončení konfigurace aktualizace. Při změně konfigurace je právě chránění, žádné změny měřítka operace nebo operace změny konfigurace může proběhnout v prostředí aplikace služeb.

## <a name="disable-tls-10"></a>Zakázání TLS 1.0 ##
Opakované otázku od zákazníků, zejména zákazníkům, kteří se zabývají dodržování předpisů PCI audity, které explicitně zakázání TLS 1.0 jejich aplikací.

TLS 1.0 můžete zakázat pomocí následující položky **clusterSettings** :

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Změnit pořadí sadu šifrování TLS ##
Další otázku od zákazníků je-li můžete upravovat seznam šifry vyjednávání serverem a toho můžete dosáhnout změnou **clusterSettings** , jak je ukázáno v následujícím příkladu. Seznam šifrování sad dostupné můžete načtená z kontingenčního seznamu [tohoto článku MSDN o] (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Pokud nesprávnými hodnotami jsou nastaveny sadu šifrování, která SChannel nerozumí, může všechny TLS communication server přestane fungovat. V takovém případě se potřebujete odebrat položce *FrontEndSSLCipherSuiteOrder* **clusterSettings** a odeslání aktualizované šablony správce prostředků se vrátit zpátky na výchozí nastavení sady šifrování.  Použijte tuto funkci s upozorněním.

## <a name="get-started"></a>Začínáme
Správce prostředků rychlý úvod Azure šablonou webu obsahuje šablony s základní definici pro [vytváření prostředí aplikace služby](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
