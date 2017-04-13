<properties 
    pageTitle="Jak nainstalovat Apache Qpid kanálem-C Linux OM | Microsoft Azure"
    description="Jak vytvořit OM Linux CentOS pomocí Azure virtuálních počítačích a jak vytvářet a nainstalovat knihovnu Apache Qpid kanálem-C."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Nainstalovat Azure OM Linux Apache Qpid kanálem C

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Tento oddíl vysvětluje, jak vytvořit OM Linux CentOS pomocí Azure virtuálních počítačích a jak stáhnout, vytvořit a nainstalovat knihovnu Apache Qpid kanálem-C spolu s jazyk vazby Python a PHP. Po dokončení těchto kroků, budou moct spuštění Python a PHP ukázek zahrnutý v této příručce.

Cílem prvního kroku je dělaly pomocí [Azure klasické portálu][]. Následující snímek obrazovky ukazuje, jak se vytvoří CentOS OM s názvem "Jiří centos":

![Kanálem na Azure Linux OM][0]

Po zajištění služby, zobrazí se následující portálu:

![Kanálem na Azure Linux OM][1]

Abyste mohli přihlásit k počítači, musíte znát port koncového bodu pro SSH. Tato hodnota můžete získat z [Azure klasické portál][] zvolíte nově vytvořený OM a kliknete na kartě **koncové body** . Následující snímek obrazovky znázorňuje, zda je veřejné SSH port pro tento počítač 57146.

![Kanálem na Azure Linux OM][2]

Teď můžete připojit k počítači pomocí SSH. Tento příklad používá nástroj nátěrové jako následující obrazovka snímek:

![Kanálem na Azure Linux OM][3]

Tento příklad používá k aplikacím Python a PHP knihoven klienta kanálem z Apache. Tyto knihovny jsou k dispozici ke stažení [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). V souboru Readme v balíček distribuce vysvětluje kroky potřebné k instalaci závislostí a vytvořte kanálem. Tady je přehled kroků:

1.  Úprava yum konfiguračního souboru (/ etc/yum.conf) a komentáře se vyloučení aktualizací jádra záhlaví (\# vyloučit = jádra\*). Toto je třeba nainstalovat kompilátoru RSZ.

2.  Instalace požadovaných balíčků:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Stáhněte knihovnu kanálem:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Extrahování kód kanálem z archivu rozdělení:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Vytvoření a nainstalujte kód pomocí následujících kroků z souboru Readme:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Po dokončení těchto kroků, je kanálem nainstalovaných v počítači a je připraven k použití.

## <a name="next-steps"></a>Další kroky

Jste připravení na další informace? Přejděte na následující odkaz:

- [AMQP Bus služby základní informace][]

[AMQP Bus služby základní informace]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure klasické portálu]: http://manage.windowsazure.com


