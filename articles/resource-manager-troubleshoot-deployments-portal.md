<properties
   pageTitle="Zobrazit nasazení operace s portálem | Microsoft Azure"
   description="Popisuje, jak používat portál Azure zjišťování chyb z nasazení Správce prostředků."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Zobrazit nasazení operace s portálem Azure

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [Prostředí PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure rozhraní příkazového řádku](resource-manager-troubleshoot-deployments-cli.md)
- [ROZHRANÍ REST API](resource-manager-troubleshoot-deployments-rest.md)

Můžete zobrazit operace nasazení prostřednictvím portálu Azure. Můžete nejdůležitější zobrazení operací, když jste přijali chybu při nasazení tak tento článek se zaměřuje na prohlížení operacích, které se nezdařil. Na portálu poskytuje rozhraní, který umožňuje snadno najít chyby a určit potenciální opravy.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Poradce při potížích s pomocí operace nasazení

Operace nasazení zobrazíte pomocí následujících kroků:

1. Skupina zdroje zahrnuté do nasazení Všimněte si stav poslední nasazení. Můžete Tenhle stav vyberete získat další informace.

    ![Stav nasazení](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Zobrazí se historii nedávných nasazení. Vyberte nasazení, které se nezdařila.

    ![Stav nasazení](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Výběr **se nezdařil. Kliknutím sem zobrazíte podrobnosti** zobrazíte popis Proč nasazení selhalo. Na následujícím obrázku je záznam DNS jedinečný.  

    ![zobrazení selhání nasazení](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Tato chybová zpráva by měl být stačit zahájíte Poradce při potížích. Pokud budete potřebovat další informace o úkoly, které byly dokončeny, operací můžete zobrazit, jak je vidět v následujících krocích.

4. Zobrazit všechny operace nasazení v zásuvné **nasazení** . Vyberte všechny operace zobrazíte více podrobností.

    ![Zobrazit operace](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    V tomto případě se zobrazí, aby byly úspěšně vytvořeny účtu úložiště, virtuální sítě a dostupnosti nastavení. Na veřejnou IP adresu se nepodařilo, a další zdroje nebyly pokus o doručení.

5. Událostí pro nasazení můžete zobrazit tak, že vyberete **události**.

    ![zobrazení události](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Zobrazení všech událostí pro nasazení a vyberte některý další podrobnosti.

    ![Zobrazí se události](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Použití protokolů auditování řešit problémy s

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Zobrazit chyby nasazení, použijte podle těchto kroků:

1. Zobrazení protokolů auditování pro skupinu prostředků tak, že vyberete **Protokolů auditování**.

    ![Vyberte protokolů auditování](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. V zásuvné **Protokolů auditování** zobrazí souhrn poslední operace pro všechny skupiny zdrojů ve vašem předplatném. Zahrnuje grafické znázornění času a stavu operace i seznam operací.

    ![Zobrazit akce](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Můžete filtrovat zobrazení protokolů auditování zaměření na určité podmínky. Vyberte **Filtr** v horní části zásuvné **protokolů auditování** .

    ![protokoly filtru](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Z zásuvné **Filtr** vyberte podmínky chcete omezit zobrazení protokolů auditování na pouze operace, které chcete zobrazit. Například můžete filtrovat operace se zobrazí jenom chyby skupina zdroje.

    ![Nastavení možností filtru](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Operace můžete dál filtrovat podle nastavení časového rozsahu. Na následujícím obrázku filtry zobrazení určitého časového rozpětí 15minutový 20.

    ![nastavit čas](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Můžete vybrat všechny operace v seznamu. Vyberte operaci, která obsahuje chybu, kterou chcete prozkoumat.

    ![Vyberte operace](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Zobrazí se všechny události pro tuto operaci. Všimněte si **ID korelace** v podokně Souhrn. Toto ID slouží ke sledování událostí souvisejících. Může být užitečné při práci se na technickou podporu k řešení problému. Můžete vybrat některou událost, zobrazí podrobnosti o události.

    ![Klikněte na událost](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Zobrazí se podrobnosti o události. Informace o chybě zejména věnujte pozornost **Vlastnosti** .

    ![Zobrazit podrobnosti protokolu auditování](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Filtr, který jste použili k protokolu auditování se zachovají při příštím zobrazení, takže budete muset tyto hodnoty rozšířit zobrazení operací změnit.

## <a name="next-steps"></a>Další kroky

- Pokud potřebujete pomoc s řešením chyby konkrétní nasazení najdete v článku [řešení běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md).
- Další informace o používání protokolů auditování ke sledování jiných typů akce, najdete v článku [auditování operace s správce prostředků](resource-group-audit.md).
- Ověřte nasazení před ho spustíte, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy.md).
