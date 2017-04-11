<properties
   pageTitle="Vytvoření serveru služby Analysis Services v Azure | Microsoft Azure"
   description="Naučte se vytvořit instance serveru služby Analysis Services v Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Vytvoření serveru služby Analysis Services
Tento článek vás provede vytvoření nového prostředku serveru služby Analysis Services v Azure předplatné.

## <a name="before-you-begin"></a>Než začnete
Začněte tím, budete potřebovat:

- **Azure předplatné**: navštivte web [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) si vytvořit účet.
- **Pole Skupina zdroje**: použití pole Skupina zdroje už máte nebo [vytvořte nový účet](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Vytvoření serveru služby Analysis Services mohou mít za následek novou fakturaci službu. Další informace najdete v tématu ceny služby Analysis Services.

## <a name="create-an-analysis-services-server"></a>Vytvoření serveru služby Analysis Services

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. Klikněte na **+ Nový** > **Intelligence + analýzy** > **Služby Analysis Services**.

3. V zásuvné **Služby Analysis Services** vyplňte požadovaná pole a stiskněte klávesu **vytvořit**.

    ![Vytvoření serveru](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Název serveru**: Zadejte jedinečný název, který odkazuje serveru.

    - **Předplatné**: vyberte tento server bills pro předplatné.

    - **Pole Skupina zdroje**: Jedná se o kontejnery navržen tak, aby vám pomůžou se správou kolekce Azure prostředků. Další informace najdete v tématu [skupiny zdrojů](../resource-group-overview.md).

    - **Umístění**: Tato Azure datacentra umístění hostitelem serveru. Vyberte umístění nejbližší největší základním uživatele.

    - **Ocenění osy**: Vyberte ceny osy. Tabulkové modely až 100 GB jsou podporovány. Ceny osy můžete kdykoli změnit později.

4. Klikněte na **vytvořit**.

Vytvoření zabere za minutu; často pár sekund. Pokud vybraná **Přidat k portálu**přejděte na portálu zobrazíte nového serveru. Nebo přejděte na **Další služby** > **Služby Analysis Services** Pokud váš server je připravená. Pokud se nezobrazuje, aktualizujte seznam.

 ![Řídicí panel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Další kroky
Po vytvoření serveru, můžete [nasazení modelu](analysis-services-deploy.md) ho pomocí SSDT nebo s SSMS.

Pokud model, který nasazení serveru připojuje k místním zdrojům dat, musíte nainstalovat [Brána místních dat](analysis-services-gateway.md) do počítače v síti.
