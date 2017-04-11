<properties
    pageTitle="Instalace RStudio serverem R na HDInsight (verze preview) | Microsoft Azure"
    description="Jak nainstalovat RStudio serverem R na HDInsight (verze preview)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Instalace RStudio serverem R na HDInsight (verze preview)

Existuje několik integrované vývojové prostředí (integrovaném vývojovém prostředí) k dispozici pro R dnes, naposledy včetně společnosti Microsoft se ohlásí, [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), Skupina nástroje pro stolní počítače a servery z [RStudio](https://www.rstudio.com/products/rstudio-server/)nebo Walware na základě zatmění [StatET](http://www.walware.de/goto/statet). Mezi nejoblíbenější na Linux je použití [RStudio serveru](https://www.rstudio.com/products/rstudio-server/) , který poskytuje integrovaném vývojovém prohlížečový prostředí pro použití vzdálené klienty.  Instalace RStudio serveru na uzel hrany clusteru HDInsight Premium nabízí celou prostředí integrovaném vývojovém prostředí pro vývoj a provádění R skripty R serveru na clusteru a může být mnohem efektivnější než výchozí konzole R.

V tomto článku se dozvíte, jak nainstalovat komunity (zdarma) verzi serveru RStudio na uzel okraj clusteru pomocí vlastní skript. Podle potřeby komerčně licencovanou verzi Pro RStudio serveru, postupujte podle pokynů k instalaci z [RStudio serveru](https://www.rstudio.com/products/rstudio/download-server/).

> [AZURE.NOTE] Kroky v tomto dokumentu vyžadují Server R HDInsight clusteru a nebude fungovat správně, pokud používáte HDInsight obrázku kde R byl nainstalovaný pomocí [Instalace akci skriptu R](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Azure HDInsight obrázku s nainstalovaným R serverem. Pokyny najdete v tématu [Začínáme s R Server místní clusterů HDInsight](hdinsight-hadoop-r-server-get-started.md).
* SSH klienta. Distribuce Linux a Unix nebo Macintosh OS X `ssh` příkaz je součástí operačního systému. Pro systém Windows doporučujeme [softwaru Cygwin](http://www.redhat.com/services/custom/cygwin/) s [možnost funkci OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU)nebo [nátěrové](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Instalace RStudio clusteru pomocí vlastní skript

1. Určení uzel okraj clusteru. HDInsight clusteru serverem R následuje konvence pro hlavy uzel a uzel okraje.

    * Hlavy uzel-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Okraje uzel-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH do postranní uzel clusteru pomocí výše uvedených naming vzorku. 
 
    * Pokud se připojujete z klienta Linux, najdete v článku [připojení k obrázku na základě Linux HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Pokud se připojujete z klienta se systémem Windows, najdete v článku [připojení k clusteru na základě Linux HDInsight pomocí nátěrové](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Jakmile jste připojeni, stane uživatel root na clusteru. V průběhu relace SSH použijte následující příkaz.

        sudo su -

4. Stáhněte si vlastní skript nainstalovat RStudio. Zadejte následující příkaz.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Změna oprávnění k souboru vlastní skript a následujícím způsobem. Použijte následující příkazy.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Pokud jste použili heslo SSH při vytváření clusteru HDInsight serverem R, můžete tento krok přeskočit a přejít na další. Pokud místo toho použít SSH klíč k vytvoření clusteru je pro vaše uživatele SSH nastavit heslo. Toto heslo budete potřebovat při připojování k RStudio. Spusťte následující příkazy. Po zobrazení výzvy k **aktuální Kerberos heslo**, stačí stisknout **ENTER**.  Všimněte si, že musíte nahradit `USERNAME` s uživatelem SSH pro svůj cluster HDInsight.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Pokud je nastavena úspěšně hesla, byste měli vidět nějaká takováto zpráva.

        passwd: password updated successfully


    Ukončení relace SSH.

7. Vytvoření SSH tunelem clusteru mapováním `localhost:8787` clusteru HDInsight na klientském počítači. Je třeba vytvořit SSH tunelem před otevřením novou relaci prohlížeče.

    * Na Linux klienta nebo klienta Windows s [softwaru Cygwin](http://www.redhat.com/services/custom/cygwin/) potom otevřete relaci Terminálové a zadejte následující příkaz.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        **Uživatelské_jméno** nahraďte uživatel SSH HDInsight clusteru a **NÁZEV_CLUSTERU** s názvem HDInsight obrázku můžete také SSH klíč, nikoli heslo přidáním`-i id_rsa_key`     

    * Pokud pak pomocí klienta Windows s nátěrové

        1.  Otevřete nátěrové a zadejte informace o připojení. Pokud znáte není nátěrové, přečtěte si článek [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md) informace o tom, jak ho použít s HDInsight.
        2.  V části **kategorie** nalevo od dialogového okna rozbalte **připojení**, rozbalte **SSH**a vyberte **tunelů**.
        3.  Ve formuláři **Možnosti řízení SSH port předávání** zadejte následující informace:

            * **Zdrojový port** - port na straně klienta, který chcete předat dál. Například **8787**.
            * **Určení** – cíle, který je potřeba namapovat do místního klientského počítače. Například **localhost:8787**.

            ![Vytvoření SSH tunelem] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Vytvoření SSH tunelem")

        4. Kliknutím na **Přidat** přidejte nastavení a potom klikněte na **Otevřít** otevřete SSH připojení.
        5. Po zobrazení výzvy, přihlaste se k serveru. Tím vytvořit relaci SSH a povolit tunelem.

8. Otevřete webový prohlížeč a zadejte následující adresu URL podle port, který jste zadali jako tunelem.

        http://localhost:8787/ 

9. Zobrazí se výzva k zadání SSH uživatelské jméno a heslo pro připojení k clusteru. Pokud jste použili při vytváření clusteru SSH klíč, je nutné zadat heslo, které jste vytvořili v předchozím kroku 5.

    ![Připojení k R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Vytvoření SSH tunelem")

10. K ověření, zda úspěšné RStudio instalaci, můžete spustit test skript, který se spustí R podle clusteru MapReduce a Spark úlohy. Přejděte zpátky na konzole SSH a zadejte následující příkazy stáhnout zkušební skript spuštění RStudio.

    * Pokud jste vytvořili Hadoop obrázku s R, pomocí tohoto příkazu.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Pokud jste vytvořili Spark obrázku s R, pomocí tohoto příkazu.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. V RStudio zobrazí se zkušební skript, který jste stáhli. Poklikejte na soubor otevřít, vyberte obsah souboru a klikněte na **Spustit**. Měli byste vidět výstup v **konzole** .
 
    ![Test instalace] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test instalace")

Další možností je `source(testhdi.r)` nebo `source(testhdi_spark.r)` spustit skript.

## <a name="see-also"></a>Viz taky

- [Výpočet kontextu možnosti R serveru na HDInsight clusterů](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure možnosti ukládání R serveru na HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
