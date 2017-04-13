<properties
   pageTitle="Řešení sady správy operace (OMS) | Microsoft Azure"
   description="Řešení rozšířit použitelnost funkce z operace správy sady (OMS) zadáním scénáře sbalené správy, které zákazníci můžete přidat do jejich OMS pracovního prostoru.  Tento článek poskytuje podrobné informace o tom, jak vlastních řešení, které vytvořil zákazníky a partnery."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Řešení pro správu v operace správy sady (OMS) (verze Preview)

>[AZURE.NOTE]Toto je předběžná si přečtěte následující dokumentaci pro řešení pro správu v OMS, které jsou v současnosti ve verzi preview.    

Řešení pro správu rozšířit použitelnost funkce z operace správy sady (OMS) zadáním scénáře sbalené správy, které zákazníci můžete přidat do svého prostředí.  Kromě [řešení poskytované společností Microsoft](../log-analytics/log-analytics-add-solutions.md)partnery a zákazníky můžete vytvořit řešení pro správu se nemusí používat v vlastní prostředí nebo k dispozici pro zákazníky pomocí komunity.

## <a name="finding-and-installing-management-solutions"></a>Hledání a instalace řešení pro správu
Vyhledání a instalace řešení pro správu podle popisu v následujících částech několika způsoby.

### <a name="azure-marketplace"></a>Azure Marketplace
Řešení pro správu poskytované společností Microsoft a důvěryhodných partnerů může nainstalovali z webu Azure Marketplace Azure portálu.

