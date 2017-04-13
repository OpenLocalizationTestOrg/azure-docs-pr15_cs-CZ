<properties 
   pageTitle="Kontaktujte podporu od Microsoftu | Microsoft Azure"
   description="Naučte se vytvořit žádost o podporu a zahájení relace podpory na zařízení s StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Kontaktujte podporu od Microsoftu

Pokud narazíte na problémy s Microsoft Azure StorSimple řešení, můžete vytvořit žádost o službu pro technické podpory. Online relace s pracovník technické podpory budete taky muset zahájit relaci podpory na zařízení s StorSimple. Tento článek vás provede:

- Jak vytvořit žádost o podporu.
- Jak zahájit relaci podporu v prostředí Windows PowerShell rozhraní StorSimple zařízení.

Seznamte se s [rozsahu StorSimple 8000 řady podpory a informace](https://msdn.microsoft.com/library/mt433077.aspx) před vytvořením žádost o podporu.

## <a name="create-a-support-request"></a>Vytvořit žádost o podporu

Provedení následujících kroků můžete vytvořit žádost o podporu:

#### <a name="to-create-a-support-request"></a>Chcete-li vytvořit žádost o podporu

1. Na [portálu Azure klasické](https://manage.windowsazure.com/), v pravém horním rohu klikněte na název účtu a klikněte na **Kontaktovat podporu Microsoftu**.

    ![Podpora kontaktů MS prostřednictvím ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Budete přesměrováni do nového Azure portálu (portal.azure.com). Klikněte na dlaždici **Nová žádost o podporu** .

    ![Podpora kontakt MS prostřednictvím nového portálu](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Na pravé straně obrazovky zobrazí se podokno **Nová žádost o podporu** . 

    ![Nové podokno žádost o podporu](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. V dialogovém okně **Základy** proveďte následující kroky:                                
    1. V rozevíracím seznamu **Typ problému** vyberte **technické**.
    2. V rozevíracím seznamu vyberte **předplatné** .
    3. V rozevíracím seznamu **Service** vyberte **StorSimple**. 
    4. **Plán podpory** vyberte z rozevíracího seznamu. Budete potřebovat plán placené podpory povolit technické podpory.

4. Klikněte na tlačítko **Další**. Zobrazí se dialogové okno **problém** .

    ![Nové podokno žádost o podporu](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. V dialogovém okně **problém** proveďte následující kroky:

    1.  V rozevíracím seznamu vyberte úroveň **závažnosti** .
    2.  V rozevíracím seznamu vyberte **Typ problému** .
    3.  V rozevíracím seznamu vyberte **kategorie** . 
    4.  V dialogovém okně **informace** popište svůj problém.
    5.  V poli **Čas snímku** označuje datum, čas a časové pásmo, které odpovídá poslední výskyt problém.
    6.  V části **soubor nahrát**klepněte na ikonu složky umožňuje přecházet na vašem balení podpory.
    7.  **Sdílení diagnostických informací** zaškrtněte políčko.

6. Klikněte na tlačítko **Další**. Zobrazí se dialogové okno **informace o kontaktu** .

    ![Nové podokno žádost o podporu](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Zadejte svoje kontaktní informace a vyberte způsob kontaktování (telefon nebo e-mailu). 

8. **Uložit změny u kontaktu pro budoucí odborná** zaškrtněte políčko.

9. Klikněte na **vytvořit**.

Poté, co jste odeslali vaši žádost, pracovníka podpory vás co nejdříve pokračujte žádosti o kontaktovat.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Spuštění relace s podporu v prostředí Windows PowerShell pro StorSimple

Při odstraňování všech problémů, ke kterým může dojít k ní StorSimple, musíte se provozovat s týmem Microsoft Support. Microsoft Support potřebovat relaci podpory přihlásit do zařízení. 

Provedení následujících kroků můžete zahájit relaci podpory:

#### <a name="to-start-a-support-session"></a>Spuštění relace s podpory

1. Přístup k zařízení přímo pomocí sériové konzoly nebo prostřednictvím rámci těchto relací ze vzdáleného počítače. K tomuto účelu postupujte podle pokynů v tématu [Použití nátěrové připojení konzoly sériové zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. V relaci, která se otevře stiskněte klávesu **Enter** získat příkazový řádek.

3. V nabídce sériové konzola vyberte možnost 1, **Přihlaste se pomocí plný přístup**.

4. Do příkazového řádku zadejte následující: 

    `Password1`

5. Do příkazového řádku zadejte tento příkaz:

    `Enable-HcsSupportAccess`

6. Šifrovaného řetězce se seznámíte se. Zkopírujte tento řetězec do textového editoru, jako je Poznámkový blok.

7. Uložte tento řetězec a jeho odeslání v e-mailové zprávy k Microsoft Support. 

> [AZURE.IMPORTANT] Podpora přístupu můžete zakázat spuštěním `Disable-HcsSupportAccess`. Zařízení StorSimple taky pokusí zakázání přístupu podpory 8 hodin od spuštěná relace. Je nejvhodnější změnit svoje přihlašovací údaje zařízení StorSimple po zahájení relace podpory.
