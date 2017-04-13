<properties 
    pageTitle="Azure RemoteApp Poradce při potížích s - chyby připojení a spuštění aplikací | Microsoft Azure" 
    description="Zjistěte, jak řešit problémy s počáteční a připojení k aplikacím v Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Poradce při potížích s Azure RemoteApp - chyby připojení a spuštění aplikací 

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Aplikace umístěné v Azure RemoteApp může selhat spustit z několika různých důvodů. Tento článek popisuje různé důvodů, proč a chybové zprávy uživatelé můžou zobrazit při pokusu o spuštění aplikace. Také pojednává o připojení k chybám. (Ale tento článek není popisují potíže při přihlašování ke klientovi Azure RemoteApp.)  

Přečtěte si informace o běžných chybových zpráv z důvodu aplikace spuštění a připojení k chybám.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Budeme se zobrazuje nastavení... Akci 10 minut.

Tato chyba znamená, že je Azure RemoteApp škálování plnit potřebné kapacity vašich uživatelů. Na pozadí jsou vytvářeny další instance Azure RemoteApp OM zpracovávání kapacita potřeb uživatele. Obvykle to trvá asi pět minut, ale může zabrat až 10 minut. V některých případech to se nestane, dost rychle zdroje je třeba a okamžitě. Například 9: 00 situace, kdy je potřeba většího počtu uživatelů pomocí aplikace v Azure RemoteAppn ve stejnou dobu. V takovém případě vám jsme povolit **režim kapacita** back-end. Můžete to udělat otevřete lístek Azure podpory a nebo e-mailové adrese [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Ujistěte se, zahrnout ID předplatného v pozvánce.  

![Budeme se zobrazují nastavení](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Nelze automaticky připojit k aplikacím, znova spusťte aplikaci  

Tato chybová zpráva je často uváděný Pokud jste používali Azure RemoteApp a vložte počítač PC pro spánek déle než 4 hodiny a potom probudila vašeho počítače a klientovi Azure RemoteApp pokusí roční znovu a překročení limitu.  Sdělte uživatelům Najděte aplikaci a pokusí se otevřít z Azure RemoteApp klienta.

![Nelze automaticky-znovu připojíte k používání aplikací](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problémy s temp profilu 

K této chybě dochází, když profilu uživatele (uživatelského profilu Disk) se nepodařilo připojit a uživatel dostali dočasný profil.  Správci mají přejděte na kolekci Azure portálu přejděte na kartu **relace** a pokusíte **Odhlásit** uživatele. To bude vynutit úplné protokol mimo relace uživatele – potom uživatelů pokusí o spuštění aplikace znovu. Pokud se nezdaří kontaktovat podporu Azure a nebo e-mailové adrese [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp přestal fungovat

Tato chybová zpráva znamená klientovi Azure RemoteApp dochází k problému a se musí restartovat. Sdělte uživatelům zavřít: vyberte **Ukončit** a znovu spusťte Azure RemoteApp klienta.  Pokud problém budou dál problémy, otevřít a podpora Azure lístek a nebo e-mailové adrese [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp přestal fungovat](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Připojení ke vzdálené ploše byla přístupu k tomuto prostředku došlo k chybě. Opakovat připojení nebo kontaktujte svého správce systému

Toto je obecné chybové zprávě - kontaktovat podporu Azure a nebo e-mailové adrese [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) tak, aby společnost Microsoft mohla prošetřit. 

![Obecná zpráva Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 