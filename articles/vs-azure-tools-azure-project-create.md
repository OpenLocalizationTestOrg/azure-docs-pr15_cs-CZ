<properties
   pageTitle="Vytvoření Azure projektu pomocí aplikace Visual Studio | Microsoft Azure"
   description="Vytvoření Azure projektu pomocí aplikace Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Vytvoření Azure projektu pomocí aplikace Visual Studio

Nástroje Azure for Visual Studio nabízejí šablonu, která umožňuje vytvořit do cloudové služby Azure. V sekci nástroje také umožňují konfigurovat, ladění a nasazení cloudové služby Azure.

Řešení služby Azure cloudu obsahuje následující typy projektů:

- **Azure projektu**

    Přidružení k projektům role má Azure projekt v řešení. Obsahuje taky definice služby a služby konfigurační soubory. Soubor definice služby definuje runtime nastavení pro aplikaci včetně rolích jsou potřeba, koncové body a velikost virtuálního počítače. Konfigurační soubor služby nakonfiguruje kolik instancí role pracují a hodnoty nastavení definované pro roli. Další informace o těchto nastaveních najdete v tématu [jak: Konfigurace role pro službu Azure cloudu pomocí aplikace Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Web role projektu**

    Role pracovníka provede zpracování na pozadí. Role pracovníka komunikovat s služby úložiště a další internetové služby. Pracovní role může mít libovolný počet HTTP, HTTPS nebo TCP koncové body.

    - **Role webového ASP.NET**pro vytváření ASP.NET aplikaci webových front-end
    - **Role webového ASP.NET MVC5**
    - **Role webového ASP.NET MVC4**
    - **Role webového ASP.NET MVC3**
    - **Role webové služby WCF**pro vytváření službě WCF
    - **Silverlight obchodní aplikací Web Role** (vyžaduje Visual Studio 2012)

- **Role pracovníka mezipaměti**

    Role, která obsahuje vyhrazené mezipaměti aplikaci.

- **Role pracovníka s Bus frontě**

    Fronta bus služba, která funguje zprávy fronty komunikovat s kolegy procesem. Další informace najdete v tématu [jak fronty Bus pro použití služby](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Vytvoření projektu služby Azure cloudu ve Visual Studiu

1. Microsoft Visual Studio spusťte jako správce.

1. Na řádku nabídek vyberte **soubor**, **Nový** **projekt**.

1. V podokně **Vlastního typu projektu** zvolte **cloudu** z aplikace project Visual Basic nebo Visual Basic uzly šablon.

1. V podokně **šablony** vyberte **Cloudové služby Azure**.

1. Určete, jakou verzi systému .NET Framework, který chcete použít se dají projektu.

1. Zadejte název a umístění pro váš projekt a název řešení. Klikněte na tlačítko **OK** .

1. V dialogovém okně **Nový projekt Azure** vyberte role, které chcete přidat a zvolte tlačítko se šipkou si je přidat do vašeho řešení. Můžete přidat tolik role podle potřeby.

1. Přejmenování roli, kterou jste přidali do projektu, najeďte myší na roli v dialogovém okně **Nový projekt Azure** a zvolte ikonu **Přejmenovat** napravo od roli. Můžete taky přejmenovat role v rámci vašeho řešení po byly přidány.
