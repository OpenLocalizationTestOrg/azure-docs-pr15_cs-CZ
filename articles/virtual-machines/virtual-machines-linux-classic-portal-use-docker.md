<properties
    pageTitle="Ve tvaru přípony Docker OM Linux | Microsoft Azure"
    description="Popisuje Docker a rozšíření Azure virtuálních počítačích a jak vytvořit Azure virtuálních počítačích, které jsou docker tabulkami hosts pomocí rozhraní příkazového řádku Azure v modelu klasické nasazení."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Rozšíření OM Docker pomocí portálu Azure klasické

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) patří mezi nejoblíbenější přístupy virtualization, která využívají [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) spíše než virtuálních počítačích tak izolace dat a výpočetních na sdíleného zdroje. Rozšíření Docker OM spravuje [Azure Linux Agent] slouží k vytvoření Docker OM, který je hostitelem libovolný počet kontejnery pro používání aplikací v Azure.

> [AZURE.NOTE] Toto téma popisuje, jak vytvořit OM Docker z portálu Microsoft Azure klasické. Jak vytvořit OM Docker na příkazovém řádku zobrazíte postup [pomocí koncovku OM Docker z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)]. Diskuse uceleném kontejnery a jejich výhody najdete v tématu [Docker vysokou úroveň Tabule](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Vytvoření nového OM z Galerie obrázků
Cílem prvního kroku vyžaduje Azure OM z Linux obrázku, který podporuje Docker OM příponu, se systémem Ubuntu 14.04 l obrázek z Galerie obrázků jako obrázek serveru vzorku a systémem Ubuntu 14.04 Desktop jako klienta. Na portálu klikněte na **+ Nový** v levém dolním k vytvoření nové instance OM a vyberte obrázek se systémem Ubuntu 14.04 l z dostupných možnostech nebo dokončeno Galerie obrázků, jak je ukázáno v následujícím příkladu.

> [AZURE.NOTE] Pouze se systémem Ubuntu 14.04 l obrázky novější než červenec 2014 v současné době podporuje rozšíření OM Docker.

![Vytvořit nový obrázek se systémem Ubuntu](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Vytvoření Docker certifikátů

Po vytvoření OM Ujistěte se, že je ve vašem klientském počítači nainstalovaný Docker. (Podrobnosti najdete v tématu [Pokyny k instalaci Docker](https://docs.docker.com/installation/#installation).)

Vytvořit certifikát a klíč soubory Docker komunikace podle [Systém Docker s https] a umístěte je v **`~/.docker`** adresáře ve vašem klientském počítači.

> [AZURE.NOTE] Rozšíření OM Docker na portálu aktuálně vyžaduje přihlašovací údaje, které jsou ve formátu Base 64 kódování.

Na příkazovém řádku pomocí **`base64`** nebo jiný nástroj Oblíbené kódování vytvořit kódováním Base 64 témata. Tím se simple sadu souborů, certifikát a klíč může vypadat nějak takto:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Přidání koncovku Docker OM
Přidání rozšíření OM Docker, vyhledejte OM instance, kterou jste vytvořili a posuňte se dolů na **rozšíření** a klepnutím vyvoláte rozšíření OM, jak je ukázáno v následujícím příkladu.
> [AZURE.NOTE] Tato funkce je podporována v portálu náhledu: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Přidání rozšíření
Klikněte **+ Přidat** zobrazte možná OM rozšíření můžete přidat tento v angličtině.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Vyberte rozšíření OM Docker
Rozšíření OM Docker, které se vyvolá Docker popis a důležité odkazy, a potom na tlačítko **vytvořit** v dolní části zahajte proces instalace.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Přidání certifikátu a důležitých souborů:

V poli formuláře zadejte kódováním Base 64 verzích certifikátu CA, certifikát serveru a kód serveru, jak je znázorněno na následujícím obrázku.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Všimněte si, že (například na předchozím obrázku). 2376 vyplníte ve výchozím nastavení. Můžete zadat libovolný koncový bod, ale dalším krokem bude otevřete odpovídající koncového bodu. Pokud změníte výchozí, zkontrolujte, že otevřete odpovídající koncového bodu v dalším kroku.

## <a name="add-the-docker-communication-endpoint"></a>Přidání koncový bod Docker komunikace
Při prohlížení skupina zdroje, které jste vytvořili, vyberte skupinu zabezpečení sítě přidružený k vaší OM a klikněte na **Příchozí pravidla zabezpečení** zobrazte pravidla, jak ukazuje tato část.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Klikněte na tlačítko **+ Přidat** přidat další pravidlo a v případě výchozí zadejte název koncového bodu (v tomto příkladu **Docker**) a. 2376 "cílový Port oblast". Nastavte hodnotu z pole protokol s **TCP**a klikněte na **OK** vytvořte pravidla.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Testování Docker klienta a Azure Docker Host (hostitel)
Nalezení a zkopírování název svého OM domény a příkazového řádku klientského počítače typ `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (kde *dockerextension* nahrazuje subdoména pro vaše OM).

Výsledek by měl vypadat nějak takto:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Jakmile dokončíte výše uvedené kroky, teď máte plně funkční Docker host spuštěný na Azure OM, nakonfigurované tak, aby být připojená ke vzdálené z jiných klientech.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Jste připraveni přejít na [Docker User Guide] a použijte Docker OM. Pokud chcete automatizovat vytváření Docker tabulkami hosts na Azure VMs prostřednictvím rozhraní příkazového řádku, přečtěte si, [jak používat rozšíření OM Docker z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Jak používat rozšíření OM Docker z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Spuštění Docker s https]: http://docs.docker.com/articles/https/
[Docker User Guide]: https://docs.docker.com/userguide/
