<properties
   pageTitle="OM restartování nebo změna velikosti problémy | Microsoft Azure"
   description="Poradce při potížích klasické nasazení se restartováním nebo změna velikosti existující Linux virtuálního počítače v Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Poradce při potížích klasické nasazení se restartováním nebo změna velikosti existující Linux virtuálního počítače v Azure

> [AZURE.SELECTOR]
- [Klasický](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Správce prostředků](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Při pokusu o spuštění virtuálního počítače pro přestal Azure (OM) nebo změna velikosti existující OM Azure je běžné chyby, ke kterým dojít k chybě přidělení. K této chybě dochází při obrázku nebo oblasti, které neobsahují prostředky k dispozici nebo nepodporuje na požadovanou velikost OM.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Protokoly shromažďovat auditování

Poradce při potížích s zahájíte shromáždění protokolů auditování k identifikaci přiřazenou problém chybu.

Na portálu Azure klikněte na tlačítko **Procházet** > **virtuálních počítačích** > _virtuálního počítače Linux_ > **Nastavení** > **protokolů auditování**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problém: Chyba při otevírání přestal OM

Pokusu o spuštění přestal OM, ale dostáváte nezdařené rozdělení.

### <a name="cause"></a>Příčina

Žádost o přestal OM musí být pokus o na původní obrázku, který je hostitelem služby cloudu. Clusteru nemá volného místa ke splnění tohoto požadavku.

### <a name="resolution"></a>Rozlišení

* Vytvoření nového Cloudová služba a přidružit buď oblasti nebo založené na oblasti virtuální síť, ale ne skupinu spřažení.

* Odstranění přestal OM.

* Znova vytvořte OM v nové cloudové službě pomocí discích.

* Spusťte znova vytvořené OM.

Pokud dojde k chybě při pokusu o vytvoření nové cloudové služby, opakujte později nebo změňte oblast cloudové služby.

> [AZURE.IMPORTANT] Nové cloudové služby bude mít nový název a VIP, takže budete muset změnit tyto informace pro všechny závislosti využívající tyto informace k existující cloudové služby.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problém: Chyba při změně velikosti existující OM

Zkusíte změnit velikost existující OM, ale dostáváte nezdařené rozdělení.

### <a name="cause"></a>Příčina

Žádost o změnit velikost OM musí být pokus o na původní obrázku, který je hostitelem cloudové služby. Clusteru nepodporuje na požadovanou velikost OM.

### <a name="resolution"></a>Rozlišení

Omezit na požadovanou velikost OM a opakování žádosti o změnu velikosti.

* Klikněte na tlačítko **Procházet vše** > **virtuálních počítačích (klasické)** > _virtuálního počítače_ > **Nastavení** > **velikost**. Podrobný postup najdete v článku [Změna velikosti virtuální počítač](https://msdn.microsoft.com/library/dn168976.aspx).

Pokud není možné zmenšit velikost OM, postupujte takto:

  * Vytvoření nového cloudové služby, zajištění nejsou spojeny do skupiny spřažení a není přidružený virtuální síti, která je propojená s spřažení skupiny.

  * Vytvoření nového, větší velikost OM v něm.

Je možné slučovat všechny VMs ve stejném cloudové služby. Pokud existující cloudové služby je přidružená k síti virtuální založené na oblasti, můžete se připojit nové cloudové služby do stávajícího virtuální sítě.

Pokud cloudovou službu existující není přidružený k síti virtuální založené na oblasti, budete muset odstranit VMs v existující cloudové službě a znovu je vytvořte v nové cloudovou službu z jejich discích. Je ale důležité pamatovat, že nové cloudové služby bude mít nový název a VIP, takže byste potřebovali aktualizovat pro všechny závislosti, které aktuálně tyto informace použít k existující cloudové služby.

## <a name="next-steps"></a>Další kroky

Pokud při používání narazíte na problémy při vytváření nové OM Linux v Azure, přečtěte si článek [Poradce při potížích nasazení vytvořit nový virtuální počítač Linux v Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
