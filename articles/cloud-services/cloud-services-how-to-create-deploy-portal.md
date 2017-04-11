<properties
    pageTitle="Vytvoření a nasazení do cloudové služby | Microsoft Azure"
    description="Naučte se vytvářet a publikovat do cloudové služby Azure portálu."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Vytvoření a nasazení do cloudové služby

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-create-deploy-portal.md)
- [Azure klasický portálu](cloud-services-how-to-create-deploy.md)

Portál Azure nabízí dva způsoby vytvoření a nasazení do cloudové služby: *Vytvořit* a *Vytvořit vlastní*.

Tento článek vysvětluje, jak používat metodu vytvořit k vytvoření nového Cloudová služba a pak pomocí **Nahrát** nahrávání a nasazení balíčku služby cloudu v Azure. Pokud tímto způsobem provede portálu Azure k dispozici užitečné odkazy pro dokončení všech požadavky průběžně. Pokud budete chtít nasazení cloudové služby, kdy ho vytvoříte, můžete udělat i ve stejnou dobu pomocí vytvořit vlastní.

> [AZURE.NOTE] Pokud chcete publikovat cloudové služby z Visual Studio týmu služeb (VSTS), použijte vytvořit a nastavení publikování VSTS z Azure rychlý úvod nebo na řídicím panelu. Další informace najdete v tématu [Nepřetržitý doručování do Azure pomocí služby týmu Visual Studio][TFSTutorialForCloudService], nebo v tématu Nápověda k **Rychlé úvodní** stránku.

## <a name="concepts"></a>Koncepty
Tři součásti jsou potřeba k nasazení aplikace jako do cloudové služby Azure:

- **Definice služby**  
  Soubor definice služby cloudu (.csdef) definuje model služby, včetně počtu role.

- **Konfigurace služby**  
  Cloudové služby konfiguračního souboru (.cscfg) obsahuje nastavení pro cloudu služby a jednotlivé role, včetně počtu role instance.

- **Balíček služeb**  
  Balíček služby (.cspkg) obsahuje kód aplikace a konfigurace a soubor definice služby.

Další informace o těchto a jak vytvořit balíček [zde](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Příprava aplikace
Před nasazením do cloudové služby, musíte vytvořit balíček služby cloudu (.cspkg) z kód aplikace a cloudové služby konfigurační soubor (.cscfg). Azure SDK obsahuje nástroje pro přípravu tyto soubory požadované nasazení. V SDK můžete nainstalovat ze stránky [Souborů ke stažení Azure](https://azure.microsoft.com/downloads/) v jazyce, ve kterém chcete vytvořit kód aplikace.

Tři cloudové služby funkce vyžadují zvláštní konfigurace před exportem balíčku služby:

- Pokud chcete nasadit do cloudové služby, který používá Secure (Sockets Layer SSL) pro šifrování SSL [nakonfigurovat aplikaci](cloud-services-configure-ssl-certificate-portal.md#modify) .

- Pokud chcete konfigurovat připojení ke vzdálené ploše na role instance [Konfigurace role](cloud-services-role-enable-remote-desktop.md) pro vzdálené plochy. To provést pouze na klasické portálu.

- Pokud chcete konfigurovat podrobného sledování cloudové služby, povolte pro cloudové služby Azure diagnostiky. *Minimální sledování* (výchozí nastavení monitorování úroveň) používá výkonnosti shromážděné z hostitele operační systémy pro role instance (virtuálních počítačích). *Sledování podrobné* shromažďuje další metriky podle data o výkonu v rámci instance role, které chcete povolit bližší analýzu problémů, ke kterým dochází při zpracování aplikace. Pokud chcete zjistit, jak povolit diagnostiky Azure, najdete v článku [Povolení Diagnostika v Azure](cloud-services-dotnet-diagnostics.md).

Pokud chcete vytvořit nasazení role webu nebo pracovního role do cloudové služby, musíte [vytvořit balíček služby](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Než začnete

- Pokud jste nenainstalovali Azure SDK, klikněte na **Nainstalovat SDK Azure** otevřete [stránky souborů ke stažení Azure](https://azure.microsoft.com/downloads/)a pak budou moct stáhnout SDK pro jazyk, ve kterém chcete vytvořit svůj kód. (Budete mít možnost to provést později.)

- Pokud všechny instance role vyžadují certifikát, vytvořte certifikáty. Cloudové služby vyžadují soubor .pfx se soukromým klíčem. [Že můžete nahrávat certifikáty Azure]() při vytvoření a nasazení cloudovou službu.

- Pokud plánujete nasazení cloudovou službu spřažení skupiny, vytvořte spřažení skupinu. Použít skupinu spřažení nasazení Cloudová služba a další služby Azure do stejného umístění v oblasti. Skupina spřažení můžete vytvořit v oblasti **sítí** portálu na stránce **skupiny spřažení** Azure klasické.


## <a name="create-and-deploy"></a>Vytvoření a nasazení

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **Nový > virtuálních počítačích**a potom přejděte dolů a klikněte na **Cloudové služby**.

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. V dolní části stránky informace, která se zobrazí klikněte na **vytvořit**. 
4. V nové zásuvné **Cloudové služby** zadejte hodnotu pro **název DNS**.
5. Vytvoření nové **Skupiny prostředků** nebo vyberte stávající.
6. Vyberte **umístění**.
7. Klikněte na **balíček**. Otevře se zásuvné **uložení balíčku** . Vyplňte požadovaná pole.  

     Pokud některý z vaše role obsahují jedna instance, zajistěte, aby **že nasazení i v případě jedné nebo více rolí obsahují jedna instance** zaškrtnuté.

8. Ujistěte se, zda je zaškrtnuto **Start nasazení** .
9. Klikněte na **OK** , které se zavře zásuvné **uložení balíčku** .
10. Pokud nemáte žádné certifikáty přidat, klikněte na **vytvořit**.

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Nahrání certifikát

Pokud balíček pro nasazení je [nakonfigurovaný na používání certifikáty](cloud-services-configure-ssl-certificate-portal.md#modify), můžete teď nahrát certifikát.

1. **Certifikáty**na zásuvné **certifikáty přidat** soubor .pfx certifikátu SSL a vyberte certifikát, zadejte **heslo**
2. Klikněte na **připojit certifikát**a klikněte na **OK** na zásuvné **Přidat certifikáty** .
3. Klikněte na **vytvořit** na zásuvné **Cloudové služby** . Při nasazení dosáhl **připravení** stavu, můžete přejít k dalším krokům.

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Ověření nasazení byla úspěšně dokončena

1. Klikněte na instance služby cloudu.

    Stav byste měli vidět, že služba není **spuštěný**.

2. V části **Základy**klikněte na **Adresu URL webu** otevřete Cloudová služba ve webovém prohlížeči.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Další kroky

* [Obecné konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* [Spravovat Cloudová služba](cloud-services-how-to-manage-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).