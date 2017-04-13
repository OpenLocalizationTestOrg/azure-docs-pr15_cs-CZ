<properties
    pageTitle="Nastavení Apache Tomcat na Linux OM | Microsoft Azure"
    description="Zjistěte, jak nastavit Apache Tomcat7 pomocí Azure virtuální počítač (OM) Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Jak nastavit Tomcat7 na počítač virtuální Linux s Microsoft Azure

Apache Tomcat (nebo jednoduše Tomcat, dřív taky Jakarta Tomcat) je otevřít zdroj webový server a servlet kontejneru vyvinuté tak, že Apache Software Foundation ASF (). Tomcat implementuje Java Servlet a specifikace JavaServer stránky (JSP) z Sun Microsystems a poskytuje jen Java HTTP prostředí webového serveru ke spouštění kódu jazyka Java. Nejjednodušší konfigurace Tomcat spuštěn ve výrobním procesu jeden operační systém. Tento proces se spustí virtuálního počítače Java (JVM). Každý požadavek protokolu HTTP z prohlížeče na Tomcat zpracován jako samostatný podproces v procesu Tomcat.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


V této příručce se nainstaluje tomcat7 nad snímkem Linux a nasadit v Microsoft Azure.  

Naučíte se:  

-   Jak vytvořit virtuální počítač v Azure.
-   Jak připravit počítač virtuální tomcat7.
-   Jak nainstalovat tomcat7.

