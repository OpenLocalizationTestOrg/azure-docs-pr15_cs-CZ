<properties
    pageTitle="Vytvoření a nasazení do cloudové služby | Microsoft Azure"
    description="Naučte se vytvářet a publikovat do cloudové služby pomocí metody vytvořit v Azure."
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Vytvoření a nasazení do cloudové služby

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-create-deploy-portal.md)
- [Azure klasické portálu](cloud-services-how-to-create-deploy.md)

Portál Azure klasické nabízí dva způsoby vytvoření a nasazení do cloudové služby: **Vytvořit** a **Vytvořit vlastní**.

Toto téma vysvětluje, jak používat metodu vytvořit k vytvoření nové Cloudová služba a pak pomocí **Nahrát** nahrávání a nasazení balíčku služby cloudu v Azure. Pokud tímto způsobem portálu Azure klasické provede dostupné užitečné odkazy pro dokončení všech požadavky průběžně. Pokud budete chtít nasazení cloudové služby, kdy ho vytvoříte, můžete udělat i ve stejnou dobu pomocí **Vytvořit vlastní**.

> [AZURE.NOTE] Pokud chcete publikovat služby cloudu z aplikace Visual Studio týmu služeb (VSTS), použijte vytvořit a nastavení publikování VSTS z **Rychlý Start** nebo na řídicím panelu. Další informace najdete v tématu [Nepřetržitý doručení Azure pomocí služby týmu Visual Studio][TFSTutorialForCloudService], nebo v tématu Nápověda k **Rychlé úvodní** stránku.

## <a name="concepts"></a>Koncepty
Při nasazení aplikace jako do cloudové služby Azure jsou vyžaduje tři součásti:

- **Definice služby**  
  Soubor definice služby cloudu (.csdef) definuje model služby, včetně počtu role.

- **Konfigurace služby**  
  Cloudové služby konfiguračního souboru (.cscfg) obsahuje nastavení pro cloudu služby a jednotlivé role, včetně počtu výskytů role.

- **Balíček služeb**  
  Balíček služby (.cspkg) obsahuje kód aplikace a konfigurace a soubor definice služby.
  
