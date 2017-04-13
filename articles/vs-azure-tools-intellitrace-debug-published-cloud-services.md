<properties 
   pageTitle="Ladění publikované Cloudová služba s IntelliTrace a Visual Studio | Microsoft Azure"
   description="Ladění publikované Cloudová služba s IntelliTrace a Visual Studio"
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

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Ladění publikované Cloudová služba s IntelliTrace a Visual Studio

##<a name="overview"></a>Základní informace

S IntelliTrace se můžete přihlásit rozsáhlé ladění informace pro instanci rolí při spuštění v Azure. Pokud potřebujete zjistit příčinu problému, můžete přecházet mezi kódu Visual Studiu, jako kdyby ho používali v Azure IntelliTrace protokoly. IntelliTrace záznamů ve skutečnosti klávesy spuštění kódu a datových prostředí, když se aplikace Azure běží jako do cloudové služby Azure a umožňuje přehrát zaznamenané dat z aplikace Visual Studio. Jako alternativu můžete vzdálené ladění přímé připojení k do cloudové služby, na kterém běží v Azure. V tématu [ladění Cloudovým službám](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace je určená pro ladění scénáře pouze a není určená k použití nasazení výroby.

>[AZURE.NOTE] Můžete použít IntelliTrace Pokud máte nainstalované Visual Studio Enterprise a Azure aplikace cílů .NET Framework 4 nebo novější. IntelliTrace shromažďuje informace o Azure role. Virtuálních počítačích těchto rolích vždy 64bitové operační systém.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Konfigurace aplikace Azure pro IntelliTrace

Povolit IntelliTrace Azure aplikace, musíte vytvořit a publikovat aplikace Visual Studio Azure projektu. Před publikováním Azure, musíte nakonfigurovat IntelliTrace Azure aplikace. Pokud publikujete aplikace bez konfigurace IntelliTrace, ale pak se rozhodnete to udělat, budete muset aplikaci znova z aplikace Visual Studio publikovat. Další informace najdete v tématu [publikování do cloudové služby používat nástroje Azure](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Když budete chtít nasazení aplikace Azure, ověřte, že vaše cíle projektu sestavení jsou nastavené **ladění**.

1. Otevření místní nabídky pro Azure projektu v Průzkumníku řešení a vyberte **Publikovat**.
 
    Zobrazí se Průvodce Publikovat Azure aplikace.

1. Získat informace o protokolech IntelliTrace aplikace při publikování v cloudu, zaškrtněte políčko **Povolit IntelliTrace** .

    >[AZURE.NOTE] Můžete povolit IntelliTrace nebo vytváření profilů při publikování Azure aplikace. Není možné povolit obojí.

1. Přizpůsobení základní konfigurace IntelliTrace, zvolte **Nastavení** hypertextový odkaz.

    Zobrazí se dialogové okno nastavení IntelliTrace, jak je znázorněno na následujícím obrázku. Můžete určit, události, které protokolu, zda účelem sběru informací o volání, které moduly a procesů, abyste shromáždit protokoly pro a zbývajícího prostoru přidělit záznamu. Další informace o IntelliTrace najdete v tématu [ladění s IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Protokol IntelliTrace je cyklický soubor protokolu maximální velikosti zadanou v nastavení IntelliTrace (výchozí hodnota je 250 MB). K souboru v systému souborů virtuálního počítače se shromažďují protokoly IntelliTrace. Při žádosti protokoly snímek pořízené v daném okamžiku a stáhl(a) do místního počítače.

Po Azure aplikace byl publikován na Azure, můžete určit, pokud IntelliTrace je povolená z Azure výpočet uzlu v Průzkumníku serveru, jak je znázorněno na následujícím obrázku:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>Stahování IntelliTrace protokoly pro instanci rolí

Instanci rolí v protokolech IntelliTrace si můžete stáhnout z uzel **Cloudovým službám** v **Průzkumníku serveru**. Rozbalte uzel **Cloudové služby** , dokud nenajdete instance, které vás zajímají, otevřete místní nabídky pro tuto instanci a zvolte **Zobrazit protokoly IntelliTrace**. Protokoly IntelliTrace stahování souboru v adresáři v místním počítači. Pokaždé, když požadavku IntelliTrace protokoly, se vytvoří nový snímek.

Po stažení protokoly Visual Studio zobrazí průběhu operace v okně protokolu činnosti Azure. Jak je znázorněno na následujícím obrázku můžete rozbalit řádkové položky pro operaci zobrazíte více podrobností.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Můžete pokračovat v práci ve Visual Studiu během stahování IntelliTrace protokoly. Po dokončení stahování protokol se automaticky otevře ve Visual Studiu.

>[AZURE.NOTE] Protokoly IntelliTrace může obsahovat výjimky rozhraní vygeneruje a následně pracuje. Vnitřní framework kód vygeneruje následujícími výjimkami součástí normálním spuštění roli, tak může bezpečně ignorovat.

## <a name="see-also"></a>Viz taky

[Ladění Cloudovým službám](https://msdn.microsoft.com/library/ee405479.aspx)

