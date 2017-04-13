<properties 
    pageTitle="Použití Azure portál k nasazení Azure zdroje | Microsoft Azure" 
    description="Umožňuje Azure portálem a spravovat zdroje Azure nasazení zdroje." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Nasazení zdroje se šablonami správce prostředků a Azure portálu

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-template-deploy.md)
- [Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [ROZHRANÍ REST API](resource-group-template-deploy-rest.md)

Toto téma ukazuje, jak pomocí služby [Azure portálu](https://portal.azure.com) [Správce prostředků Azure](azure-resource-manager/resource-group-overview.md) nasazení Azure zdroje. Další informace o správě svých prostředcích, najdete v článku [Správa Azure zdrojů prostřednictvím portálu](./azure-portal/resource-group-portal.md).

V současné době podporuje některé služba portál nebo správce prostředků. U těchto služeb budete muset pomocí [klasické portálu](https://manage.windowsazure.com). Stav každé služby najdete v článku [Azure portálu dostupnost grafu](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Vytvoření pole Skupina zdroje

1. Pokud chcete vytvořit skupinu prázdných prostředků, vyberte **Nový** > **správy** > **Pole Skupina zdroje**.

    ![Vytvoření prázdného zdroje skupiny](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Zadejte jeho název a umístění a v případě potřeby vyberte předplatné. Potřebujete poskytuje místo, kde skupina zdroje, protože skupina zdroje ukládá metadata o zdroji. Dodržování předpisů důvodů můžete určit, kde je uložena že metadata. Obecně doporučujeme, abyste zadejte umístění, kde se bude nacházet většina prostředků. Pomocí na stejném místě, můžete zjednodušit vaši šablonu.

    ![nastavení skupiny hodnot](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Nasazení zdrojů z Marketplace

Po vytvoření skupiny zdrojů nástroje můžete nasazovat prostředky k němu ze Marketplace. Tržiště obsahuje předdefinovaná řešení pro běžné scénáře.

1. Spusťte na nasazení, vyberte **Nový** a zadejte zdroj, který chcete nasadit. Vyhledejte konkrétní verzi zdroj, který chcete nasadit.

    ![nasazení zdroje](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Pokud nevidíte konkrétní řešení, které chcete nasadit, můžete hledat Tržiště ho.

    ![hledání marketplace](./media/resource-group-template-deploy-portal/search-resource.png)

3. V závislosti na typu vybraný zdroj máte kolekci vlastnosti důležité nastavit před nasazením. Tyto možnosti nejsou zobrazené, jak budou lišit v závislosti na typu zdroje. Pro všechny typy musíte vybrat cílové skupině zdroje. Následující obrázek ukazuje, jak vytvořit web appu a nasadit na skupina zdroje, který jste vytvořili.

    ![Vytvoření pole Skupina zdroje](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Můžete taky můžete vytvořit skupinu zdrojů při nasazení zdroje. Vyberte **vytvořit nový** a zadejte název skupiny zdrojů.

    ![Vytvoření nové skupiny prostředků](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Nasazení, nebude zahájen. Nasazení může trvat několik minut. Při nasazení dokončí, zobrazí oznámení.

    ![Zobrazit oznámení](./media/resource-group-template-deploy-portal/view-notification.png)

5. Po zavedení zdroje, můžete přidat další materiály ke skupině zdrojů pomocí příkazu **Přidat** na zásuvné skupina zdroje.

    ![Přidání zdrojů](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Nasazení zdrojů z vlastní šablony

Pokud chcete provést nasazení, ale ne vyberte některou ze šablon Tržiště, můžete vytvořit vlastní šablonu, která definuje infrastrukturu pro řešení. Další informace o vytváření šablon najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).

1. Chcete-li nasadit vlastní šablonu prostřednictvím portálu, vyberte **Nový**a vyhledáte **Nasazení šablony** , dokud můžete vybírat z možností.

    ![nasazení šablony hledání](./media/resource-group-template-deploy-portal/search-template.png)

2. Vyberte **Šablonu nasazení** dostupné zdroje.

    ![Vyberte šablonu nasazení](./media/resource-group-template-deploy-portal/select-template.png)

3. Po spuštění nasazení šablony, otevřete prázdnou šablonu, která je k dispozici pro přizpůsobení.

    ![Vytvoření šablony](./media/resource-group-template-deploy-portal/show-custom-template.png)

    V editoru přidejte syntaxi JSON, která definuje prostředky, které chcete nasadit. Zaškrtněte políčko **Uložit** po dokončení. Psaní syntaxe JSON, najdete v článku [Správce prostředků šablony návodu](resource-manager-template-walkthrough.md).

    ![Úprava šablony](./media/resource-group-template-deploy-portal/edit-template.png)

4. Nebo můžete vybrat existující šablony z [Azure rychlý úvod šablony](https://azure.microsoft.com/documentation/templates/). Tyto šablony jsou poskytnutý komunity. Průvodní mnoho obvyklé scénáře a někdo přidal šablonu, která je podobná co chcete nasadit. Je možné hledat šablony na něco hledáte, který odpovídá nefunguje.

    ![Vyberte šablonu rychlý úvod](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    Vybrané šablony můžete zobrazit v editoru.

5. Po zadání všech hodnot, vyberte možnost **vytvořit** zavést šablonu. 

    ![nasazení šablony](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Nasazení zdroje ze šablony uložené ke svému účtu

Na portále k uložení šablony k účtu Azure a přeinstalujte později. Další informace o práci s těmito dokumenty uložené šablony [Začínáme se šablonami soukromé na portálu Azure](./marketplace-consumer/mytemplates-getstarted.md).

1. Vyhledání uložené šablony, vyberte možnost **Vyhledat** > **šablony**.

    ![procházení šablon](./media/resource-group-template-deploy-portal/browse-templates.png)

2. V seznamu šablony uložené ke svému účtu vyberte tu, kterou chcete pracovat.

    ![uložené šablony](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Vyberte **instalovat** na přeinstalujte uloženou šablonu.

    ![nasazení uložené šablony](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Další kroky

- Zobrazení protokolů auditování, najdete v článku [auditování operace s správce prostředků](resource-group-audit.md).
- Poradce při potížích nasazení, najdete v článku [nasazení skupina zdroje Poradce při potížích s Azure portálu](resource-manager-troubleshoot-deployments-portal.md).
- Načtení šablony ze nasazení nebo skupina zdroje, najdete v článku [Export správce prostředků Azure šablony z existujících zdrojů](resource-manager-export-template.md).





