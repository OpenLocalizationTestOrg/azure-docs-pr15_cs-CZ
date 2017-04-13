<properties 
   pageTitle="Správa zařízení s StorSimple snímek správce | Microsoft Azure"
   description="Popisuje, jak používat modulu snap-in konzoly MMC StorSimple snímek správce k připojení a správa StorSimple zařízení."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Připojení a StorSimple zařízení můžete spravovat pomocí StorSimple snímek správce

## <a name="overview"></a>Základní informace

Můžete uzlů v podokně StorSimple snímek správce **obor** ověření importovaná data StorSimple zařízení a obnovení připojení úložiště zařízení. Navíc když kliknete na uzel **zařízení** , uvidíte seznam připojených zařízení a odpovídající informací o stavu v podokně **výsledků** .

![Připojení zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Obrázek 1: Připojení StorSimple snímek Správce zařízení** 

V závislosti na nastavení **zobrazení** v podokně **výsledků** zobrazí tyto informace o jednotlivých zařízeních. (Další informace o konfiguraci zobrazení, přejděte do [zobrazení nabídky](storsimple-use-snapshot-manager.md#view-menu).


| Výsledky sloupce  |Popis          |
|:----------------|:--------------------| 
| Jméno            | Název zařízení nakonfigurovaná v portálu Azure klasické|
| Model           | Číslo modelu zařízení|
| Verze         | Verzi softwaru nainstalovaného na zařízení |
| Stav          | Zda je k dispozici zařízení |
| Naposled synchronizoval     | Datum a čas, kdy byla zařízení poslední synchronizace |
| Sériové ne.      | Pořadové číslo pro zařízení |
 
Pokud pravým tlačítkem myši na uzel **zařízení** v podokně **obor** , můžete vybrat z následujících akcí:

- Přidání nebo nahrazení zařízení 
- Připojte zařízení a ověřte importy 
- Aktualizace propojených zařízení 

Pokud kliknete na uzel **zařízení** a klikněte pravým tlačítkem myši název zařízení v podokně **výsledků** , můžete vybrat z následujících akcí:

- Ověření zařízení 
- Zobrazení podrobností o zařízení 
- Aktualizace zařízení 
- Odstranění konfigurace zařízení 
- Změna hesla zařízení

>[AZURE.NOTE] Všechny tyto akce jsou dostupné v podokně **akcí** .
 
Tento kurz vysvětluje, jak lze pomocí Správce snímek StorSimple připojení a správa zařízení a dělat tyto věci:

- Přidání nebo nahrazení zařízení 
- Připojte zařízení a ověřte importy 
- Aktualizace propojených zařízení 
- Ověření zařízení 
- Zobrazení podrobností o zařízení 
- Aktualizace jednotlivé zařízení 
- Odstranění konfigurace zařízení 
- Změna hesla vypršela platnost zařízení
- Nahrazení selhalo zařízení

>[AZURE.NOTE] Obecné informace o používání rozhraní StorSimple snímek správce přejděte na [Správce snímek StorSimple uživatelského rozhraní](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Přidání nebo nahrazení zařízení

Použijte následující postup přidání nebo nahrazení StorSimple zařízení.

#### <a name="to-add-or-replace-a-device"></a>Přidání nebo nahrazení zařízení

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem myši na uzel **zařízení** a pak klikněte na **Konfigurovat zařízení**. Zobrazí se dialogové okno **Konfigurovat zařízení** .

    ![Konfigurace StorSimple zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. V rozevíracím seznamu **zařízení** vyberte IP adresu zařízení nebo virtuální. 

4. Do textového pole **heslo** zadejte heslo správce StorSimple snímek, který jste vytvořili pro zařízení Azure klasické portálu. Klikněte na **OK**. Správce snímek StorSimple vyhledá zařízení, které jste určili. 

    - Pokud je k dispozici, přidá StorSimple snímek Správce připojení. 

    - Pokud zařízení není k dispozici z nějakého důvodu, vrátí funkce StorSimple snímek správce chybová zpráva. Kliknutím na **OK** zavřete chybovou zprávu a potom klikněte na **Zrušit** a zavřete dialogové okno **Konfigurovat zařízení** .

## <a name="connect-a-device-and-verify-imports"></a>Připojte zařízení a ověřte importy

Pomocí následujícího postupu připojte zařízení StorSimple a ověřit, že všechny existující skupiny hlasitost s přidruženými zálohy naimportují.

#### <a name="to-connect-a-device-and-verify-imports"></a>Připojte zařízení a ověření importuje

1. Připojte zařízení správci snímek StorSimple, postupujte podle pokynů uvedených v článku Přidání nebo nahrazení zařízení. Když se připojí k zařízení, StorSimple snímek správce odpoví následujícím způsobem:

    - Pokud zařízení není k dispozici z nějakého důvodu, vrátí funkce StorSimple snímek správce chybová zpráva. 

   - Pokud je k dispozici, přidá StorSimple snímek Správce připojení. Vyberte zařízení, se zobrazí v podokně **výsledků** a pole stav označuje, že zařízení při **k dispozici**. Správce snímek StorSimple importuje všechny skupiny hlasitost nakonfigurován pro zařízení, za předpokladu, že skupin hlasitost přidruženými zálohy. Zálohování zásady nebudou importovány. Hlasitost skupin, které nemají přidružené zálohy nebudou importovány.

2. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

3. Klikněte pravým tlačítkem myši na nejvyšší uzel podokna **rozsahu** a potom klikněte na **Přepnout importy zobrazení**.

    ![Vyberte přepnout importy zobrazení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. **Přepnout importy zobrazení** dialogové okno, zobrazující stav importovaných hlasitost skupiny a zálohování. Klikněte na **OK**. 

Po úspěšném importovaného hlasitost skupiny a zálohy můžete StorSimple snímek správce pro správu, stejně, jako by Správa skupin hlasitost a zálohy, které jste vytvořili a nakonfigurovali s StorSimple snímek správce. 

## <a name="refresh-connected-devices"></a>Aktualizace propojených zařízení

Pomocí následujícího postupu synchronizovat připojená StorSimple zařízení s StorSimple snímek správce.

####<a name="to-refresh-connected-devices"></a>Chcete-li aktualizovat připojená zařízení

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem na **zařízení**a pak klikněte na **Aktualizovat zařízení**. Tato funkce se synchronizuje připojená zařízení s StorSimple snímek správce, aby mohli zobrazit hlasitost skupiny a zálohování, včetně poslední doplňky. 

    ![Aktualizace StorSimple zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
Akce **Aktualizovat zařízení** načte všechny nové skupiny hlasitost a všechny přidružené zálohy z připojeného zařízení. Na rozdíl od **Prohledat svazky** akce umožňující uzel **svazky** **Aktualizace zařízení** neobnoví záložní registru.

## <a name="authenticate-a-device"></a>Ověření zařízení

Pomocí následujícího postupu ověření StorSimple zařízení s StorSimple snímek správce.

#### <a name="to-authenticate-a-device"></a>Ověření zařízení

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte na **zařízení**.

3. V podokně **výsledků** klikněte pravým tlačítkem myši na název zařízení a pak klikněte na **Ověřit**.

4. Zobrazí se dialogové okno **Ověřit** . Zadejte heslo zařízení a pak klikněte na **OK**.

    ![Dialogové okno ověření](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Zobrazení podrobností o zařízení

Pomocí následujícího postupu a zobrazit podrobnosti o zařízení StorSimple, v případě potřeby synchronizovat s StorSimple snímek Správce zařízení.

#### <a name="to-view-and-resynchronize-device-details"></a>Prohlížení a synchronizovat podrobnosti o zařízení

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte na **zařízení**.

3. V podokně **výsledků** klikněte pravým tlačítkem myši na název zařízení a pak klikněte na **Podrobnosti**. 

4 zobrazí se dialogové okno na **Podrobnosti o zařízení** . Toto pole zobrazuje název, model, verze, pořadové číslo, stav, cílové iSCSI kvalifikovaný název IQN () a poslední synchronizace datum a čas. 

   - Klikněte na tlačítko **synchronizován** k synchronizaci zařízení.

   - Klikněte na **OK** nebo **Zrušit** a zavřete dialogové okno.

    ![Podrobnosti o zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Aktualizace jednotlivé zařízení

Pomocí následujícího postupu synchronizaci jednotlivé StorSimple zařízení s StorSimple snímek správce.

#### <a name="to-refresh-a-device"></a>Chcete-li aktualizovat zařízení

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte na **zařízení**. 

3. V podokně **výsledků** klikněte pravým tlačítkem myši na název zařízení a pak klikněte na **Aktualizovat zařízení**. Tato funkce se synchronizuje zařízení s StorSimple snímek správce. 

## <a name="delete-a-device-configuration"></a>Odstranění konfigurace zařízení

Chcete-li odstranit jednotlivé konfigurace zařízení StorSimple StorSimple snímek ve Správci systému pomocí následujícího postupu.

#### <a name="to-delete-a-device-configuration"></a>Chcete-li odstranit konfigurace zařízení

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte na **zařízení**. 

3. V podokně **výsledků** klikněte pravým tlačítkem myši na název zařízení a pak klikněte na **Odstranit**. 

4. Zobrazí se tato zpráva. Klikněte na **Ano** a odstranit konfiguraci nebo klikněte na **bez** následované.

    ![Odstranění konfigurace zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Změna hesla vypršela platnost zařízení

Je nutné zadat heslo pro ověření StorSimple zařízení s StorSimple snímek správce. Při použití rozhraní prostředí Windows PowerShell pro nastavení zařízení nakonfigurujete toto heslo. Však můžete vypršení platnosti hesla. V takovém případě můžete Azure klasické portálu změnit heslo. Potom protože zařízení nakonfigurovanou ve Správci snímek StorSimple před vypršením heslo, je třeba znovu ověřit zařízení ve Správci StorSimple snímek. 

#### <a name="to-change-the-expired-password"></a>Chcete-li změnit vypršela platnost hesla

1. Na portálu Azure klasické spusťte službu StorSimple správce.

2. Klikněte na **zařízení** > **Konfigurovat** pro zařízení.

3. Přejděte dolů do části StorSimple snímek správce. Zadejte heslo, které je znaků 14-15. Ujistěte se, že heslo obsahuje kombinaci velká, malá písmena, číselné a speciální znaky.

4. Zadejte znovu heslo pro potvrzení.

5. Klepněte na tlačítko **Uložit** v dolní části stránky.

#### <a name="to-re-authenticate-the-device"></a>Znovu ověřovat zařízení

1. Spusťte správce StorSimple snímek.

2. V podokně **obor** klikněte na **zařízení**. Seznam nakonfigurované zařízení se zobrazí v podokně **výsledků** . 

3. Vyberte zařízení, klikněte pravým tlačítkem myši a potom klikněte na **Ověřit**.

4. V okně **Ověřit** zadejte nové heslo. 

5. Vyberte zařízení, klikněte pravým tlačítkem myši a vyberte **aktualizovat zařízení**. Tato funkce se synchronizuje zařízení s StorSimple snímek správce. 

## <a name="replace-a-failed-device"></a>Nahrazení selhalo zařízení

Pokud je zařízení StorSimple selže a nahrazuje zařízení úsporném režimu (převzetí), použít podle těchto kroků pro připojení na nové zařízení a zobrazit související zálohování.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Připojení k nové zařízení po překlopení

1. Znovu iSCSI připojení na nové zařízení. Pokyny najdete v tématu "krok 7: připojení, inicializace a formát svazku" v [zařízení StorSimple v místním nasazení](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Pokud na nové zařízení StorSimple má stejné IP adresu jako starý, bude pravděpodobně možné připojení staré konfigurace. 

2. Zastavte službu pro správu Microsoft StorSimple:

    1. Spusťte správce serveru.

    2. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**. 

    3. V okně **služby** vyberte **Microsoft StorSimple Management Service**. 

    4. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Zastavit službu**. 

3. Odebrání informace o konfiguraci související s staré zařízení: 

    1. V Průzkumníku souborů vyhledejte C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Odstraňte soubory ve složce BACatalog. 

4. Restartujte službu pro správu Microsoft StorSimple: 

    1. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**. 

    2. V okně **služby** vyberte **Microsoft StorSimple Management Service**. 

    3. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Restartovat službu**. 

5. Spusťte správce StorSimple snímek. 

6. Pokud chcete nakonfigurovat nové StorSimple zařízení, postupujte podle pokynů v kroku 2: Připojte zařízení StorSimple [nasazení StorSimple snímek](storsimple-snapshot-manager-deployment.md)správce. 

7. Klikněte pravým tlačítkem myši nejvyšší úrovně v podokně **rozsahu** (StorSimple snímek správce v příkladu) a potom klikněte na **Přepnout importy zobrazení**. 

8. Zpráva zobrazí, když se zobrazí ve Správci snímek StorSimple importovaných hlasitost skupiny a zálohování. Klikněte na **OK**. 

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Zjistěte, jak lze [pomocí Správce StorSimple snímku můžete zobrazit a spravovat svazky](storsimple-snapshot-manager-manage-volumes.md).

