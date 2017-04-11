<properties
   pageTitle="Azure kontejneru služba Správa kontejneru s Docker Swarm | Microsoft Azure"
   description="Nasazení kontejnery Docker Swarm kontejneru služby Azure"
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

# <a name="container-management-with-docker-swarm"></a>Správa kontejneru prostřednictvím Docker Swarm

Docker roj poskytuje prostředí pro nasazení kontejnerizovaná úloh přes fondu sadu Docker tabulkami hosts. Docker roj používá nativní rozhraní API Docker. Že je pracovní postup pro správu kontejnerů na Docker Swarm téměř shodné s co je na hostiteli jednoho kontejneru. Tento dokument obsahuje jednoduché Příklady nasazení kontejnerizovaná úloh v instanci služby Azure kontejneru Docker Swarm. Další podrobné si přečtěte následující dokumentaci na Docker Swarm najdete v článku [Docker Swarm na Docker.com](https://docs.docker.com/swarm/).

Předpoklady cvičení v tomto dokumentu:

[Vytvoření clusteru roj kontejneru služby Azure](container-service-deployment.md)

[Spojení s clusteru roj kontejneru služby Azure](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Nasaďte novou kontejneru

Vytvoření nového kontejneru v Docker Swarm, můžete `docker run` příkaz (zajistit, že jste otevřeli SSH tunelem předlohy podle výše uvedených požadavky). Tento příklad vytvoří kontejneru z `yeasy/simple-web` obrázek:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Po vytvoření kontejneru použít `docker ps` k vrácení informací o kontejneru. Oznámení, že je uvedená roj agent, který je hostitelem kontejner:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Nyní máte přístup, na kterém běží v tomto kontejneru prostřednictvím název veřejné DNS pro vyrovnávání zatížení agent roj aplikace. Tyto informace najdete v Azure portálu:  


![Skutečné navštivte výsledků](media/real-visit.jpg)  

Ve výchozím nastavení obsahuje Vyrovnávání zatížení porty 80, 443 a 8080 otevřít. Pokud se chcete připojit na jiném portu, budete muset otevřete daný port na Vyrovnávání zatížení Azure agenta skupiny.

## <a name="deploy-multiple-containers"></a>Nasazení více kontejnery

Více kontejnery jsou zahájené, provádění docker spustit několikrát, můžete použít `docker ps` zjistíte, který hostuje kontejnery spuštěných pro. V následujícím příkladu jsou tři kontejnery rovnoměrné rozdělení přes tři agentů roj:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Nasazení kontejnery pomocí vytvořte Docker

Vytvořte Docker můžete použít k automatizaci nasazení a konfiguraci více kontejnery. Pokud chcete provést, zkontrolujte, jestli vytvořené tunelem zabezpečené prostředí (SSH) a že proměnnou DOCKER_HOST byl nastaven (viz výše předpoklady).

Vytvoření souboru docker compose.yml v místním systému. K tomuto účelu použijte tento [Ukázkový](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Spuštění `docker-compose up -d` zahájíte nasazení kontejner:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Nakonec bude vrácena seznam spuštěných kontejnery. Tento seznam odráží, která byla nasazená pomocí Docker vytvořte kontejnery:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Samozřejmě můžete použít `docker-compose ps` zkoumat pouze kontejnery podle svého `compose.yml` soubor.

## <a name="next-steps"></a>Další kroky

[Další informace o Docker Swarm](https://docs.docker.com/swarm/)