1. Přihlaste se k portálu Azure.
2. V levém podokně vyberte **Další služby**.
3. Posuňte se dolů na **řešení** nebo zadejte *řešení* v dialogovém okně **Filtr** .
4. Kliknutím na tlačítko **+ Přidat** .
5. Hledání řešení, která vás zajímá buď procházení, klikněte na tlačítko **Filtr** nebo zadáním do pole **Hledat Everthing** .
6. Klikněte na položku marketplace zobrazíte její podrobnosti.
4. Klikněte na **vytvořit** otevřete podokno **Přidat řešení** .
5. Zobrazí se výzva k požadované informace, například [OMS workspace a automatizaci účtu](#oms-workspace-and-automation-account) kromě hodnoty všech parametrů v řešení.
6. Klikněte na **vytvořit** nainstalovat řešení.

### <a name="oms-portal"></a>Portál OMS
Řešení pro správu poskytované společností Microsoft může být nainstalovaný z Galerie řešení na portálu OMS.

1. Přihlaste se k portálu OMS.
2. Klikněte na dlaždici **Galerie řešení** .
2. Na stránce Galerie řešení OMS informace o všech dostupných řešení. Klikněte na název řešení, které chcete přidat do OMS.
3. Na stránce řešení, které jste se rozhodli zobrazí se podrobné informace o řešení. Klikněte na **Přidat**.
4. Nová dlaždice řešení, které jste přidali, zobrazí se na stránku v OMS a můžete začít s ním pracovat po službu OMS zpracuje dat přehledu.

### <a name="azure-quickstart-templates"></a>Rychlý úvod Azure šablony
Členové komunity můžete odeslat řešení pro správu k šablonám rychlý úvod Azure.  Můžete stáhnout tyto šablony pro pozdější instalaci nebo kontrola, zda je se dozvíte, jak [vytvořit vlastní řešení](#creating-a-solution).

1. Pokračovat v [OMS workspace a automatizaci účtu](#oms-workspace-and-automation-account) na odkaz pracovní prostor a účtu.
2. Přejděte na [Azure rychlý úvod šablony](https://azure.microsoft.com/documentation/templates/).  
3. Hledání řešení, které vás zajímá.
4. Vyberte řešení ve výsledcích zobrazíte její podrobnosti.
5. Klikněte na tlačítko **instalovat na Azure** .
6. Zobrazí se výzva k zadání informace, například pole Skupina zdroje a umístění kromě hodnoty pro všechny parametry v řešení.
7. Klepněte na položku **zakoupit** nainstalovat řešení.

### <a name="deploy-azure-resource-manager-template"></a>Nasazení šablony správce prostředků Azure
Řešení, můžete získat od komunity, které [sami vytvoříte](#creating-a-solution) implementace jako šablonu správce prostředků nebo použijte některou z standardní metod pro [nasazení šablony](../resource-group-template-deploy-portal.md).  Všimněte si, že před instalací řešení, musíte vytvořit a odkaz [pracovní prostor OMS a automatizaci účtu](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>Pracovní prostor OMS a automatizaci účtu
Většina řešení pro správu vyžadují [pracovního prostoru OMS](../log-analytics/log-analytics-manage-access.md) má být zobrazení a [automatizaci účet](../automation/automation-security-overview.md#automation-account-overview) má být runbooks a související materiály. Pracovní prostor a účet musí splňovat následující kritéria.

- Řešení můžete použít jenom jeden pracovní prostor OMS a automatizaci účtů.  
- Pracovní prostor OMS a automatizaci účet řešení musí být propojeny vzájemně. Pracovní prostor OMS může být propojený pouze s jedním klientem automatizace a účet automatizaci může být propojený pouze jeden OMS pracovního prostoru.
- Propojení, pracovního prostoru OMS a automatizaci účtu musí být ve stejné pole Skupina zdroje a oblasti.  Výjimky je OMS prostoru v oblasti východoasijských USA a a účet automatizaci východoasijských USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Vytvoření propojení mezi OMS workspace a automatizaci účtu
Jak určit pracovní prostor OMS a automatizaci účtu závisí na způsob instalace pro vaše řešení.

- Při instalaci řešení společnosti Microsoft prostřednictvím portálu OMS hned po instalaci používat v aktuálním pracovním prostoru OMS a vyžaduje se účet žádná automatizaci.

- Při instalaci řešení prostřednictvím webu Azure Marketplace se zobrazí výzva pro pracovní prostor OMS a automatizaci účet a se vytvoří propojení mezi nimi.  

- Řešení mimo Azure Marketplace musí propojíte OMS workspace a automatizaci účtu před instalací řešení.  Lze provést výběrem žádné řešení v Azure Marketplace a výběrem pracovního prostoru OMS a automatizaci účtu.  Nemusíte skutečně nainstalovat řešení, protože odkaz vytvoří se hned, jak je vybráno OMS pracovní prostor a automatizaci účtu.  Po vytvoření odkazu můžete pomocí tohoto pracovního prostoru OMS a automatizaci účtu pro žádné řešení. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Ověření propojení mezi OMS workspace a automatizaci účtu
Propojení mezi OMS workspace a automatizaci účet pomocí následujícího postupu můžete ověřit.

1. Vyberte účet, automatizaci Azure portálu.
2. Přejděte do dolní části podokna **Nastavení** .
3. Pokud není oddíl s názvem **OMS zdrojů** v podokně **Nastavení** , je připojen tento účet do pracovního prostoru OMS.
4. Vyberte **pracovního prostoru** zobrazíte podrobné informace o pracovním prostoru OMS spojená s tímto účtem automatizaci.


## <a name="listing-management-solutions"></a>Seznam řešení pro správu
Pomocí následujícího postupu do zobrazíte řešení pro správu v pracovních prostorech propojené s Azure předplatné.

1. Přihlaste se k portálu Azure.
2. V levém podokně vyberte **Další služby**.
3. Posuňte se dolů na **řešení** nebo zadejte *řešení* v dialogovém okně **Filtr** .
4. Řešení nainstalovaný v pracovních prostorech budou uvedené.

Všimněte si, že můžete zobrazit jenom řešení Microsoft nainstalovaných v aktuálním pracovním prostoru pomocí portálu OMS.

## <a name="removing-a-management-solution"></a>Odebrání řešení pro správu
Při odebrání řešení pro správu odebrány také všechny zdroje v řešení.  

1. Na portálu Azure pomocí postupu v [seznamu řešení](#listing-solutions)najděte řešení.
2. Vyberte řešení, které chcete odebrat.
3. Klikněte na tlačítko **Odstranit** .

## <a name="creating-a-management-solution"></a>Vytvoření řešení pro správu
Úplné pokyny týkající se vytváření řešení pro správu jsou k dispozici [vytváření řešení v sadě operace Management (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Další kroky

- Hledání [Šablon rychlý úvod Azure](https://azure.microsoft.com/documentation/templates) příklady různých šablon správce prostředků.
- Vytvořte vlastní [řešení pro správu](operations-management-suite-solutions-creating.md).
 