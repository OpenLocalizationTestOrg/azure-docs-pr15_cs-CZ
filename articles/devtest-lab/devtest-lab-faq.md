<properties
    pageTitle="Azure DevTest Labs nejčastější dotazy týkající se | Microsoft Azure"
    description="Přečtěte si odpovědi na běžné otázky Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs časté otázky

Tento článek odpovídá na některé nejčastější dotazy týkající se Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Obecné
- [Co když Moje byste nenašli odpověď otázku tady?](#what-if-my-question-isnt-answered-here)
- [Proč mám používat Azure DevTest Labs?](#why-should-i-use-azure-devtest-labs) 
- [Co znamená "bez obav, samoobslužné"?](#what-does-quotworry-free-self-servicequot-mean)
- [Použití Azure DevTest Labs](#how-can-i-use-azure-devtest-labs) 
- [Jak mám fakturované pro Azure DevTest Labs?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Zabezpečení 
- [Jaké jsou různých úrovních zabezpečení v Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Jak vytvořit roli, kterou chcete uživatelům umožnit provádění s konkrétním úkolem?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>Integrace odvětví/CD & automatizaci 
 
- [Integrace Azure DevTest Labs s Moje odvětví/CD toolchain?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuálních počítačích 
 
- [Proč nevidím určitých VMs zásuvné virtuálních počítačích Azure, která se zobrazí v rámci Azure DevTest Labs?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Jaký je rozdíl mezi vlastní obrázky a vzorci?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Jak můžu vytvořit více VMs ze stejné šablony v celém dokumentu?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Jak přesunout svůj stávající VMs Azure do mé laboratorní Azure DevTest Labs?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Můžete připojit více disků k Moje VMs?](#can-i-attach-multiple-disks-to-my-vms) 
- [Jak můžu automatizovat proces nahrávání souborů virtuálního pevného disku, které chcete vytvořit vlastní obrázky?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Jak můžu automatizovat odstranění všech VMs v mém laboratoři?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Artefakty 
 
- [Jaké jsou artefakty?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Konfigurace laboratoři 
 
- [Jak vytvořit laboratoři z šablony správce prostředků Azure?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Proč moje VMs vytvořené v jiné skupiny prostředků libovolného názvy? Můžete přejmenovat nebo změnit tyto skupiny zdrojů?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Kolik labs můžete vytvořit v rámci stejného předplatného?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Kolik VMs můžete vytvořit za laboratorní?](#how-many-vms-can-i-create-per-lab)
- [Jak sdílet přímý odkaz na svůj laboratoři](#how-do-i-share-a-direct-link-to-my-lab)
- [Co je účet Microsoft?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Řešení potíží 
 
- [Moje artefakt se nepovedlo při vytváření OM. Jak řešit ho?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Není Proč moje existující virtuální síť ukládání správně?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Co když Moje byste nenašli odpověď otázku tady?
Pokud tady není uvedené svou otázku, napište nám a my vám najít odpověď.

- Zadejte dotaz v [Disqus vlákna](#comments) konci v tomto článku a zapojit s týmem Azure mezipaměti a ostatní členové komunity o tohoto článku.
- Kontaktovat na širší publikum, odeslat dotaz na [fórum Azure DevTest Labs MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)a zapojit s týmem Azure DevTest Labs a ostatní členové komunity.
- Chcete-li žádost o funkci, odeslání žádosti a nápady [Azure DevTest Labs uživatele hlasovou](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Proč mám používat Azure DevTest Labs? 
Azure DevTest Labs můžete uložit času týmu a peníze. Vývojáři můžete vytvořit vlastní prostředí pomocí několika různých základů a použijte artefakty k rychlému nasazení a konfiguraci aplikací. Použití vlastní obrázky a vzorců, virtuálních počítačích lze uložit jako šablony a snadno opakovat. Labs navíc nabízí několik konfigurovatelné zásad, které umožňují správcům laboratorní plýtvání a správu týmových prostředí. Tyto zásady zahrnout automaticky ukončení, náklady mezní maximální VMs za uživatele a maximální velikosti OM. Podrobné vysvětlení Azure DevTest Labs přečtěte si [Přehled](devtest-lab-overview.md) nebo podívejte se na [úvodní video](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Co znamená "bez obav, samoobslužné"?
"Bez obav, samoobslužné" znamená, že vývojáři a testeři vytvořit svoje vlastní prostředí podle potřeby a správci mají zabezpečení vědět, že Azure DevTest Labs pomáhá minimalizovat nakládání a řídit náklady. Správci můžete určit, které OM velikosti jsou povoleny, maximální počet VMs, a při spuštění a ukončení VMs. Azure DevTest Labs také usnadňuje sledovat náklady a nastavení upozorňování na budete mít přehled o použití zdrojů v testovacím prostředí. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Použití Azure DevTest Labs 
Azure DevTest Labs je užitečná, když vyžadují zařízením nebo testovacím prostředí a chcete reprodukujte rychle a/nebo Správa s náklady ukládání zásady. 

Tady jsou některé scénáře, které naši zákazníci používají Azure DevTest Labs pro: 

- Správa vývojáře a testovací prostředí na jednom místě, využití zásady snížit náklady a vlastní obrázky sdílení vytvoří přes týmu.
- Vytvoření aplikace pomocí vlastní obrázky uložte stav disk v průběhu vývoje fáze.
- Sledování nákladů vzhledem k průběhu. 
- Vytvoření hromadnou test prostředí pro účely testování assurance kvalitu.
- Použití artefakty a vzorce můžete snadno konfigurace a reprodukování aplikace v různých prostředích. 
- Distribuce VMs pro hackathons (odchylka nebo test týmovém) a potom snadno zrušit zřizování je po ukončení události. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Jak mám fakturované pro Azure DevTest Labs? 
Azure DevTest Labs je bezplatná služba, což znamená, že je zadarmo labs vytváření a konfigurace zásad, šablon a artefakty. Můžete buď zaplatit pouze Azure zdrojů použitých v rámci labs, například virtuálních počítačích, úložiště účty a virtuální sítě. Další informace o náklady na zdroje laboratorní přečtěte si o [Azure DevTest Labs ceny](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Jaké jsou různých úrovních zabezpečení v Azure DevTest Labs?  
Zabezpečení Accessu, je určený podle [Azure Role-Based přístup ovládacího prvku (RBAC)](../active-directory/role-based-access-built-in-roles.md). Pokud chcete zjistit, jak funguje aplikace access, je dobré znát rozdíly mezi oprávnění, role a obor způsobem definovaným ve RBAC.

- **Oprávnění** – oprávnění jsou definované přístupu k určité akce. Například oprávnění může být přístup pro čtení na všech virtuálních počítačích. 
- **Role** - role je sada oprávnění, které lze seskupovat a se uživateli přiřadí. Například "vlastník předplatného" má přístup pro všechny zdroje v rámci předplatného. 
- **Rozsah** – obor je úroveň v hierarchii Azure zdroje. Obor může být například skupina zdroje nebo jeden laboratorní nebo celý odběr. 
 
V rozsahu Azure DevTest Labs existují dva typy rolí definovat uživatelská oprávnění: laboratorní vlastníka a laboratorní uživatele.

- **Laboratorní vlastník** - laboratorní vlastník má přístup k zdroje v testovacím prostředí. Proto jsou můžete upravovat zásady, číst a psát všechny VMs, změnit virtuální sítě atd. 
- **Laboratorní uživatele** - laboratorní uživatele můžete zobrazit všechny laboratorní zdroje, jako je třeba VMs zásady a virtuální sítích, ale nemohou měnit zásad nebo libovolnou VMs vytvořené jinými uživateli. Je také možné vytvořit vlastní role v Azure DevTest Labs a zjistíte postup v článku [udělení uživatelských oprávnění k určité laboratorní zásady](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Protože obory hierarchické, pokud má uživatel oprávnění na určitý obor, jsou automaticky udělena tato oprávnění v každé nižší úrovně oboru zahrnuta. Například pokud uživatel přiřazen k roli vlastník předplatného, pak mají přístup k pro všechny zdroje v předplatné. Sem patří všechny virtuálních počítačích a všechny virtuální sítě a všechny praktická cvičení. Tedy vlastník předplatného automaticky zdědí role laboratorní vlastník. Však opakem neplatí. Laboratorní vlastník má přístup k laboratorní, což je obor nižší než úroveň předplatného. Laboratorní vlastník tedy není vidět virtuálních počítačích nebo virtuální sítě nebo zdroje, které jsou mimo testovacím prostředí. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Jak vytvořit roli, kterou chcete uživatelům umožnit provádění s konkrétním úkolem?
Úplný článek o tom, jak vytvořit vlastní role a oprávnění přiřaďte určitou roli najdete tady. Tady je příklad skript, který vytvoří role "DevTest Labs Upřesnit uživatele", který má oprávnění k spustit a zastavit všechny VMs v testovacím prostředí:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Integrace Azure DevTest Labs s Moje odvětví/CD toolchain? 
Pokud používáte VSTS, je [rozšíření Azure DevTest Labs úkoly](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , které umožňuje automatizovat vydání kanálu v Azure DevTest Labs. Použití tuto linku patří:

- Vytvoření a nasazení virtuálního počítače automaticky a konfigurace posledním buildu pomocí kopírování souborů Azure nebo prostředí PowerShell VSTS úkoly. 
- Automaticky zachycení stavu virtuálního počítače po testování reprodukovat chybu na stejné OM pro další vyšetřování. 
- Odstranění OM konci kanálu vydání, pokud již není potřeba. 

Následující příspěvků na blogu jsou uvedeny pokyny a informace o používání koncovku VSTS:
 
- [Azure DevTest Labs – VSTS rozšíření](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Nasazení nové OM v existující AzureDevTestLab z VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Správa vydání VSTS nepřetržitý nasazení AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Pro ostatní toolchains odvětví/CD výše uvedených scénáře, které lze dosáhnout rozšíření úloh VSTS lze podobně docílit nasazování [šablon správce prostředků Azure](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) pomocí [Azure PowerShell](../resource-group-template-deploy.md) a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Můžete taky [REST API pro DevTest Labs](http://aka.ms/dtlrestapis) integrovat s vaší toolchain.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Proč nevidím určitých VMs zásuvné virtuálních počítačích Azure, která se zobrazí v rámci Azure DevTest Labs?
Po vytvoření virtuálního počítače v Azure DevTest Labs je uveden oprávnění pro přístup k této OM. Je možné zobrazit v zásuvné labs zásuvné **virtuálních počítačích** i. Uživatelé v roli DevTest Labs vidí všechny virtuálních počítačích vytvořený v testovacím prostředí prostřednictvím zásuvné laboratoři **všechny virtuálních počítačích** . Uživatelé v roli DevTest Labs však nejsou automaticky udělena přístup pro čtení na OM zdroje, které ostatní vytvořili. Proto tyto VMs nezobrazí v zásuvné **virtuálních počítačích** . 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Jaký je rozdíl mezi vlastní obrázky a vzorci? 
Vlastní obrázek je virtuálního pevného disku (virtuální pevný disk), že vzorec je obrázek, který můžete nakonfigurovat další nastavení, které můžete uložit a reprodukování. Vlastní obrázek může být vhodnější, pokud chcete rychle vytvořit několik prostředí s stejný obrázek základní, neměnný. Vzorec může být lepší, když budete chtít reprodukovat konfigurace OM nejnovější bitů, virtuální podsítě nebo přesné velikosti. Podrobnější vysvětlení naleznete v článku [Comparing vlastní obrázky a vzorců v DevTest Labs](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Jak můžu vytvořit více VMs ze stejné šablony v celém dokumentu? 
Při vytváření OM a [nasazení šablony správce prostředků Azure z prostředí Windows PowerShell](../resource-group-template-deploy.md)můžete používat [rozšíření úloh VSTS](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) nebo [Generovat šablonu správce prostředků Azure](devtest-lab-add-vm-with-artifacts.md#save-arm-template) . 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Jak přesunout svůj stávající VMs Azure do mé laboratorní Azure DevTest Labs? 
Jsme navrhujete řešení přejděte přímo VMs Azure DevTest Labs, ale právě můžete zkopírovat existující VMs Azure DevTest Labs následujícím způsobem: 

1. Zkopíroval soubor virtuálního pevného disku svého existující OM pomocí tohoto [skriptu prostředí Windows PowerShell](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [Vytvořit vlastní obrázek](devtest-lab-create-template.md) uvnitř vaší laboratoři Azure DevTest Labs. 
1. Vytvoření virtuálního počítače v laboratoři z vlastní obrázek 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Můžete připojit více disků k Moje VMs? 
Připojení více disků VMs je podporovaná.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Jak můžu automatizovat proces nahrávání souborů virtuálního pevného disku, které chcete vytvořit vlastní obrázky? 
Dvěma způsoby:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) mohou sloužit k kopírovat nebo nahrání souborů virtuální pevný disk na úložiště účet spojený s testovacím prostředí.
- [Microsoft Azure úložiště Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatné aplikace, která poběží na Windows, OSX a Linux.   
 
Najít cílového úložiště účtu spojeného s vaší laboratoři, postupujte takto:

1. Přihlaste se k [portálu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Vyberte v levém panelu **Skupiny zdrojů** . 
1. Najděte a vyberte přidružený k vaší laboratoři skupina zdroje. 
1. Na zásuvné **Přehled** vyberte jednu z účtů úložiště. 
1. Výběr **objektů BLOB**.
1. Přehled pro ukládání v seznamu. Pokud žádná existuje, vraťte se ke kroku #4 a jiný účet úložiště.
1. Pomocí **adresy URL** jako cíle v příkazu AzCopy.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Jak můžu automatizovat odstranění všech VMs v mém laboratoři?

Kromě odstranění VMs z vaší laboratoři na portálu Azure, můžete odstranit všechny VMs vaší laboratoři pomocí skriptů Powershellu. V následujícím příkladu jednoduše změňte hodnoty parametrů pod komentářem **hodnot a ty pak změnit** . Můžete získat `subscriptionId`, `labResourceGroup`, a `labName` hodnoty z zásuvné laboratorní Azure portálu. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Jaké jsou artefakty? 
Artefakty jsou přizpůsobitelné prvky, které mohou sloužit k instalaci nejnovější bitů nebo nástroje vývojáře do virtuálního počítače. Jsou připojené k vaší OM při vytváření několika kliknutími jednoduchý a po zřízení OM artefaktům nasazení a konfiguraci vašeho OM. Existují různé stávajících artefakty v naší [veřejné Github úložiště](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), ale můžete taky snadno [vytvářet vlastní artefakty](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Jak vytvořit laboratoři z šablony správce prostředků Azure? 
Uvádíme [Github úložišti laboratorní správce prostředků Azure šablon](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) , které jako nástroje můžete nasazovat- nebo upravit vytváření vlastních šablon pro vaše labs. Každá z těchto šablon obsahuje odkaz, který vás přesměruje na nasazení médiu jako-je v části vlastní Azure předplatného, nebo můžete přizpůsobit šablony a [nasazení pomocí prostředí PowerShell nebo Azure rozhraní příkazového řádku](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Proč moje VMs vytvořené v jiné skupiny prostředků libovolného názvy? Můžete přejmenovat nebo změnit tyto skupiny zdrojů? 
Skupiny zdrojů vytvořené tímto způsobem, aby Azure DevTest Labs ke správě uživatelských oprávnění a přístup k virtuálních počítačích. Během OM můžete přesunout do jiného pole Skupina zdroje s názvem vaší požadované, provedete tak se nedoporučuje. Pracujeme na vylepšení toto prostředí umožňuje větší flexibilitu.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Kolik labs můžete vytvořit v rámci stejného předplatného? 
Neexistuje žádné zvláštní omezení počtu labs vytvořené jedno předplatné. Zdrojů použitých jsou ale omezený na jedno předplatné. Další informace o [omezení a kvót ukládané Azure předplatná](../azure-subscription-service-limits.md) a [jak zvýšit tato omezení](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Kolik VMs můžete vytvořit za laboratorní? 
Neexistuje žádné zvláštní omezení počtu VMs vytvořené za laboratorní. Však v současné době médiu podporuje pouze 40 VMs spuštěné ve stejnou dobu v standardní úložiště a 25 VMs souběžně aplikaci premium úložiště. Pracujeme na zvýšení tato omezení. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Jak sdílet přímý odkaz na svůj laboratoři

Sdílení přímý odkaz laboratorní uživatelům můžete provést následující postup.

1. Přejděte na testovacím prostředí Azure portálu.
2. Zkopírujte adresu URL laboratorní v prohlížeči a sdílejte ho s jinými uživateli laboratorní. 

>[AZURE.NOTE] Pokud uživatelé laboratorní se externí uživatelé s [MSA účtu](#what-is-a-microsoft-account) a nepatří do služby Active directory vaší společnosti, se může zobrazit chybová při přechodu na zadaného odkazu. Pokud se zobrazí chyba, požádejte je kliknutím na její jméno v pravém horním rohu portálu Azure a vyberte adresář, pokud existuje médiu z **adresáře** části nabídky.

### <a name="what-is-a-microsoft-account"></a>Co je účet Microsoft?

Účet Microsoft je použitý pro téměř všechno, co můžete dělat s zařízení Microsoft a služby. Je-mailovou adresu a heslo, které používáte pro přihlášení ke Skypu, Outlook.com, Onedrivu, Windows Phone a Xbox LIVE – a to znamená, že soubory, fotky, kontaktů a nastavení můžete vás sledují na libovolném zařízení. 

>[AZURE.NOTE] Účet Microsoft používá volat "Windows Live ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Moje artefakt se nepovedlo při vytváření OM. Jak řešit ho? 
Podívejte se do příspěvku na blogu [jak řešit problémy s nefunkční artefakty v AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - napsal jednu z našich Professionals - se dozvíte, jak získat protokoly o nezdařeném uložení artefakt. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Není Proč moje existující virtuální síť ukládání správně?  
Jednou z možností je, že virtuální síťový název obsahuje období. Pokud ano, zkuste odebrání období nebo nahrazení pomlčky a pak to zkuste znovu uložit virtuální sítě.
