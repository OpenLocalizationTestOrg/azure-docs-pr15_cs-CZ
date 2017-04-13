<properties 
   pageTitle="Správa účtu úložiště StorSimple | Microsoft Azure"
   description="Vysvětluje, jak můžete na stránce Správce StorSimple konfigurovat můžete přidat, upravit, odstranit nebo otočit klíče zabezpečení účtu úložiště."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Správa účtu úložiště pomocí službu StorSimple správce

## <a name="overview"></a>Základní informace

Na stránce **Konfigurovat** představuje všechny parametry globální služby vytvořené ve službě StorSimple správce. Tyto parametry se dají použít pro všechny zařízení připojených ke službě a patří:

- Účty úložiště 
- Šablony šířky pásma 
- Ovládací prvek záznamů z aplikace Access 

Tento kurz vysvětluje, jak můžete na stránce **Konfigurovat** rychle přidat, upravit, nebo odstranit účty úložiště a otočit klíče zabezpečení účtu úložiště.

 ![Konfigurace stránky](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Účty úložiště obsahují přihlašovací údaje, které zařízení používá pro přístup k účtu úložiště u vašeho poskytovatele služeb cloudu. U účtů Microsoft Azure úložiště jedná se o přihlašovací údaje, jako je název účtu a primární přístupové klávesy. 

Na stránce **Konfigurovat** všechny účty úložiště, vytvořené pro fakturace předplatného se zobrazí v tabulkovém formátu obsahující následující informace:

- **Název** – jedinečný název přiřazené k tomuto účtu, když byl vytvořen.
- **Protokol SSL povolen** – jestli SSL je povolený a komunikace zařízení do cloudu, je dlouhodobě zabezpečeného kanálu.
- **Používá** – počet svazky pomocí účtu úložiště.

Běžné úkoly týkající se úložiště účtů, které lze provádět na stránce **Konfigurovat** jsou:

- Přidání účtu úložiště 
- Úprava účtu úložiště 
- Odstranění účtu úložiště 
- Klíčové otočení v prostoru úložiště účtů 

## <a name="types-of-storage-accounts"></a>Typy úložiště účtů

Existují tři typy úložiště účty, které můžete používat se svým zařízením StorSimple.

- **Automaticky generované úložiště účty** – jak naznačuje název tohoto typu účtu úložiště automaticky vygeneruje se při prvním vytvoření službu. Další informace o vytvoření tohoto účtu úložiště najdete v tématu [Krok 1: vytvoření nové služby](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) v [zařízení StorSimple v místním nasazení](storsimple-deployment-walkthrough.md). 
- **Účty úložiště v předplatné služby** – jedná se o Azure úložiště účty, které jsou přidružené k předplatnému stejné jako u služby. Další informace o tom, jak tyto úložiště vytvoření účtů, najdete v článku [O úložiště účty adresářové služby Azure](../storage/storage-create-storage-account.md). 
- **Účty úložiště mimo předplatné služby** – jedná se o Azure úložiště účty, které nejsou spojené s vašimi službami a pravděpodobně existoval před vytvořením službu.

## <a name="add-a-storage-account"></a>Přidání účtu úložiště

Přidat účet úložiště poskytnutím jedinečný a popisný název a přístupových pověření, která jsou spojená s účtem úložiště (u poskytovatele služeb určený shluk). Máte taky možnost povolit režim secure sockets layer (SSL) k vytvoření zabezpečené kanálu pro síťová komunikace mezi zařízení a cloudu.

Vytvořit víc účtů pro dané cloudu poskytovatele služeb. Mějte na paměti, ale po vytvoření účtu úložiště, nemůžete změnit poskytovatele služby cloudu.

Při uložení účtu úložiště služby pokusí komunikace s poskytovateli služeb cloudu. V současné době budou ověřeny pověření a přístup materiál, který jste zadali. Účet úložiště je vytvořen jenom v případě, že ověření úspěšně. Pokud neúspěšného ověření bude vhodné chybová zpráva zobrazí.

Správce prostředků úložiště účtů vytvořených v Azure portál jsou taky podporované s StorSimple. Správce prostředků úložiště účty se neobjeví v rozevíracím seznamu pro výběr při pokusu o vytvoření kontejneru hlasitost pouze úložiště účtů vytvořených v portálu Azure klasické se zobrazí. Správce prostředků úložiště účty muset přidat pomocí postup přidání účtu úložiště píše níže.

> [AZURE.NOTE] Postup pro přidání účtu úložiště liší v závislosti na verzi softwaru StorSimple, kterou používáte. Ujistěte se, správný postup pro vaši verzi StorSimple.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Úprava účtu úložiště

Můžete upravit účet úložiště, který používá kontejneru hlasitost. Pokud upravíte úložiště účet, který právě používá, pouze pole k dispozici pro úpravy je přístupová klávesa účtu úložiště. Můžete zadat nový přístupová klávesa úložiště a uložte aktualizovaný nastavení.

#### <a name="to-edit-a-storage-account"></a>Chcete-li upravit účet úložiště

1. Na cílovou stránku služby vyberte služby, poklikejte na název služby a potom klikněte na **Konfigurovat**.

2. Klikněte na **Přidat/upravit úložiště účty**.

3. V dialogovém okně **Přidat/upravit úložiště účty** :

  1. V rozevíracím seznamu **Úložiště účty**vyberte existující účet, který chcete upravit. Také mohl úložiště účty, které byly vytvořeny automaticky při prvním vytvoření službu.
  2. V případě potřeby můžete změnit výběr **Zapnout režim SSL** .
  3. Můžete otočit vašeho úložiště účtu přístupové klávesy. Další informace o tom, jak provádět střídání najdete v článku [Otočení klávesu úložiště účtů](#key-rotation-of-storage-accounts) .
  4. Klikněte na ikonu kontrola ![zaškrtněte ikony](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) uložte nastavení. Nastavení se aktualizuje na stránce **Konfigurovat** . Klepněte na tlačítko **Uložit** uložte nově aktualizované nastavení.

     ![Úprava účtu úložiště](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště

> [AZURE.IMPORTANT] Účet úložiště můžete odstranit jenom v případě, že se nepoužívá kontejneru hlasitost. Pokud účet úložiště používá kontejneru objem, odstraňte nejdříve kontejneru hlasitost a potom odstraňte účtu přidružené úložiště.

#### <a name="to-delete-a-storage-account"></a>Odstranění účtu úložiště

1. Na stránce Správce StorSimple služby cílová vyberte služby, poklikejte na název služby a potom klikněte na **Konfigurovat**.

2. V tabulkových seznam účtů úložiště najeďte myší na účet, který chcete odstranit.

3. Ikonu Odstranit (**x**) se zobrazí v pravém sloupci krajní tohoto účtu úložiště. Klikněte na ikonu **x** a odstranit její přihlašovací údaje.

4. Po zobrazení výzvy k potvrzení klepněte na tlačítko **Ano** pokračujte odstranění. Tabulkovém výpisu bude aktualizována změny.

## <a name="key-rotation-of-storage-accounts"></a>Klíčové otočení v prostoru úložiště účtů

Z bezpečnostních důvodů střídání bývá požadavek v datacentrech. 

> [AZURE.NOTE] Tyto informace klíčových otočení v prostoru a otočení postup platí pro Microsoft Azure úložiště jenom účty. Pokud používáte jiného poskytovatele služby cloudu, můžete spravovat úložiště účtu klíče pomocí tohoto poskytovatele řídicího panelu.
 
Každého předplatného Microsoft Azure může mít jeden nebo více účtů přidružené úložiště. Přístup k tyto účty řízené pomocí klávesy předplatné a přístup pro každý účet úložiště. 

Při vytváření účtu úložiště Microsoft Azure vygeneruje dva 512-bit úložiště přístupové klávesy, které se používají pro ověřování při přístupu k účtu úložiště. Máte dvě úložiště přístupových kláves umožňuje obnovit klávesy s přístup ke službě nebo nijak nepřeruší služba úložiště. Klávesy, která je aktuálně používaných je *primární* klíč a klávesu záložní nazývá *sekundárního* klíče. Jednu z těchto dvou klíčů je nutné zadat při zařízení Microsoft Azure StorSimple přistupuje poskytovatele cloudové úložiště služby.

## <a name="what-is-key-rotation"></a>Co je střídání?

Obvykle aplikace použít pouze jeden z klíčů pro přístup k datům. Po určité době dobu může mít aplikace přepnutím na používání druhý klíč. Po Přepnuli jste používání aplikací sekundárního klíče, můžete vyřadit prvního klíče a pak vytvořte nový klíč. Pomocí dvou klíčů tímto způsobem umožňuje aplikace přístup k datům bez by všechny výpadek služeb.

Klávesy účtu úložiště jsou vždy uložené ve službě zašifrované. Tyto však můžete obnovit pomocí služby StorSimple správce. Služba dostane primární klíč a vedlejší klíč pro všechny účty úložiště v rámci stejného předplatného, včetně účtů vytvořených v služba úložiště, stejně jako výchozí úložiště vytvořené účty generovaného při prvním službu StorSimple Správce služby. Služba StorSimple správce bude vždy získat klávesy z portálu Microsoft Azure klasické a uložte je šifrované způsobem.

## <a name="rotation-workflow"></a>Pracovní postup otočení v prostoru

Správce Microsoft Azure můžete obnovit nebo změna klíče primární a sekundární tak, že přímý přístup k účtu úložiště (přes službu úložišti tabulek Microsoft Azure). Služba StorSimple správce nezobrazuje této změny automaticky.

Informovat službu StorSimple správce změny, můžete bude nutné pro přístup ke službě StorSimple správce přístup k účtu úložiště a potom synchronizujte klávesu primární a sekundární (podle toho, který z nich naposledy změnil). Služba potom získá poslední klíč, jsou šifrovány klávesy a odešle zašifrovaný klíč zařízení.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Pokud chcete synchronizovat klávesové zkratky pro účty úložiště ve stejném předplatném jako služba (pouze Azure)

1. Na stránce **služby** klikněte na kartu **Konfigurovat** .

2. Klikněte na **Přidat/upravit úložiště účty**.

3. V dialogovém okně postupujte takto:

  1. Vyberte účet, úložiště s klíčem, který chcete synchronizovat. Klávesy účtu úložiště jsou šifrovány při zobrazují.
  2. Ve službě StorSimple správce budete muset aktualizovat klávesy, která byla dříve změnit ve službě úložišti tabulek Microsoft Azure. Pokud primární přístupová klávesa naposledy změnil (regenerované), klikněte na **synchronizovat primární klíč**. Pokud sekundárního klíče naposledy změnil, klikněte na **synchronizovat sekundárního klíče**.

    ![synchronizace klíče](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Pokud chcete synchronizovat klávesové zkratky pro účty úložiště mimo předplatné služeb

1. Na stránce **služby** klikněte na kartu **Konfigurovat** .

2. Klikněte na **Přidat/upravit úložiště účty**.

3. V dialogovém okně postupujte takto:

  1. Vyberte účet, úložiště s přístupový kód, který chcete aktualizovat.
  2. Bude potřeba aktualizovat přístupová klávesa úložiště ve službě StorSimple správce. V tomto případě uvidíte přístupová klávesa úložiště. Nový klíč zadejte do pole **Klíč účtu přístup úložiště**y. 
  3. Uložte provedené změny. Teď je třeba aktualizovat svůj klíč účtu přístup úložiště.

## <a name="next-steps"></a>Další kroky

- Další informace o [StorSimple zabezpečení](storsimple-security.md).
- Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