Další informace o těchto a jak vytvořit balíček [zde](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Příprava aplikace
Před nasazením do cloudové služby, musíte vytvořit balíček služby cloudu (.cspkg) z aplikace kódu a cloudové služby konfigurační soubor (.cscfg). Azure SDK obsahuje nástroje pro přípravu tyto soubory povinná nasazení. V SDK můžete nainstalovat ze stránky [Souborů ke stažení Azure](https://azure.microsoft.com/downloads/) v jazyce, ve kterém chcete vytvořit kód aplikace.

Tři cloudové služby funkce vyžadují zvláštní konfigurace před exportem balíčku služby:

- Pokud chcete nasadit do cloudové služby, který používá Secure (Sockets Layer SSL) pro šifrování SSL [nakonfigurovat aplikaci](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) .

- Pokud chcete konfigurovat připojení ke vzdálené ploše na role instance [Konfigurace role](cloud-services-role-enable-remote-desktop.md) pro vzdálené plochy.

- Pokud chcete konfigurovat podrobného sledování cloudové služby, povolte pro cloudové služby Azure diagnostiky. *Minimální sledování* (výchozí nastavení monitorování úroveň) používá výkonnosti shromážděné z hostitele operační systémy pro role instance (virtuálních počítačích). "Podrobného sledování * shromažďuje další metriky podle data o výkonu v rámci instance role, které chcete povolit bližší analýzu problémů, ke kterým dochází při zpracování aplikace. Pokud chcete zjistit, jak povolit diagnostiky Azure, najdete v článku [Povolení Diagnostika v Azure](cloud-services-dotnet-diagnostics.md).

Pokud chcete vytvořit nasazení role webu nebo pracovního role do cloudové služby, musíte [vytvořit balíček služby](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Než začnete

- Pokud jste nenainstalovali Azure SDK, klikněte na **Nainstalovat SDK Azure** otevřete [stránky souborů ke stažení Azure](https://azure.microsoft.com/downloads/)a pak budou moct stáhnout SDK pro jazyk, ve kterém chcete vytvořit svůj kód. (Budete mít možnost to provést později.)

- Pokud všechny instance role vyžadují certifikát, vytvořte certifikáty. Cloudové služby vyžadují soubor .pfx se soukromým klíčem. Můžete [Odeslat certifikáty Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) při vytvoření a nasazení cloudovou službu.

- Pokud plánujete nasazení cloudovou službu spřažení skupiny, vytvořte spřažení skupinu. Použít skupinu spřažení nasazení Cloudová služba a další služby Azure do stejného umístění v oblasti. Skupina spřažení můžete vytvořit v oblasti **sítí** portálu na stránce **skupiny spřažení** Azure klasické.


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Postup: vytvoření do cloudové služby pomocí vytvořit

1. V [Azure klasické portál](http://manage.windowsazure.com/), klikněte na **Nový**>**Výpočet**>**Cloudové služby**>**Vytvořit**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. Ve webové části **adresy URL**zadejte název subdoménu používat ve veřejné adresy URL pro přístup ke cloudové služby ve výrobním nasazení. Je formát adresy URL výrobní nasazení: http://*myURL*. cloudapp.net.

3. V **oblasti nebo spřažení skupiny**vyberte zeměpisná oblast spřažení skupiny pro nasazení cloudové služby na. Pokud chcete nasadit cloudové služby do stejného umístění jako jiných Azure služeb v oblasti, vyberte skupinu spřažení.

4. Klikněte na **vytvořit cloudové služby**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Můžete sledovat stav procesu v oblasti zprávy v dolní části okna.

    Oblasti **Cloudové služby** , otevře se nové cloudovou službu zobrazí. Jakmile se stavem změní na vytvořeno, byla úspěšně dokončena vytváření služby cloudu.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Postup: odeslat certifikát do cloudové služby

1. [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Cloudové služby**, klikněte na název cloudovou službu a klepněte na položku **certifikáty**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Klikněte na možnost **Odeslat certifikát** nebo **Odeslat**.

3. V **souboru**použijte **Procházet** a vyberte certifikát (soubor .pfx).

4. Do pole **heslo**zadejte privátním klíčem certifikátu.

5. Klikněte na **OK** (zaškrtnuté).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Můžete sledovat průběh nahrát do oblasti zprávy ukázáno v následujícím příkladu. Po dokončení nahrávání certifikát bude přidán do tabulky. Do oblasti zprávy klikněte na OK zprávu zavřete.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Postup: nasazení do cloudové služby

1. [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Cloudové služby**, klikněte na název cloudovou službu a potom klikněte na **řídicí panel**.

2. Klikněte na **Nahrát nový provozní nasazení** nebo **Odeslat**.

3. **Nasazení popisek**zadejte název nového nasazení – například MyCloudServicev4.

3. Do **balíčku**umožňuje **Procházet** vyberte soubor balíčku služby (.cspkg) používat.

4. V **konfiguraci** **vyhledejte** vyberte pomocí služby konfigurovat tento soubor (.cscfg).

5. Pokud cloudovou službu bude obsahovat žádné role s jedinou instance, **nasazení i v případě jedné nebo více rolí obsahují jedna instance** zaškrtněte políčko Povolit nasazení pokračovat.

    Azure pouze zaručit 99.95 procent přístup ke cloudové služby během aktualizací služby a údržbu Pokud jednotlivých rolích obsahuje aspoň dvě instance. V případě potřeby můžete přidat další roli instance na stránce **Měřítko** po nasazení cloudovou službu. Další informace najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

6. Kliknutím na tlačítko **OK** (zaškrtnutí) začněte nasazení služby cloudu.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Stav nasazení do oblasti zprávy můžete sledovat. Kliknutím na OK skrýt zprávy.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Ověření nasazení byla úspěšně dokončena

1. Klikněte na **řídicí panel**.

    Stav byste měli vidět, že služba není **spuštěný**.

2. V části **můžete rychle podívat**na adresu URL webu otevřete Cloudová služba ve webovém prohlížeči.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Další kroky

* [Obecné konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Spravovat Cloudová služba](cloud-services-how-to-manage.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).