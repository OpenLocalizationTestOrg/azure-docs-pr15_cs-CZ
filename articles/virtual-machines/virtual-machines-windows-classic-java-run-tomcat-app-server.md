<properties
    pageTitle="Tomcat na počítač virtuální | Microsoft Azure"
    description="Tento kurz využívá zdroje vytvořená pomocí klasické nasazení modelu a ukazuje, jak vytvořit virtuální Windows počítač a nakonfigurujte ji spustit Apache Tomcat aplikační server."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>K tomu Java aplikační server na virtuálních počítač vytvořená pomocí klasické nasazení modelu

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Azure můžete pomocí virtuálního počítače poskytovat funkce serveru. Jako příklad je možné konfigurovat virtuální počítač se systémem Azure hostovat aplikační server Java, například Apache Tomcat. Po dokončení tohoto průvodce, budete mít pochopení toho, jak vytvořit virtuální počítač se systémem Azure a nakonfigurujte ji spustit Java aplikační server.

Naučíte se:

* Jak vytvořit virtuální počítač, který má Java Development Kit (JDK) už nainstalovaný.
* Jak vzdáleně přihlásit virtuálního počítače.
* Jak nainstalovat Java aplikační server na virtuálních počítač.
* Postup vytvoření koncového bodu virtuálního počítače.
* Jak otevřít port v bráně firewall pro aplikační server.

Pro účely tohoto kurzu aplikační server Apache Tomcat nainstaluje na počítač virtuální. Dokončení instalace povede Tomcat instalace následující.

