<properties
    pageTitle="Instalace Azure sada nástrojů pro Eclipse | Microsoft Azure"
    description="Zjistěte, jak nainstalovat Azure sada nástrojů pro Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalace Azure sada nástrojů pro Eclipse

Tato sada Azure pro Eclipse obsahuje šablony a funkce, které umožňují snadno vytvářet, vývoj, otestujte a nasazovat Azure aplikace pomocí zatmění vývojové prostředí. Azure sada nástrojů pro Eclipse je projekt otevřít zdroj, jehož zdrojový kód je k dispozici v části licence MIT projektovém webu na GitHub na následující adresu URL:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Podle těchto kroků ukazují, jak nainstalovat sadu Azure pro Eclipse.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Chcete-li nainstalovat Azure sada nástrojů pro Eclipse

1. Spusťte zatmění.

1. Až se otevře zatmění, klikněte na nabídku **Nápověda** a potom klikněte na **Instalace nových softwaru**, jak je znázorněno na následujícím obrázku.

    ![Instalace Azure sada nástrojů pro Eclipse][01]

1. V dialogovém okně **Dostupné Software** v textovém poli **práce s** zadejte **http://dl.microsoft.com/eclipse** následovaný klávesu **Enter** .

1. V podokně **název** zkontrolujte **Azure sada nástrojů pro Eclipse**a zrušte zaškrtnutí políčka **kontakt aktualizovat všechny weby během instalace k vyhledání požadovaného softwaru**. Na obrazovce by měla vypadat podobně jako tento:

    ![Instalace Azure sada nástrojů pro Eclipse][02]

1. Pokud rozdělíte **Azure sada nástrojů pro Eclipse**, zobrazí se následující položky:

    * **Modul plug-in přehledy aplikace jazyka Java**: Tato součást umožňuje společnosti Azure telemetrie protokolování a analýzy služby pro aplikace a instance serveru.
    * **Azure přístup ovládací prvek služby filtr**: Tato součást podporuje ověřování uživatelů aplikace s Azure ACS, podpora scénářů jednotného přihlašování a externalizing identity logiky z aplikace.
    * **Běžné modul plug-in Azure**: Tato součást poskytuje funkci běžné potřeby dalšími součástmi sady nástrojů.
    * **Azure Explorer pro Eclipse**: Tato součást poskytuje funkci běžné potřeby dalšími součástmi sady nástrojů.
    * **Modul plug-in Azure pro Eclipse s Java**: Tato součást podporuje vývoje projektech, které pomáhají vytvářet, otestujte a nasazení aplikace Java pro Microsoft Azure cloudu v zatmění a pomocí příkazového řádku.
    * **Modul plug-in Web Apps Azure pomocí jazyka Java**: Tato součást podporuje nasazení Java webových aplikací pro kontejnery Microsoft Azure Web App.
    * **Microsoft JDBC ovladač 4.2 pro systém SQL Server**: Tato součást poskytuje JDBC rozhraní API pro SQL Server a databázi Microsoft Azure SQL pro Java platformy Enterprise Edition 8.
    * **Balíček pro Apache Qpid klienta knihovny JMS**: Tato součást poskytuje JMS komponentu z projectu Apache Qpid povolit aplikaci pro použití AMQP zasílání zpráv v aplikaci Microsoft Azure.
    * **Balíček pro knihovny Microsoft Azure Java**: Tato součást poskytuje rozhraní API pro přístup ke službám Microsoft Azure, například úložiště služby bus, runtime služby, atd.

1. Klikněte na tlačítko **Další**. (Pokud se setkáte neobvyklé zpoždění při instalaci této sady, zajistěte, aby byl **kontakt aktualizovat všechny weby během instalace zobrazíte software požadovaný** zrušené zaškrtnutí políčka.)

1. V dialogovém okně **Podrobnosti o instalaci** klikněte na **Další**.

    ![Podrobné informace o instalaci revize][03]

1. V dialogovém okně **Zkontrolovat licence** přečtěte si podmínky licenční smlouvy. Pokud přijměte podmínky licenční smlouvy klepněte na tlačítko **přijmout podmínky licenční smlouvy** a potom klikněte na **Dokončit**. (Zbývajících kroků použijeme že přijměte podmínky licenční smlouvy. Pokud není přijměte podmínky licenční smlouvy, k ukončení procesu instalace.)

    ![Kontrola licence][04]

    Zatmění stáhnout a nainstalovat potřebné balíčků.

    ![Průběh instalace][05]

1. Pokud se zobrazí výzva k restartování zatmění dokončete instalaci, klikněte na **Ano**.

    ![Znovu spustit dotaz][06]

## <a name="see-also"></a>Viz taky

Další informace o sadách Azure pro Java IDEs najdete v následujících tématech:

- [Azure sada nástrojů pro Eclipse]
  - *Instalace Azure sada nástrojů pro Eclipse (Tento článek)*
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]
  - [Co je nového v Azure sada nástrojů pro Eclipse]
- [Azure sada nástrojů pro IntelliJ]
  - [Instalace Azure sada nástrojů pro IntelliJ]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]
  - [Co je nového v Azure sada nástrojů pro IntelliJ]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

<!-- URL List -->

[Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Co je nového v Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

