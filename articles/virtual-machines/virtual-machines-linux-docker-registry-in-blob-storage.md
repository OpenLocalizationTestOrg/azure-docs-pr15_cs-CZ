<properties 
  pageTitle="Nasazení vlastní soukromé Docker registru na Azure | Microsoft Azure"
  description="Popisuje, jak můžete Docker registru hostovat kontejneru obrázků ve službě úložišti objektů Blob Azure."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Nasazení vlastní soukromé Docker registru na Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



Tento dokument popisuje co Docker soukromé registru je a ukazuje, jak nasazením obrázek kontejneru Docker registru 2.0 Docker soukromé registru na Microsoft Azure pomocí úložiště objektů Blob Azure.

Tento dokument předpokládá:

1. Víte, jak pomocí Docker a Docker obrázky, které chcete uložit. (Není? [Další informace o Docker](https://www.docker.com))
2. Máte serveru, který má Docker engine nainstalovaný. (Není? [To rychle udělat v Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Co je soukromé registru Docker?

K dodávat kontejnerizovaná aplikace do cloudu, vytvořte obrázek kontejneru Docker a uložit jinam, postupujte tak, aby lze jej využít podle sebe a ostatní uživatelé. 

Při vytváření kontejneru obrázek a dodávky do cloudu snadno, je výzvu k uložení generovaného obrázku problémy se spolehlivým. Z tohoto důvodu Docker nabízí centralizované službu s názvem [Docker centrální] [ docker-hub] pro ukládání kontejneru obrázky v cloudu a umožňuje vytvořit kontejnery kdykoli pomocí těmito obrázky.

I když [Docker centrální] [ docker-hub] placené služby pro ukládání soukromé aplikace kontejneru obrázky, Docker ohledem vývojáři potřeb a poskytuje otevřít zdroj nástrojů pro ukládání obrázků ve vlastní soukromé registru Docker za bránu firewall nebo místních bez zasažení veřejné Internet.
Protože snadno zabezpečené úložiště objektů Blob Azure se můžete rychle vytvářet a používat soukromé registru Docker v Azure, které sami určit.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Proč jste hostitelem Docker registru na Azure?

Hostitelské instance Docker registru na Microsoft Azure a ukládání obrázků v úložišti objektů Blob Azure, máte několik výhod:

**Zabezpečení:** Obrázky Docker nenechávejte Azure datacentrech tak, aby se přes veřejnou Internet jako byste používali Docker centrální.
  
**Výkonu:** Obrázky Docker jsou uložené ve stejné datacentra nebo oblasti jako aplikace. To znamená, že na některých obrázcích bude doplněné rychleji a další problémy se spolehlivým ve srovnání s Docker centrální.

**Spolehlivost:** Pomocí úložiště objektů Blob Microsoft Azure můžete vytvářet, používat mnoho vlastnosti úložiště jako dostupnost, redundance premium úložiště (disky SSD) a tak dál.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Konfigurace Docker registru můžete úložišti objektů Blob Azure

(Doporučujeme přečíst [si přečtěte následující dokumentaci Docker registru 2.0][registru dokumenty] před pokračováním.)

Je možné [Konfigurovat] [ registry-config] registru Docker dvěma různými způsoby.
Můžeš:

1. Použití `config.yml` soubor. V tomto případě je bude potřeba vytvořit samostatný obraz Docker nad `registry` obrázek.
2. Přepsat konfiguračního souboru výchozí prostřednictvím proměnné: získá úkolech bez vytváření a udržování samostatné Docker obrázek.

Za účelem zjednodušení spočívá v tomto tématu možnost 2, pomocí proměnných prostředí.

Chcete-li spustit Docker registru instance, který:

* použití účtu úložiště Azure pro ukládání obrázků
* sleduje virtuální počítač port 5000
* má ověřování nakonfigurované (nedoporučuje se, viz poznámku dole)

potřebujete spusťte tento příkaz Docker v terminálu flám (nahradit `<storage-account>` a `<storage-key>` pomocí svých přihlašovacích údajů):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Jakmile příkazu zavře, uvidíte kontejneru hostingu soukromé instance Docker registru spuštěním `docker ps` příkazu na hostitele:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Konfigurace zabezpečení pro registru Docker není uvedené v tomto dokumentu a registru bude přístupný pro kohokoli, bez ověřování ve výchozím nastavení, pokud otevřete port registru portu endpoint virtuálního počítače nebo služba Vyrovnávání zatížení, pokud použijete příkaz nasazení.
>
> Přečtěte si [Konfigurace registru Docker] [ registry-config] si přečtěte následující dokumentaci se dozvíte, jak zajistit instance registru a obrázky.

## <a name="next-steps"></a>Další kroky

Až budete mít nastavení registru, je potřeba přejít použít některé další. Začněte s docker [registru dokumenty]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[dokumenty registru]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
