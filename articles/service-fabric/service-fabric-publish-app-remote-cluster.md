<properties
    pageTitle="Publikování aplikace do vzdálené obrázku s Visual Studio | Microsoft Azure"
    description="Zjistěte, jak pomocí aplikace Visual Studio publikování aplikace clusteru struktury vzdálené služby."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Publikování aplikace vzdálené clusteru pomocí aplikace Visual Studio

> [AZURE.SELECTOR]
- [Prostředí PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Struktury služby Azure rozšíření Visual Studia poskytují snadný opakující a scriptable způsob, jak publikování aplikace služby struktury clusteru.

## <a name="the-artifacts-required-for-publishing"></a>Artefakty potřebných pro publikování

### <a name="deploy-fabricapplicationps1"></a>Nasazení FabricApplication.ps1

Toto je skript Powershellu, který používá cesta profilu publikovat jako parametr pro publikování aplikace služby struktury. Od tohoto skriptu je součástí aplikace, měli byste být Vítá vás podle potřeby aplikace upravit.

### <a name="publish-profiles"></a>Publikování profilů

Složky v projektu aplikace služby struktury s názvem **PublishProfiles** obsahuje soubory XML, které obsahují základní informace pro publikování aplikace, jako například:

- Parametry připojení služby struktury obrázku
- Cesta k souboru parametr aplikace
- Nastavení upgradu

Ve výchozím nastavení aplikace bude obsahovat dva publikovat profily: Local.xml a Cloud.xml. Můžete přidat další profily zkopírováním a vložením některou z výchozích souborů.

### <a name="application-parameter-files"></a>Soubory aplikace parametrů

Složky v projektu aplikace služby struktury s názvem **ApplicationParameters** obsahuje soubory XML pro hodnoty zadané uživatelem aplikace seznamu parametrů. Seznamu soubory aplikace můžete s parametry, aby mohli používat různé hodnoty pro nastavení nasazení. Další informace o parametrizaci aplikace najdete v tématu [Správa více prostředí v struktury služby](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Objekt actor státní měli byste vytvořit project nejprve před pokusíte upravit tento soubor v editoru nebo prostřednictvím dialogu publikovat. Důvodem je součástí seznamu souborů vygeneruje během sestavení.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Publikování aplikace pomocí dialogu Publikovat struktury aplikace služby

Podle těchto kroků ukazují, jak publikovat aplikace pomocí dialogu **Publikování aplikace služby struktury** poskytovanou nástroje struktury služby Visual Studio.

1. V místní nabídce aplikace služby struktury projektu vyberte **publikovat...** Pokud chcete zobrazit dialogové okno **Publikovat struktury aplikace služby** .

    ![** Publikovat služby struktury aplikace ** dialogové okno][0]

    Soubor vybraný na **cílový profil** rozevíracího seznamu je, kde jsou uloženy všechny možnosti kromě **Manifest verze**. Můžete znovu použít existující profil nebo vytvořte nový účet výběrem **< spravovat profily... >** v poli **cílový profil** rozevírací seznam. Zvolte profil publikovat obsah zobrazí v odpovídající pole dialogového okna. Uložte provedené změny kdykoli, zvolte odkaz **Uložit profil** .    

2. V části **připojení koncového bodu** určete publikování koncový bod místní nebo vzdálené služby struktury clusteru. Pokud chcete přidat nebo změnit koncový bod připojení, klikněte na rozevírací seznam **Připojení koncového bodu** . V seznamu jsou uvedeny dostupné clusteru struktury služby připojení koncové body ke kterým je možné publikovat založené na Azure předplatného. Všimněte si, že pokud nejste již přihlášeni do Visual Studiu, zobrazí se výzva k tomu nevyzve.

    Použijte dialogové okno Výběr obrázku a zvolte ze sady dostupné předplatná a clusterů.

    ![** Vyberte služby struktury clusteru ** dialogové okno][1]

    >[AZURE.NOTE] Pokud chcete publikovat do libovolného koncového bodu (například stran obrázku), najdete v části **publikování koncový bod libovolného obrázku** dole.

    Po výběru koncový bod Visual Studio ověří spojení vybraného obrázku struktury služby. Není-li clusteru zabezpečené, Visual Studio můžete se připojit k němu okamžitě. Ale pokud clusteru je zabezpečení, musíte nainstalovat certifikát na místním počítači před pokračováním. Další informace najdete v tématu [jak nakonfigurovat zabezpečené připojení](service-fabric-visualstudio-configure-secure-connections.md) . Až budete hotovi, klikněte na tlačítko **OK** . V dialogovém okně **Publikování aplikace služby struktury** se zobrazí vybraného obrázku.

3. V rozevíracím seznamu **Souboru parametr aplikace** přejděte na soubor parametr aplikace. Soubor aplikace parametr obsahuje uživatel zadal hodnoty parametrů v aplikaci soubor. Chcete-li přidat nebo změnit parametr, klikněte na tlačítko **Upravit** . Zadání nebo změna hodnoty parametru v mřížce **Parametry** . Až budete hotovi, klikněte na tlačítko **Uložit** .

    ![** Upravit parametry ** dialogové okno][2]

4. Zaškrtávací políčko **upgradovat aplikaci** můžete určit, jestli to publikovat akce slouží upgrade. Upgrade publikovat akce se liší od normální publikovat akce. Seznam rozdíly naleznete v tématu [Služby struktury aplikace upgradu](service-fabric-application-upgrade.md) . Konfigurace nastavení upgradu, zvolte odkaz **Konfigurovat nastavení upgradu** . Zobrazí se editor upgradu parametr. Přečtěte si téma [Konfigurace upgrade aplikace služby struktury](service-fabric-visualstudio-configure-upgrade.md) zobrazíte další informace o upgradu parametry.

5. Zvolte **Manifest verze...** tlačítko zobrazíte dialogové okno **Upravit verze** . Budete muset aktualizovat aplikace a služby verze pro upgrade proběhnout. Viz [aplikace služby struktury upgrade kurzu](service-fabric-application-upgrade-tutorial.md) se dozvíte, jak aplikace a přihlašovacích údajů manifest verze dopad proces upgradu.

    ![** Upravit verze ** dialogové okno][3]

    Používáte-li na aplikace a služby verze sémantického správy verzí například 1.0.0 nebo číselných hodnot ve formátu 1.0.0.0, vyberte možnost **automaticky aktualizovat aplikace a služby verze** . Pokud vyberete tuto možnost, služby a čísla verze aplikace se automaticky aktualizují při každém kódu, konfigurace nebo dat balíček verze se aktualizuje. Pokud chcete upravit verze ručně, zrušte zaškrtnutí políčka k zakázání této funkce.

    >[AZURE.NOTE] U všech položek balíček pro projekt actor zobrazení nejdřív vytvořte projektu k vytvoření položek v seznamu soubory Manifest služby.

6. Po dokončení zadávání všechny potřebné možnosti klikněte na tlačítko **Publikovat** publikovat aplikaci vybraného obrázku struktury služby. Nastavení, které jste zadali, použijí se při procesu publikování.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publikování na koncový bod libovolného obrázku (včetně stran clusterů)

Visual Studio publikování prostředí je optimalizována pro publikování ve vzdáleném clusterů přidružené jedno předplatné Azure. Však je možné publikovat do libovolného koncové body (například služba struktury stran clusterů) přímo úpravou profilu publikování XML. Jak jsme je popsali výše, dvě publikovat profily podle výchozí –**Local.xml** a **Cloud.xml**–, ale jste Vítá vás vytvořit další profily na jiném prostředí. Můžete například vytvořit profil pro publikování clusterů stran, možná s názvem **Party.xml**.

Pokud se připojujete ke nezabezpečenou clusteru, potřebného stačí koncový bod připojení obrázku, například `partycluster1.eastus.cloudapp.azure.com:19000`. Připojení koncového bodu v profilu publikování v takovém případě vypadat nějak takto:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Pokud se připojujete ke zabezpečené clusteru, musíte taky poskytují podrobnosti používaného certifikátu klienta z místní úložiště se použije pro ověření. Další informace najdete v tématu [Konfigurace zabezpečené připojení do služby struktury clusteru](service-fabric-visualstudio-configure-secure-connections.md).

  Po nastavení profilu publikovat si můžete odkázat v dialogovém okně Publikovat jak je ukázáno v následujícím příkladu.

  ![Nové publikovat profilu v dialogové okno Publikovat][4]

  Všimněte si, že v tomto případě nový profil publikovat odkazuje na jednu z soubory parametr výchozí aplikace. Je to vhodné, pokud chcete publikovat stejné konfigurace aplikace na počet prostředí. Naopak v případech, kde chcete mít různé konfigurace pro každý prostředí, které chcete publikovat, ho dává smysl vytvořit soubor parametr odpovídající aplikaci.

## <a name="next-steps"></a>Další kroky

Informace o k automatizaci procesu publikování v prostředí nepřetržitý integrace najdete v tématu [Nastavení nepřetržitý integrace služby struktury](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
