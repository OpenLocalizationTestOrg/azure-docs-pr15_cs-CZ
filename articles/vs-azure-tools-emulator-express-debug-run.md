<properties
   pageTitle="Použití emulátoru Express ke spouštění a ladění do cloudové služby v místním počítači | Microsoft Azure"
   description="Použití emulátoru Express ke spouštění a ladění do cloudové služby v místním počítači"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Použití emulátoru Express ke spouštění a ladění do cloudové služby v místním počítači

Pomocí emulátoru Express můžete otestovat a ladění do cloudové služby bez spuštění aplikace Visual Studio jako správce. Můžete nastavit nastavení projektu použít emulátoru Express nebo celé emulátoru podle toho, požadavky cloudové služby. Další informace o celou emulátoru najdete v článku [spuštění aplikace Azure v emulátoru výpočet](./storage/storage-use-emulator.md). Emulátoru Express jste obdrželi nejdřív v Azure SDK 2.1 a od Azure SDK 2.3 je emulátoru výchozí.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Použití emulátoru Express ve Visual Studiu integrovaném vývojovém prostředí

Při vytváření nového projektu v Azure SDK 2.3 nebo novější emulátoru Express už zaškrtnuté. Pro existující projekty, které byly vytvořené ve starší verzi sady SDK postupujte vyberte emulátoru Express.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Konfigurace projektu použít emulátoru Express

1. V místní nabídce Azure projektu vyberte **Vlastnosti**a klikněte na kartu **Web** .

1. V části **Místního serveru vývoj**vyberte **použít službu IIS Express přepínací** tlačítko. Emulátoru Express není kompatibilní s IIS webový Server.

1. V části **emulátoru**zvolte přepínač **Použít emulátoru Express** .

    ![Emulátoru Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Spuštění emulátoru Express příkazového řádku

Na příkazovém řádku můžete spustit express verzi Azure výpočet emulátor, csrun.exe, pomocí možnosti /useemulatorexpress.

## <a name="limitations"></a>Omezení

Před použitím emulátoru Express, byste měli znát určitá omezení:

- Emulátoru Express není kompatibilní s IIS webový Server.

- Cloudová služba může obsahovat více rolí, ale každou roli se omezí na jedna instance.

- Nemáte přístup ke čísla portů nižší než 1 000. Používáte poskytovatele ověřování, který obvykle používá port pod 1000, můžete je potřeba změnit tuto hodnotu na číslo portu, která je větší než 1 000.

- Omezení, které platí pro výpočet emulátoru Azure také použít emulátoru Express. Například nemůže mít víc než 50 instancí role na nasazení. Zjistit, jestli [Azure aplikace ve výpočetním emulátoru](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Další kroky

[Ladění Cloudovým službám](https://msdn.microsoft.com/library/azure/ee405479.aspx)
