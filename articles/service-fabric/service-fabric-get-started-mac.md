<properties
   pageTitle="Nastavení prostředí vývoj v systému Mac OS X | Microsoft Azure"
   description="Nainstalujte modul runtime, SDK a nástroje a vytvoření místní vývoj clusteru. Po dokončení tohoto nastavení se bude připravená k vytváření aplikací v systému Mac OS X."
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
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Nastavení prostředí vývoj v systému Mac OS X

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Je možné vytvářet služby struktury spouštět aplikace na Linux clusterů pomocí systému Mac OS X. Tento článek obsahuje informace o nastavení počítače Mac pro vývoj.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Služba struktury nespustí nativně na OS X. Místní služba struktury clusteru spustíte nabízíme předkonfigurovaná virtuální počítač se systémem Ubuntu pomocí Vagrant a VirtualBox. Než začnete, musíte:

- [Vagrant (v1.8.4 nebo novější)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Vytvoření místní OM

Pokud chcete vytvořit místní OM obsahující clusteru služby struktury uzel 5, postupujte takto:

1. Klonovat Vagrantfile repo

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Přejděte na místní klonovat repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Volitelné) Změnit výchozí nastavení OM

    Ve výchozím nastavení místní OM nakonfigurovaný následujícím způsobem:

    - 3 GB paměti přidělit
    - Soukromé hostitelské sítě nakonfigurované IP 192.168.50.50 povolení průchod provoz hostiteli Mac

    Můžete změnit některou z těchto nastavení nebo můžete přidat další konfiguraci OM v Vagrantfile. Najdete v [dokumentaci Vagrant](http://www.vagrantup.com/docs) úplný seznam možností konfigurace.

4. Vytvoření OM

    ```bash
    vagrant up
    ```

    Tento krok je možné stáhnout předkonfigurované obrázek OM spouštění ji místně a pak nastavení místní sítě služby cluster v nich. Můžete očekávat to trvat několik minut. Pokud úspěšném dokončení instalace uvidíte zprávu do výstupu označující, že se spouští clusteru.

    ![Clusteru nastavení spuštění po zřízení OM][cluster-setup-script]

5. Otestujte, clusteru je nastavené správně tak, že přejdete do služby struktury Průzkumník na http://192.168.50.50:19080/Explorer (za předpokladu, že je k dispozici IP výchozí privátní sítě).

    ![Služba struktury Explorer zobrazit hostiteli Mac][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Instalace modulu plug-in služby struktury zatmění Neónová (volitelné)

Služba struktury poskytuje modulu plug-in integrovaném vývojovém prostředí zatmění Neónová, které můžete zjednodušit proces vytvoření a nasazení služeb Java.

1. V zatmění Ujistěte se, že máte Buildship verze 1.0.17 nebo novější. Kontrola verze součásti výběrem **Nápověda > podrobné informace o instalaci**. Je možné aktualizovat Buildship postupujte podle pokynů [v tomto poli][buildship-update].

2. K instalaci modulu plug-in služby struktury, zvolte **Nápověda > Instalace nových softwaru...**

3. "Funguje s" do textového pole, zadejte: http://dl.windowsazure.com/eclipse/servicefabric.

4. Klikněte na Přidat.

    ![Modul plug-in Neónová zatmění pro službu struktury][sf-eclipse-plugin-install]

5. Zvolte služby struktury modul plug-in a klikněte na další.

6. Proveďte instalaci a přijměte podmínky licenční smlouvy s koncovým uživatelem.

## <a name="next-steps"></a>Další kroky

- [Vytvoření první aplikace služby struktury Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Vytvoření struktury služby obrázku na portálu Azure](service-fabric-cluster-creation-via-portal.md)
- [Vytvoření struktury služby clusteru pomocí Správce prostředků Azure](service-fabric-cluster-creation-via-arm.md)
- [Principy modelu služby struktury aplikace](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
