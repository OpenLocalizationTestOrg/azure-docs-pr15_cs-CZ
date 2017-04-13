<properties
    pageTitle="Jak používat modul plug-in Azure podřízený nepřetržitý integrací Jenkins | Microsoft Azure"
    description="Popisuje, jak pomocí Jenkins nepřetržitý integrace služby Azure podřízený modulu plug-in."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Jak používat modul plug-in Azure podřízený nepřetržitý integrací Jenkins

Azure podřízený modul plug-in pro Jenkins slouží k poskytování podřízený uzlů v Azure při spuštění distribuované sestavení.

## <a name="install-the-azure-slave-plug-in"></a>Instalace modulu plug-in Azure podřízený

1. Na řídicím panelu Jenkins klikněte na **Spravovat Jenkins**.

1. Na stránce **Spravovat Jenkins** klepněte na položku **Spravovat doplňky**.

1. Klikněte na kartu **k dispozici** .

1. Do pole filtru nad seznamem dostupných moduly plug-in zadejte **Azure** omezit seznamu a odpovídající moduly plug-in.

    Pokud procházejte seznam doplňků k dispozici moduly, zjistíte Azure podřízený Plug-inu v části **Správa clusteru a Distributed sestavíte** .

1. **Modul plug-in podřízený Azure** zaškrtněte políčko.

1. Klikněte na **nainstalovat bez restart** nebo **Stáhnout a nainstalovat po restartování**.

Teď modulu plug-in je nainstalovaný, je dalším krokům nakonfigurovat modul plug-in se svým profilem Azure předplatné a vytvořit šablonu, která se použije při vytváření virtuálního počítače podřízeného uzlu.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurace Azure podřízený modul plug-in se svým profilem předplatného

Profil předplatné taky uvedená nastavení publikování, je soubor XML, který obsahuje zabezpečené přihlašovací údaje a některé další informace, které budete muset pracovat s Azure ve vývojovém prostředí. Abyste mohli nakonfigurovat modul plug-in Azure podřízený, musíte:

* Id předplatného
* Správa certifikátů pro vaše předplatné

Tyto najdete ve svém [profilu předplatného]. Tady je příklad profil předplatného.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Až budete mít profilu předplatného, nakonfigurovat modul plug-in Azure podřízený takto:

1. Na řídicím panelu Jenkins klikněte na **Spravovat Jenkins**.

1. Klikněte na tlačítko **Konfigurace systému**.

1. Přejděte na stránce **cloudu** oddíl dolů.

1. Klikněte na **Přidat nové cloudu > Microsoft Azure**.

    ![oddíl cloudu][cloud section]

    Tím se zobrazí pole místo, kam budete muset zadat podrobnosti předplatného.

    ![Konfigurace předplatného][subscription configuration]

1. Zkopírujte hodnoty předplatné id a správa certifikátů ze svého předplatného profilu a vložte je do příslušných polí.

    Při kopírování předplatné id a správa certifikátů, nezahrnujte nabídek, které uzavřete hodnoty.

1. Klikněte na tlačítko **Ověřte konfiguraci**.

1. Po konfiguraci ověření správně, klikněte na **Uložit**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Nastavení virtuálního počítače šablony pro Azure podřízený modulu plug-in

Šablona virtuálního počítače definuje parametrů, které modul plug-in použijete k vytváření podřízeného uzlu na Azure. V následujících krocích vytvoříme šablony pro virtuální počítač se systémem Ubuntu.

1. Na řídicím panelu Jenkins klikněte na **Spravovat Jenkins**.

1. Klikněte na tlačítko **Konfigurace systému**.

1. Přejděte na stránce **cloudu** oddíl dolů.

1. V části **cloudu** najděte **Šablonu Azure virtuálního počítače přidat**a potom klikněte na **Přidat**.

    ![Přidání šablony OM][add vm template]

    Zobrazí toto pole kde zadejte podrobnosti o šablonu, kterou vytváříte.

    ![prázdné obecné konfigurace][blank general configuration]

1. Do pole **název** zadejte název služby Azure cloudu. Pokud název, který jste zadali odkazuje na existující cloudové služby, budou v této službě zřízení virtuální počítač. V opačném Azure vytvoří novou.

1. Do pole **Popis** zadejte text, který popisuje šablonu, kterou vytváříte. Toto je pouze pro záznamy a nepoužívá zřizování virtuálního počítače.

1. Pole **popisky** slouží k identifikaci šablonu, kterou vytváříte a následně se použije při vytváření úlohy Jenkins neodkazuje na šablonu. Pro naše účel zadejte do tohoto pole **linux** .

