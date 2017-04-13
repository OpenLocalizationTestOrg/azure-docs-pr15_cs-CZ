<properties
   pageTitle="OM restartování nebo změna velikosti problémy | Microsoft Azure"
   description="Poradce při potížích nasazení Správce prostředků s restartováním nebo změna velikosti existující Windows virtuálního počítače v Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-windows"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-windows"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Poradce při potížích nasazení Správce prostředků s restartováním nebo změna velikosti existující Windows virtuálního počítače v Azure

Při pokusu o spuštění virtuálního počítače pro přestal Azure (OM) nebo změna velikosti existující OM Azure je běžné chyby, ke kterým dojít k chybě přidělení. K této chybě dochází při obrázku nebo oblasti, které neobsahují prostředky k dispozici nebo nepodporuje na požadovanou velikost OM.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Protokoly shromažďovat auditování

Poradce při potížích s zahájíte shromáždění protokolů auditování k identifikaci přiřazenou problém chybu. Následující odkazy obsahují podrobné informace o procesu:

[Poradce při potížích nasazení skupina zdroje s portálem Azure](../resource-manager-troubleshoot-deployments-portal.md)

[Auditování operace s správce prostředků](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problém: Chyba při otevírání přestal OM

Pokusu o spuštění přestal OM, ale dostáváte nezdařené rozdělení.

### <a name="cause"></a>Příčina

Žádost o přestal OM musí být pokus o na původní obrázku, který je hostitelem služby cloudu. Clusteru nemá volného místa ke splnění tohoto požadavku.

### <a name="resolution"></a>Rozlišení

*   Ukončení všechny VMs v sadě dostupnost a restartujte každý OM.

  1. Klikněte na **skupiny zdrojů** > _Skupina zdroje_ > **zdroje** > _nastavení vaší dostupnosti_ > **virtuálních počítačích** > _virtuálního počítače_ > **Zastavit**.

  2. Až všechny VMs ukončíte výběru jednotlivých přestal VMs a klikněte na tlačítko Start.

*   Opakování žádosti o restart později.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problém: Chyba při změně velikosti existující OM

Zkusíte změnit velikost existující OM, ale dostáváte nezdařené rozdělení.

### <a name="cause"></a>Příčina

Žádost o změnit velikost OM musí být pokus o na původní obrázku, který je hostitelem cloudové služby. Clusteru nepodporuje na požadovanou velikost OM.

### <a name="resolution"></a>Rozlišení

* Opakování žádosti o pomocí zmenšené OM.

* Pokud nemůžete změnit velikost požadované OM:

  1. Ukončete všechny VMs množiny dostupnosti.

    * Klikněte na **skupiny zdrojů** > _Skupina zdroje_ > **zdroje** > _nastavení vaší dostupnosti_ > **virtuálních počítačích** > _virtuálního počítače_ > **Zastavit**.

  2. Po všechny VMs ukončíte velikost OM požadované velikosti.
  3. Vyberte změněnou velikostí OM a klikněte na tlačítko **Start**a pak začněte všech přestal VMs.

## <a name="next-steps"></a>Další kroky

Pokud při používání narazíte na problémy při vytváření nové OM Windows v Azure, přečtěte si článek [Poradce při potížích nasazení vytvořit nový virtuální počítač Windows v Azure](../virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md).
