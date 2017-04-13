<properties
   pageTitle="Vytvoření plně kvalifikovaný název domény pro virtuálního počítače Azure portálu | Microsoft Azure"
   description="Naučte se vytvářet plně kvalifikovaný název domény nebo plně kvalifikovaný název domény pro odpovědi zdroje na základě virtuálního počítače na portálu Azure."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Vytvoření plně kvalifikovaný název domény na portálu Azure
Při vytváření virtuálních počítače (OM) [Azure portál](https://portal.azure.com) pomocí nasazení modelu správce prostředků, vytvoří se automaticky veřejné zdroje IP virtuálního počítače. Použijte tuto IP adresu vzdálený přístup k OM. I když portálu nevytvoří na [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, ve výchozím nastavení, můžete ho vytvořit po vytvoření OM. Tento článek uvádí postup vytvoření název DNS nebo plně kvalifikovaný název domény.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Můžete se teď může připojit vzdáleně angličtině pomocí tohoto názvu DNS jako s `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## <a name="next-steps"></a>Další kroky
Nyní má vaše OM veřejnou IP a DNS název a nasazením běžné rámců aplikací nebo služeb, jako je nginx, MongoDB, Docker, atd.

Můžete taky Další informace o [použití Správce prostředků](../azure-resource-manager/resource-group-overview.md) pro tipy pro vytváření Azure nasazení.