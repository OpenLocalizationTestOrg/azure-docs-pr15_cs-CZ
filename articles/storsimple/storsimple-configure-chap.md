<properties 
   pageTitle="Konfigurace CHAP pro zařízení s StorSimple | Microsoft Azure"
   description="Popisuje, jak nakonfigurovat ověřovací kód programu ověřování CHAP (Handshake Protocol) na StorSimple zařízení."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurace CHAP pro zařízení s StorSimple

Tento kurz vysvětluje, jak nakonfigurovat CHAP StorSimple zařízení. Postup uvedené v tomto článku platí pro StorSimple 8000 řady, jakož i StorSimple 1200 zařízení.

CHAP zastupuje protokol ověřovací kód programu. Je režimu ověřování používaný službou k ověření identity vzdálené klienty. Ověření se podle sdílené heslo nebo tajná. CHAP může být jednosměrné (Jednosměrný) nebo vzájemné (obousměrné). Jednosměrný CHAP je při cílem způsobit. Vzájemné nebo zpětného protokol na druhé straně vyžaduje, aby cílové ověření vyzývající a poté vyzývající ověřte cíl. Ověřování vyzývající lze provést bez cílové ověření. Však cílové ověřování provádět jenom v případě, že vyzývající ověřování také implementovaná. 

Osvědčený doporučujeme použít CHAP zvýšit zabezpečení iSCSI.

>[AZURE.NOTE] Mějte na paměti, že IPSEC není aktuálně podporován StorSimple zařízení.

Nastavení CHAP na zařízení StorSimple možné konfigurovat následujícími způsoby:

- Jednosměrný nebo jednosměrné ověřování

- Obousměrné nebo vzájemné nebo zpětného ověřování

V každém z těchto případů portálu pro zařízení a software vyzývající iSCSI server vyžaduje konfiguraci. Podrobný postup pro tuto konfiguraci jsou popsané v následující kurz.

## <a name="unidirectional-or-one-way-authentication"></a>Jednosměrný nebo jednosměrné ověřování

Jednosměrný ověřování ověří cíl vyzývající. Toto ověření vyžaduje konfigurace nastavení vyzývající CHAP na zařízení StorSimple a iSCSI vyzývající software na hostiteli. Podrobné pokyny pro StorSimple zařízení a Windows host jsou popsána níže.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Konfigurace zařízení jednosměrné ověřování

1. Na portálu na stránce **zařízení** Azure klasické klikněte na kartu **Konfigurovat** .

    ![Autor CHAP](./media/storsimple-configure-chap/IC740943.png)

2. Přejděte dolů na této stránce a v části **CHAP vyzývající** :
                                                    
    1. Zadejte uživatelské jméno pro CHAP vyzývající.

    2. Zadejte heslo pro CHAP vyzývající.

         > [AZURE.IMPORTANT] Uživatelské jméno CHAP musí obsahovat méně než 233 znaků. Heslo CHAP musí být 12 až 16 znaků. Neúspěšné ověření na hostiteli Windows povede delší uživatelské jméno a heslo.
    
    3. Potvrďte heslo.

4. Klikněte na **Uložit**. Zobrazí se potvrzovací zpráva. Kliknutím na **OK** uložte změny.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Pro nastavení jednosměrné ověřování systému Windows server Host (hostitel)

1. Na hostitelském serveru Windows spusťte iSCSI vyzývající.

2. V okně **iSCSI vyzývající vlastnosti** proveďte následující kroky:
                                                    
    1. Klikněte na kartu **vyhledávání** .

        ![Vlastnosti vyzývající iSCSI](./media/storsimple-configure-chap/IC740944.png)

    2. Klikněte na tlačítko **zjišťování portálu**.

3. V dialogovém okně **Seznamte se s cílový portál** :
                                                    
    1. Zadejte IP adresu vašeho zařízení.

    3. Klikněte na tlačítko **Upřesnit**.

        ![Seznamte se s cílový portál](./media/storsimple-configure-chap/IC740945.png)

4. V dialogovém okně **Upřesnit nastavení** :
                                                    
    1. Zaškrtněte políčko **Povolit CHAP přihlásit** .

    2. Do pole **název** zadejte uživatelské jméno, které jste zadali pro vyzývající CHAP klasické portálu.

    3. V poli **cílové tajná** zadejte heslo, které jste zadali pro vyzývající CHAP klasické portálu.

    4. Klikněte na **OK**.

        ![Upřesňující nastavení Obecné](./media/storsimple-configure-chap/IC740946.png)

