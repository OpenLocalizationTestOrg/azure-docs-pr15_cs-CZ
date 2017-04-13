<properties 
   pageTitle="Prostředí PowerShell pro správu zařízení StorSimple | Microsoft Azure"
   description="Naučte se používat Windows PowerShell pro StorSimple ke správě StorSimple zařízení."
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
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Umožňuje spravovat zařízení s Windows PowerShell pro StorSimple

## <a name="overview"></a>Základní informace

Prostředí Windows PowerShell pro StorSimple poskytuje rozhraní příkazového řádku, které můžete použít ke správě Microsoft Azure StorSimple zařízení. Jak naznačuje název je založený na prostředí Windows PowerShell rozhraním příkazového řádku, která je integrovaná v omezené prostředí runspace. Z pohledu uživatele na příkazovém řádku prostředí omezené runspace zobrazuje se jako s omezeným přístupem verzi prostředí Windows PowerShell. A zachovat část základních funkcí prostředí Windows PowerShell, má toto rozhraní jsou zaměřené spravovat zařízení Microsoft Azure StorSimple další vyhrazené rutinách. 

Tento článek popisuje prostředí Windows PowerShell pro StorSimple funkcí, včetně způsob připojení k tomuto rozhraní a obsahuje odkazy na podrobné postupy nebo pracovním postupům prováděné pomocí tohoto rozhraní. Pracovní postupy zahrnout jak zaregistrovat zařízení, konfigurace síťové rozhraní na vašem zařízení a nainstalovat aktualizace, které vyžadují zařízení, které má být v režimu údržby, změnit stav zařízení a při odstraňování všech problémů, které byste mohli narazit.

Po přečtení v tomto článku, budou moct:

- Připojení k zařízení StorSimple pomocí Windows Powershellu pro StorSimple.

- Správa zařízení StorSimple pomocí Windows Powershellu pro StorSimple.

- Získání nápovědy v prostředí Windows PowerShell pro StorSimple.

>[AZURE.NOTE]   