Předpokládá se, že čtenáře už má předplatné Azure.  V opačném případě můžete zaregistrovat bezplatnou zkušební verzi na [http://azure.microsoft.com](https://azure.microsoft.com/). Pokud máte předplatné MSDN, přečtěte si článek [Microsoft Azure zvláštní ceny: MSDN, MPN a Bizspark výhody](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Další informace o Azure najdete v tématu [Co je Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Toto téma předpokládá, že máte základní pracovní znalost tomcat a Linux.  

##<a name="phase-1-create-an-image"></a>Fáze 1: Vytvoření obrázku
V této fázi vytvoříte virtuálního počítače pomocí obrázku Linux v Azure.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Krok 1: Vytvoření SSH ověřovací klíč
SSH je důležitým nástrojem pro správce systému. Konfigurace zabezpečení Accessu podle hesla určují lidské je však není doporučený postup. Uživatelé se zlými úmysly můžete rozdělit do vašeho systému na základě uživatelského jména a hesla slabými.

Dobrá zpráva je způsob, jak ponechání otevřeného vzdáleného přístupu a mít nechcete bát hesla. Metoda sestává z ověřování s asymetrické šifrování. Privátní klíč uživatele je ten, který poskytuje ověřování. Můžete i uzamknout uživatelského účtu chcete úplně zakázat ověřování hesla.

Další výhodou tento způsob je nepotřebujete různých hesel pro přihlášení k jiné servery. Může ověřovat pomocí osobní privátním klíčem na všech serverech, které zabrání museli mějte na paměti několik hesla.

Je také možné k přihlášení pomocí hesla tímto způsobem.

Tímto postupem generovat klávesu SSH ověřování.

1.  Stažení a instalace puttygen z následujících umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Spusťte PUTTYGEN. EXE.
3.  Klikněte na **Generovat** k vygenerování klíčů. V procesu zvyšují náhodnosti přesunutím myši na prázdné místo v okně.  
![][1]
4.  Po dokončení generovat zobrazí Puttygen.exe generovaného klíče. Příklad:  
![][2]
5.  Vyberte a zkopírujte veřejným klíčem **klíče** a uložte ho do souboru nazvaného publicKey.pem. Neklikejte na **uložit veřejný klíč**, protože se liší od veřejný klíč, které chceme formát souboru uloženého veřejným klíčem.
6.  Klikněte na tlačítko **Uložit privátním klíčem** a uložte ho do souboru nazvaného privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Krok 2: Vytvoření obrázku na portálu Azure.
V [Azure portál](https://portal.azure.com/)klikněte na **Nový** na hlavním panelu vytvořit obrázek, zvolte obrázek Linux podle vašich potřeb. V následujícím příkladu se systémem Ubuntu 14.04 obrázek.
![][3]

Pro **Název hostitele** zadejte název pro adresu URL, kterou Internet klientů bude používat pro přístup k tento virtuální počítač. Definování poslední část názvu DNS, například tomcatdemo a Azure vygeneruje adresu URL jako tomcatdemo.cloudapp.net.  

**Klíč pro SSH ověřování**zkopírujte hodnoty klíče z **publicKey.pem** soubor, který obsahuje generované puttygen veřejným klíčem.  
![][4]

Konfigurovat další nastavení, v případě potřeby a pak klikněte na vytvořit.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fáze 2: Příprava počítače virtuální Tomcat7
V této fázi konfigurace koncový bod pro přenos tomcat a připojte k nový virtuální počítač.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Krok 1: Otevřete port HTTP přístupu na web
Koncové body v Azure sestávat protokolu (TCP a UDP) spolu s portem veřejných a privátních. Privátní číslo portu je port, který službu listening na virtuální počítač. Veřejné port je číslo portu, která Azure cloudové služby je naslouchají na externě příchozí, internetové přenosy.  

TCP port 8080 je výchozí číslo portu, na které sleduje tomcat. Otevření tohoto s Azure koncový bod vám umožní je a jiných přístup k Internetu klientů tomcat stránky.  

1.  Na portálu Azure klikněte na tlačítko **Procházet** -> **virtuálního počítače**a potom klikněte na virtuální počítač, který jste vytvořili.  
![][5]
2.  Pokud chcete přidat koncový bod do virtuálního počítače, zaškrtněte políčko **koncové body** .
![][6]
3.  Klikněte na **Přidat**.  
    1.  **Koncový bod**zadejte název koncového bodu v koncového bodu a zadejte 80 do **Portu veřejné**.  

        Pokud má argument ho 80, není potřeba přidat číslo portu v adrese URL, která vám umožní přístup k tomcat. Například http://tomcatdemo.cloudapp.net.    

        Pokud je nastavena na jinou hodnotu, například 81, budete muset přidat číslo portu na adresu URL pro přístup k tomcat. Například http://tomcatdemo.cloudapp.net:81 /.
    2.  Zadejte 8080 Port soukromé. Ve výchozím nastavení tomcat sleduje na portu TCP 8080. Pokud jste změnili výchozí poslech port tomcat, je třeba aktualizovat soukromé Port být že stejný jako tomcat poslech port.  
    ![][7]

4.  Klikněte na **OK** přidáte koncový bod do virtuálního počítače.



###<a name="step-2-connect-to-the-image-you-created"></a>Krok 2: Připojte obrázek, který jste vytvořili.
Můžete použít libovolný SSH nástroj pro připojení k virtuálního počítače. V tomto příkladu použijeme nátěrové.  

Nejprve získáte název DNS virtuálního počítače z portálu Microsoft Azure. **Klikněte na tlačítko Procházet** -> **virtuálních počítačích** -> název počítače virtuální -> **Vlastnosti**a podívejte se do pole **Název domény** dlaždice **Vlastnosti** .  

Pokud potřebujete číslo portu SSH připojení z pole **SSH** . Tady je příklad.  
![][8]

Stahování nátěrové [tady](http://www.putty.org/) .  

Po stažení, klikněte na spustitelný soubor nátěrové. EXE. Konfigurace základní možnosti s názvem host (hostitel) a získat z vlastnosti virtuálního počítače číslo portu. Tady je příklad:  
![][9]

V levém podokně klikněte na **připojení** -> **SSH** -> **ověření** a potom klikněte na **Procházet** a zadejte umístění souboru **privateKey.ppk** , která obsahuje privátním klíčem generovaných puttygen ve fázi 1: Vytvoření obrázku. Tady je příklad:  
![][10]

Klikněte na **Otevřít**. Může být upozorněni tak, že se zprávou. Pokud jste nakonfigurovali název DNS a číslo portu správně, klikněte na **Ano**.
![][11]  


Měli byste vidět takto:  
![][12]

Zadejte své uživatelské jméno zadané při vytváření virtuální počítač ve fázi 1: Vytvoření obrázku. Zobrazí se nějak takto:  
![][13]





##<a name="phase-3-install-software"></a>Fáze 3: Instalace softwaru
V této fázi můžete nainstalovat prostředí Java runtime, tomcat a další součásti tomcat.  

###<a name="java-runtime-environment"></a>Prostředí runtime Java
Tomcat psaných Java. Existují dva typy jazyka Java Development Kit (JDKs) (OpenJDK a Oracle JDK) a můžete si vybrat tu, kterou chcete.  

>AZURE. Poznámka: Obou JDKs mít téměř téhož kódu pro třídy v rozhraní API Java, ale kód virtuální počítač se skutečně liší. Když přijde do knihoven, obvykle OpenJDK pro použití otevřít knihoven při Oracle může používat skrytých z nich. Oracle JDK obsahuje informace, ale tříd a některé pevný chyby a Oracle JDK stabilní více než OpenJDK.

Následující příkazy stáhnout různých JDKs.  

Otevřít jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle jdk  

-   Pokud si chcete stáhnout JDK z webu Oracle:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   Vytvoření adresář, který bude obsahovat JDK souborů:  

        sudo mkdir /usr/lib/jvm  

-   Chcete-li extrahovat JDK soubory do adresáře/usr/knihovny/jvm /:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Chcete-li nastavit jako výchozí JVM Oracle JDK:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
Chcete-li otestovat nainstalovanou verzí prostředí Java runtime správně můžete použijte příkaz jako následující:  

    java -version  

Pokud jste si nainstalovali jdk otevřít, měli byste vidět zpráva podobná této:![][14]

Pokud jste si nainstalovali oracle jdk, měli byste vidět zpráva podobná této:![][15]

###<a name="tomcat7"></a>Tomcat7
Pomocí následujícího příkazu nainstalovat tomcat7:  

    sudo apt-get install tomcat7  

Pokud nepoužíváte tomcat7, použijte příslušné změny tohoto příkazu.  

####<a name="test"></a>Test:

Zaškrtněte, pokud je tomcat7 úspěšně nainstalovaný, přejděte na název serveru tomcat DNS (http://tomcatexample.cloudapp.net/ je ukázková adresa URL v tomto článku). Pokud se zobrazí stránka jako následující, máte nainstalované tomcat7 správné.
![][16]


###<a name="install-other-tomcat-components"></a>Instalace jiné součásti Tomcat
Existují další součásti volitelné tomcat, které můžete taky nainstalovat.  

Pomocí příkazu **sudo výstižný mezipaměti hledání tomcat7** zobrazíte všechny dostupné součásti. Následující příkazy jsou příklady nainstalovat některé užitečné části.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Fáze 4: Konfigurace Tomcat
V této fázi spravovat tomcat.
###<a name="start-and-stop-tomcat7"></a>Spustit a zastavit tomcat7
Tomcat7 server bude automaticky vytvořena při instalaci. Můžete taky začít ji sami s Tento příkaz:   

    sudo /etc/init.d/tomcat7 start

Ukončení tomcat7:  

    sudo /etc/init.d/tomcat7 stop

Chcete-li zobrazit stav tomcat7:  

    sudo /etc/init.d/tomcat7 status

Pokud chcete znovu spustit tomcat služby:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Správa tomcat
Můžete upravit tento soubor Tomcat uživatele konfigurace nastavit svých přihlašovacích údajů správce pomocí následujícího příkazu:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Tady je příklad:  
![][17]  

>AZURE. Poznámka: Vytvoření silného hesla pro správu uživatelské jméno.  

Po úpravě tento soubor, je třeba restartovat tomcat7 služby pomocí následujícího příkazu zajistit, že se změny projeví:  

    sudo /etc/init.d/tomcat7 restart  

Otevřete prohlížeč a zadejte adresu URL **http://<your tomcat server DNS name>/správce/html**. Adresa URL je například v tomto článku http://tomcatexample.cloudapp.net/manager/html.  

Po připojení, měli byste vidět podobnou následující:  
![][18]

##<a name="common-issues"></a>Běžné problémy

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Nemáte přístup ke virtuálního počítače s Tomcat a Moodle z Internetu

-   **Příznaku**  
Tomcat běží, ale nevidíte Tomcat výchozí stránky v prohlížeči.
-   **Hlavní případu**   
    1.  Poslech port tomcat není stejný jako soukromé Port virtuálního počítače koncový bod pro přenos tomcat.  

        Zkontrolujte nastavení koncový bod Port veřejné a privátní Port a ujistěte se, že Port soukromé je že stejná jako tomcat poslech port. Zobrazit fázi 1: Vytvoření obrázek pokyny ke konfiguraci koncové body virtuálního počítače.  

        Určit číslo portu poslech tomcat, otevřete /etc/httpd/conf/httpd.conf (červená klobouk vydaná verze) nebo /etc/tomcat7/server.xml (Debian vydaná verze). Ve výchozím nastavení je port poslech tomcat 8080. Tady je příklad:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Pokud používáte virtuálního počítače jako Debian nebo se systémem Ubuntu a chcete změnit výchozí port z Tomcat poslech (například 8081), měli byste taky otevírat port pro operační systém. Nejdřív otevřete profil:  

            sudo vi /etc/default/tomcat7  

        Potom zrušte komentář poslední řádek a změňte "bez", "Ano".  

            AUTHBIND=yes

    2.  Brána firewall zakázal poslech port tomcat.

        Pokud se zobrazí pouze Tomcat výchozí stránky z místního počítače, je problém s největší pravděpodobností port, který je data podle tomcat je blokován funkcí bránu firewall. Pomocí nástroje w3m přejděte na webovou stránku. Následující příkazy instalace w3m a přejděte na stránku výchozí Tomcat:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Řešení**
    1. Pokud tomcat poslech port není stejný jako soukromé Port koncový bod pro umožnění datových přenosů do virtuálního počítače, třeba změnit Port soukromé být že stejný jako tomcat poslech port.   

    2.  Pokud brána firewall/iptables způsobuje problém, přidejte do /etc/sysconfig/iptables následující řádky:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Všimněte si, že na druhém řádku stačí přenosů https.  

        Ujistěte se, že to nad všechny řádky, které by globálně omezit přístup, například takto:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Nové načtení iptables, spusťte tento příkaz:  

            service iptables restart  

        To byla otestovat na CentOS 6.3.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Oprávnění odepřen kdy nahrát soubory do /var/lib/tomcat7/webapps projektu /  

-   **Příznaku**  
Použijete-li se připojit k počítači virtuální a přejděte na /var/lib/tomcat7/webapps/publikovat svůj web libovolného SFTP klienta (například FileZilla), se zobrazí chybová zpráva podobná této:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Hlavní případu** Nemáte oprávnění pro přístup ke složce /var/lib/tomcat7/webapps.  
-   **Řešení**  
Potřebujete Získejte oprávnění z kořenového účtu. Vlastnictví této složky z kořenového adresáře můžete změnit uživatelské jméno, které jste použili při zřizování počítač. Tady je příklad s názvem azureuser účtu:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    Pomocí možnosti -R příliš nastavit oprávnění pro všechny soubory uvnitř adresář.  

    Všimněte si, že tento příkaz funguje taky pro adresáře. Možnost -R změnil oprávnění pro všechny soubory a adresáře uvnitř adresář. Tady je příklad:  

        sudo chown -R username:group directory  

    Tento příkaz změní vlastnictví (uživatelů a skupin) pro všechny soubory a adresáře uvnitř a adresář samotné.  

    Následující příkaz pouze změny oprávnění ke složce adresáře ale necháte souborů a složek v adresáři samostatně.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
