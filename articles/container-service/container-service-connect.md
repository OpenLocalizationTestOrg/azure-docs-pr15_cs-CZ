<properties
   pageTitle="Připojení k obrázku služba Azure kontejneru | Microsoft Azure"
   description="Připojení k obrázku kontejneru služby Azure pomocí SSH tunelem."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontejnerů, Micro služby, Datacentrum/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Připojení k obrázku kontejneru služby Azure

Datacentrum/OS a Docker Swarm clusterů, které jsou používaný službou Azure kontejneru vystavit ZBÝVAJÍCÍ koncové body. Tyto koncové body však nejsou otevřené vnější světě. Abyste mohli spravovat tyto koncové body, musíte vytvořit tunelem zabezpečené prostředí (SSH). Po SSH tunelem bylo zjištěno, můžete kontrolovat koncové body clusteru příkazy a zobrazit clusteru uživatelského rozhraní pomocí prohlížeče na vlastní systému. Tento dokument vás provede vytváření SSH tunelem z Linux, OS X a Windows.

>[AZURE.NOTE] Můžete vytvořit relaci SSH s systému řízení obrázku. Však nedoporučujeme to. Pracovat přímo na systém pro správu zpřístupňuje rizikům změny neúmyslné konfigurace.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Vytvoření SSH tunelem na Linux nebo OS X

První věc, když vytvoříte SSH tunelem na Linux nebo OS X je vyhledejte název veřejné DNS pro vyrovnávání zatížení předloh. K tomuto účelu rozbalte skupiny zdrojů tak, aby se zobrazil jednotlivé zdroje. Najděte a vyberte veřejnou IP adresu na předlohu. Otevře se nahoru zásuvné, který obsahuje informace o veřejnou IP adresu, která obsahuje název DNS. Uložte tento název pro pozdější použití. <br />


![Veřejné název DNS](media/pubdns.png)

Nyní otevřete představuje prostředí a spusťte tento příkaz kde:

**Je port koncového bodu, který chcete vystavit.** U roj je to. 2375. Datacentrum/s operačním systémem použijte port 80.  
**Uživatelské jméno** je uživatelské jméno, které při nasazení clusteru.  
**DNSPREFIX** je předponou DNS, které jste zadali při nasazení clusteru.  
Oblast, ve kterém se nachází skupina zdroje je **oblast** .  
**PATH_TO_PRIVATE_KEY** [Volitelné] představuje cestu k soukromý klíč, který odpovídá veřejným klíčem, který jste zadali při vytvoření služba kontejneru obrázku. Tuto možnost použít -i příznak.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Připojení port SSH je 2200 – ne standardní port 22.

## <a name="dcos-tunnel"></a>Tunelem Datacentrum/OS

Otevřete tunelem pro koncové body Datacentrum/OS-týkající se spuštění příkazu, který je podobná této:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Teď máte přístup ke Datacentrum/OS-týkající se koncové body v:

- DATACENTRUM/OPERAČNÍ SYSTÉM:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Podobně se dostanete rest API pro každou aplikaci tento tunelem.

## <a name="swarm-tunnel"></a>Roj tunelem

Tunelem koncový bod roj spustíte provedení příkazu, který bude vypadat podobně jako tento:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Teď můžete nastavit proměnnou prostředí DOCKER_HOST takto. Můžete dál používat rozhraní vašeho Docker příkazového řádku (rozhraní příkazového řádku) jako normálně.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Vytváření SSH tunelem v systému Windows

Existuje několik možností pro vytváření SSH tunelů v systému Windows. Tohoto dokumentu bude popisují, jak používat nátěrové akce.

Stáhněte si nátěrové do systému Windows a spustit aplikaci.

Zadejte název hostitele, který se skládá ze clusteru správce uživatelské jméno a název veřejné DNS první předlohu clusteru. **Název hostitele** bude vypadat takto: `adminuser@PublicDNS`. Zadejte **Port**2200.

![Konfigurace nátěrové 1](media/putty1.png)

Zaškrtněte políčka **SSH** a **ověření**. Přidání souboru privátní klíče pro ověřování.

![Konfigurace nátěrové 2](media/putty2.png)

Vyberte **tunelů** a nakonfigurujte následující předáním porty:
- **Zdrojový Port:** Předvolbách – použití 80 Datacentrum/OS nebo. 2375 pro roj.
- **Cíl:** Použití localhost:80 Datacentrum/OS nebo localhost:2375 pro roj.

Následující příklad nakonfigurovaný pro Datacentrum/OS, ale bude vypadat podobně jako u Docker Swarm.

>[AZURE.NOTE] Port 80 nesmí být se používá při vytváření tento tunelem.

![Konfigurace nátěrové 3](media/putty3.png)

Když jste hotoví, uložte konfigurace připojení a připojit nátěrové relace. Když se připojíte, uvidíte konfiguraci portů v protokolu událostí nátěrové.

![Protokol událostí nátěrové](media/putty4.png)

Pokud máte nastavený tunelem pro Datacentrum/OS, dostanete se k související koncový bod na:

- DATACENTRUM/OPERAČNÍ SYSTÉM:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Když máte nastavený tunelem pro Docker Swarm, můžete přistupovat clusteru roj prostřednictvím Docker rozhraní příkazového řádku. Nejdřív musíte nakonfigurovat proměnnou prostředí Windows s názvem `DOCKER_HOST` s hodnotou ` :2375`.

## <a name="next-steps"></a>Další kroky

Nasazením a správou kontejnery s operačním systémem Datacentrum/nebo roj:

- [Práce se službou Azure kontejner a Datacentrum/OS](container-service-mesos-marathon-rest.md)
- [Práce se službou Azure kontejner a Docker roj](container-service-docker-swarm.md)
