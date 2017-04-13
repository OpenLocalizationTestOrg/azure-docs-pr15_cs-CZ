<properties
    pageTitle="Jak používat modul plug-in Azure podřízený nepřetržitý integrací Hudsonem | Microsoft Azure"
    description="Popisuje, jak pomocí Hudsonem nepřetržitý integrace služby Azure podřízený modulu plug-in."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Jak používat modul plug-in Azure podřízený souvislé integrací Hudsonem

Modul plug-in pro Hudsonem Azure podřízený umožňuje zajistit podřízený uzlů v Azure při spuštění distribuované sestavení.

## <a name="install-the-azure-slave-plug-in"></a>Instalace modulu plug-in Azure podřízený

1. Na řídicím panelu Hudsonem klikněte na **Spravovat Hudsonem**.

1. Na stránce **Spravovat Hudsonem** klikněte na **Správa modulů plug-in**.

1. Klikněte na kartu **k dispozici** .

1. Klikněte na **Hledat** a zadejte **Azure** omezit seznamu a odpovídající moduly plug-in.

    Pokud rozhodnout pro procházejte seznam dostupných moduly plug-in zjistíte Azure podřízený Plug-inu v části **Správa clusteru a Distributed vytvořit** na kartě **ostatní** .

1. Zaškrtněte políčko u **Azure podřízený modulu plug-in**.

1. Klikněte na **instalovat**.

1. Restartujte Hudsonem.

Teď modulu plug-in je nainstalovaný, bude další kroky a nakonfigurovat modul plug-in se svým profilem Azure předplatné vytvořit šablonu, která se použije při vytváření OM podřízeného uzlu.

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

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Až budete mít profilu předplatného, nakonfigurovat modul plug-in Azure podřízený takto:

1. Na řídicím panelu Hudsonem klikněte na **Spravovat Hudsonem**.

1. Klikněte na tlačítko **Konfigurace systému**.

1. Přejděte na stránce **cloudu** oddíl dolů.

1. Klikněte na **Přidat nové cloudu > Microsoft Azure**.

    ![Přidání nového cloudu][add new cloud]

    Tím se zobrazí pole místo, kam budete muset zadat podrobnosti předplatného.

    ![Konfigurace profilu][configure profile]

1. Zkopírujte certifikát id a správy předplatného ze svého předplatného profilu a vložte je do příslušných polí.

    Při kopírování předplatné id a správa certifikátů, **co nedělat** zahrnout nabídek, které uzavřete hodnoty.

1. Klikněte na **Ověřit konfigurace**.

1. Po úspěšném ověření konfigurace klikněte na **Uložit**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Nastavení virtuálního počítače šablony pro podřízený Azure modulu plug-in

Šablona virtuálního počítače definuje parametrů, které modul plug-in použijete k vytváření podřízeného uzlu na Azure. V následujících krocích jsme budete vytvoření šablony pro OM systémem Ubuntu.

1. Na řídicím panelu Hudsonem klikněte na **Spravovat Hudsonem**.

1. Klikněte na **Konfigurovat systému**.

1. Přejděte na stránce **cloudu** oddíl dolů.

1. V části **cloudu** najděte **Přidat Azure virtuálního počítače šablonu** a klikněte na tlačítko **Přidat** .

    ![Přidání šablony OM][add vm template]

1. Do pole **název** zadejte název služby cloudu. Pokud název, který zadáte odkazuje na existující cloudové služby, budou v této službě zřízení OM. V opačném Azure vytvoří novou.

1. Do pole **Popis** zadejte text, který popisuje šablonu, kterou vytváříte. Tyto informace jsou určeny pouze pro účely dokumentů a nepoužívá v zřizování virtuálního počítače.

1. V poli **popisky** zadejte **linux**. Štítek se používá k identifikaci šablonu, kterou vytváříte a následně se použije při vytváření úlohy Hudsonem neodkazuje na šablonu.

1. Vyberte oblast, kde se OM vytvoří.

1. Vyberte odpovídající velikost OM.

1. Zadejte účet úložiště tam, kde bude OM vytvořili. Ujistěte se, že je v oblasti stejný jako cloudové služby, které budete používat. Pokud chcete vytvořit nové úložiště, můžete, ponechejte toto pole prázdné.

1. Doba uchovávání informací určuje počet minut, než Hudsonem odstraní nedělá podřízený. Nechte toto na výchozí hodnotu 60.

1. V části **použití**vyberte příslušné podmínce při tomto podřízeného uzlu se použijí. Nyní vyberte **Utilize tento uzel co nejvíc**.

    V tomto okamžiku formulář by vypadala poněkud vypadat asi takto:

    ![Konfigurace šablony][template config]

1. **Obrázek řady** nebo Id budete muset zadat, nainstaluje se systém obrázky do svého OM. Můžete vybrat ze seznamu rodiny obrázek nebo zadejte vlastní obrázek.

    Pokud chcete vybrat ze seznamu rodiny obrázek, zadejte první znak (malá a velká písmena) příjmení obrázek. Například psát **U** zobrazíte seznam rodiny se systémem Ubuntu serveru. Po výběru ze seznamu Jenkins při vytváření vašeho OM použije nejnovější verzi této systém obrázek z rodiny.

    ![Rodinný seznam OS][OS family list]

    Pokud máte vlastní obrázek, který chcete použít místo toho, zadejte název vlastní obrázek. Vlastní obrázek názvy nejsou zobrazeny v seznamu, je nutné zajistit, že název se zadává správně.    

    Tento kurz zadejte **U** vyvolejte seznam obrázků se systémem Ubuntu a potom vyberte **Systémem Ubuntu serveru 14.04 l**.

1. **Spuštění metoda**vyberte **SSH**.

1. Skript dole kopírování a vkládání v textovém poli **Inicializace skript** .

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

    **Inicializace skript** bude spuštěn po vytvoření OM. V tomto příkladu se nainstaluje skript Java libovolná a a.

1. V polích **uživatelské jméno** a **heslo** zadejte upřednostňované hodnoty vytvořenou na vaše OM účtu správce.

1. Klikněte na **Ověřit šablony** zkontrolujte, jestli jsou platné parametrů, které jste zadali.

1. Klikněte na **Uložit**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Vytvoření Hudsonem úlohy, která poběží na podřízeného uzlu v Azure

V této části vytváříte Hudsonem úkol, který se spustí podřízeného uzlu na Azure.

1. Na řídicím panelu Hudsonem klikněte na **Nový projekt**.

1. Zadejte název projektu, který vytváříte.

1. Typ práce vyberte **vytvořit úlohy software zase uvolnit styl**.

1. Klikněte na **OK**.

1. Na stránce konfigurace projektu vyberte **omezit, kde je možné spustit tento projekt**.

1. Vyberte **uzel a popisek nabídky** a vyberte **linux** (jsme štítek při vytváření zadat šabloně virtuálního počítače v předchozí části).

1. V části **vytvořit** klikněte na **Přidat sestavit kroku** a vyberte **spustit prostředí**.

1. Upravit tento skript, nahrazení **{github názvu vašeho účtu}**, **{název vašeho projektu}**a **{adresáři projektu}** příslušné hodnoty a vložte upravený skript v oblasti textu, která se zobrazí.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Klikněte na **Uložit**.

1. Na řídicím panelu Hudsonem úlohu jste právě vytvořili a klikněte na ikonu **plánu sestavení** najdete.

Hudsonem pak vytvoření podřízeného uzlu pomocí vytvořený v předchozí části a spusťte skript, který jste zadali v kroku sestavení pro daný úkol.

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/
[předplatné profilu]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