![Virtuální počítač Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Vytvoření virtuálního počítače

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2. Klikněte na **Nový**, klikněte na **Výpočet**, klikněte na **virtuálního počítače**a klikněte na **Z Galerie**.
3. V dialogovém okně **Vybrat obrázek virtuálního počítače** vyberte **JDK 7 systému Windows Server 2012**.
Všimněte si, že je k dispozici, pokud máte starší verze aplikace, které nejsou spustit ve JDK 7 **JDK 6 systému Windows Server 2012** .
4. Klikněte na tlačítko **Další**.
5. V dialogovém okně **Konfigurace virtuálního počítače** :
    1. Zadejte název pro virtuální počítač.
    2. Určete velikost pro účely virtuální počítač.
    3. Do pole **Uživatelské jméno** zadejte název pro správce. Nezapomeňte tento název a heslo, které pak zadáte, budete je používat při vzdáleně přihlášení do virtuálního počítače.
    4. Zadejte heslo do pole **heslo** a znovu ho zadat do pole **potvrzení** . To je heslo účtu správce.
    5. Klikněte na tlačítko **Další**.
6. V dalším dialogovém okně **Konfigurace virtuálního počítače** :
    1. Ke **cloudové služby**použijte výchozí **vytvořit nové cloudové služby**.
    2. Hodnotu **cloudové služby DNS název** musí být jedinečná přes cloudapp.net. V případě potřeby upravte tuto hodnotu tak, že Azure označuje, že bude jedinečný.
    2. Zadejte oblast, spřažení skupiny nebo virtuální sítě. Pro účely tohoto kurzu zadejte oblast například **Západní USA**.
    2. **Úložiště účet**vyberte **použít účet automaticky generované úložiště**.
    3. **Nastavte dostupnost**vyberte **(žádné)**.
    4. Klikněte na tlačítko **Další**.
7. V konečném dialogové okno **Konfigurace virtuálního počítače** :
    1. Přijměte výchozí koncový bod položky.
    2. Klikněte na **dokončení**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Přihlašovat vzdáleně do virtuálního počítače

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2. Klikněte na **virtuálních počítačích**.
3. Klikněte na název virtuální počítač, který chcete pro přihlášení k aplikaci.
4. Po spuštění virtuální počítač místní nabídku v dolní části stránky umožňuje připojení.
5. Klikněte na **Připojit**.
6. Odpovězte na dotazy v případě potřeby se připojit k počítači virtuální. To by měl mít za následek uložení nebo otevření souboru RDP, který obsahuje podrobné informace o připojení. Možná budete muset zkopírujte adresu url: port jako poslední část první řádek souboru RDP a vložit ho do vzdálené aplikace přihlašovací.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Chcete-li nainstalovat aplikační server Java v počítači virtuální

Aplikační server Java můžete zkopírovat do virtuálního počítače nebo můžete nainstalovat aplikační server Java přes instalační program.

Pro účely tohoto kurzu nainstaluje Tomcat.

1. Když jste přihlášení k počítači virtuální, otevřete relaci prohlížeče [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Poklikejte na odkaz **32-bit/64bitovou služby Instalační služby**systému Windows. Pomocí tohoto postupu Tomcat nainstaluje jako služba Windows.
3. Po zobrazení výzvy vyberte spustit instalační program.
4. V průvodci **Nastavením Tomcat Apache** postupujte podle pokynů nainstalujte Tomcat. Pro účely tohoto kurzu je v pořádku přijmout výchozí nastavení. Až dojdete dialogovým oknem **dokončení Průvodce instalací Tomcat Apache** , můžete volitelně zkontrolovat, že **Spustit Tomcat Apache** mít Tomcat začít nyní. Klikněte na **Dokončit** dokončete proces instalace Tomcat.

## <a name="to-start-tomcat"></a>Spusťte Tomcat
Pokud ke spuštění Tomcat v dialogovém okně **dokončení Průvodce instalací Tomcat Apache** nerozhodnete, začněte otevřením příkazového řádku v počítači virtuální a systémem **čistého start Tomcat7**.

Teď byste měli vidět Tomcat systémem-li spustit prohlížeče virtuálního počítače a otevřít <http://localhost:8080>.

Zobrazíte Tomcat spuštění z externích počítačích potřebujete vytvoření koncového bodu a otevření portu.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Vytvoření koncového bodu virtuálního počítače
1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2. Klikněte na **virtuálních počítačích**.
3. Klikněte na název virtuálního počítače, na kterém běží aplikační server Java.
4. Klikněte na **koncové body**.
5. Klikněte na **Přidat**.
6. V dialogovém okně **Přidat koncový bod** ověřte **koncový bod přidat samostatný** zaškrtnuté a klikněte na tlačítko **Další**.
7. V dialogovém okně **nové podrobnosti o koncového bodu** :
    1. Zadejte název koncového bodu; například **HttpIn**.
    2. Určete protokolu **TCP** .
    3. Určení **80** veřejné číslo portu.
    4. Zadejte **8080** privátní číslo portu.
    5. Klikněte na tlačítko **Dokončit** zavřete dialogová okna. Koncový bod teď se vytvoří.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Chcete-li otevřít port v bráně firewall virtuálního počítače
1. Přihlaste se k počítači virtuální.
2. Klikněte na tlačítko **Start systému Windows**.
3. Klikněte na **Ovládací panely**.
4. Klikněte na **systém a zabezpečení**, klikněte na **Brána Windows Firewall**a pak klikněte na **Upřesnit nastavení**.
5. Klikněte na **Příchozí pravidla**a potom klikněte na **Nové pravidlo**.
 ![Nové pravidlo pro příchozí][NewIBRule]
6. **Typ pravidla**vyberte **Port**a klikněte na tlačítko **Další**.
 ![Nový port příchozí pravidla][NewRulePort]
7. Na obrazovce **protokol a porty** **TCP**vyberte, zadejte **8080** jako **místní odchozího**a klikněte na tlačítko **Další**.
 ![Nové pravidlo pro příchozí][NewRuleProtocol]
8. Na obrazovce **Akce** zaškrtněte políčko **Povolit připojení**a klikněte na tlačítko **Další**.
 ![Novou akci příchozí pravidla][NewRuleAction]
9. Na obrazovce **profilu** zajistit, aby nedošlo k výběru **domény** **soukromé**a **veřejné** a klikněte na tlačítko **Další**.
 ![Nový profil příchozí pravidla][NewRuleProfile]
10. Na obrazovce **název** zadejte název pravidla, jako je **HttpIn** (název pravidla není musí shodovat s názvem koncový bod však) a potom klikněte na **Dokončit**.  
 ![Název nového příchozí pravidla][NewRuleName]

V tomto okamžiku webu Tomcat by měl být viditelný z externích prohlížeče pomocí adresy URL formuláře * *http://*vaše\_DNS\_název*. cloudapp.net**kde ** *vaše\_DNS\_název*** je název DNS jste zadali při vytváření virtuálního počítače.

## <a name="application-lifecycle-considerations"></a>Poznámky k aplikacím životního cyklu
* Může vytvořit vlastní webové aplikace archiv (WAR) a přiřadit ji někomu **webapps** složky. Například vytvořit projekt dynamické webové služby základní Java služby stránky (JSP) a exportovat jako soubor WAR, zkopírujte VÁLKOU do složky Apache Tomcat **webapps** počítače virtuální, spusťte ho v prohlížeči.
* Ve výchozím nastavení při instalaci služby Tomcat je nastavena na ruční spuštění. Můžete přepnout tak, aby se spouštěl automaticky pomocí modulu snap-in služby. Spusťte modulu snap-in služby klepnutím na tlačítko **Start systému Windows**, **Nástroje pro správu**a potom **Services**. Poklepejte na službu **Apache Tomcat** a nastavení **typu spouštění** na **automaticky**.

    ![Nastavení služby, aby se spouštěl automaticky][service_automatic_startup]

    Výhodou Tomcat start automaticky je, že bude spuštěn, pokud virtuální počítač po restartování (například po instalaci aktualizací softwaru, které vyžadují restartovat počítač).

## <a name="next-steps"></a>Další kroky
Informace o jiných služeb (například Azure úložiště služby bus a databáze SQL), které chcete zahrnout s aplikacemi Java zobrazením informovanosti [Středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