>- Prostředí Windows PowerShell pro StorSimple rutiny umožňují spravovat vaše zařízení StorSimple sériové konzole nebo vzdáleně prostřednictvím Vzdálená prostředí Windows PowerShell. Další informace o jednotlivých jednotlivé rutin, které lze použít v této rozhraní najdete v článku [rutiny prostředí Windows PowerShell pro StorSimple pro](https://technet.microsoft.com/library/dn688168.aspx).

>- Rutiny prostředí PowerShell StorSimple Azure jsou různé kolekce rutin, která umožňují automatizaci StorSimple úrovni služeb a úkoly při migraci z příkazového řádku. Další informace o rutinách Powershellu Azure StorSimple najdete [reference pro rutiny Azure StorSimple](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Máte přístup k prostředí Windows PowerShell pro StorSimple pomocí jedné z těchto způsobů:

- [Připojení konzoly sériové StorSimple zařízení](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Připojení vzdálených StorSimple pomocí Windows Powershellu](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Připojení k prostředí Windows PowerShell pro StorSimple pomocí sériové konzoly zařízení

Můžete si [Stáhnout nátěrové](http://www.putty.org/) nebo podobné terminálu emulace software pro připojení k prostředí Windows PowerShell pro StorSimple. Potřebujete konfigurovat nátěrové speciálně pro přístup k Microsoft Azure StorSimple zařízení. Následující témata obsahují podrobný postup o tom, jak konfigurovat nátěrové a připojte k zařízení. Různé možnosti nabídky v konzole sériové taky jsou vysvětleny.

### <a name="putty-settings"></a>Nastavení nátěrové

Ujistěte se, použijte následující nastavení nátěrové se připojit k v prostředí Windows PowerShell rozhraní konzole sériové.

#### <a name="to-configure-putty"></a>Konfigurace nátěrové

1. V dialogovém okně nátěrové **Konfigurace** v podokně **kategorie** vyberte **klávesnice**.

2. Ujistěte se, že nejsou vybrány z následujících možností (Toto jsou výchozí nastavení, když začnete novou relaci). 

  	|Položky klávesnice|Vyberte|
  	|---|---|
  	|Klávesy BACKSPACE|Ovládací prvek-? (127)|
  	|Home a End klíče|Standardní|
  	|Funkční klávesy a klávesnice|ESC [n ~|
  	|Počáteční stav klíče kurzor|Normální|
  	|Počáteční stav numerické části klávesnice|Normální|
  	|Povolení funkcí navíc klávesnice|CTRL + ALT + se liší od AltGr|

    ![Podporované nátěrové nastavení](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Klikněte na tlačítko **použít**.

4. V podokně **kategorie** vyberte položku **překlad**.

5. V seznamu **vzdálené znakové sady** vyberte **UTF-8**.

6. Ve skupinovém rámečku **zpracování Perokresba znaků**vyberte **použít Unicode Perokresba soustavě**. Následující obrázek znázorňuje správné hodnoty nátěrové.

    ![UTF nátěrové nastavení](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Klikněte na tlačítko **použít**.


Teď můžete nátěrové připojení konzoly sériové zařízení provedením následujících kroků.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Informace o konzole sériové

Kdy získáváte přístup k rozhraní StorSimple zařízení pomocí sériové konzoly zprávu nápis jsou uvedeny prostředí Windows PowerShell, následovaný možnosti nabídky. 

Nápis zpráva obsahuje základní informace o zařízení StorSimple třeba modelu, název, nainstalovaná verze softwaru a stav řadiče domény které přistupujete. Následující obrázek znázorňuje příklad nápis zprávy.

![Sériové nápis zprávy](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Nápis zprávy můžete použít k určení, zda je na zařízení, které jste připojeni k aktivní nebo pasivní.

Na tomto obrázku vidíte různé možnosti prostředí runspace, které jsou k dispozici v nabídce sériové konzola.

![Registrace zařízení 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Máte na výběr z následujících nastavení:

1. **Přihlaste se pomocí plný přístup** Tato možnost umožňuje spojení (s správné přihlašovací údaje) prostředí runspace **SSAdminConsole** místní řadiči. (Místní řadiče je řadiče domény které aktuálně přistupujete pomocí sériové konzoly zařízení StorSimple.) Tuto možnost lze také povolit Support společnosti Microsoft pro přístup k neomezený prostředí runspace (relaci podpora) při odstraňování všech problémů zařízení. Po přihlášení pomocí možnost 1, můžete povolit zpětnou analýzu Support společnosti Microsoft pro přístup k neomezený prostředí runspace spuštěním konkrétní rutiny. Podrobné informace podívejte se do [Zahájit relaci podpory](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Přihlaste se k mezi dvěma účastníky řadiče domény s úplným přístupem** Tato možnost je stejná jako možnost 1, s tím rozdílem, že se můžete připojit (plus pár správné přihlašovací údaje) k prostředí runspace **SSAdminConsole** řadiči mezi dvěma účastníky. Vzhledem k tomu zařízení StorSimple dostupnost zařízení s dva řadiče v konfiguraci aktivní pasivní, mezi dvěma účastníky odkazuje na jiné zařízení v zařízení, které přistupujete pomocí sériové konzoly).
Podobně jako možnost 1, tuto možnost lze také umožňuje Support společnosti Microsoft pro přístup k neomezený prostředí runspace řadiči mezi dvěma účastníky.

3. **Připojení s omezeným přístupem** Tato možnost slouží k přístupu k prostředí Windows PowerShell rozhraní v omezené režimu. Žádná výzva k zadání přihlašovacích údajů aplikace access. Tato možnost se připojí k omezenější prostředí runspace ve srovnání s možností 1 a 2.  Některé úkoly, které jsou k dispozici prostřednictvím možnost 1, **nejde* provést v tomto prostředí runspace jsou:

    - Obnovení tovární nastavení
    - Změna hesla
    - Zapnutí nebo vypnutí podporu přístupu
    - Použití aktualizací
    - Instalace hotfix 
                                                

    >[AZURE.NOTE] **To je upřednostňovaná možnost, pokud zapomněli jste heslo správce zařízení a nemůžete připojit pomocí možnosti 1 nebo 2.**

4. **Změnit jazyk** Tato možnost umožňuje změnit jazyk zobrazení v prostředí Windows PowerShell rozhraní. Jazyky podporované jsou angličtina, japonština, ruština, francouzština, korejština Jih, španělština, italština, němčina, čínština a Brazilská portugalština.


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Připojení vzdálených StorSimple pomocí Windows Powershellu pro StorSimple

Vzdálená prostředí Windows PowerShell můžete připojit k StorSimple zařízení. Když se připojíte tímto způsobem, neuvidíte nabídku. (Vidíte nabídku jenom v případě, že použijete konzole sériové na zařízení k připojení. Připojení vzdáleně přejdete přímo na ekvivalent "možnost 1 – plný přístup" na konzole sériové.) Pomocí prostředí Windows PowerShell Vzdálená připojit k určité prostředí runspace. Můžete taky určit jazyk zobrazení. 

Jazyk zobrazení nezávisí na jazyk, ve kterém můžete nastavit pomocí možnosti **Změnit jazyk** v nabídce sériové konzola. Vzdálený PowerShell bude automaticky vystopovat národní prostředí zařízení se připojujete z Pokud není zadán žádný.

>[AZURE.NOTE] Pokud pracujete s Microsoft Azure virtuální hosts vybavením StorSimple virtuální, můžete vzdálené prostředí Windows PowerShell a hostiteli virtuální připojení k virtuální zařízení. Pokud jste nastavili sdílené umístění na hostiteli na které chcete uložit informace z prostředí Windows PowerShell relace, je třeba si uvědomit u položky všichni hlavní obsahuje jenom ověřené uživatele. Proto pokud jste nastavili sdílet umožníte přístup tak, že všichni a připojíte bez zadání přihlašovacích údajů, neověřených anonymní jistinu se použijí a zobrazí se chyba. Tento problém vyřešit, ve sdílené složce můžete hostovat musíte povolit účtu hosta a potom zadejte hosta účtu plný přístup ke sdílené složce nebo je nutné zadat pověření spolu s rutiny prostředí Windows PowerShell.

Připojení přes Windows PowerShell Vzdálená můžete pomocí protokolu HTTP nebo HTTPS. Postupujte podle pokynů v následující kurzy:

- [Vzdálené připojení pomocí protokolu HTTP](storsimple-remote-connect.md#connect-through-http)
- [Vzdálené připojení pomocí HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Otázky bezpečnosti pro připojení

Při výběru způsobu připojení k prostředí Windows PowerShell pro StorSimple, zvažte následující skutečnosti:

- Přímo na konzole sériové zařízení se můžete připojit zabezpečené, ale připojení ke konzole sériové přes síťové přepínače není. Buďte obezřetní rizika zabezpečení při připojování k zařízení sériové přes síťové přepínače.

- Připojení přes HTTP relaci může nabízejí větší zabezpečení než připojením pomocí sériové konzoly přes síť. Není to však nejbezpečnější způsob, je přípustné v důvěryhodném sítích.

- Připojení přes protokol HTTPS v relaci je nejbezpečnější a doporučené možnost.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Správa zařízení StorSimple pomocí Windows Powershellu pro StorSimple
Následující tabulka obsahuje souhrn všech běžné úlohy správy a složitých pracovních postupů, které lze provádět v rámci rozhraní prostředí Windows PowerShell StorSimple zařízení. Další informace o jednotlivých pracovního postupu klikněte na příslušnou položku v tabulce.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Prostředí Windows PowerShell StorSimple pracovních postupů

|Pokud chcete, můžete to udělat...|Pomocí tohoto postupu.|
|---|---|
|Registrace zařízení|[Nakonfigurovat a zaregistrovat zařízení pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Konfigurace proxy serveru webové</br>Nastavení proxy serveru webové zobrazení|[Konfigurace proxy serveru webové StorSimple zařízení](storsimple-configure-web-proxy.md)|
|Úprava nastavení rozhraní 0 sítě dat na vašem zařízení|[Úprava dat 0 síťové StorSimple zařízení](storsimple-modify-data-0.md)|
|Ukončení řadiči </br> Restartování nebo vypnutí řadiči </br> Vypnutí zařízení</br>Resetujte tovární výchozí nastavení|[Správa řadiče zařízení](storsimple-manage-device-controller.md)|
|Nainstalujte aktualizace režimu údržby a hotfix|[Aktualizovat zařízení](storsimple-update-device.md)|
|Přechod do režimu údržby </br>Ukončení režimu údržby|[Režimy StorSimple zařízení](storsimple-device-modes.md)|
|Vytvoření balíčku podpory</br>Dešifrování a upravit balíček podpory|[Vytváření a Správa balíčku podpory](storsimple-create-manage-support-package.md)|
|Spuštění relace s podpory</br>|[Spuštění relace s podporu v prostředí Windows PowerShell pro StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Získání nápovědy v prostředí Windows PowerShell pro StorSimple

Ve Windows Powershellu pro StorSimple rutina nápovědy neexistuje. Online, aktuální verzi této nápovědy je také k dispozici, které slouží k aktualizaci nápovědy v počítači.

Získání nápovědy v tomto rozhraní je podobné jako v prostředí Windows PowerShell a bude fungovat většina rutiny související Nápověda. Nápověda pro Windows PowerShell online můžete najít v knihovně TechNet: [skriptování ve Windows Powershellu](http://go.microsoft.com/fwlink/?LinkID=108518).

Následujícím obrázku je stručný popis typy nápovědy pro toto prostředí Windows PowerShell rozhraní, včetně aktualizaci Nápověda.

#### <a name="to-get-help-for-a-cmdlet"></a>Získání nápovědy pro rutinu

- Rutina nebo funkce potřebujete pomoct, použijte tento příkaz:`Get-Help <cmdlet-name>`

- Zobrazit online nápovědu pro všechny rutinu, použijte rutinu předchozí s `-Online` parametr:`Get-Help <cmdlet-name> -Online`

- Úplné potřebujete pomoc, můžete použít `–Full` parametr a příklady, použít `–Examples` parametr.

#### <a name="to-update-help"></a>Chcete-li aktualizovat nápovědy

Můžete snadno aktualizovat Nápověda ke službě rozhraní prostředí Windows PowerShell. Proveďte následující kroky k aktualizaci nápovědy v počítači.

#### <a name="to-update-cmdlet-help"></a>Chcete-li aktualizovat rutina nápovědy

1. Spusťte Windows PowerShell s možností **Spustit jako správce** .

1. Na příkazovém řádku zadejte: `Update-Help`

1. Nainstaluje aktualizované soubory nápovědy.

1. Po instalaci soubory nápovědy zadejte: `Get-Help Get-Command`. Tím zobrazíte seznam rutiny pro které je k dispozici nápověda.


>[AZURE.NOTE] Seznam všech dostupných rutin prostředí runspace získáte přihlaste na odpovídající položku nabídky a spusťte `Get-Command` rutiny.

## <a name="next-steps"></a>Další kroky
Pokud máte jakékoli problémy se zařízením StorSimple provedením jedné z výše uvedených postupů, podívejte se do [Nástroje pro řešení potíží s StorSimple nasazení](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

