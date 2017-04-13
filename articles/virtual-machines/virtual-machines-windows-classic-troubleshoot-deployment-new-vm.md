<properties
   pageTitle="Poradce při potížích s Windows OM nasazení – klasické | Microsoft Azure"
   description="Když vytvoříte nový počítač virtuální Windows v Azure problémů klasické nasazení"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Poradce při potížích klasické nasazení vytvořit nový počítač virtuální Windows Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Protokoly shromažďovat auditování

Poradce při potížích s zahájíte shromáždění protokolů auditování k identifikaci přiřazenou problém chybu.

Na portálu Azure klikněte na tlačítko **Procházet** > **virtuálních počítačích** > *počítači se systémem Windows virtuální* > **Nastavení** > **protokolů auditování**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Pokud operační systém Windows generalized a je nahráli nebo zachyceny s generalized nastavení, pak nebude možné všechny chyby. Podobně pokud je operační systém Windows specializovaný a je nahráli nebo zachyceny s specializované nastavení a pak nebudou některé chyby.

**Nahrajte chyby:**

**N<sup>1</sup>:** Pokud je operační systém Windows generalized a uložit jako specializovaný, zobrazí se zřizovací chyby vypršení časového limitu s OM zarazí na obrazovce počáteční nastavení počítače.

**N<sup>2</sup>:** Pokud operační systém Windows specializované a nahráním jako generalized, zobrazí se vám zřizovací chybou s OM zarazí na obrazovce počáteční nastavení počítače, protože nové OM pracuje s původní název počítače, uživatelské jméno a heslo.

**Řešení:**

Řešení obou těchto chyb, nahrajte původního souboru dostupné místní, stejné nastavení jako pro OS (generalized/specializované). Pokud chcete odeslat jako generalized, pamatujte na spuštění sysprep nejdřív. Další informace naleznete v tématu [Vytvoření a odeslání virtuálního pevného disku serveru Windows na Azure](virtual-machines-windows-classic-createupload-vhd.md) .

**Zachycení chyb:**

**N<sup>3</sup>:** Pokud je operační systém Windows generalized a zaznamenávání jako specializovaný, zobrazí se zřizovací chyby vypršení časového limitu protože původní OM není použitelné, jako je označen jako generalized.

**N<sup>4</sup>:** Pokud operační systém Windows specializované a se zaznamenává jako generalized, zobrazí se vám zřizovací chybou protože nové OM pracuje s původní název počítače, uživatelské jméno a heslo. Navíc původní OM není použitelné vzhledem k tomu, že je označený jako specializované.

**Řešení:**

Řešení obou těchto chyb, odstraňte aktuální obrázek z portálu a [znovu ji z aktuální VHD zachytit](virtual-machines-windows-classic-capture-image.md) stejné nastavení jako pro OS (generalized/specializované).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problém: Vlastní / galerie / obrázek marketplace; Chyba při rozdělení
Tato chyba nastane v situacích po odeslání nová žádost o OM clusteru, který nemá volného místa tak, aby zahrnoval žádost nebo nepodporuje velikost OM požadovány. Není možné kombinovat různých řadu VMs ve stejném cloudové služby. Takže pokud chcete vytvořit nové OM než cloudové služby, které je podporují různé velikost, žádost výpočetním se nezdaří.

V závislosti na omezení cloudové služby, které používáte k vytvoření nové OM může dojít k chybě způsobeno jedním ze dvou situací.

**Způsobit 1:** Cloudové služby se Připne konkrétní obrázku nebo propojené do skupiny spřažení a tedy připnuté určitý cluster záměrné. Požadavky na tak nový výpočetním zdroje v tomto spřažení skupiny jsou použity ve stejném clusteru hostitelem existujících zdrojů. Však stejného clusteru nemusí podporovat na požadovanou velikost OM nebo máte dostatek místa, výsledkem je chyba přidělení. Toto je PRAVDA, zda se vytvářejí nové zdroje prostřednictvím nového Cloudová služba nebo existující cloudové služby.

**Řešení 1:**

- Vytvořit nové Cloudová služba a přidružit oblasti nebo založené na oblasti virtuální sítě.
- Vytvoření nového OM v nové cloudové služby.
  Pokud dojde k chybě při pokusu o vytvoření nové cloudové služby, opakujte později nebo změňte oblast cloudové služby.

> [AZURE.IMPORTANT] Pokud jste chtěli vytvořit nové OM v existující cloudové služby, ale nelze a bylo nutné vytvořit nové Cloudová služba pro nové OM, můžete sloučit všechny VMs ve stejném cloudové služby. Odstranění VMs v existující cloudové službě a znovu je zachytit z jejich disků v nové cloudové službě. Je ale důležité pamatovat, že nové cloudové služby bude mít nový název a VIP, takže byste potřebovali aktualizovat pro všechny závislosti, které aktuálně tyto informace použít k existující cloudové služby.

**Způsobit 2:** Cloudové služby je přidružený virtuální síti, která je propojená s skupinu spřažení tak, aby se Připne na konkrétní obrázku tak, že návrh. Požadavky na všechny nové výpočetním zdroje v tomto spřažení skupiny jsou proto vyzkoušeli ve stejném clusteru hostitelem existujících zdrojů. Však stejného clusteru nemusí podporovat na požadovanou velikost OM nebo máte dostatek místa, výsledkem je chyba přidělení. Toto je PRAVDA, zda se vytvářejí nové zdroje prostřednictvím nového Cloudová služba nebo existující cloudové služby.

**Řešení 2:**

- Vytvoření nové místní virtuální sítě.
- Vytvoření nového OM v nové virtuální sítě.
- [Připojit existující virtuální síti](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) nová virtuální síť. Další informace o [místní virtuální sítě](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Můžete taky můžete [migrovat na základě spřažení skupiny virtuální síti místní virtuální sítě](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)a pak vytvořte nové OM.

## <a name="next-steps"></a>Další kroky
Pokud při používání narazíte na problémy při spuštění přestal OM Windows nebo změna velikosti existující OM Windows v Azure, přečtěte si článek [Poradce při potížích klasické nasazení se restartováním nebo změna velikosti existující Windows virtuálního počítače v Azure](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).
