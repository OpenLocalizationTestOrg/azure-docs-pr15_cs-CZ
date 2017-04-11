<properties
   pageTitle="Načtení zůstatek kontejnery v kontejneru služby Azure clusteru | Microsoft Azure"
   description="Vyrovnávání zatížení napříč několika kontejnery v kontejneru služby Azure obrázku."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontejnery Micro služby, Datacentrum/s operačním systémem, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Načtení zůstatek kontejnery v kontejneru služby Azure obrázku

V tomto článku jsme budete zjistit, jak vytvořit Vyrovnávání zatížení vnitřní v Datacentrum/OS spravovaných kontejneru služby Azure pomocí Marathon LB. To vám umožní zobrazit aplikace ve vodorovném směru. Je taky umožní využít Agent veřejných a privátních clusterů umístěním Vyrovnávání zatížení na veřejné obrázku a kontejnery aplikace soukromé clusteru.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[Nasazení instanci služby Azure kontejneru](container-service-deployment.md) typu orchestrator Datacentrum/OS a [Ujistěte se, můžete na svůj cluster připojit svému klientovi](container-service-connect.md). 

## <a name="load-balancing"></a>Vyrovnávání zatížení

V kontejneru služby obrázku, kterou jsme vytvoří jsou dvě Vyrovnávání zatížení vrstvy: 

  1. Azure Vyrovnávání zatížení poskytuje veřejný vstupní body (ty, které koncoví uživatelé se strefíte). Není uvedený automaticky kontejneru službou Azure a je ve výchozím nastavení nakonfigurované tak, aby vystavit port 80, 443 a 8080.
  2. Vyrovnávání zatížení Marathon (marathon lb) směruje příchozí požadavky na kontejner instance, které tyto žádosti o služby. Jak můžeme zobrazit kontejnery poskytující naše webové služby, marathon lb dynamicky přizpůsobuje. Tento Vyrovnávání zatížení není k dispozici ve výchozím nastavení služby kontejneru, ale je velmi jednoduché nainstalovat.

## <a name="marathon-load-balancer"></a>Vyrovnávání zatížení Marathon

Vyrovnávání zatížení Marathon dynamicky úprav podle kontejnerů, které jste nasazeny. Je taky pružné ke ztrátě kontejneru nebo agent – Pokud k tomu dojde, Apache Mesos jednoduše restartuje kontejneru někde jinde a marathon lb přizpůsobí.

Chcete-li nainstalovat Vyrovnávání zatížení Marathon můžete použít buď Datacentrum/operačního systému webové uživatelského rozhraní nebo příkazového řádku.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Instalace Marathon LB používat uživatelské rozhraní webu Datacentrum/OS

  1. Klikněte na "Universe"
  2. Vyhledejte Marathon kg
  3. Klikněte na tlačítko "nainstalovat

![Instalace marathon lb prostřednictvím webového rozhraní Datacentrum/OS](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Instalace Marathon LB pomocí rozhraní příkazového řádku Datacentrum/OS

Po instalaci rozhraní příkazového řádku Datacentrum/OS a za ověření oprávněnosti můžete připojit k obrázku, spusťte tento příkaz z klientského počítače:

```bash
dcos package install marathon-lb
```

Tento příkaz automaticky nainstaluje Vyrovnávání zatížení veřejné agentů obrázku.

## <a name="deploy-a-load-balanced-web-application"></a>Nasazení zatížení rovnováha webovou aplikaci

Teď, když máme balíček marathon lb, jsme nasazení kontejneru aplikace, která přejeme Vyrovnávání zatížení. V tomto příkladu jsme bude nasadit na jednoduché webový server pomocí následující konfigurace:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Nastavte hodnotu `HAProxy_0_VHOST` plně kvalifikovaný název domény Vyrovnávání zatížení pro vaši zástupci. Toto je ve formuláři `<acsName>agents.<region>.cloudapp.azure.com`. Pokud vytvoříte služba kontejneru obrázku s názvem například `myacs` v oblasti `West US`, bude FQDN `myacsagents.westus.cloudapp.azure.com`. To můžete najít vyhledáním Vyrovnávání zatížení s "agent" v názvu při hledání prostřednictvím zdrojů ve skupině zdroje, který jste vytvořili pro kontejner služby [Azure portálu](https://portal.azure.com).
  * Nastavit servicePort port > = 10 000. Určuje službu spuštěné v tomto kontejneru – marathon lb pomocí této funkce určení služeb, které by měl zůstatek přes.
  * Nastavit `HAPROXY_GROUP` popisek na "externí".
  * Nastavení `hostPort` na hodnotu 0. To znamená, že Marathon libovolně přidělit port k dispozici.
  * Nastavení `instances` počty instance, kterou chcete vytvořit. Můžete kdykoli zmenšit tyto nahoru a dolů později.

Je vhodné noing, ve výchozím nastavení Marathon nasadí soukromé clusteru, to znamená, že výše nasazení pouze přístupných vaší Vyrovnávání zatížení tedy obvykle chování, které jsme své oblíbené.

### <a name="deploy-using-the-dcos-web-ui"></a>Nasazení pomocí rozhraní webových Datacentrum/OS

  1. Navštivte stránku Marathon na http://localhost/marathon (po nastavení [SSH tunelem](container-service-connect.md) a klikněte na`Create Appliction`
  2. V `New Application` klikněte na dialog `JSON Mode` v pravém horním rohu
  3. Vložte výše JSON do editoru
  4. Klikněte na`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Nasazení pomocí rozhraní příkazového řádku Datacentrum/OS

Nasazení tuto aplikaci s rozhraní příkazového řádku Datacentrum/OS jednoduše zkopírovat výše JSON do souboru s názvem `hello-web.json`a spuštění:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Vyrovnávání zatížení Azure

Ve výchozím nastavení Vyrovnávání zatížení Azure zpřístupňuje porty 80 8080 a 443. Pokud používáte některou z těchto tří porty (stejně jako jsme ve výše uvedeném příkladu) a potom nic, které musíte udělat. By měla strefíte vaše Vyrovnávání zatížení agent plně kvalifikovaný název domény – a pokaždé, když aktualizujete, budete stisknutí nějaká tři webových serverů kruhového způsobem. Ale pokud používáte jiného portu, musíte přidat kruhového pravidle a zkušební na Vyrovnávání zatížení číslo portu, který jste použili. Můžete to udělat z [Azure rozhraní příkazového řádku](../xplat-cli-azure-resource-manager.md), s příkazy `azure network lb rule create` a `azure network lb probe create`. Můžete taky udělat toto pomocí portálu Azure.


## <a name="additional-scenarios"></a>Další scénáře

Můžete mít situace místo, kam můžete pomocí různých domén vystavit různé služby. Příklad:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Dosáhnout, najdete v tématech [virtuální hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), které umožňují přiřadit domény na konkrétní marathon lb cesty.

Můžete taky můžete vystavit různé porty a změnit jejich mapování správné služby za marathon lb. Příklad:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Další kroky

V dokumentaci k Datacentrum/OS pro další na [marathon lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/).
