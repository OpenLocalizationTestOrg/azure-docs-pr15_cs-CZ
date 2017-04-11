<properties
   pageTitle="Aplikaci nebo službu Marathon specifické pro uživatele | Microsoft Azure"
   description="Vytvoření aplikace nebo služby Marathon specifické pro uživatele"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontejnery, Marathon, Micro služby, Datacentrum/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Vytvoření aplikace nebo služby Marathon specifické pro uživatele

Služba Azure kontejneru k dispozici sadu hlavní servery, které jsme předem Apache Mesos a Marathon. Tyto mohou sloužit k organizovat vaše aplikace clusteru, ale je nejlepší používat hlavní servery k tomuto účelu. Například doladění konfigurace Marathon vyžaduje protokolování do předlohy servery samotné a změn – to podporuje jedinečné předlohy serverech trochu jinak, než standardu a potřebujete péče a správy nezávisle na sobě. Konfigurace vyžadované jednoho týmu navíc nemusí být optimální konfiguraci pro jiné týmu.

V tomto článku jsme budete vysvětlují postup přidání aplikaci nebo službu Marathon specifické pro uživatele.

Protože tuto službu bude patří do jednoho uživatele nebo týmu, jsou zadarmo ji nakonfigurovat žádným způsobem, který nadcházejících událostech. Služba Azure kontejneru také zajistíte, že služba pokračuje. Neúspěšné služby Azure kontejneru služba ho restartuje. Ve většině případů nebude i zjistíte, že byl výpadek služeb.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[Nasazení instanci služby Azure kontejneru](container-service-deployment.md) typu orchestrator Datacentrum/OS a [Ujistěte se, můžete na svůj cluster připojit svému klientovi](container-service-connect.md). Proveďte následující kroky.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Vytvoření aplikace nebo služby Marathon specifické pro uživatele

Začněte tak, že vytvoříte JSON konfigurační soubor, který definuje název aplikace služba, která chcete vytvořit. Tady máme použít `marathon-alice` jako název framework. Uložte soubor jako nějak `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Při instalaci Marathon instance s možnostmi, které se nastavují v souboru konfigurace pak použijte Datacentrum/OS rozhraní příkazového řádku:

```bash
dcos package install --options=marathon-alice.json marathon
```

Teď byste měli vidět váš `marathon-alice` služba spuštěná na kartě služby UI Datacentrum/OS. Uživatelské rozhraní budou `http://<hostname>/service/marathon-alice/` požadovaná přímý přístup.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Nastavení Datacentrum/OS rozhraní příkazového řádku pro přístup ke službě

Volitelně můžete nakonfigurovat rozhraní příkazového řádku svého Datacentrum/OS pro přístup k této novou službu nastavením `marathon.url` vlastnost tak, aby ukazovaly `marathon-alice` instance následujícím způsobem:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Můžete ověřit, která instance Marathon, vaše rozhraní příkazového řádku spolupracuje proti `dcos config show` příkaz. Můžete se vrátit k používání služby předlohy Marathon pomocí příkazu `dcos config unset marathon.url`.
