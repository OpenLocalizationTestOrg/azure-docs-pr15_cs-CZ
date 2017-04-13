<properties
   pageTitle="Vytvoření první aplikace služby struktury na Linux pomocí C# | Microsoft Azure"
   description="Vytvoření a nasazení aplikace služby struktury pomocí C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Vytvoření první aplikace struktury služby Azure

> [AZURE.SELECTOR]
- [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Služba struktury obsahuje SDK pro vytváření služby základní .NET a Java Linux. V tomto kurzu podíváme na to, jak vytvořit aplikaci pro Linux a sestavíte služba pomocí C# (.NET Core).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete, ujistěte se, že máte [nastavení vývojové prostředí Linux](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete [nastavit prostředí Linux jedno pole do virtuálního počítače pomocí Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Vytvoření aplikace

Aplikace služby struktury může obsahovat jedné nebo víc služeb, každá s konkrétní rolí v přináší funkce aplikací. Služby struktury Linux obsahuje [Yeoman](http://yeoman.io/) generátor, který umožňuje snadno vytvářet první služby a přidat další později. Vytvoření aplikace jednoho službou použijeme Yeoman.

1. Do terminálu zadejte tento příkaz začít vytvářet generování uživatelského rozhraní:`yo azuresfcsharp`

2. Zadejte název aplikace.

3. Zvolte typ první služby a nazvěte ji. Pro účely tohoto kurzu budeme zvolte spolehlivé službu Actor.

  ![Generátor Yeoman struktury služby pro C#][sf-yeoman]

>[AZURE.NOTE] Další informace o těchto možnostech naleznete v tématu [Přehled modelu programování struktury služby](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Vytvoření aplikace

Šablony služby struktury Yeoman patří vytvořit skript, který umožňuje vytvářet aplikace z terminál (po přechodu na složky aplikace).

  ```bash
 cd myapp 
 ./build.sh 
  ```

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

## <a name="start-the-test-client-and-perform-a-failover"></a>Spusťte test klienta a dělat selhání

Objekt actor projekty není nijak na vlastní. Potřebují jiné služby nebo klienta jim můžete poslat zprávy. Šablona actor obsahuje jednoduchá skript, který slouží k interakci se službou actor.

1. Spusťte skript pomocí nástroje pro sledování zobrazíte výstup službu actor.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. V Průzkumníku služby struktury vyhledejte uzel hostingu primární otevřené pro službu actor. V následující obrázek je uzel 3.

    ![Hledání primární otevřené v Průzkumníkovi struktury služby][sfx-primary]

3. Klikněte na uzel, který jste našli v předchozím kroku a potom v nabídce Akce vyberte **deaktivovat (restart)** . Tato akce restartuje jeden z pěti uzlů v místním clusteru vynucení přepojení sekundární otevřené na jiný uzel. Jak provedení této akce věnujte pozornost výstup z testovací klient a Všimněte si, že čítači nadále zvýšit bez ohledu na záložní.


## <a name="next-steps"></a>Další kroky

- [Další informace o spolehlivé účastníky](service-fabric-reliable-actors-introduction.md)
- [Interakce s služby struktury clusterů pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
