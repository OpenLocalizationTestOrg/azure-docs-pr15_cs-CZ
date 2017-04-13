<properties 
   pageTitle="Konfigurace MPIO pro zařízení s StorSimple | Microsoft Azure"
   description="Popisuje, jak nakonfigurovat funkce v/v (MPIO) StorSimple zařízení připojené k host spuštěný Windows Server 2012 R2."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Nastavení této funkci pro zařízení s StorSimple

Microsoft vytvořené podpory, aby nápovědy vysoce dostupné, chybám SAN konfigurace sestavení funkce funkce v/v (MPIO) v systému Windows Server. MPIO používá součásti nadbytečné cest – adaptéry kabely a přepínače – vytvoření logických cest mezi serverem a úložné zařízení. Dojde k selhání součásti příčinou logického selhání, použije cest logika alternativní cesty pro vstup a výstup tak, aby aplikace můžete pořád přístup ke svým datům. Dále v závislosti na konfiguraci MPIO můžete taky zlepšíte výkon při znovu Vyrovnávání zatížení přes tyto cesty. Další informace najdete v tématu [Přehled MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO přehled a funkce").  

Maximum-dostupnosti StorSimple řešení by měly být MPIO nakonfigurovány na zařízení s StorSimple. Po instalaci MPIO na serverech Host (hostitel) se systémem Windows serveru 2012 R2 servery pak nevadí vám odkazu, síť nebo selhání rozhraní. 

MPIO volitelnou funkcí v systému Windows Server a není nainstalovaná ve výchozím nastavení. Měli byste mít nainstalované jako funkce pomocí Správce serveru. Toto téma popisuje kroky, řiďte se nainstalovat a používat funkci MPIO na host spuštěný Windows Server 2012 R2 a připojení k StorSimple fyzické zařízení.

>[AZURE.NOTE] **Tento postup platí pro pouze řada StorSimple 8000. MPIO není aktuálně podporován StorSimple virtuální zařízení.**

Budete potřebovat k provedení těchto kroků pro konfiguraci MPIO StorSimple zařízení:

- Krok 1: Instalace MPIO na hostiteli systému Windows Server

- Krok 2: Nakonfigurujte MPIO pro StorSimple svazky

- Krok 3: Připojení StorSimple svazky na hostiteli

- Krok 4: Nastavení MPIO pro dostupnost a vyrovnávání zatížení

V následujících částech jsou uvedeny všech výše uvedené kroky.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Krok 1: Instalace MPIO na hostiteli systému Windows Server

Pokud chcete nainstalovat tuto funkci na hostitele systému Windows Server, postupujte takhle.

#### <a name="to-install-mpio-on-the-host"></a>Chcete-li nainstalovat MPIO na hostiteli

1. Spusťte správce serveru v systému Windows Server hostiteli. Ve výchozím nastavení správce serveru, spustí se členem skupiny Administrators přihlásí k počítači, na kterém běží Windows Server 2012 R2 nebo Windows Server 2012. Pokud správce serveru ještě není otevřený, klikněte na **Start > Správce serveru**.
![Správce serveru](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Klikněte na **správce serveru > řídicí panel > Přidat rolí a funkcí**. Spustí se průvodce **přidáním rolí a funkce** .
![Přidání funkce Průvodce 1 a rolí](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. V průvodci **Přidat rolí a funkcí** postupujte takto:

    - Na stránce, **než začnete** klikněte na **Další**.
    - Na stránce **Vyberte typ instalace** přijměte výchozí nastavení **na základě rolí nebo založeného na funkcích** instalace. Klikněte na tlačítko **Další**. ![Přidat rolí a Průvodce funkcí 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Na stránce **Vyberte cílový server** zvolte **Vybrat server z fondu serveru**. Host serveru by měl zjištěny automaticky. Klikněte na tlačítko **Další**.
    - Na stránce **Vybrat role serveru** klikněte na **Další**.
    - Na stránce **Vyberte funkce** vyberte **Této funkci**a klikněte na tlačítko **Další**. ![Přidat rolí a Průvodce funkcí 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Na stránce **potvrzení vybrané možnosti instalace** potvrďte výběr a pak vyberte **automaticky v případě potřeby restartujte cílovém serveru**, jak je ukázáno v následujícím příkladu. Klikněte na **instalovat**. ![Přidat rolí a Průvodce funkcí 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Zobrazí se oznámení po dokončení instalace. Klikněte na **Zavřít** zavřete průvodce. ![Přidat rolí a Průvodce funkcí 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Nakonfigurujte MPIO pro StorSimple svazky

MPIO, musí být nakonfigurované pro identifikaci StorSimple svazky. Abyste mohli nakonfigurovat MPIO rozpoznatelný StorSimple svazky, proveďte následující kroky.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>Konfigurace MPIO pro StorSimple svazky

1. Otevřete **konfiguraci MPIO**. Klikněte na **správce serveru > řídicí panel > Nástroje > MPIO**.

2. V dialogovém okně **Vlastnosti MPIO** vyberte kartu **Zjistit více cest** .

3. Vyberte **Přidat podporu pro zařízení iSCSI**a potom klikněte na **Přidat**.  
![Vlastnosti MPIO Seznamte se s víc cesty](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Restartujte server po zobrazení výzvy.
5. V dialogovém okně **Vlastnosti MPIO** klikněte na kartu **Zařízení MPIO** . Klikněte na **Přidat**.
    </br>![MPIO vlastnosti MPIO zařízení](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. V dialogovém okně **Přidat podporu MPIO** pod **ID hardwaru zařízení**zadejte číslo svého zařízení. Pořadové číslo zařízení můžete získat přístup ke službě StorSimple správce a přejděte na **zařízení > řídicího panelu**. Pořadové číslo zařízení se zobrazí v pravém podokně **Snadné přehledně uspořádané** řídicího panelu zařízení.
    </br>![Přidání MPIO podpory](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Restartujte server po zobrazení výzvy.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Krok 3: Připojení StorSimple svazky na hostiteli

Po konfiguraci MPIO v systému Windows Server svazky vytvořili na zařízení StorSimple lze připojit a pak využít MPIO pro redundance. Připojení svazku, proveďte následující kroky.

#### <a name="to-mount-volumes-on-the-host"></a>Můžete připojit svazky na hostiteli

1. Otevřete okno **iSCSI vyzývající vlastnosti** na hostiteli systému Windows Server. Klikněte na **správce serveru > řídicí panel > Nástroje > iSCSI vyzývající**.
2. V dialogovém okně **iSCSI vyzývající vlastnosti** klikněte na kartu zjišťování a klikněte na **Zjistit cílový portál**.
3. V dialogovém okně **Seznamte se s cílový portál** postupujte takto:
    
    - Zadejte IP adresu port dat StorSimple zařízení (třeba zadat DATA 0).
    - Klikněte na **OK** se vraťte do dialogového okna **iSCSI vyzývající vlastnosti** .

    >[AZURE.IMPORTANT] **Pokud používáte pro připojení iSCSI privátní sítě, zadejte IP adresu port dat, který je připojen k privátní sítě.**

4. Opakujte kroky 2-3 pro druhý síťové rozhraní (například DATA 1) na vašem zařízení. Mějte na paměti, že tato rozhraní používat pro iSCSI. Další informace o tomto najdete v tématu [změnit síťová rozhraní](storsimple-modify-device-config.md#modify-network-interfaces).
5. V dialogovém okně **iSCSI vyzývající vlastnosti** vyberte kartu **cílů** . Měli byste vidět cílové zařízení StorSimple IQN v části **Zjištěné cíle**.
 ![iSCSI vyzývající karta cíle](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Klikněte na **Připojit** k vytvoření relace iSCSI se svým zařízením StorSimple. Zobrazí se dialogové okno **připojit k cílové** .

7. V dialogovém okně **připojit k cílové** zaškrtněte políčko **Povolit více cestu** . Klikněte na tlačítko **Upřesnit**.

8. V dialogovém okně **Upřesnit nastavení** postupujte takto:                                       
    -    V rozevíracím seznamu **Místní adaptér** vyberte **Microsoft iSCSI vyzývající**.
    -    V rozevíracím seznamu **Vyzývající IP** vyberte IP adresu hostiteli.
    -    V rozevíracím seznamu **Cílový portál** IP vyberte IP rozhraní zařízení.
    -    Klikněte na **OK** se vraťte do dialogového okna **iSCSI vyzývající vlastnosti** .

9. Klikněte na **Vlastnosti**. V dialogovém okně **Vlastnosti** klikněte na **Přidat relaci**.
10. V dialogovém okně **připojit k cílové** zaškrtněte políčko **Povolit více cestu** . Klikněte na tlačítko **Upřesnit**.
11. V dialogovém okně **Upřesnit nastavení** :                                        
    -  V rozevíracím seznamu **Místní adaptér** vyberte Microsoft iSCSI vyzývající.
    -  V rozevíracím seznamu **Vyzývající IP** vyberte IP adresu odpovídající hostiteli. V tomto případě se připojujete dvě síťová rozhraní na zařízení jedné sítě rozhraní na hostiteli. Rozhraní tedy stejný jako kterých byly čerpány pro první relaci.
    -  V rozevíracím seznamu **Cíl portál IP** vyberte IP adresa rozhraní druhý dat povolený na zařízení.
    -  Klikněte na **OK** se vraťte do dialogového okna Vlastnosti vyzývající iSCSI. Přidání druhé relace byste do cíle.

12. **Správa počítače** spustíte tak, že přejdete do **správce serveru > řídicí panel > Správa počítače**. V levém podokně klikněte na **úložiště > Správa disků**. Svazku vytvořil StorSimple zařízení, které jsou zobrazeny na toto tlačítko se zobrazí v části **Správa disků** jako nový discích.

13. Inicializace disk a vytvořte nové hlasitost. Během procesu formát vyberte velikost bloku 64 kB.
![Správa disků](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. V části **Správa disku**klikněte pravým tlačítkem myši na **disku** a vyberte **Vlastnosti**.
15. V části StorSimple Model ### dialogové okno **Vlastnosti zařízení disku více cestami** pole, klikněte na kartu **MPIO** .
![StorSimple 8100 DeviceProp více cestami disku.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. V části **Název DSM** klikněte na **Podrobnosti** a ověřte, že parametry jsou nastavené výchozí parametry. Výchozí hodnoty parametrů jsou:

    - Cesta k ověření období = 30
    - Počet opakování = 3
    - CHOP odebrat období = 20
    - Interval opakování = 1
    - Cesta k ověření povolené = zrušené zaškrtnutí políčka.


>[AZURE.NOTE] **Neměňte výchozí parametry.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Krok 4: Nastavení MPIO pro dostupnost a vyrovnávání zatížení

Pro více cestami na základě dostupnost a vyrovnávání zatížení, musí být více relací ručně přidán deklarovat různé cesty k dispozici. Například pokud hostitel má dvě rozhraní připojení k SAN a má dvě rozhraní připojení k SAN zařízení a pak budete potřebovat čtyři relace nakonfigurována správné cesty permutací (jenom dvě relace-li provést každý dat hostitele rozhraní a je na jiné IP adres podsítí a směrovat).

>[AZURE.IMPORTANT] **Doporučujeme, aby nepoužívejte 1 GbE 10 GbE rozhraní a sítě. Pokud používáte dvě síťová rozhraní, musí být oba rozhraní identickými typu.**

Následující postup popisuje, jak přidat relace při připojení StorSimple zařízení s dvě síťová rozhraní na hostitele mít dvě rozhraní sítě.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Konfigurace MPIO vysoké dostupnosti a vyrovnávání zatížení

1. Provést zjišťování cíl: v dialogovém okně **iSCSI vyzývající vlastnosti** na kartě **zjišťování** klikněte na **Zjistit portálu**.
2. V dialogovém okně **připojit k cílové** zadejte IP adresu některého z síťových rozhraní zařízení.
3. Klikněte na **OK** se vraťte do dialogového okna **iSCSI vyzývající vlastnosti** .

4. V dialogovém okně **iSCSI vyzývající vlastnosti** vyberte kartu **cílů** , zvýrazněte zjištěnou cíl a pak klikněte na **Připojit**. Zobrazí se dialogové okno **připojit k cílové** .

5. V dialogovém okně **připojení do cíle** :
    
    - Nechte vybranou cílové výchozí nastavení pro **Přidání tohoto připojení** do seznamu oblíbených cílů. To bude automaticky pokusí restartujte připojení pokaždé, když tento restartování zařízení.
    - Zaškrtněte políčko **Povolit více cestu** .
    - Klikněte na tlačítko **Upřesnit**.

6. V dialogovém okně **Upřesnit nastavení** :
    - V rozevíracím seznamu **Místní adaptér** vyberte **Microsoft iSCSI vyzývající**.
    - V rozevíracím seznamu **Vyzývající IP** vyberte IP adresu hostiteli.
    - V rozevíracím seznamu **Cíl portál IP** vyberte IP adresu rozhraní dat povolený na zařízení.
    - Klikněte na **OK** se vraťte do dialogového okna Vlastnosti vyzývající iSCSI.

7. Klikněte na **Vlastnosti**a v dialogovém okně **Vlastnosti** klikněte na **Přidat relaci**.

8. V dialogovém okně **připojit k cílové** zaškrtněte políčko **Povolit více cestu** a potom klikněte na **Upřesnit**.

9. V dialogovém okně **Upřesnit nastavení** :
    1. V rozevíracím seznamu **Místní adaptér** vyberte **Microsoft iSCSI vyzývající**.
    2. V rozevíracím seznamu **Vyzývající IP** vyberte IP adresu odpovídající druhé rozhraní na hostiteli.
    3. V rozevíracím seznamu **Cíl portál IP** vyberte IP adresa rozhraní druhý dat povolený na zařízení.
    4. Klikněte na **OK** se vraťte do dialogového okna **iSCSI vyzývající vlastnosti** . Právě jste přidali druhou relaci byste do cíle.

10. Opakujte kroky 8 10 přidat další relace (cesty) do cíle. Dvě rozhraní na hostiteli a dva na zařízení můžete přidat celkem čtyři relace.

11. Po přidání požadované relace (cesty), v dialogovém okně **iSCSI vyzývající vlastnosti** , vyberte cíl a klikněte na **Vlastnosti**. Na kartě relace v dialogovém okně **Vlastnosti** Poznámka čtyři relace identifikátory, které odpovídají permutací možných cest. Zrušit relaci, zaškrtněte políčko vedle identifikátor relace a pak klikněte na tlačítko **Odpojit**.

12. Pokud chcete zobrazit zařízení prezentovány v rámci relace, klikněte na kartu **zařízení** . Abyste mohli nakonfigurovat MPIO zásad pro vybrané zařízení, klikněte na **MPIO**. Zobrazí se dialogové okno **Podrobnosti o zařízení** . Na kartě **MPIO** můžete vybrat odpovídající nastavení **Zásad vyrovnávání zatížení** . Můžete taky zobrazit typ cesty **aktivní** nebo **úsporném režimu** .

## <a name="next-steps"></a>Další kroky

Další informace o [použití služby StorSimple správce k úpravám konfiguraci StorSimple zařízení](storsimple-modify-device-config.md).
 
