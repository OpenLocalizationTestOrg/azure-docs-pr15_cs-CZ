<properties
    pageTitle="Seznam účet Azure úložiště"
    description="Správa nastavení účtu úložiště pomocí nástrojů Azure pro Eclipse"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Seznam účet Azure úložiště #

Azure úložiště účtů povolit stáhnout umístění pro použití pro JDK, aplikační server a libovolného součásti i pro ukládání stavu, při používání, ukládání do mezipaměti. Zatmění udržuje seznam známých úložiště účty, které jsou k dispozici pro projekty v pracovním prostoru zatmění. Otevřete dialogové okno **Účty úložiště** , který slouží ke správě tento seznam v rámci zatmění, klikněte na tlačítko **okno**, klikněte na **Předvolby**, rozbalte **Azure**a potom klikněte na **Úložiště účty**.

Na následujícím obrázku je dialogové okno **Účty úložiště** .

![][ic719496]

Toto dialogové okno můžete otevřít také propojení **účty** v dialogových oknech, které využívají účty úložiště, třeba takto:

* Karta **JDK** dialogové okno **Konfigurace serveru** .
* Na **serveru** kartu v dialogovém okně **Konfigurace serveru** .
* Dialogové okno **Přidat součásti** .
* Dialogové okno Vlastnosti **ukládání do mezipaměti** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>K importu účtů úložiště pomocí souboru nastavení publikování ##

1. V dialogovém okně **Účty úložiště** klikněte na **Importovat ze souboru nastavení publikování**.
2. (Tento krok přeskočit, pokud jste už uložený soubor nastavení publikování místního počítače.) V dialogovém okně **Informace o předplatném importovat** klikněte na **Stáhnout soubor nastavení publikování**. Pokud si nejste ještě přihlášení k účtu Azure, zobrazí se výzva k přihlášení. Pak budete vyzváni k uložení Azure publikovat soubor nastavení. (Můžete ignorovat výsledné pokyny pro přihlašovací stránky - budou jsou k dispozici Azure portálem a jsou určeny pro uživatele aplikace Visual Studio.) Si ji uložte do místního počítače.
3. Pořád v dialogovém okně **Informace o předplatném importovat** klikněte na tlačítko **Procházet** , vyberte soubor nastavení publikování uložených místně dříve a klepněte na tlačítko **Otevřít**.
4. Klikněte na **OK** zavřete dialogové okno **Informace o předplatném importovat** .

## <a name="to-create-a-new-storage-account"></a>Vytvořte nový účet úložiště ##

1. V dialogovém okně **Účty úložiště** klikněte na **Přidat**.
2. V dialogovém okně **Přidat účet úložiště** klikněte na **Nový**.
3. V dialogovém okně **Nový účet úložiště** zadejte hodnoty pro takto:
    * Název účtu úložiště.
    * Umístění účtu úložiště.
    * Popis účtu úložiště.
    * Předplatné, ke kterému patří účtu úložiště.
4. Klikněte na **OK** zavřete dialogové okno **Nový účet úložiště** .

Může trvat několik minut účtu úložiště vytvořit. Po vytvoření otevřela, klikněte na **OK** zavřete dialogové okno **Přidat účet úložiště** a vašemu novému účtu úložiště se přidají do seznamu volného účty.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Přidání existujícího účtu úložiště do seznamu ##

1. Pokud už nemáte účet Azure úložiště, vytvořte pomocí následujících kroků uvedených v části **Vytvoření nového oddílu účtu úložiště** nad. (Případně můžete vytvořit nový účet úložiště na [Portálu Správa Azure][].)
2. V dialogovém okně **Účty úložiště** klikněte na **Přidat**.
3. V dialogovém okně **Přidat účet úložiště** zadejte hodnoty **název** a **Přístupové klávesy**. Pro existující účet Azure úložiště musí být klíč účtu název a přístup. V části **úložiště** na [Portálu Správa Azure][] slouží k zobrazení názvy účtů úložiště a klíče. Dialogové okno **Přidat účet úložiště** bude vypadat podobně jako tento.

    ![][ic719497]

4. Klikněte na **OK** zavřete dialogové okno **Přidat účet úložiště** .

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Chcete-li změnit úložiště účtu použít nové přístupová klávesa ##

1. V dialogovém okně **Účty úložiště** klikněte na úložiště účet, který chcete upravit a potom klikněte na **Upravit**.
2. V dialogovém okně **Upravit úložiště účtu přístupová klávesa** upravte hodnotu **Přístupové klávesy** .
3. Klikněte na **OK** zavřete dialogové okno **Upravit úložiště účtu přístupové klávesy** .

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Chcete-li odebrat účet úložiště ze seznamu v zatmění zachovaná ##

1. V dialogovém okně **Účty úložiště** klikněte na úložiště účet, který chcete upravit a potom klikněte na **Odebrat**.
2. Klikněte na **OK** po zobrazení výzvy k odebrání účtu úložiště.

>[AZURE.NOTE] Odebrání účtu úložiště prostřednictvím dialogu **Úložiště účty** pouze ji odebere ze seznamu zobrazit v rámci zatmění úložiště účtů. Neodebere účtu úložiště u předplatného Azure. Navíc úložiště účtu může zobrazovat ve vašem seznamu po zatmění znovu načte Podrobnosti předplatného.

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Instalace Azure sada nástrojů pro Eclipse][] 

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Portál Správa Azure]: http://go.microsoft.com/fwlink/?LinkID=512959
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
