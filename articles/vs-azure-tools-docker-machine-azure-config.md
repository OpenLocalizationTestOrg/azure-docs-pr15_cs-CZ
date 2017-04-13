<properties
   pageTitle="Vytvoření Docker tabulkami hosts v Azure s Docker počítači | Microsoft Azure"
   description="Popisuje použití počítače Docker vytvoření docker tabulkami hosts v Azure."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Vytvoření Docker tabulkami Hosts v Azure s Docker počítače

Spuštění [Docker](https://www.docker.com/) kontejnery vyžaduje host spuštěný démona docker OM.
Toto téma popisuje, jak pomocí příkazu [docker počítače](https://docs.docker.com/machine/) můžete vytvořit nový VMs Linux nakonfigurována démona Docker spuštěné v Azure. 

**Poznámka:** 
- *V tomto článku závisí na verzi docker počítače 0.7.0 nebo vyšší*
- *Kontejnery Windows se nebude podporovat prostřednictvím docker počítače v blízké budoucnosti*

## <a name="create-vms-with-docker-machine"></a>Vytvoření VMs s Docker počítače

Vytvoření docker hostitele VMs v Azure s `docker-machine create` příkaz pomocí `azure` ovladač. 

Ovladač Azure potřebovat svého předplatného ID. [Rozhraní příkazového řádku Azure](xplat-cli-install.md) nebo [Portál Azure](https://portal.azure.com) slouží k načtení předplatného Azure. 

**Pomocí portálu Azure**
- Na stránce levém navigačním panelu vyberte předplatné a zkopírujte id předplatného.

**Použití Azure rozhraní příkazového řádku**
- Typ ```azure account list``` a zkopírujte id předplatného.

Typ `docker-machine create --driver azure` zobrazíte možnosti a jeho výchozí hodnoty.
Najdete v článku [si přečtěte následující dokumentaci Docker Azure ovladač](https://docs.docker.com/machine/drivers/azure/) dalších informací. 

V následujícím příkladu vycházejí z předpokladu, výchozí hodnoty, ale pokud chcete otevřít portu 80 OM pro přístup k Internetu. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Zvolte hostitel docker s docker počítače
Až budete mít položky ve počítače docker vašeho hostitele, můžete nastavit výchozí hostitel při spuštění docker příkazy.
##<a name="using-powershell"></a>Pomocí prostředí PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Použití flám

```bash
eval $(docker-machine env MyDockerHost)
```

Nyní je možné spustit docker příkazy před zadaným hostitelem

```
docker ps
docker info
```

## <a name="run-a-container"></a>Spuštění kontejneru

U hostitele nakonfigurovali můžete teď spustit jednoduchý webový server k ověření, zda byla správně nakonfigurované vašeho hostitele.
Tady jsme pomocí obrázku standardních nginx určit, zda by měly naslouchají relacím na port 80 a, pokud se restartuje hostiteli OM kontejneru bude restartujte i (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Výstup by měl vypadat nějak takto:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testování kontejneru

Zkontrolujte pracovního kontejnery pomocí `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

A zkontrolujte pracovního kontejner, zadejte `docker-machine ip <VM name>` zobrazíte IP adresa zadejte v prohlížeči:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Průběžný ngnix kontejneru](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Souhrn
S docker počítače můžete snadno zřizujete docker tabulkami hosts v Azure pro ověření vaší jednotlivé docker Host (hostitel).
Výrobní hostitelem kontejnerů, najdete v článku [Kontejneru služby Azure](http://aka.ms/AzureContainerService)

K vývoji aplikací Core .NET s Visual Studiu, najdete v článku [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)
