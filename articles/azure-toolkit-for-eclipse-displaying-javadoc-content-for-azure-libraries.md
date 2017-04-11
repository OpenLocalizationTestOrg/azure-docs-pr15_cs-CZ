<properties
    pageTitle="Zobrazení obsahu Javadoc v zatmění balíčku Azure knihoven jazyka Java"
    description="Jak zobrazit Javadoc obsahu u knihoven Azure v zatmění."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Zobrazení obsahu Javadoc v zatmění balíčku Azure knihoven jazyka Java #

Javadoc obsahu pro knihovny Azure Java můžete zobrazit v prostředí zatmění přidružením Javadoc obsah do knihoven Azure jazyka Java. Podle těchto kroků předvedení použití této funkce v rámci zatmění.

Tento postup předpokládá, že už přidáte knihovnu Azure jazyka Java cestu Tvůrce dotazů.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>K zobrazení obsahu Javadoc v zatmění pro knihovny Azure Java ##

* V rámci jeho zatmění Průzkumník projektu v části **Odkazuje knihoven** projektu, otevřete místní nabídku pro knihovnu Azure pro Java JAR. Například **microsoft windowsazure rozhraní api 0.1.0.jar** (číslo verze může být různé, závisí na verzi máte nainstalované).
* Klikněte na **Vlastnosti**.
* V dialogovém okně **Vlastnosti** v podokně vlevo klikněte na **Umístění Javadoc**. Zobrazí se dialog **Javadoc umístění** .
* Zadání **Adresy URL Javadoc**nebo **Javadoc do archivu v**.
    * Pokud se rozhodnete zadejte adresu **Javadoc URL**, pomocí adresy URL například **http://dl.windowsazure.com/javadoc** nebo **http://dl.windowsazure.com/storage/javadoc**.
    * Pokud se rozhodnete sdělit nám **Javadoc do archivu v**, můžete určit externího souboru nebo soubor pracovního prostoru.
    Rozhodování a procházet/ověřit podle potřeby. Následující příklad přiřadí knihoven Azure jazyka Java odpovídající Javadoc JAR staženou místně do složky s názvem **c:\MyAzureJARs**.
    ![][ic553487]
* *Volitelné*: klikněte na **Ověřit**. Možné problémy s Javadoc JAR může se zobrazit tady.
* Klikněte na **OK**.

Jakmile přidružené knihovny obsah Javadoc by měl zobrazit v rámci vašeho integrovaném vývojovém prostředí zatmění. Například pokud `blob` je definován typu `CloudBlockBlob` v rámci vašeho kódu následuje příklad Javadoc obsahu, který se zobrazí při psaní `blob.acquireLease` v kódu:

![][ic553488]

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

[Instalace Azure sada nástrojů pro Eclipse][] 

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png