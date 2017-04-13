<properties 
   pageTitle="Správa účtu úložiště StorSimple | Microsoft Azure"
   description="Vysvětluje, jak můžete pomocí stránce StorSimple Správce konfigurace přidat, upravit, odebrat nebo otočit zabezpečení Key přidruženého pole virtuální StorSimple účtu úložiště."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Správa úložiště účtů pro StorSimple virtuální pole pomocí služby StorSimple správce

## <a name="overview"></a>Základní informace

Na stránce **Konfigurovat** představuje parametry globální služby vytvořené ve službě StorSimple správce. Tyto parametry se dají použít pro všechny zařízení připojených ke službě a patří:

- Účty úložiště 
- Ovládací prvek záznamů z aplikace Access 

Tento kurz vysvětluje, jak můžete použít na stránce **Konfigurovat** můžete přidat, upravit nebo odstranit účty úložiště pro vaše StorSimple virtuální pole. Informace v tomto kurzu platí jenom pro pole virtuální StorSimple systém březen 2016 GA verzi softwaru.

 ![Konfigurace stránky](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Účty úložiště obsahují přihlašovací údaje, které zařízení používá pro přístup k účtu úložiště u vašeho poskytovatele služeb cloudu. U účtů Microsoft Azure úložiště jedná se o přihlašovací údaje, jako je název účtu a primární přístupové klávesy. 

Na stránce **Konfigurovat** všechny účty úložiště, vytvořené pro fakturace předplatného se zobrazí v tabulkovém formátu obsahující následující informace:

- **Název** – jedinečný název přiřazené k tomuto účtu, když byl vytvořen.
- **Protokol SSL povolen** – jestli SSL je povolený a komunikace zařízení do cloudu, je dlouhodobě zabezpečeného kanálu.

Běžné úkoly týkající se úložiště účtů, které lze provádět na stránce **Konfigurovat** jsou:

- Přidání účtu úložiště 
- Úprava účtu úložiště 
- Odstranění účtu úložiště 


## <a name="types-of-storage-accounts"></a>Typy úložiště účtů

Existují tři typy úložiště účty, které můžete používat se svým zařízením StorSimple.

- **Automaticky generované úložiště účty** – jak naznačuje název tohoto typu účtu úložiště automaticky vygeneruje se při prvním vytvoření službu. Další informace o vytvoření tohoto účtu úložiště najdete v tématu [Vytvoření nové služby](storsimple-ova-manage-service.md#create-a-service). 
- **Účty úložiště v předplatné služby** – jedná se o Azure úložiště účty, které jsou přidružené k předplatnému stejné jako u služby. Další informace o tom, jak tyto úložiště vytvoření účtů, najdete v článku [O úložiště účty adresářové služby Azure](../storage/storage-create-storage-account.md). 
- **Účty úložiště mimo předplatné služby** – jedná se o Azure úložiště účty, které nejsou spojené s vašimi službami a pravděpodobně existoval před vytvořením službu.

Každý StorSimple virtuální pole vytvoří kontejneru (s předponou hcs) v okně účet přidružené úložiště. Tento kontejner obsahuje údaj cloudu pro svoje zařízení. Neodstraňujte tento kontejner přístupem prostřednictvím služby Azure úložiště, jak tuto akci dojde ke ztrátě dat.

## <a name="add-a-storage-account"></a>Přidání účtu úložiště

Konfigurace služby StorSimple správce můžete přidat účet úložiště poskytnutím jedinečný a popisný název a přístupových pověření, která jsou spojená s účtem úložiště. Máte taky možnost povolit režim secure sockets layer (SSL) k vytvoření zabezpečené kanálu pro síťová komunikace mezi zařízení a cloudu.

Vytvořit víc účtů pro dané cloudu poskytovatele služeb. Při uložení účtu úložiště služby pokusí komunikace s poskytovateli služeb cloudu. V současné době budou ověřeny pověření a přístup materiál, který jste zadali. Účet úložiště je vytvořen jenom v případě, že ověření úspěšně. Pokud neúspěšného ověření bude vhodné chybová zpráva zobrazí.

Správce prostředků úložiště účtů vytvořených v Azure portál jsou taky podporované s StorSimple. Správce prostředků úložiště účty se neobjeví v rozevíracím seznamu pro výběr, pouze úložiště, které se zobrazí účty vytvořené v portálu Azure klasické. Správce prostředků úložiště účty muset přidat pomocí postup přidání účtu úložiště podle níže uvedeného popisu.

Postup pro přidání účtu Azure klasické úložiště jsou uvedeny níže.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Úprava účtu úložiště

Můžete upravit účet úložiště používané zařízení. Pokud upravíte úložiště účet, který právě používá, pole k dispozici pro úpravy jsou přístupové klávesy a režimem SSL účtu úložiště. Můžete zadat nový přístupová klávesa úložiště nebo změnit výběr **Povolit SSL režimu** a uložte aktualizovaný nastavení.

#### <a name="to-edit-a-storage-account"></a>Chcete-li upravit účet úložiště

1. Na cílovou stránku služby vyberte služby, poklikejte na název služby a potom klikněte na **Konfigurovat**.

2. Klikněte na **Přidat/upravit úložiště účty**.

3. V dialogovém okně **Přidat/upravit úložiště účty** :

  1. V rozevíracím seznamu **Úložiště účty**vyberte existující účet, který chcete upravit. 
  2. V případě potřeby můžete změnit výběr **Zapnout režim SSL** .
  3. Je možné obnovit vašeho úložiště účtu přístupové klávesy. Další informace najdete v tématu [obnovovat klíče účtu úložiště](storage-create-storage-account.md#manage-your-storage-access-keys). Zadejte nový klíč účtu úložiště. Pro účet Azure úložiště je primární přístupové klávesy. 
  4. Klikněte na ikonu kontrola ![zaškrtněte ikony](./media/storsimple-ova-manage-storage-accounts/checkicon.png) uložte nastavení. Nastavení se aktualizuje na stránce **Konfigurovat** . 
  5. V dolní části stránky klikněte na **Uložit** uložte nově aktualizované nastavení. 

     ![Úprava účtu úložiště](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště

> [AZURE.IMPORTANT] Účet úložiště můžete odstranit jenom v případě, že není použití. Pokud úložiště účet se používá, zobrazí se oznámení.

#### <a name="to-delete-a-storage-account"></a>Odstranění účtu úložiště

1. Na stránce Správce StorSimple služby cílová vyberte služby, poklikejte na název služby a potom klikněte na **Konfigurovat**.

2. V tabulkových seznam účtů úložiště najeďte myší na účet, který chcete odstranit.

3. Ikonu Odstranit (**x**) se zobrazí v pravém sloupci krajní tohoto účtu úložiště. Klikněte na ikonu **x** a odstranit její přihlašovací údaje.

4. Po zobrazení výzvy k potvrzení klepněte na tlačítko **Ano** pokračujte odstranění. Tabulkovém výpisu bude aktualizována změny.

5. V dolní části stránky klikněte na **Uložit** uložte nově aktualizované nastavení.


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [Spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).
