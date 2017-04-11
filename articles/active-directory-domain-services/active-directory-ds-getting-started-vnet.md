<properties
    pageTitle="Azure AD Domain Services: Vytvoření nebo výběr virtuální sítě | Microsoft Azure"
    description="Začínáme s Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Vytvoření nebo výběr virtuální sítě pro službu Azure AD Domain Services

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Pokyny k výběru Azure virtuální sítě
> [AZURE.NOTE] **Než začnete**: Podívejte se do [sítě aspektech pro Azure AD Domain Services](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Úkol 2: Vytvoření Azure virtuální sítě
Je další úkol konfigurace vytvářet Azure virtuální sítě a podsítě v něm obsažené. V síti virtuální povolení služby Azure AD doménu v tomto podsítě. Pokud už máte existující virtuální sítě, který chcete použít, můžete tento krok přeskočit.

> [AZURE.NOTE] Ujistěte se, že Azure virtuální sítě můžete vytvořit nebo vybrat Services Azure AD Domain pomocí patří Azure oblast, která podporuje Azure AD Domain Services. Najdete na stránce [služby Azure podle regionů](https://azure.microsoft.com/regions/#services/) znát Azure oblasti, ve kterých je k dispozici Azure AD Domain Services.

Poznamenejte si název virtuální sítě tak vyberete vpravo virtuální sítě po povolení služby Azure AD domény v kroku další konfiguraci.

Proveďte následující kroky konfigurace vytvořit Azure virtuální síť, ve kterém chcete povolit Azure AD Domain Services.

1. Přejděte na **portál Azure klasické** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Vyberte uzel **sítí** v levém podokně.

    ![Sítě uzel](./media/active-directory-domain-services-getting-started/networks-node.png)

3. V podokně úloh v dolní části stránky klikněte na **Nový** .

    ![Virtuální sítě uzel](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. V uzel **Síťové služby** vyberte **Virtuální sítě**.

5. Klikněte na **Vytvořit** vytvořit virtuální sítě.

    ![Virtuální sítě – rychlé vytvoření](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Zadejte **název** pro virtuální sítě. Můžete taky nakonfigurovat **adresní prostor** nebo **OM maximální počet** této sítě. Nyní můžete ponechat nastavení **serveru DNS** nastavený na hodnotu "žádný. Je možné aktualizovat nastavení po povolení Azure AD Domain služby serveru DNS.

7. Ujistěte se, vyberte podporované Azure oblast v rozevíracím seznamu **umístění** . Najdete na stránce [služby Azure podle regionů](https://azure.microsoft.com/regions/#services/) znát Azure oblasti, ve kterých je k dispozici Azure AD Domain Services.

8. Pokud chcete vytvořit virtuální sítě, klikněte na tlačítko **vytvořit virtuální sítě** .

    ![Vytvořte virtuální sítě pro službu Azure AD domény.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Po vytvoření virtuální sítě vyberte virtuální sítě a klikněte na kartu **KONFIGUROVAT** .

    ![Vytvoření podsítě](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Přejděte do části **virtuální sítě adresu mezery** . Klikněte na tlačítko **Přidat podsítě** a zadat podsítě s názvem **AaddsSubnet**. Klikněte na **Uložit** vytvořte podsítě.

    ![Vytvoření podsítě služby Azure AD domény.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Úkol 3 – povolit Azure AD Domain Services
Další konfigurace úkolu je to [povolíte Azure AD Domain Services](active-directory-ds-getting-started-enableaadds.md).
