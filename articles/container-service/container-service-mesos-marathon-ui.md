<properties
   pageTitle="Azure Správa kontejneru kontejneru služby prostřednictvím webového uživatelského rozhraní | Microsoft Azure"
   description="Nasazení kontejnery ke službě clusteru kontejneru služby Azure pomocí webového Marathon uživatelského rozhraní."
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
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Správa kontejneru prostřednictvím webového uživatelského rozhraní

Datacentrum/OS poskytuje prostředí pro nasazení a stejné měřítko skupinový úloh, při abstracting základním hardwaru. Horní části Datacentrum/s operačním systémem je rámec, který má na starosti plánování a provádění úloh pro využití.

Když rámce nejsou k dispozici pro mnoho oblíbených úloh, tento dokument bude popisují, jak můžete vytvořit a změnit velikost kontejneru nasazení s Marathon. Před pracovních přes tyto příklady, budete potřebovat clusteru Datacentrum/OS nakonfigurovaný služby Azure kontejner. Taky musíte mít vzdáleného připojení k tomuto clusteru. Další informace o těchto položek najdete v následujících článcích:

- [Nasazení služby Azure kontejneru obrázku](container-service-deployment.md)
- [Připojení k obrázku kontejneru služby Azure](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Prozkoumání Datacentrum/operační systém uživatelského rozhraní

S zabezpečené prostředí (SSH) tunelem zřídit přejděte na http://localhost/. Načte web Datacentrum/OS uživatelského rozhraní a s informacemi o obrázku, například použité zdroje, aktivní agentů a spuštění služeb.

![UŽIVATELSKÉ ROZHRANÍ DATACENTRUM/OS](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Prozkoumání Marathon uživatelského rozhraní

Uživatelské rozhraní Marathon zobrazíte procházením http://localhost/Marathon. Na této obrazovce můžete začít kontejneru nové nebo do jiné aplikace clusteru Azure kontejneru služby Datacentrum/operační systém. Najdete v článku informace o tom, jak kontejnerů a aplikací.  

![Marathon uživatelského rozhraní](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Nasazení Docker formátování kontejneru

Abyste mohli nasadit nového kontejneru pomocí Marathon, klikněte na tlačítko **Vytvořit aplikaci** a zadejte tyto informace do formuláře:

Pole           | Hodnota
----------------|-----------
ID              | nginx
Obrázek           | nginx
Sítě         | Přidat do mostu
Host (hostitel) Port       | 80
Protocol (protokol)        | TCP

![Nový v uživatelském rozhraní aplikace – obecné](media/dcos/dcos4.png)

![Nové aplikace uživatelského rozhraní – Docker kontejneru](media/dcos/dcos5.png)

![Nový v uživatelském rozhraní aplikace – porty a protokoly zjištění služby](media/dcos/dcos6.png)

Pokud budete chtít statického mapování port kontejneru porty agent, budete muset použití režimu JSON. K tomu, přepněte do **Režimu JSON** pomocí přepínacího tlačítka Průvodce nové aplikace. Zadejte pod nadpisem `portMappings` část definici aplikace. Tento příklad vytvoří vazbu port 80 kontejneru port 80 agenta Datacentrum/OS. Tento průvodce mimo režim JSON můžete přecházet po provedení této změny.

```none
"hostPort": 80,
```

![Nové aplikace uživatelského rozhraní – příklad port 80](media/dcos/dcos13.png)

Shluk Datacentrum/OS nasazení se sadou soukromé a veřejné agenti. Pro clusteru moct získat přístup k aplikacím z Internetu budete muset nasazení aplikace do veřejné agent. Postup, vyberte kartu **volitelné** Průvodce novou aplikaci a zadejte **slave_public** **Přijaté role zdroje**.

![Nový v uživatelském rozhraní aplikace – veřejné agent nastavení](media/dcos/dcos14.png)

Zpět na hlavní stránce Marathon uvidíte stav nasazení kontejner.

![Hlavní stránky Marathon uživatelského rozhraní – stav nasazení kontejneru](media/dcos/dcos7.png)

Když přepnete zpátky na web Datacentrum/OS uživatelského rozhraní (http://localhost/), zobrazí se, jestli je spuštěný úkolu (v tomto případě kontejneru formátovaných Docker) clusteru Datacentrum/OS.

![Web Datacentrum/OS uživatelského rozhraní – úkolu spuštěné v clusteru](media/dcos/dcos8.png)

Najdete v článku clusteru spuštěný na daný úkol.

![Datacentrum/OS web uživatelského rozhraní – clusteru úkolu](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Změnit velikost vaší kontejnery

Uživatelské rozhraní Marathon můžete zobrazit instance počet hodnot v kontejneru. Postup, přejděte na stránku **Marathon** , vyberte kontejner, který chcete změnit měřítko a klikněte na tlačítko **Měřítko** . V dialogovém okně **Měřítko aplikace** zadejte počet instancí kontejneru, které chcete a vyberte **Aplikaci měřítko**.

![Marathon uživatelského rozhraní – dialogové okno měřítko aplikace](media/dcos/dcos10.png)

Po dokončení operace měřítko, zobrazí se více instancí stejný úkol rozšířit mezi agentů Datacentrum/OS.

![Řídicího panelu Uživatelské rozhraní webu Datacentrum/OS – úloh rozložení přes agenti](media/dcos/dcos11.png)

![Web Datacentrum/OS uživatelského rozhraní – uzlů](media/dcos/dcos12.png)

## <a name="next-steps"></a>Další kroky

- [Práce s Datacentrum/OS a rozhraní API Marathon](container-service-mesos-marathon-rest.md)

Hloubkové postupy pro službu Azure kontejneru s Mesos

> [AZURE. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos VIDEO]]
