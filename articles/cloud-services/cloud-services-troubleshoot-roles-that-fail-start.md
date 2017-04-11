<properties
   pageTitle="Poradce při potížích s role, které nespustí | Microsoft Azure"
   description="Tady je několik běžných příčin, proč může selhat roli Cloudová služba spustit. Slouží také řešení těchto problémů."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Poradce při potížích s rolí cloudové služby, které nespustí

Tady jsou některé běžné problémy a řešení související ke Cloudovým službám Azure role, které se nespustí.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Chybějící DLL a závislosti

Odpovídat rolí a, které jsou cyklické mezi **Probíhá inicializace**, **zaneprázdněn(a)**a **zastavení** státy může být způsobeno chybějící soubory DLL nebo sestavení.

Příznaky chybějící DLL nebo sestavení může být:

- Role instance přepínání mezi **Probíhá inicializace**, **zaneprázdněn(a)**a **zastavení** státy.
- Role instance přesunul na **připravená** , ale pokud přejděte na webovou aplikaci na stránce se nezobrazí.

Existuje několik doporučené postupy pro zjišťování těchto problémů.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Diagnostika problémů s chybějící DLL v roli web

Při přechodu na web, který je nasazený ve webovém rolí a prohlížeč zobrazí chyba serveru podobně jako tento, může to znamenat, že knihovna DLL chybí.

![Chyba serveru "/" aplikace.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostika problémů s vypnutím vlastní chyby

Podrobnější informace o chybě můžete zobrazit tak, že konfigurace web.config pro web roli chcete nastavit vlastní chybové režimu vypnutí a opětovné nasazení služby.

Chcete-li zobrazit detailní chyby bez použití Vzdálená plocha:

1. Otevřete v aplikaci Microsoft Visual Studio řešení.

2. V **Okně Průzkumník**vyhledejte nastavení(Web.config)) a otevřete ho.

3. V souboru web.config vyhledejte části System.Web a přidejte následující řádek:

    ```xml
    <customErrors mode="Off" />
    ```

4. Uložte soubor.

5. Opětovnému vytvoření balíčku a přeinstalujte službu.

Jakmile je znovu nasadit služby, zobrazí se chybová zpráva s názvem chybějící sestavení nebo DLL.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Diagnostika problémů s vzdáleně zobrazením chyby do schránky

Vzdálené plochy umožňuje přístup k roli a vzdáleně zobrazit podrobnější informace o chybě. Zobrazení chyb pomocí vzdálené plochy pomocí následujících kroků:

1. Ujistěte se, že je nainstalovaný Azure SDK 1.3 nebo novější.

2. Při nasazení řešení pomocí aplikace Visual Studio zvolte "Konfigurovat připojení ke vzdálené ploše...". Další informace o konfiguraci připojení ke vzdálené ploše najdete v článku [Pomocí vzdálené plochy s rolemi Azure](../vs-azure-tools-remote-desktop-roles.md).

3. Klasický portálu Microsoft Azure po instance zobrazuje stav **připravená**, klepněte na jednu instancí role.

4. Klikněte na ikonu **Připojit** v oblasti **Vzdáleného přístupu** na pásu karet.

5. Přihlaste se do virtuálního počítače pomocí přihlašovacích údajů určených během konfigurace Vzdálená plocha.

6. Otevřete okno příkazového řádku.

7. Typ `IPconfig`.

8. Poznamenejte si adresu IPV4 hodnotu.

9. Spusťte aplikaci Internet Explorer.

10. Zadejte adresu a název webové aplikace. Například `http://<IPV4 Address>/default.aspx`.

Přechod na web teď vrátit podrobnější chybové zprávy:

* Chyba serveru "/" aplikace.

* Popis: Neošetřené výjimce došlo k během zpracování požadavku na aktuální web. Zkontrolujte zásobníku Další informace o chybě a kde vznikl v kódu.

* Podrobnosti o výjimce: System.IO.FIleNotFoundException: Nelze načíst soubor nebo sestavení "Microsoft.WindowsAzure.StorageClient, verze = 1.1.0.0, jazykové verze = neutrální, PublicKeyToken = 31bf856ad364e35" nebo některé z jejích závislostí. Jestli chcete systém nemůžete najít soubor určený.

Příklad:

![Chyba explicitní serveru "/" aplikace](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnostika problémů s pomocí emulátoru výpočetním

Můžete emulátoru výpočetním Microsoft Azure a Diagnostika problémů chybějících závislostí a web.config chyby.

Nejlepších výsledků při používání této metody diagnostiky by měl použití počítače nebo virtuální počítač, který má čisté instalace systému Windows. Nejlepší simulovat Azure prostředí, použijte Windows Server 2008 R2 x64.

1. Nainstalujte samostatné verzi [Azure SDK](https://azure.microsoft.com/downloads/).

2. V počítači vývoj vytvořte projekt služby cloudu.

3. V programu Průzkumník Windows přejděte do složky bin\debug projekt služby cloudu.

4. Zkopírujte soubor složku a .cscfg .csx k počítači, který používáte ladění problémů.

5. Vyčistit počítače, otevřete okno příkazového SDK Azure a zadejte `csrun.exe /devstore:start`.

6. Na příkazovém řádku zadejte `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Při spuštění roli, zobrazí se podrobné informace o chybě v aplikaci Internet Explorer. Můžete také standardní Windows nástroje pro řešení potíží a další diagnostikovat problém.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostika problémů s pomocí IntelliTrace

Pracovní a web používající .NET Framework 4 můžete [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), která je dostupná v [Aplikaci Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Tímto postupem s IntelliTrace povolené nasadit služby:

1. Potvrďte, že je nainstalovaný Azure SDK 1.3 nebo novější.

2. Pomocí aplikace Visual Studio nasazení řešení. Při nasazení zaškrtněte políčko **Povolit IntelliTrace pro .NET 4 role** .

3. Po spuštění instance spusťte **Průzkumníka serveru**.

4. Rozbalení **Azure\\Cloudovým službám** uzel a vyhledejte nasazení.

5. Rozbalte nasazení se zobrazila instance role. Klikněte pravým tlačítkem myši na jednu z instancí.

6. Zvolte **zobrazení IntelliTrace protokoly**. Otevře se **IntelliTrace souhrn** .

7. Vyhledejte části výjimky na souhrn. Pokud jsou výjimek, v části popisky **Data výjimky**.

8. Rozbalte **Data výjimky** a vyhledejte **System.IO.FileNotFoundException** chyby podobná této:

![Data výjimky, chybí soubor nebo sestavy](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Řešit chybějící DLL a montáží

Při řešení chybějící DLL a sestavení chyby, postupujte takto:

1. Otevřete řešení ve Visual Studiu.

2. V **Okně Průzkumník**otevřete složku **odkazy** .

3. Klikněte na sestavení identifikované v článku chyba.

4. V podokně **Vlastnosti** vyhledejte **místní kopie vlastnost** a nastavte hodnotu **PRAVDA**.

5. Přeinstalujte cloudovou službu.

Jakmile jste ověřili, že byly odstraněny všechny chyby, můžete nasadit služby bez zaškrtnutí políčka **Povolit IntelliTrace pro .NET 4 role** .

## <a name="next-steps"></a>Další kroky

Zobrazte další [Poradce při potížích s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) při cloudovým službám.

Další řešení problémů s cloudové služby rolí pomocí Azure PaaS počítačových diagnostiky dat najdete v tématu [Jan Williamson řada](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
