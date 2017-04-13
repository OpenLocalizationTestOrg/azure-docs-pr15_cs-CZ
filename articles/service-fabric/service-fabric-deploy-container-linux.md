<properties
   pageTitle="Služby struktury a nasazení kontejnery v Linux | Microsoft Azure"
   description="Služba struktury a používání Docker kontejnery nasadit microservice aplikace. Tento článek popisuje funkce, které služby struktury poskytuje kontejnerů a nasazení obrázek kontejneru Docker do clusteru"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Nasazení kontejneru Docker struktury služby

> [AZURE.SELECTOR]
- [Nasazení kontejnerem systému Windows](service-fabric-deploy-container.md)
- [Nasazení Docker kontejneru](service-fabric-deploy-container-linux.md)

Tento článek vás provede vytváření kontejnerizovaná služby v kontejnerech Docker na Linux.

Služba struktury má několik kontejneru funkcí, které vám pomohou s vytváření aplikací, které se skládají z microservices, které jsou kontejnerizovaná. Nazývají kontejnerizovaná služby.

Možnosti zahrnují;

- Kontejner obrázek nasazení a aktivace
- Zásady správného řízení zdrojů
- Ověřování úložiště
- Kontejner port mapování portů Host (hostitel)
- Kontejner kontejneru zjišťování a hodnocení komunikace
- Možnost konfigurace a nastavení proměnné


## <a name="packaging-a-docker-container-with-yeoman"></a>Balení kontejneru docker s yeoman
Pokud balení kontejneru na Linux, můžete buď použít yeoman šablony a [Ruční vytvoření balíčku aplikace](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Aplikace služby struktury může obsahovat jedno nebo více kontejnerů, každá s konkrétní rolí v přináší funkce aplikací. Služby struktury Linux obsahuje [Yeoman](http://yeoman.io/) generátor, který umožňuje snadno vytvářet aplikace a přidání obrázku na kontejner. Vytvoření nové aplikace s kontejnerem jednoho Docker s názvem *SimpleContainerApp*použijeme Yeoman. Můžete přidat že další služby později úpravou generované manifest soubory.

## <a name="create-the-application"></a>Vytvoření aplikace

1. Do terminálu zadejte **Jo azuresfguest**.

2. Rámci vyberte **kontejner** .

3. Název aplikace například SimpleContainerApp

4. Zadání adresy URL pro kontejner obrázek z DockerHub repo. To je představována [repo] / [název obrázku]

![Generátor Yeoman struktury služby pro kontejnery][sf-yeoman]

## <a name="deploy-the-application"></a>Nasazení aplikace

Vytvořenou aplikaci, můžete ho nasadit do místní clusteru pomocí rozhraní příkazového řádku Azure.

1. Připojení k místní služba struktury obrázku.

    ```bash
    azure servicefabric cluster connect
    ```

2. Pomocí skriptu nainstalovat uvedenou v šabloně ke zkopírování balíčku aplikace clusteru obrázek Store a zaregistrovat typ aplikace, vytvořit instanci aplikace.

    ```bash
    ./install.sh
    ```

3. Otevřete prohlížeč a přejděte na služby struktury Explorer na http://localhost:19080/Explorer (nahradit localhost soukromé IP OM používáte-li Vagrant v systému Mac OS X).

4. Rozbalte položku aplikace a Všimněte si, že je teď položky pro váš typ aplikace a jiné pro první instance tohoto typu.

5. Pomocí skriptu odinstalovat uvedenou v šabloně odstranit instance aplikace a zrušení registrace typ aplikace.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Další kroky

- [Přehled služeb a kontejnery](service-fabric-containers-overview.md)
- [Interakce s služby struktury clusterů pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

