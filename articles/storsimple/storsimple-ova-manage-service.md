<properties 
   pageTitle="Nasazení službu StorSimple správce pro virtuální pole StorSimple | Microsoft Azure"
   description="Vysvětluje, jak vytvářet a odstraňovat službu StorSimple správce na portálu Azure klasické a popisuje, jak spravovat klíč registrace služby."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>Nasazení službu StorSimple správce pro StorSimple virtuální pole

## <a name="overview"></a>Základní informace

Služba StorSimple správce spustí v Microsoft Azure a pak se připojí na několika zařízeních StorSimple. Po vytvoření službu používáte ho ke správě zařízení z portálu Microsoft Azure klasické spuštěné v prohlížeči. Umožňuje sledovat všechny zařízení připojených k StorSimple Správce služeb z jednoho centrální umístění, a tím minimalizace administrativní zátěž.

Cílová stránka StorSimple správce uvádí všechny StorSimple Správce služby, které můžete použít ke správě zařízení StorSimple úložiště. Pro každou službu StorSimple správce tyto informace prezentovány na stránce Správce StorSimple:

- **Název** – název, který byl přiřazen StorSimple Správce služby při vytváření. Po vytvoření službu nelze změnit název služby.

- **Stav** – stav služby, které můžou být **aktivní**, **vytváření**nebo **Online**.

- **Umístění** – geografické polohy, ve kterém má být nasazené StorSimple zařízení.

- **Předplatné** – fakturace předplatného, které souvisí s vašimi službami.

Běžné úkoly, které lze provést pomocí stránce Správce StorSimple jsou:

- Vytvoření služby
- Odstranění služby
- Získání klíč registrace služby
- Obnovit klíč registrace služby

Tento kurz popisuje, jak provádět všechny úkoly. Informace obsažené v tomto článku platí pouze pro StorSimple virtuální matice. Další informace o StorSimple 8000 řady přejděte na [nasazení StorSimple Správce služby](storsimple-manage-service.md).

## <a name="create-a-service"></a>Vytvoření služby

Umožňuje vytvořit StorSimple Správce služby, pokud chcete nasadit zařízení StorSimple možnost **Vytvořit** . Pokud chcete vytvořit službu, potřebujete:

- Předplatné s Enterprise Agreement
- Aktivní úložiště účet Microsoft Azure
- Fakturačních údajů používanou pro správu přístupu

Můžete taky generovat výchozí účet úložiště při vytváření službu.

Služba jednotného zvládne víc zařízení. Zařízení však nemůže zahrnovat více služeb. Velké organizace nemůže mít víc instancí služby pro práci s různých předplatných, organizace nebo dokonce nasazení umístění.  

> [AZURE.NOTE] Budete potřebovat samostatný instancí služby StorSimple správce ke správě StorSimple 8000 řady zařízení a StorSimple virtuální matice.

Proveďte následující postup vytvoření služby.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>Odstranění služby

Před odstraněním službu Ujistěte se, že žádný připojená zařízení jsou s ním pracovat. Pokud je služba používá, deaktivujte připojená zařízení. Operace Deaktivovat server připojení mezi zařízení a službách, ale zachovat data zařízení v cloudu. 

> [AZURE.IMPORTANT] Po odstranění službu operace nejde vrátit zpět. 

Proveďte následující kroky pro odstranění služby.

### <a name="to-delete-a-service"></a>Chcete-li odstranit služby

1. Na stránce **Správce StorSimple službu** vyberte službu, kterou chcete odstranit.

1. Klepněte na příkaz **Odstranit** v dolní části stránky.

1. V oznámení s potvrzením kliknutím na **Ano** . Může trvat několik minut pro službu odeberou.

## <a name="get-the-service-registration-key"></a>Získání klíč registrace služby

Po úspěšném vytvoření služby, musíte zaregistrovat zařízení StorSimple se službou. Zaregistrovat zařízení první StorSimple, budete potřebovat klíč registrace služby. Zaregistrujte další zařízení s existující StorSimple službu, budete potřebovat registračního klíče a služby šifrovacího klíče dat (což je generováno na prvním zařízení při registraci). Další informace o šifrovací klíč data service najdete v článku [získání šifrovací klíč služby data z místního webu uživatelského rozhraní](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key). 

Takto zobrazíte klíč registrace služby.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Podržte stisknutou klávesu registrace služby bezpečné místo. Budete potřebovat tento klíč, jakož i služby dat šifrovací klíč pro registraci další zařízení s tuto službu. Po získání klíč registrace služby, musíte nakonfigurovat zařízení přes Windows PowerShell pro rozhraní StorSimple.

## <a name="regenerate-the-service-registration-key"></a>Obnovit klíč registrace služby

Bude muset obnovit registračního klíče služby, pokud jsou potřeba k provedení střídání nebo seznam Správci služeb se změnil. Při obnovení klávesu nový klíč se používá pouze k registraci dalších zařízeních. Zařízení, která jste již registrována nejsou ovlivněny tento proces.

Proveďte následující kroky k opětovnému vytvoření registračního klíče služby.

### <a name="to-regenerate-the-service-registration-key"></a>Chcete-li obnovit klíč registrace služby

1. Na stránce **Správce StorSimple služby** klikněte na **Registračního klíče**.

1. V dialogovém okně **Klíč registrace služby** klikněte na **Obnovit**.

1. Zobrazí se potvrzovací zpráva. Kliknutím na **OK** se obnovování pokračovat.

1. Nový klíč registrace služby se zobrazí.

1. Zkopírujte tento klíč a uložte jej zaregistrovali tuto službu na novém zařízení.

1. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-ova-manage-service/image7.png) Toto dialogové okno zavřete.


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [začít](storsimple-ova-deploy1-portal-prep.md) s polem virtuální StorSimple.
    
- Zjistěte, jak [Spravovat StorSimple zařízení](storsimple-ova-web-ui-admin.md).

 
