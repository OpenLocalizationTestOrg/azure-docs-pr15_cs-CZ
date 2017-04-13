<properties
    pageTitle="Vytvoření OM systém MySQL | Microsoft Azure"
    description="Vytvoření Azure virtuálního počítače s Windows serveru 2012 R2 a pomocí klasické nasazení modelu databáze MySQL."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>Nainstalovat MySQL na počítač virtuální vytvořená pomocí klasické nasazení modelu systémem Windows Server 2012 R2

[MySQL](http://www.mysql.com) je Oblíbené otevřít zdroj databáze SQL. Tento kurz se dozvíte, jak nainstalovat a spustit komunity verzi MySQL 5.6.23 jako MySQL Server do virtuálního počítače se systémem Windows serveru 2012 R2. Pokyny k instalaci MySQL na Linux: [Jak nainstalovat MySQL na Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Vytvoření virtuálního počítače s Windows serveru 2012 R2

Pokud ještě nemáte OM systémem Windows Server 2012 R2, můžete tento [kurz](virtual-machines-windows-classic-tutorial.md) vytvoření virtuálního počítače. 

## <a name="attach-a-data-disk"></a>Připojení dat disku

Po vytvoření virtuálního počítače, můžete volitelně připojit disku další data. To je vhodné pro výrobní pracovního vytížení a chcete-li předejít nedostatku místa Frames OS (), který obsahuje operačního systému.

Přečtěte si, [jak připojit disk dat do virtuálního počítače Windows](virtual-machines-windows-classic-attach-disk.md) a postupujte podle pokynů k připojení prázdný disk. Nastavení mezipaměti hostitele **žádné** nebo **jen pro čtení**.

## <a name="log-on-to-the-virtual-machine"></a>Přihlaste se k virtuálního počítače

Pak můžete se [přihlásit k virtuální počítač](virtual-machines-windows-classic-connect-logon.md) abyste mohli nainstalovat MySQL.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Instalace a spuštění Server MySQL komunity na virtuálního počítače

Tímto postupem nainstalovat, nakonfigurovat a spustit komunity verzi MySQL Server:

> [AZURE.NOTE] Tyto kroky platí pro 5.6.23.0 komunity verzi MySQL a Windows Server 2012 R2. Uživatelského prostředí můžou být odlišná na různé verze aplikací MySQL nebo Windows Server.

1.  Po připojení k počítači virtuální pomocí vzdálené plochy, klikněte na **Internet Explorer** na obrazovce start.
2.  Klikněte na tlačítko **Nástroje** v pravém horním rohu (ikona cogged kolo) a potom klikněte na **Možnosti Internetu**. Klikněte na kartu **zabezpečení** , klikněte na ikonu **Důvěryhodné servery** a potom klikněte na tlačítko **weby** . Přidání http://*. mysql.com do seznamu důvěryhodných webů. Klikněte na * *Zavřít**a potom klikněte na * *OK**.
3.  V adresu aplikace Internet Explorer, zadejte http://dev.mysql.com/downloads/mysql/.
4.  Použijte web resetování MySQL vyhledejte a stáhněte si nejnovější verzi MySQL Instalační služby systému Windows. Při výběru instalační program MySQL, stáhněte si verzi, která obsahuje vyplňte souboru nastavení (například mysql-instalační program komunity 5.6.23.0.msi s velikostí souboru 282.4 MB) a uložte instalační program.
5.  Po dokončení stahování instalačního programu klepnutím na tlačítko **Spustit** spusťte instalační program.
6.  Na stránce **Podmínky licenční smlouvy** přijměte podmínky licenční smlouvy a klikněte na tlačítko **Další**.
7.  Na stránce **Vyberte typ instalace** klikněte na požadovaný typ instalace a klikněte na tlačítko **Další**. Následující kroky předpokládají výběr typu instalace **jenom Server** .
8.  Na stránce **instalace** klikněte na **Spustit**. Po dokončení instalace klikněte na **Další**.
9.  Na stránce **Konfigurace produktu** klikněte na **Další**.
10. Na stránce **Typ a sítě** zadejte požadovaná konfigurace typu a připojení možností, včetně TCP port v případě potřeby. Vyberte **Zobrazit rozšířené možnosti**a potom na tlačítko **Další**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Na stránce **účty a role** zadejte silné heslo kořenové MySQL. Podle potřeby přidejte další MySQL uživatelské účty a potom na tlačítko **Další**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Na stránce **Služby Windows** zadejte změny do výchozího nastavení pro spuštění MySQL Server jako služba Windows podle potřeby a klikněte na tlačítko **Další**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Na stránce **Upřesnit možnosti** upřesnit změny možností protokolování v případě potřeby a pak klikněte na **Další**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Na stránce **Použití konfigurace serveru** klikněte na **Spustit**. Po dokončení konfigurace kroky klikněte na **Dokončit**.
15. Na stránce **Konfigurace produktu** klikněte na **Další**.
16. Na stránce **Dokončení instalace** klikněte na **Log kopírovat do schránky** , pokud chcete prozkoumat později a potom klikněte na **Dokončit**.
17. Na obrazovce start zadejte **mysql**a klikněte na **MySQL 5.6 příkazového řádku klienta**.
18. Zadejte heslo kořenové, které jste zadali v kroku 11 a zobrazí se uvedené s výzvou místo, kam můžete vydávat příkazy pro nastavení MySQL. Podrobné informace o příkazech a syntaxi najdete v tématu [MySQL referenční příručky](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Konfigurace výchozího nastavení serveru, jako jsou základní a data adresáře a jednotky, můžete taky nakonfigurovat s položkami v souboru 5.6\my-default.ini C:\Program Files (x86) \MySQL\MySQL serveru. Další informace najdete v tématu [5.1.2 výchozí konfigurace serveru](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Konfigurace koncové body

Pokud chcete službu MySQL Server k dispozici pro klientské počítače MySQL na Internetu, musíte konfigurace koncového bodu port TCP, na kterém je služba MySQL Server přijímá a vytvořit další pravidlo brána Windows Firewall. Toto je TCP port 3306, pokud není zadáno jiný port na stránce **Typ a sítí** (krok 10 z předchozího postupu).


> [AZURE.NOTE] Měli byste pečlivě zvážit důsledky zabezpečení tím, protože tím zpřístupníte službu MySQL Server do všech počítačů v Internetu. Můžete definovat sadu zdroj IP adresy, které jsou povoleny používat koncový bod se seznam řízení přístupu (ACL). Další informace najdete v tématu [jak nastavit koncové body do virtuálního počítače](virtual-machines-windows-classic-setup-endpoints.md).


Konfigurace koncový bod služby MySQL Server:

1.  Na portálu Azure klasické klikněte na **virtuálních počítačích**, klikněte na název virtuálního počítače MySQL a klikněte na **koncové body**.
2.  V řádku nabídek klikněte na **Přidat**.
3.  Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na šipku doprava.
4.  Pokud používáte výchozí port MySQL TCP 3306, klikněte na **MySQL** do pole **název**a klikněte na značku zaškrtnutí.
5.  Pokud používáte různé porty TCP, zadejte jedinečný název do pole **název**. Vyberte v protokolu **TCP** , zadejte číslo portu **Port veřejné** a **Privátní Port**a klepněte na značku zaškrtnutí.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Přidání pravidla brány Firewall systému Windows umožňující přenosy MySQL

Chcete-li přidat pravidlo Brána Firewall systému Windows, které umožňuje MySQL přenos z Internetu, spusťte tento příkaz na příkazovém řádku prostředí Windows PowerShell v počítači virtuální server MySQL.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Vyzkoušejte vzdáleného připojení


Vyzkoušejte vzdáleného připojení ke službě MySQL Server Azure virtuální počítač se systémem, musíte nejdřív určete odpovídající cloudové služby, který obsahuje počítač virtuální MySQL Server název DNS.

1.  Na portálu Azure klasické klikněte na **virtuálních počítačích**, klikněte na název virtuálního počítače server MySQL a potom klikněte na **řídicí panel**.
2.  Na řídicím panelu virtuálního počítače všimněte hodnoty **Název DNS** části **Získat rychlý přehled** . Tady je příklad:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Z místního počítače se systémem MySQL nebo MySQL klienta spusťte tento příkaz k přihlášení jako uživatel MySQL.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    Například pro dbadmin3 MySQL uživatelské jméno a název testmysql.cloudapp.net DNS pro virtuální počítač, zadejte následující příkaz.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Další kroky

Další informace o tom, jak MySQL, najdete v [Dokumentaci MySQL](http://dev.mysql.com/doc/).
