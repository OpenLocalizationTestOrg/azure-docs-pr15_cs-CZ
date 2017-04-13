<properties
 pageTitle="Odeslání úlohy balík HPC cluster v Azure | Microsoft Azure"
 description="Zjistěte, jak nastavit místním počítači můžou odeslat úlohy HPC Pack clusteru v Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Odeslat HPC úlohy z místního počítače k clusteru HPC Pack nasazenou v Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Konfigurace klientského počítače k místním úlohy do [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) obrázku v Azure. Tento článek popisuje, jak nastavit místního počítače s klientských nástrojích odeslání úlohy prostřednictvím HTTPS clusteru v Azure. Tímto způsobem můžete odeslat několik uživatelů clusteru úlohy ke cloudové HPC Pack clusteru, ale bez připojení přímo do hlavního uzlu OM nebo přístupu k předplatným Azure.

![Odeslání úlohy cluster v Azure][jobsubmit]

## <a name="prerequisites"></a>Zjistit předpoklady pro

* **Hlavy uzel HPC Pack nasazenou v Azure OM** - doporučujeme použít automatické nástrojů, jako jsou pro [šablonu Azure rychlý úvod](https://azure.microsoft.com/documentation/templates/) nebo [skript Powershellu Azure](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) nasadit hlavního uzlu a obrázku. Potřebujete název DNS hlavního uzlu a přihlašovací údaje Správce clusteru postupujte podle pokynů v tomto článku.

* **Klientský počítač** - potřebujete Windows nebo Windows Server klientský počítač, mohlo by umožnit spuštění HPC Pack klientské nástroje (viz téma [Systémové požadavky](https://technet.microsoft.com/library/dn535781.aspx)). Pokud chcete použít HPC Pack webového portálu nebo rozhraní REST API posílat úkoly, můžete použít libovolný klientský počítač podle svého výběru.

* Není k dispozici z [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=328024) **HPC Pack instalační médium** - nainstalovat HPC Pack klientské nástroje volný instalační balíček pro nejnovější verzi sady HPC (HPC Pack 2012 R2). Ujistěte se, stáhnout stejnou verzi aplikace, které máte nainstalované na uzel hlavy OM HPC Pack.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Krok 1: Instalace a konfigurace webové součásti na uzel hlavy

Chcete-li povolit rozhraní REST odeslání úlohy clusteru prostřednictvím HTTPS, nakonfigurujte webové součásti HPC Pack na uzel hlavy HPC Pack. Pokud již nejsou nainstalovány, nejprve nainstalujte webové součásti spuštěním HpcWebComponents.msi instalační soubor. Nakonfigurujte součásti spuštěním skriptu HPC PowerShell **Set-HPCWebComponents.ps1**.

Podrobné pokyny naleznete v tématu [Instalace Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Některé šablony Azure rychlý úvod pro HPC Pack instalace a konfigurace webové součásti automaticky. Pokud použijete [skript pro nasazení HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) k vytvoření clusteru, můžete volitelně nainstalovat a nakonfigurovat webové součásti jako součást nasazení.

**Chcete-li nainstalovat součásti webu**

1. Připojení k hlavního uzlu OM pomocí přihlašovacích údajů Správce clusteru.

2. Ve složce instalace sady HPC spouštět HpcWebComponents.msi na uzel hlavy.

3. Postupujte podle pokynů v průvodci Instalace součásti webu

**Konfigurace webové součásti**

1. Na uzel hlavy spusťte jako správce HPC Powershellu.

2. Změna adresáře umístění skript konfiguraci, zadejte tento příkaz:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Ke konfiguraci rozhraní REST a spuštění HPC webové služby, zadejte tento příkaz:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Po zobrazení výzvy vyberte certifikát, vyberte certifikát, který odpovídá názvu veřejné DNS hlavy uzlu. Třeba když nasadíte hlava uzel OM pomocí klasické nasazení modelu, název certifikátu vypadá CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Pokud používáte nasazení modelu správce prostředků, názvu certifikátu vypadá CN =&lt;*HeadNodeDnsName*&gt;. &lt; *oblast*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Certifikát později při vyberete odeslat úlohy hlavy uzel z místního počítače. Nevyberete ani konfigurace certifikát, který odpovídá názvu počítače hlavou uzlů v doméně služby Active Directory (například CN =*MyHPCHeadNode.HpcAzure.local*).

5. Konfigurace webového portálu pro odeslání projektu, zadejte tento příkaz:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Po dokončení skriptu zastavení a nové spuštění Plánovač úloh HPC zadáním následujících příkazů:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Krok 2: Instalace klientské nástroje HPC Pack na místním počítači

Pokud chcete nainstalovat klientské nástroje HPC Pack ve vašem počítači, stáhněte instalační soubory HPC Pack (úplná instalace) ze [Služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=328024). Jakmile začnete instalace, zvolte možnost instalace produktu **HPC Pack klientské nástroje**.

Použít klientských nástrojích HPC Pack úlohy do hlavního uzlu OM, musíte taky exportovat certifikát z hlavního uzlu a nainstalujte si ho v klientském počítači. Certifikát musí být v. Formát CER.

**Export certifikátu z hlavou uzel**

1. Na uzel hlavy přidání modulu snap-in Certifikáty Microsoft Management Console účtu místního počítače. Postup přidání modulu snap-in najdete v článku [Přidání modulu Snap-in Certifikáty do konzoly MMC](https://technet.microsoft.com/library/cc754431.aspx).

2. V konzole, rozbalte **certifikáty – místní** > **osobní**a potom na položku **certifikáty**.

3. Vyhledejte certifikát, který jste nakonfigurovali pro HPC Pack web součástí [Krok 1: instalace a konfigurace webové součásti na uzel hlavy](#step-1:-install-and-configure-the-web-components-on-the-head-node) (například CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Klikněte pravým tlačítkem myši certifikát a klikněte na **Všechny úkoly** > **Exportovat**.

5. V Průvodci exportem certifikátu klikněte na tlačítko **Další**a vyberte **Ne, nebudou exportovány privátním klíčem** .

6. Provedením zbývajících kroků průvodce a exportujte certifikát v binární X.509 kódováním DER (. Formát CER).


**Import certifikátu v klientském počítači**


1. Zkopírujte certifikát, který jste exportovali z hlavního uzlu do složky v klientském počítači.

2. V klientském počítači spusťte příkaz certmgr.msc.

3. Ve Správci certifikátů rozbalte **Certifikáty – aktuální uživatel** > **Důvěryhodné kořenové certifikační autority**, klikněte pravým tlačítkem **certifikáty**a pak klikněte na **Všechny úkoly** > **importovat**.

4. V Průvodci importem certifikátu klikněte na tlačítko **Další** a postupujte podle pokynů k importu certifikát, který jste exportovali z hlavního uzlu k úložišti důvěryhodných kořenových certifikátů.



>[AZURE.TIP] Může se zobrazit upozornění zabezpečení, protože certifikační autority na uzel hlavy nebyl rozpoznán tak, že klientský počítač. Pro účely testování můžete ignorovat upozornění a dokončete import certifikátu.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Krok 3: Spustit testovací úlohy na clusteru

Pro ověření vaší konfigurace, spusťte úlohy clusteru v Azure z místního počítače. Například můžete HPC Pack grafické nástroje nebo příkazy úlohy do clusteru. Portál založené na webu můžete taky odeslat úlohy.


**Spuštění úlohy odeslání příkazy v klientském počítači**


1. V klientském počítači nainstalovanou klientské nástroje HPC Pack spusťte příkazový řádek.

2. Zadejte příkaz vzorku. Například, seznam všechny úlohy v clusteru, zadejte příkaz podobně jako jednu z následujících akcí v závislosti na úplný název DNS hlavního uzlu:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    nebo
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Použijte úplný název DNS hlavního uzlu není IP adresu do pole Adresa URL plánovač. Pokud zadáte IP adresu, chyba se zobrazí podobný "certifikát serveru musí mít platný řetěz zabezpečení nebo umístěná v úložišti Důvěryhodné kořenové."

3. Po zobrazení výzvy zadejte uživatelské jméno (ve formuláři &lt;název_domény&gt;\\&lt;uživatelské jméno&gt;) a hesla správce HPC obrázku nebo jiného obrázku, které jste nakonfigurovali. Můžete uložit místně pro další operace úlohy její přihlašovací údaje.

    Zobrazí se seznam úloh.


**Pomocí Správce úloh HPC v klientském počítači**

1. Pokud jste se vlastně uložíte dříve domény pověření pro uživatele clusteru při odesílání úlohy, můžete přidat její přihlašovací údaje správce přihlašovacích údajů.

    na. V Ovládacích panelech v klientském počítači spusťte Správce přihlašovacích údajů.

    b. Klikněte na **Přihlašovací údaje Windows** > **Přidat obecný přihlašovacích údajů**.

    c. Určení internetovou adresu (například https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler nebo https://&lt;HeadNodeDnsName&gt;.&lt; oblast&gt;.cloudapp.azure.com/HpcScheduler) a uživatelské jméno (&lt;název_domény&gt;\\&lt;uživatelské jméno&gt;) a hesla správce obrázku nebo jiného obrázku, které jste nakonfigurovali.

2. V klientském počítači spusťte Správce úloh HPC.

3. V dialogovém okně **Vyberte uzel vedoucí** zadejte adresu URL na hlavní uzel Azure (například https://&lt;HeadNodeDnsName&gt;. cloudapp.net nebo https://&lt;HeadNodeDnsName&gt;.&lt; oblast&gt;. cloudapp.azure.com).

    Správce úloh HPC otevře a zobrazí se seznam úlohy na uzel hlavy.

**Používat portál web spuštěna hlavou uzel**

1. Spusťte webový prohlížeč na klientském počítači a zadejte jednu z následujících adres, podle toho, úplný název DNS hlavního uzlu:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    nebo
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. V dialogovém okně zabezpečení zadejte doménu přihlašovací údaje správce HPC obrázku. (Můžete také přidat jiní uživatelé obrázku v různé role. V tématu [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335.aspx).)

    Webového portálu otevře zobrazení seznamu projektu.

3. Odeslání úlohy vzorku, který vrátí řetězec "Ahoj světe" z clusteru, klikněte na **Nový projekt** v levém navigačním panelu.

4. Na stránce **Nový projekt** v části **ze stránky odeslání**, klikněte na **Hello World**. Zobrazí se stránka odeslání projektu.

5. Klikněte na **Odeslat**. Pokud se zobrazí výzva, zadejte doménu pověření správce clusteru HPC. Odeslání úlohy a ID úlohy se zobrazí na stránce **Moje úlohy** .

6. Aby se zobrazily výsledky projektu, ze kterého jste odeslali, klikněte na ID úlohy a pak klikněte na **Zobrazení úkolů** k zobrazení výstupu příkazového řádku (ve skupinovém rámečku **výstup**).

## <a name="next-steps"></a>Další kroky

* Taky můžete odeslat úlohy Azure clusteru s [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).

* Pokud chcete odeslat úlohy obrázku z klienta Linux, najdete v článku ukázku Python [HPC Pack 2012 R2 SDK a ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
