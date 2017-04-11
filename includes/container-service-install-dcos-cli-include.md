<properties
   pageTitle="Instalace Datacentrum/operační systém rozhraní příkazového řádku | Microsoft Azure"
   description="Nainstalujte Datacentrum/operační systém rozhraní příkazového řádku."
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
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Toto je pro práci s Datacentrum/OS-based ACS clusterů. Je potřeba udělat u na základě roj ACS clusterů.

Nejdřív se [připojit k obrázku Datacentrum/OS na základě ACS](../articles/container-service/container-service-connect.md). Po dokončení, můžete nainstalovat Datacentrum/OS rozhraní příkazového řádku v klientském počítači pomocí následujících příkazů:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Pokud používáte starší verze Python, mohou být některé "InsecurePlatformWarnings". Můžete ignorovat tyto.

Abyste mohli začít pracovat bez restartování vaše prostředí, spusťte:

```bash
source ~/.bashrc
```

Tento krok není nutné při spuštění nové prostředí.

Teď můžete zkontrolovat, že je nainstalovaná rozhraní příkazového řádku:

```bash
dcos --help
```