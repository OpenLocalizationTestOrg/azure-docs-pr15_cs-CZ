<properties 
    pageTitle="Stáhněte si Azure SDK jazyka Java" 
    description="Zjistěte, jak stáhnout SDK Azure jazyka Java, s ukázkový kód pro Maven projekty a základní kroky pro instalací přidělený Tookit Azure pro Eclipse." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="download-the-azure-sdk-for-java"></a>Stáhněte si Azure SDK jazyka Java

Tento článek obsahuje pokyny ke stažení a instalaci knihoven Azure jazyka Java.

**Poznámka:** V části [licence Apache, verze 2.0]rozloženy knihoven Azure jazyka Java[license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure knihoven jazyka Java – ruční stahování

Ruční instalace knihoven Azure jazyka Java, klikněte na <http://go.microsoft.com/fwlink/?LinkId=690320> pro stažení souboru ZIP, která obsahuje všechny knihoven a všechny závislosti.

Po stažení souboru zip s vaším počítačem extrahování obsahu a použijte jeden z následujících možností do projektu přidat soubory SKLENICE:

* Importujte souborů SKLENICE do jazyka Java projektu v zatmění.

* Konfigurace **Cesta k vytvoření** plánu projektu Java v zatmění uvedení cesty k souboru SKLENICE.

Podrobné informace o nastavení dráhy Tvůrce dotazů v zatmění naleznete v článku [Java cesta k vytvoření] webu zatmění.

**Poznámka:** Podívejte se na license.txt a ThirdPartyNotices.txt soubor uvnitř POŠTOVNÍHO licence a další informace.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure knihoven jazyka Java – vytváření s Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Krok 1: nastavení projektu pro účely Maven Tvůrce dotazů

Vytvoření Maven projekty v zatmění, je použít Azure knihoven pro Java, postupujte podle pokynů v [Začínáte pracovat s knihovny správy Azure jazyka Java] [ maven-getting-started] článek. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Krok 2 – nakonfiguroval nastavení Maven s nutné závislostí

Po konfiguraci projektu pro účely vytvoření Maven, můžete přidat požadované závislosti pom.xml soubor pomocí syntaxe jako v následujícím příkladu. Všimněte si, že není potřeba přidání každé závislosti, která je uvedena v následujícím příkladu; potřebujete jenom přidat konkrétní závislosti, které váš projekt vyžaduje.

> [AZURE.NOTE] V rámci každé `<version>` prvek v následujícím příkladu nahraďte zástupný text "n.n.n" v tomto příkladu platná verze čísla, která lze získat z [Azure knihoven úložiště na Maven].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalace Azure sada nástrojů pro Eclipse

Tato část obsahuje základní pokyny k instalaci sady nástrojů Azure pro Eclipse; podrobné pokyny najdete v tématu [instalace Azure sada nástrojů pro Eclipse].

### <a name="prerequisites"></a>Zjistit předpoklady pro

1. Systémy Windows operting uvedené v článku [Co je nového v Azure sada nástrojů pro Eclipse] .
1. Macintosh nebo Linux operting systémy uvedené v článku [Co je nového v Azure sada nástrojů pro Eclipse] .
1. -Zatmění integrovaném vývojovém prostředí pro vývojáře í Java džínovinu nebo novější. Můžete stáhnout z <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Základní kroky instalace

1. V zatmění vyberte v nabídce **Nápověda** **Instalace nových softwaru**.
1. Zadejte umístění webu <http://dl.microsoft.com/eclipse> a stiskněte **Enter**.
1. Vyberte položky, které chcete nainstalovat a klikněte na tlačítko **Dokončit**.

Azure sada nástrojů pro Eclipse používá nejnovější verzi Azure SDK. Tím se dají stáhnout pomocí webové platformy (WebPI) na <http://go.microsoft.com/fwlink/?LinkID=252838>. Ale pokud nemáte nainstalovaný, při vytváření prvního Azure projekt nasazení, Azure sada nástrojů pro Eclipse nainstaluje automaticky odpovídající verzi Azure SDK.

## <a name="see-also"></a>Viz taky

[Azure sada nástrojů pro Eclipse]

[Instalace Azure sada nástrojů pro Eclipse] 

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure knihoven úložiště na Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Cesta sestavení Java]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Co je nového v Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333