5. Na kartě **cíle** okna **iSCSI vyzývající vlastnosti** se stav zařízení objevit jako **Připojit**. Pokud používáte zařízení StorSimple 1200, pak každý hlasitost bude připojit jako cíle iSCSI jak je ukázáno v následujícím příkladu. Proto kroky 3-4 při instalaci potřebovat se opakuje každý hlasitost.

    ![Svazky připojit jako samostatný cíle](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Pokud změníte název iSCSI, nový název se použije pro nové iSCSI relace. Pro existující relací, dokud se odhlásíte a přihlásit nejsou znovu použít nové nastavení.

Další informace o konfiguraci CHAP na hostitelském serveru Windows najdete [Další informace](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Obousměrné nebo vzájemné ověřování

V ověřování obousměrný cíl ověří vyzývající a potom vyzývající ověří cíl. Při této akci musí uživatel pro konfiguraci nastavení vyzývající CHAP i zpětně CHAP nastavení zařízení a iSCSI vyzývající software na hostiteli. Následující postupy popisují kroky pro nastavení vzájemné ověřování na zařízení a na hostiteli Windows.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Konfigurace zařízení vzájemné ověřování

1. Na portálu na stránce **zařízení** Azure klasické klikněte na kartu **Konfigurovat** .

    ![Cíl CHAP](./media/storsimple-configure-chap/IC740948.png)

2. Přejděte dolů na této stránce a v části **CHAP cíl** :
                                                    
    1. Zadejte **zpětné CHAP uživatelské jméno** pro zařízení.

    2. Zadejte **heslo Převrátit CHAP** pro svoje zařízení.

    3. Potvrďte heslo.

3. V části **CHAP vyzývající** :
                                                
    1. Zadejte **uživatelské jméno** pro svoje zařízení.

    1. Zadejte **heslo** pro svoje zařízení.

    3. Potvrďte heslo.

4. Klikněte na **Uložit**. Zobrazí se potvrzovací zpráva. Kliknutím na **OK** uložte změny.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Konfigurace ověřování obousměrný v systému Windows server Host (hostitel)

1. Na hostitelském serveru Windows spusťte iSCSI vyzývající.

2. V okně **iSCSI vyzývající vlastnosti** klikněte na kartu **Konfigurace** .

3. Klikněte na tlačítko **CHAP**.

4. V dialogovém okně **iSCSI vyzývající vzájemné CHAP tajná** :
                                                    
    1. Zadejte **Převrátit CHAP heslo** , které jste nakonfigurovali Azure klasické portálu.

    2. Klikněte na **OK**.

        ![tajná vzájemné CHAP iSCSI Autor](./media/storsimple-configure-chap/IC740949.png)

5. Klikněte na kartu **cílů** .

6. Klikněte na tlačítko **Připojit** . 

7. V dialogovém okně **Připojení k cílové** klikněte na **Upřesnit**.

8. V dialogovém okně **Upřesnit vlastnosti** :
                                                    
    1. Zaškrtněte políčko **Povolit CHAP přihlásit** .

    2. Do pole **název** zadejte uživatelské jméno, které jste zadali pro vyzývající CHAP klasické portálu.

    3. V poli **cílové tajná** zadejte heslo, které jste zadali pro vyzývající CHAP klasické portálu.

    4. Zaškrtněte políčko **provést vzájemné ověření** .

        ![Vzájemné ověřování upřesňující nastavení](./media/storsimple-configure-chap/IC740950.png)

    5. Klikněte na **OK** dokončete konfiguraci CHAP
     
Další informace o konfiguraci CHAP na hostitelském serveru Windows najdete [Další informace](#additional-considerations).

## <a name="additional-considerations"></a>Další informace

Funkce **Rychlé připojení** nepodporuje připojení, které mají CHAP povolené. Pokud je povoleno CHAP, ujistěte se, použijte tlačítko **Připojit** , která je dostupná na kartě **cíle** se připojit k cíl.

![Připojení k cílové](./media/storsimple-configure-chap/IC740947.png)

V dialogovém okně **připojit k cílové** , které jsou uvedeny zaškrtněte políčko **Přidat toto připojení do seznamu oblíbených cílů** . Zajistíte tím, že pokaždé, když restartování počítače, je pokus o obnovení připojení k cílům iSCSI Oblíbené.

## <a name="errors-during-configuration"></a>Chyby při konfiguraci

Pokud je nesprávná konfigurace CHAP, pak budete pravděpodobně zobrazí chybová zpráva **Neúspěšné ověření** .

## <a name="verification-of-chap-configuration"></a>Ověření CHAP konfigurace

Můžete ověřit, že CHAP používá proveďte následující kroky.

#### <a name="to-verify-your-chap-configuration"></a>Ověření CHAP konfigurace

1. Klikněte na položku **Oblíbené plány**.

2. Vyberte cíl, pro kterou jste povolili ověřování.

3. Klikněte na **Podrobnosti**.

    ![iSCSI vyzývající vlastnosti Oblíbené cíle](./media/storsimple-configure-chap/IC740951.png)

4. V dialogovém okně **Podrobnosti o oblíbených cílové** Všimněte hodnota zadaná do pole **ověření** . Pokud konfigurace byl úspěšný, by například **CHAP**.

    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Další kroky

- Další informace o [StorSimple zabezpečení](storsimple-security.md).

- Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
