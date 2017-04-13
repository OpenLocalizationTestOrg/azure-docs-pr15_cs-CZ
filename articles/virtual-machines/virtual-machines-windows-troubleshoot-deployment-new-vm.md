<properties
   pageTitle="Poradce při potížích s Windows OM nasazení SV | Microsoft Azure"
   description="Řešení potíží nasazení Správce prostředků při vytváření nové virtuálního počítače Windows v Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Poradce při potížích nasazení Správce prostředků vytvořit nový počítač virtuální Windows Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Protokoly shromažďovat auditování

Poradce při potížích s zahájíte shromáždění protokolů auditování k identifikaci přiřazenou problém chybu. Následující odkazy obsahují podrobné informace o postupu můžete sledovat.

[Poradce při potížích s nasazení skupina zdroje s portálem Azure](../resource-manager-troubleshoot-deployments-portal.md)

[Auditování operace s správce prostředků](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Pokud operační systém Windows generalized a je nahráli nebo zachyceny s generalized nastavení, pak nebude možné všechny chyby. Podobně pokud je operační systém Windows specializovaný a je nahráli nebo zachyceny s specializované nastavení a pak nebudou některé chyby.

**Nahrajte chyby:**

**N<sup>1</sup>:** Pokud je operační systém Windows generalized a uložit jako specializovaný, zobrazí se zřizovací chyby vypršení časového limitu s OM zarazí na obrazovce počáteční nastavení počítače.

**N<sup>2</sup>:** Pokud operační systém Windows specializované a nahráním jako generalized, zobrazí se vám zřizovací chybou s OM zarazí na obrazovce počáteční nastavení počítače, protože nové OM pracuje s původní název počítače, uživatelské jméno a heslo.

**Rozlišení**

Řešení obou těchto chyb, můžete [Přidat AzureRmVhd nahrát původní virtuálního pevného disku](https://msdn.microsoft.com/library/mt603554.aspx), k dispozici v místním prostředí, stejné nastavení jako pro OS (generalized/specializované). Pokud chcete odeslat jako generalized, pamatujte na spuštění sysprep nejdřív.

**Zachycení chyb:**

**N<sup>3</sup>:** Pokud je operační systém Windows generalized a zaznamenávání jako specializovaný, zobrazí se zřizovací chyby vypršení časového limitu protože původní OM není použitelné, jako je označen jako generalized.

**N<sup>4</sup>:** Pokud operační systém Windows specializované a se zaznamenává jako generalized, zobrazí se vám zřizovací chybou protože nové OM pracuje s původní název počítače, uživatelské jméno a heslo. Navíc původní OM není použitelné vzhledem k tomu, že je označený jako specializované.

**Rozlišení**

Řešení obou těchto chyb, odstraňte aktuální obrázek z portálu a [znovu ji z aktuální VHD zachytit](virtual-machines-windows-vhd-copy.md) stejné nastavení jako pro OS (generalized/specializované).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Problém: Vlastní/galerie/marketplace obrázek; Chyba při rozdělení
Tato chyba vznikl v situacích při nová žádost o OM se Připne obrázku, který nepodporuje velikost OM požadovány nebo nemá volného místa tak, aby zahrnoval žádost.

**Způsobit 1:** Clusteru nepodporuje na požadovanou velikost OM.

**Řešení 1:**

- Opakování žádosti o pomocí zmenšené OM.
- Pokud nemůžete změnit velikost požadované OM:
  - Ukončete všechny VMs množiny dostupnosti.
  Klikněte na **skupiny zdrojů** > *Skupina zdroje* > **zdroje** > *nastavení vaší dostupnosti* > **virtuálních počítačích** > *virtuálního počítače* > **Zastavit**.
  - Po všechny VMs ukončíte vytvořte nové OM do požadované velikosti.
  - Zahájení nového OM nejdřív výběru jednotlivých přestal VMs a klikněte na tlačítko **Start**.

**Způsobit 2:** Clusteru nemá bezplatné zdroje informací.

**Řešení 2:**

- Opakování žádosti později.
- Pokud nové OM může být součást sady různých dostupnosti
  - Vytvoření nového OM v různých dostupnost nastavit (ve stejné oblasti).
  - Přidání nového OM do stejné virtuální sítě.

## <a name="next-steps"></a>Další kroky
Pokud při používání narazíte na problémy při spuštění přestal OM Windows nebo změna velikosti existující OM Windows v Azure, přečtěte si téma [Poradce při potížích s správce prostředků nasazení se restartováním nebo změna velikosti existující Windows virtuálního počítače v Azure](virtual-machines-windows-restart-resize-error-troubleshooting.md).
