<properties
   pageTitle="Nastavení prostředí vývoj na Linux | Microsoft Azure"
   description="Nainstalujte modul runtime a SDK a vytvořte clusteru místní vývoj na Linux. Po dokončení tohoto nastavení, pak budete připraveni k vytvoření aplikace."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Příprava vývojové prostředí na Linux


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Nasazení a spouštět [struktury služby Azure aplikace](service-fabric-application-model.md) v počítači Linux vývoj, nainstalujte modul runtime a běžných SDK. Můžete taky nainstalovat volitelné SDK Java a .NET Core.

## <a name="prerequisites"></a>Zjistit předpoklady pro
### <a name="supported-operating-system-versions"></a>Podporované operační systém verze
Pro vývoj jsou podporovány následující operační systém verze:

- Systémem Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Aktualizace výstižný zdrojů

K instalaci SDK a balíčku přidružené runtime prostřednictvím výstižný get, musíte nejdřív aktualizujete výstižný zdrojů.

1. Otevřete terminál.
2. Přidání repo struktury služby do seznamu zdrojů.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Přidejte nový klíč GPG výstižný klíčů.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Aktualizujte svoje vlastní seznamy balíčku založené na nově přidaný úložištích.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Instalace a nastavení v SDK

Až se aktualizují svých pramenů, že nainstalujete SDK.

1. Nainstalujte balíček SDK struktury služby. Zobrazí se výzva k potvrzení instalaci a přijměte podmínky licenční smlouvy.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Spusťte instalační skript SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Nastavení rozhraní příkazového řádku Azure různé platformy

[Azure rozhraní příkazového řádku různé platformy] [ azure-xplat-cli-github] obsahuje příkazy pro komunikaci s entity služby struktury, včetně clusterů a aplikací. Je založená na Node.js Ano, [Ujistěte se, že máte nainstalovanou uzel] [ install-node] před pokračováním níže uvedených pokynů.

1. Klonovat repo github do počítače vývoj.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Přepnutí do klonovaný repo a nainstalujte závislostí rozhraní příkazového řádku pomocí Správce balíčků uzel (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Vytvoření symlink ze složky Koš/azure klonovaný repo do /usr/bin/azure tak, aby se přidají do vašeho cesty a příkazy jsou k dispozici z libovolného adresáře.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Nakonec povolte automatické dokončování služby struktury příkazy.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Nastavení místní obrázku

Pokud všechno, co byl úspěšně nainstalovaný, je třeba moct uživatelé zahajovat místní obrázku.

1. Spusťte instalační skript obrázku.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Otevřete webový prohlížeč a přejděte na http://localhost:19080/Průzkumníka. Pokud clusteru začala, byste měli vidět řídicího panelu služeb struktury Průzkumníka.

    ![Služba struktury Průzkumníka na Linux][sfx-linux]

V tomto okamžiku se nasadit předdefinovaných balíčků aplikací služby struktury nebo nové na základě hosta kontejnerů nebo spustitelných Host. Pokud chcete vytvořit nové služby pomocí jazyka Java nebo .NET Core SDK, postupujte následujícím způsobem volitelné nastavení.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Instalace modulu plug-in Java SDK a zatmění Neónová (volitelné)

Java SDK poskytuje knihovny a šablony potřebné k vytvoření struktury služby služby pomocí jazyka Java.

1. Nainstalujte balíček Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Spusťte instalační skript SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Nainstalujte modul plug-in zatmění pro službu struktury z v integrovaném vývojovém zatmění Neónová prostředí.

1. V zatmění Ujistěte se, že máte Buildship verze 1.0.17 nebo novější. Kontrola verze součásti výběrem **Nápověda > podrobné informace o instalaci**. Je možné aktualizovat Buildship postupujte podle pokynů [v tomto poli][buildship-update].

2. K instalaci modulu plug-in služby struktury, zvolte **Nápověda > Instalace nových softwaru...**

3. "Funguje s" do textového pole, zadejte: http://dl.windowsazure.com/eclipse/servicefabric

4. Klikněte na Přidat.

    ![Modul plug-in zatmění][sf-eclipse-plugin]

5. Zvolte služby struktury modul plug-in a klikněte na další.

6. Proveďte instalaci a přijměte podmínky licenční smlouvy s koncovým uživatelem.

## <a name="install-the-net-core-sdk-optional"></a>Instalace SDK Core .NET (volitelné)

Základní SDK .NET poskytuje knihovny a šablony potřebné k vytvoření struktury služby služby pomocí základní .NET různé platformy.

1. Nainstalujte balíček .NET Core SDK.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Spusťte instalační skript SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Další kroky

- [Vytvoření první Java aplikace na Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Příprava vývojové prostředí na OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