1. V seznamu **oblast** klikněte na požadovanou oblast, kde bude vytvořen virtuální počítač.

1. V seznamu **Velikost virtuálního počítače** klikněte na příslušný formát.

1. Do pole **Název účtu úložiště** zadejte účet úložiště místo, kam se bude vytvořena virtuální počítač. Ujistěte se, že je v oblasti stejný jako cloudové služby, které budete používat. Pokud chcete vytvořit nové úložiště, můžete toto políčko ponechat prázdné.

1. Doba uchovávání informací určuje počet minut, než Jenkins odstraní nedělá podřízený. Nechte toto na výchozí hodnotu 60. Můžete taky ukončit podřízený namísto jeho odstranění nečinný. Aby je dostala, zaškrtněte políčko **Pouze vypnutí (neodstraňujte) po dobu uchovávání informací** .

1. V seznamu **využití** klikněte na příslušné podmínce při tomto podřízeného uzlu se použijí. Nyní klikněte na **Utilize tento uzel co nejvíc**.

    V tomto okamžiku formulář by měl vypadat poněkud vypadat asi takto:

    ![Konfigurace obecných šablony kontrolní bod][checkpoint general template config]

    Dalším krokem je poskytování údajů o obrázek operační systém, který má podřízený byly vytvořeny v.

1. V dialogovém okně **Obrázek řady nebo Id** budete muset zadat, nainstaluje systém obrázky v počítači virtuální. Můžete vybrat ze seznamu rodiny obrázek nebo zadejte vlastní obrázek.

    Pokud chcete vybrat ze seznamu obrázek skupiny, zadejte prvnímu znaku (malá a velká písmena) příjmení obrázek. Například psát **U** zobrazíte seznam rodiny se systémem Ubuntu serveru. Po výběru ze seznamu Jenkins budou používat nejnovější verzi této systém obrázek z rodiny při zřizování virtuálního počítače.

    ![Ukázka obrázku s operačním systémem seznamu][OS Image list sample]

    Pokud máte vlastní obrázek, který chcete použít místo toho, zadejte název vlastní obrázek. Vlastní obrázek názvy nejsou zobrazeny v seznamu, je nutné zajistit, že název se zadává správně.

    Pro účely tohoto návodu zadejte **U** vyvoláte seznam obrázků se systémem Ubuntu a klikněte na **Systémem Ubuntu serveru 14.04 l**.

1. V seznamu **Metoda Snadné spuštění** klikněte na **SSH**.

1. Skript dole zkopírovat a vložit ho do pole **Skript inicializace** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Skript inicializace bude spouštět po vytvoření virtuálního počítače. V tomto příkladu se nainstaluje skript Java libovolná a a.

1. V polích **uživatelské jméno** a **heslo** zadejte upřednostňované hodnoty vytvořenou v počítači virtuální účtu správce.

1. Klikněte na **Ověřit šablony** , které zkontrolujte, jestli jsou platné parametrů, které jste zadali.

1. Klikněte na **Uložit**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Vytvoření Jenkins úlohy, která poběží na podřízeného uzlu v Azure

V této části vytváříte Jenkins úkol, který se spustí podřízeného uzlu na Azure. Musíte mít vlastní projekt na GitHub ke sledování.

1. Na řídicím panelu Jenkins klikněte na **Nová položka**.

1. Zadejte název pro úkol, který vytváříte.

1. Typ projektu klikněte na **projekt volný styl**.

1. Klikněte na **Ok**.

1. Na stránce konfigurace úkolu vyberte **omezit, kde je možné spustit tento projekt**.

1. Do pole **Výraz popisek** zadejte **linux**. V předchozí části jsme vytvořili podřízený šablony, že pojmenovali jsme ho **linux**, což je co zadáváme tady.

1. V části **vytvořit** klikněte na **Přidat sestavit kroku** a vyberte **spustit prostředí**.

1. Upravte tento skript, nahraďte příslušné hodnoty **(GitHub název svého účtu)**, **(vaše jméno projekt)**a **(adresáři projektu)** a vložte upravený skript v oblasti textu, která se zobrazí.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Klikněte na **Uložit**.

1. Na řídicím panelu Jenkins najeďte myší na úkol, který jste právě vytvořili a klikněte na šipku rozevíracího seznamu zobrazíte možnosti úkolů.

1. Klikněte na **vytvořit**.

Jenkins pak vytvoření podřízeného uzlu pomocí vytvořený v předchozí části a spusťte skript, který jste zadali v kroku sestavení pro daný úkol.

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/
[předplatné profilu]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png