<properties
   pageTitle="Odstranění Azure obrázku a jeho zdroje | Microsoft Azure"
   description="Zjistěte, jak úplně odstranit struktury služby clusteru odstranění skupina zdroje obsahující clusteru nebo selektivním vymazáním zdroje."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Odstranění obrázku struktury služby Azure a prostředky, které používá

Služba struktury clusteru je tvořen mnoho dalších Azure zdrojů kromě samotné zdroje obrázku. Tak můžete úplně odstranit clusteru služby struktury bude potřeba odstranit všechny zdroje, které je tvořen.
Máte dvě možnosti: buď odstranit skupinu zdrojů, clusteru je v (které slouží k odstranění zdroje obrázku a dalších zdrojů ve skupině prostředků) nebo konkrétně odstranit zdroje obrázku a máte přidruženou zdroje (ale ne další zdroje ve skupině prostředků).

>[AZURE.NOTE] Odstranění zdroje obrázku **nemá** odstranit všechny další zdroje, které služby struktury clusteru je tvořen.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Odstranění skupině celý zdroj (RG) cluster struktury služby

Toto je nejjednodušší způsob, jak zajistit, že odstraníte všechny zdroje přidružený k obrázku, včetně skupina zdroje. Odstranění skupiny zdrojů pomocí prostředí PowerShell nebo prostřednictvím portálu Azure. Pokud vaše skupina zdroje prostředky, které nesouvisí služby struktury obrázku, můžete odstranit konkrétní zdroje.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Odstranění skupiny zdrojů pomocí prostředí PowerShell Azure

Skupina zdroje můžete taky odstranit spuštěním těchto rutin prostředí PowerShell Azure. Ujistěte se PowerShell 1.0 Azure nebo větší nainstalovaný v počítači. Pokud jste to před neudělali, postupujte podle kroků uvedených v [instalace a konfigurace prostředí PowerShell Azure.](../powershell-install-configure.md)

Otevřete okno prostředí PowerShell a spusťte tyto rutiny PS:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Zobrazí se výzva k potvrzení odstranění, pokud jste nepoužili *– vyšší* možnost. V potvrzovacím RG a všechny zdroje, které obsahuje odstraněny.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Odstranění skupiny zdroje na portálu Azure  

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Přejděte do služby struktury obrázku, který chcete odstranit.
3. Klikněte na název pole Skupina zdroje na stránce essentials obrázku.
4. Tím se vyvolá stránce **Essentials skupina zdroje** .
5. Klikněte na **Odstranit**.
6. Postupujte podle pokynů na této stránce dokončete odstranění skupina zdroje.

![Odstranění pole Skupina zdroje][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Odstranění zdroje obrázku a prostředky, které používá, ale ne další zdroje ve skupině zdroje

Pokud vaše skupina zdroje obsahuje pouze zdroje, které se vztahují k služby struktury obrázku, který chcete odstranit, je jednodušší odstranit skupinu celý zdroj. Pokud chcete odstranit selektivním zdroje jeden po druhém ve skupině prostředků, postupujte takto:

Nasazení svůj cluster na portálu nebo pomocí jedné ze šablon Správce služeb struktury zdrojů z Galerie šablon na všechny zdroje, které používá clusteru označení se následující dvě čísla. Je můžete se rozhodnout, které zdroje, který chcete odstranit.

***Značku #1:*** Klíč = název_clusteru, hodnota = "název clusteru"

***Značku #2:*** Klíč = resourceName, hodnota = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Odstranění určité zdroje v portálu Azure

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Přejděte do služby struktury obrázku, který chcete odstranit.
3. Přejděte na **všechna nastavení** na zásuvné essentials.
4. Klikněte na **značky** v části **Správa zdrojů** v zásuvné nastavení.
5. Klikněte na jednu **značky** na zásuvné značky zobrazíte seznam všech zdrojů s značka zmíněná.

    ![Značky zdroje][ResourceTags]

6. Až budete mít seznam příznakem zdrojů, klikněte na všechny zdroje a odstraňte je.

    ![Označení zdrojů][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Odstranění zdroje pomocí prostředí PowerShell Azure

Odstranění zdroje jeden po druhém spuštěním těchto rutin prostředí PowerShell Azure. Ujistěte se PowerShell 1.0 Azure nebo větší nainstalovaný v počítači. Pokud jste to před neudělali, postupujte podle kroků uvedených v [instalace a konfigurace prostředí PowerShell Azure.](../powershell-install-configure.md)

Otevřete okno prostředí PowerShell a spusťte tyto rutiny PS:

```powershell
Login-AzureRmAccount
```
Pro všechny zdroje chcete odstranit, spusťte následující:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Odstranění zdroje obrázku, spusťte následující:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Další kroky
Přečtěte si následující taky Další informace o upgradu clusteru a rozdělování služby:

- [Další informace o upgrade obrázku](service-fabric-cluster-upgrade.md)
- [Další informace o rozdělení stavové služby pro maximální měřítko](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
