<properties
   pageTitle="Azure Správa kontejneru kontejneru služby prostřednictvím rozhraní REST API | Microsoft Azure"
   description="Nasazení kontejnery clusteru Mesos kontejneru služby Azure pomocí rozhraní REST API Marathon."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontejnerů, Micro služby, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>Správa kontejneru prostřednictvím rozhraní REST API

Datacentrum/OS poskytuje prostředí pro nasazení a stejné měřítko skupinový úloh, při abstracting základním hardwaru. Horní části Datacentrum/s operačním systémem je rámec, který má na starosti plánování a provádění úloh pro využití.

Přestože rámce jsou k dispozici pro mnoho oblíbených zatížení, tento dokument popisuje jak můžete vytvořit a změnit velikost kontejneru nasazení pomocí Marathon. Před pracovních přes tyto příklady, musíte clusteru Datacentrum/OS nakonfigurovaný služby Azure kontejner. Taky musíte mít vzdáleného připojení k tomuto clusteru. Další informace o těchto položek najdete v následujících článcích:

- [Nasazení služby Azure kontejneru obrázku](container-service-deployment.md)
- [Připojení k obrázku kontejneru služby Azure](container-service-connect.md)

Když jste připojeni k obrázku služba Azure kontejneru, máte přístup k Datacentrum/OS a související REST API prostřednictvím http://localhost:local portu. Příklady v tomto dokumentu Předpokládejme, že jsou tunneling na portu 80. Například koncový bod Marathon zastižení `http://localhost/marathon/v2/`. Další informace o různých rozhraní API najdete v článku mezosféře dokumentaci k rozhraní [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) a rozhraní [Chronos API](https://mesos.github.io/chronos/docs/api.html)a Apache dokumentaci k rozhraní [API Mesos plánovači](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Shromážděte informace ze Datacentrum/OS a Marathon

Před nasazením kontejnerů clusteru Datacentrum/OS získat informace o Datacentrum/OS clusteru, jako jsou jména a aktuální stav agentů Datacentrum/OS. Chcete-li provést dotaz `master/slaves` koncový bod rozhraní REST API Datacentrum/OS. V případě všechno dobře, zobrazí se seznam Datacentrum/OS agentů a několik vlastností pro jednotlivá pole.

```bash
curl http://localhost/mesos/master/slaves
```

Teď, použít Marathon `/apps` koncový bod zjistit aktuální nasazení aplikace Datacentrum/OS clusteru. Pokud je to nový clusteru, zobrazí se prázdné pole pro aplikace.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Nasazení Docker formátování kontejneru

Nasazení formátovaných Docker kontejnery prostřednictvím Marathon pomocí JSON souboru, který popisuje zamýšleného nasazení. V následujícím příkladu nasadí kontejneru Nginx vazby port 80 agenta Datacentrum/OS s portem 80 kontejner. Navíc nezapomeňte, že je nastavena "acceptedResourceRoles" na "slave_public". Kontejneru to bude nasadit agent v sadě agent veřejných webových měřítko.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Abyste mohli nasadit kontejneru formátovaných Docker, vytvořte souboru JSON nebo použijte uvedené ukázkové v [kontejneru služby Azure ukázku](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Uložte ho do dostupného místa. Další abyste mohli nasadit kontejneru, spusťte tento příkaz. Zadejte název souboru JSON.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Výstup bude vypadat podobně jako takto:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Teď Pokud dotaz Marathon aplikací toto nové aplikace se zobrazí výstupu.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Změnit velikost vaší kontejnery

Můžete také rozhraní API Marathon rozšiřování nebo zobrazit v aplikaci nasazení. V předchozím příkladu nasazené jednou instancí aplikace. Pojďme přizpůsobit tento přes tři instance aplikace. Postup vytvoření souboru JSON pomocí následující text JSON a uložte ho do dostupného místa.

```json
{ "instances": 3 }
```

Spusťte tento příkaz rozšiřování aplikace.

>[AZURE.NOTE] Identifikátor URI bude http://localhost/marathon/v2/apps/ a potom ID aplikace zobrazit. Pokud používáte Nginx vzorku, který není uvedený tady, identifikátor URI by http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Nakonec dotaz Marathon koncový bod pro aplikace. Zobrazí se nyní Zde jsou tři kontejnerů Nginx.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Pomocí tohoto cvičení: Marathon REST API interakce s prostředím PowerShell

Tyto stejné akce lze provést pomocí prostředí PowerShell příkazy v systému Windows.

Získat informace o Datacentrum/OS clusteru, jako jsou jména agent a stav agent, spusťte tento příkaz.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Nasazení formátovaných Docker kontejnery prostřednictvím Marathon pomocí JSON souboru, který popisuje zamýšleného nasazení. V následujícím příkladu nasadí kontejneru Nginx vazby port 80 agenta Datacentrum/OS s portem 80 kontejner.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Vytvoření souboru JSON nebo použijte uvedené ukázkové v [kontejneru služby Azure ukázku](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Uložte ho do dostupného místa. Další abyste mohli nasadit kontejneru, spusťte tento příkaz. Zadejte název souboru JSON.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Můžete také rozhraní API Marathon rozšiřování nebo zobrazit v aplikaci nasazení. V předchozím příkladu nasazené jednou instancí aplikace. Pojďme přizpůsobit tento přes tři instance aplikace. Postup vytvoření souboru JSON pomocí následující text JSON a uložte ho do dostupného místa.

```json
{ "instances": 3 }
```

Spusťte tento příkaz rozšiřování aplikace.

> [AZURE.NOTE] Identifikátor URI bude http://localhost/marathon/v2/apps/ a potom ID aplikace zobrazit. Pokud používáte ukázku Nginx uvedený tady, identifikátor URI by http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Další kroky

- [Přečtěte si další informace o koncové body Mesos HTTP]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Přečtěte si další informace o rozhraní REST API Marathon]( https://mesosphere.github.io/marathon/docs/rest-api.html).
