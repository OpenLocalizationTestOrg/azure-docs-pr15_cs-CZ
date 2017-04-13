<properties 
   pageTitle="Konfigurace MPIO na hostitele virtuální pole StorSimple | Microsoft Azure"
   description="Popisuje, jak nakonfigurovat funkce v/v (MPIO) pro váš StorSimple virtuální pole připojené k host spuštěný Windows Server 2012 R2."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfigurace této funkci na hostiteli systému Windows Server pro pole virtuální StorSimple

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak nainstalovat funkce této funkci (MPIO) hostitele systému Windows Server, použít konkrétní konfigurace nastavení pro jen StorSimple svazky a pak ověřte MPIO pro StorSimple svazky. Postup předpokládá, že své StorSimple 1200 virtuální pole s dvě síťová rozhraní připojen k hostiteli systému Windows Server s dvě síťová rozhraní. Informace obsažené v tomto článku platí pouze pro virtuální matice. Další informace o StorSimple 8000 řady zařízení přejděte na [Konfigurovat MPIO StorSimple hostitele](storsimple-configure-mpio-windows-server.md). 

Funkce MPIO v systému Windows Server pomáhá vytvořit vysoce dostupné, chybám úložiště konfigurace. MPIO používá součásti nadbytečné cest – adaptéry kabely a přepínače – vytvoření logických cest mezi serverem a úložné zařízení. Dojde k selhání součásti příčinou logického selhání, použije cest logika alternativní cesty pro vstup a výstup tak, aby aplikace můžete pořád přístup ke svým datům. Dále v závislosti na konfiguraci MPIO můžete taky zlepšíte výkon při znovu Vyrovnávání zatížení přes tyto cesty. Další informace najdete v tématu [Přehled MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO přehled a funkce").  

Maximum-dostupnosti StorSimple řešení nakonfigurujte MPIO v systému Windows Server hosts připojené k vaší StorSimple 1200 virtuální pole (označovaná taky jako místní virtuální zařízení). Servery hostitele klikněte nevadí vám odkazu, síť nebo selhání rozhraní. 

Potřebujete ke konfiguraci MPIO takto: 

- Konfigurace požadavky

- Krok 1: Instalace MPIO na hostiteli systému Windows Server

- Krok 2: Nakonfigurujte MPIO pro StorSimple svazky

- Krok 3: Připojení StorSimple svazky na hostiteli

V následujících částech jsou uvedeny všech výše uvedené kroky.


## <a name="prerequisites"></a>Zjistit předpoklady pro

Tato část Podrobné informace o konfiguraci požadavcích na hostiteli systému Windows Server a virtuální pole.

### <a name="on-windows-server-host"></a>V systému Windows Server Host (hostitel)

-  Zajistěte hostitele systému Windows Server 2 síťová rozhraní povolena.


### <a name="on-storsimple-virtual-array"></a>Virtuální matici StorSimple

- Virtuální pole by měl nakonfigurování jako iSCSI serveru. Další informace najdete v tématu [nastavení virtuální matice jako iSCSI serveru](storsimple-ova-deploy3-iscsi-setup.md). Jedno nebo více rozhraní sítě je třeba povolit u pole.   

- Rozhraní sítě virtuální matici by měl být dostupný od hostitele systému Windows Server.

- Jeden nebo více svazky se vytvoří na své StorSimple virtuální pole. Další informace najdete v tématu [Přidání svazku](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) na své StorSimple 1200 virtuální pole. V tomto postupu jsme vytvořili 3 svazky (2 místně připnutých a 1 vrstvené objem jak je znázorněno níže) pro virtuální matici.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardwarová konfigurace StorSimple virtuální matice

Následující obrázek ukazuje konfigurace hardwaru dostupnost a vyrovnávání zatížení cest pro vašeho hostitele systému Windows Server a StorSimple virtuální pole použité v tomto postupu.  

![konfigurace hardwaru MPIO](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Jak ukazuje předchozí obrázek:

- Vaše StorSimple virtuální je argument matice zřízení na Hyper-V jedné uzel aktivní zařízení nakonfigurování jako iSCSI serveru.

- Dvě rozhraní virtuální sítě jsou povoleny na své pole. V místní web uživatelského rozhraní své virtuální pole 1200 povolte dvě síťová rozhraní se tak, že přejdete do **Nastavení sítě** , jak je ukázáno v následujícím příkladu:

    ![Síťová rozhraní zapnuta 1200](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Podívejte se adresy IPv4 pro rozhraní povolené síťové (Ethernet, 2 Ethernet ve výchozím nastavení) a uložit pro pozdější použití na hostiteli.

- Dvě rozhraní sítě jsou povoleny na hostitele systému Windows Server. Pokud připojeného rozhraní pro Host (hostitel) a pole, které jsou na stejné podsítě, pak bude 4 cesty k dispozici. To bylo u tohoto postupu. Ale pokud každé síťové rozhraní rozhraní Host (hostitel) a pole se na různé IP adres podsítí (a nikoli směrovatelné), potom jenom 2 cesty budou dostupné.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Krok 1: Instalace MPIO na hostiteli systému Windows Server

MPIO volitelnou funkcí v systému Windows Server a není nainstalovaná ve výchozím nastavení. Měli byste mít nainstalované jako funkce pomocí Správce serveru. Pokud chcete nainstalovat tuto funkci na hostitele systému Windows Server, postupujte takhle.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Nakonfigurujte MPIO pro StorSimple svazky

MPIO, musí být nakonfigurované pro identifikaci StorSimple svazky. Abyste mohli nakonfigurovat MPIO rozpoznatelný StorSimple svazky, proveďte následující kroky.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Krok 3: Připojení StorSimple svazky na hostiteli

Po konfiguraci MPIO v systému Windows Server svazky vytvořil pole StorSimple lze připojit a pak využít MPIO pro redundance. Připojení svazku, proveďte následující kroky.

#### <a name="to-mount-volumes-on-the-host"></a>Můžete připojit svazky na hostiteli

1. Otevřete okno **iSCSI vyzývající vlastnosti** na hostiteli systému Windows Server. Klikněte na **správce serveru > řídicí panel > Nástroje > iSCSI vyzývající**.
2. V dialogovém okně **iSCSI vyzývající vlastnosti** klikněte na kartu zjišťování a klikněte na **Zjistit cílový portál**.
3. V dialogovém okně **Seznamte se s cílový portál** postupujte takto:
    
    - Zadejte IP adresu první rozhraní povolené síťové na své StorSimple virtuální pole. Ve výchozím nastavení to bude **Ethernet**. 
    - Klikněte na **OK** se vraťte do dialogového okna **iSCSI vyzývající vlastnosti** .

    >[AZURE.IMPORTANT] **Pokud používáte pro připojení iSCSI privátní sítě, zadejte IP adresu port dat, který je připojen k privátní sítě.**

4. Opakujte kroky 2-3 pro druhý síťové rozhraní (například Ethernet 2) na své pole. 

5. V dialogovém okně **iSCSI vyzývající vlastnosti** vyberte kartu **cílů** . Pro virtuální matice byste měli vidět každý hlasitost povrch jako cíl v části **Zjištěné cíle**. V tomto případě by zjistil tři cílů (odpovídající tři svazky).

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Klikněte na **Připojit** k vytvoření relace iSCSI s polem StorSimple. Zobrazí se dialogové okno **připojit k cílové** . Zaškrtněte políčko **Povolit více cestu** . Klikněte na tlačítko **Upřesnit**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. V dialogovém okně **Upřesnit nastavení** postupujte takto:                                       
    -    V rozevíracím seznamu **Místní adaptér** vyberte **Microsoft iSCSI vyzývající**.
    -    V rozevíracím seznamu **Vyzývající IP** vyberte IP adresu hostiteli.
    -    V rozevíracím seznamu **Cílový portál** IP vyberte IP rozhraní pole.
    -    Klikněte na **OK** se vraťte do dialogového okna **iSCSI vyzývající vlastnosti** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Klikněte na **Vlastnosti**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. V dialogovém okně **Vlastnosti** klikněte na **Přidat relaci**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. V dialogovém okně **připojit k cílové** zaškrtněte políčko **Povolit více cestu** . Klikněte na tlačítko **Upřesnit**.
11. V dialogovém okně **Upřesnit nastavení** :                                        
    -  V rozevíracím seznamu **Místní adaptér** vyberte Microsoft iSCSI vyzývající.
    -  V rozevíracím seznamu **Vyzývající IP** vyberte IP adresu odpovídající hostiteli. V tomto případě se připojujete dvě síťová rozhraní pro matici jedné sítě rozhraní na hostiteli. Rozhraní tedy stejný jako kterých byly čerpány pro první relaci.
    -  V rozevíracím seznamu **Cíl portál IP** vyberte IP adresa rozhraní druhý dat povolený na pole.
    -  Klikněte na **OK** se vraťte do dialogového okna Vlastnosti vyzývající iSCSI. Přidání druhé relace byste do cíle.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Po přidání požadované relace (cesty), v dialogovém okně **iSCSI vyzývající vlastnosti** , vyberte cíl a klikněte na **Vlastnosti**. Na kartě relace v dialogovém okně **Vlastnosti** Poznámka čtyři relace identifikátory, které odpovídají permutací možných cest. Zrušit relaci, zaškrtněte políčko vedle identifikátor relace a pak klikněte na tlačítko **Odpojit**.
 
    - Pokud chcete zobrazit zařízení prezentovány v rámci relace, klikněte na kartu **zařízení** . Abyste mohli nakonfigurovat MPIO zásad pro vybrané zařízení, klikněte na **MPIO**. **
    -  Podrobnosti o** se zobrazí dialogové okno. Na **MPIO** karta, můžete vybrat příslušné **zásad vyrovnávání zatížení** nastavení. Můžete taky zobrazit **aktivní** nebo **zadejte cestu úsporném režimu **.

10. Zopakujte krok 8-11 přidat další relace (cesty) do cíle. Dvě rozhraní na hostiteli a dvě virtuální matici můžete přidat celkem čtyři relací pro každý cíl. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Je třeba opakujte tento postup u každého objem (ploch jako cíle).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. **Správa počítače** spustíte tak, že přejdete do **správce serveru > řídicí panel > Správa počítače**. V levém podokně klikněte na **úložiště > Správa disků**. Svazky vytvořil StorSimple virtuální pole, které jsou viditelné na toto tlačítko se zobrazí v části **Správa disků** jako nový discích.

13. Inicializace disk a vytvořte nové hlasitost. Během procesu formát vyberte velikost jednotku přidělení (Austrálie) 64 kB. Opakujte postup pro všechny dostupné svazky.

    ![Správa disků](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. V části **Správa disku**klikněte pravým tlačítkem myši na **disku** a vyberte **Vlastnosti**.

15. V dialogovém okně **Vlastnosti zařízení disku více cestami** klikněte na kartu **MPIO** .

    ![Vlastnosti disku](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. V části **Název DSM** klikněte na **Podrobnosti** a ověřte, že parametry jsou nastavené výchozí parametry. Výchozí hodnoty parametrů jsou:

    - Cesta k ověření období = 30
    - Počet opakování = 3
    - CHOP odebrat období = 20
    - Interval opakování = 1
    - Cesta k ověření povolené = zrušené zaškrtnutí políčka.

    >[AZURE.NOTE] **Neměňte výchozí parametry.**


## <a name="next-steps"></a>Další kroky

Další informace o [použití služby StorSimple správce ke správě své StorSimple virtuální pole](storsimple-ova-manager-service-administration.md).
 
