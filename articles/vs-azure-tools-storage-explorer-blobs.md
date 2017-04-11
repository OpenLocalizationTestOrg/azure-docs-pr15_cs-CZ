<properties
    pageTitle="Přidávání a používání zdrojů úložišti objektů Blob Azure pomocí Průzkumníka úložišť (verze Preview) | Microsoft Azure"
    description="Správa objektů Blob Azure kontejnery a objekty BLOB v Průzkumníkovi úložiště (verze Preview)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Přidávání a používání zdrojů úložišti objektů Blob Azure pomocí Průzkumníka úložišť (verze Preview)

## <a name="overview"></a>Základní informace

[Úložiště objektů Blob Azure](./storage/storage-dotnet-how-to-use-blobs.md) je služba pro ukládání velkých objemů Nestrukturovaná data, například text nebo binární data, která je přístupný z kdekoli na světě prostřednictvím protokolu HTTP nebo HTTPS.
Úložiště objektů Blob můžete použít, jak vystavit data veřejně na světě nebo k ukládání dat aplikace soukromě. V tomto článku se dozvíte, jak používat úložiště Explorer (verze Preview) pro práci s objektů blob kontejnery a objekty BLOB.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Kroky v tomto článku, musíte mít takto:

- [Stažení a instalace úložiště Explorer (verze preview)](http://www.storageexplorer.com)
- [Připojení k Azure úložiště účet nebo službu](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Vytvoření kontejneru objektů blob

Všechny objekty BLOB musí nacházet v kontejneru objektů blob, což je jednoduše logické seskupení objektů BLOB. Účet může obsahovat neomezený počet kontejnery a každý kontejneru mohou být uloženy neomezený počet objektů BLOB.

Následující kroky popisují, jak vytvořit kontejner objektů blob v úložišti Explorer (verze Preview).

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte účtu úložiště, ve kterém chcete vytvořit kontejner objektů blob.
1.  Klikněte pravým tlačítkem myši **Kontejnery objektů Blob**a - z místní nabídky – vyberte **Vytvořit kontejner objektů Blob**.

    ![Vytvoření objektů blob kontejnery místní nabídky][0]

1.  Textové pole se zobrazí pod složkou **Objektů Blob kontejnery** . Zadejte název pro váš objektů blob kontejner. Naleznete v části [pravidla pojmenování kontejneru](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) seznam pravidel a omezení na názvy objektů blob kontejnery.

    ![Vytvořte textové pole objektů Blob kontejnery][1]

1.  Stisknutím klávesy **Enter** po dokončení vytvořit kontejner objektů blob nebo klávesy **Esc** . Po úspěšném vytvoření kontejneru objektů blob zobrazí se ve složce **Objektů Blob kontejnery** účtu vybrané úložiště.

    ![Vytvoření kontejneru objektů BLOB][2]

## <a name="view-a-blob-containers-contents"></a>Zobrazení obsahu objektů blob kontejneru

Kontejnery objektů BLOB obsahovat objekty BLOB a složek (, které mohou také obsahovat objekty BLOB).

Podle těchto kroků ukazují, jak zobrazit obsah kontejneru objektů blob v rámci úložiště Explorer (verze Preview):

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte úložiště účet obsahující kontejneru objektů blob, že chcete zobrazit.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Klikněte pravým tlačítkem myši kontejneru objektů blob, kterou chcete zobrazit a - z místní nabídky - otevřených objektů Blob kontejneru možnost **Redaktor**zvolte.
Poklikejte na položku kontejneru objektů blob, kterou chcete zobrazit.

    ![Místní nabídky editoru kontejneru otevřených objektů blob][19]

1.  Hlavní podokno se zobrazí objektů blob kontejneru obsah.

    ![Editor kontejneru objektů BLOB][3]

## <a name="delete-a-blob-container"></a>Odstranění objektů blob kontejneru

Kontejnery objektů BLOB můžete snadno vytvořili a Odstraněná podle potřeby. (Podívejte se, jak odstranit jednotlivé objekty BLOB, najdete pod odkazy v části [Správa objektů BLOB v kontejneru objektů blob](./#managing-blobs-in-a-blob-container).)

Podle těchto kroků ukazují, jak odstranit kontejneru objektů blob v rámci úložiště Explorer (verze Preview):

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte úložiště účet obsahující kontejneru objektů blob, že chcete zobrazit.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Klikněte pravým tlačítkem kontejneru objektů blob, kterou chcete odstranit a - z místní nabídky – vyberte **Odstranit**.
Můžete také stisknout **Odstranit** chcete odstranit aktuálně vybraných objektů blob kontejner.

    ![Odstranění objektů blob kontejneru místní nabídky][4]

1.  Potvrzovacím dialogovém okně vyberte **Ano** .

    ![Odstranění objektů blob potvrzení kontejneru][5]

## <a name="copy-a-blob-container"></a>Kopírování objektů blob kontejneru

Úložiště Explorer (verze Preview) umožňuje kontejneru objektů blob zkopírování do schránky a vložte do jiného účtu úložiště objektů blob kontejneru. (Podívejte se, jak zkopírovat jednotlivé objekty BLOB, najdete pod odkazy v části [Správa objektů BLOB v kontejneru objektů blob](./#managing-blobs-in-a-blob-container).)

Následující kroky popisují, jak zkopírovat kontejneru objektů blob z jednoho úložiště účtu.

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte úložiště účet obsahující kontejneru objektů blob, že chcete kopírovat.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Klikněte pravým tlačítkem kontejneru objektů blob, které chcete kopírovat a - z místní nabídky – vyberte **Kopírovat objektů Blob kontejner**.

    ![Kopírování objektů blob kontejneru místní nabídky][6]

1.  Klikněte pravým tlačítkem na požadovaný "cílový" úložiště účet, do kterého chcete vložit kontejneru objektů blob a - z místní nabídky – vyberte **Vložte kontejner objektů Blob**.

    ![Vložení objektů blob kontejneru místní nabídka][7]

## <a name="get-the-sas-for-a-blob-container"></a>Získat přidružení zabezpečení pro kontejneru objektů blob

[Sdílený přístup k podpisu (přidružení zabezpečení)](./storage/storage-dotnet-shared-access-signature-part-1.md) obsahuje delegované přístupu k prostředkům ve vašem účtu úložiště.
To znamená, že udělit klienta omezené oprávnění k objektům ve vašem účtu úložiště stanovený počet minut a zadaný sadu oprávnění, aniž by bylo nutné sdílet svůj účet přístupové klávesy.

Podle těchto kroků ukazují, jak vytvořit přidružení kontejneru objektů blob:

1.  Otevřete Průzkumníka úložišť (verze Preview).
1.  V levém podokně rozbalte účtu úložiště objektů blob kontejneru, u kterého chcete dostat přidružení zabezpečení obsahující.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Klikněte pravým tlačítkem kontejneru požadované objektů blob a - z místní nabídky – vyberte **Získat sdílené přístup podpis**.

    ![Získání přidružení zabezpečení místní nabídky][8]

1.  V dialogovém okně **Sdílené podpis přístupu** zadejte zásady, datum zahájení a vypršení platnosti, časové pásmo a přístup k úrovně, které chcete použít pro zdroje.

    ![Zobrazení možností přidružení zabezpečení][9]

1.  Po dokončení nastavení možností přidružení zabezpečení, vyberte možnost **vytvořit**.

1.  Druhý dialogové okno **Sdílet podpisu přístup** se zobrazí, který obsahuje kontejneru objektů blob spolu s adresy URL a QueryStrings můžete použít pro přístup k prostředku úložiště.
Zvolte **Kopírovat** vedle adresy URL, kterou chcete zkopírovat do schránky.

    ![Kopírování adresy URL přidružení zabezpečení][10]

1.  Po dokončení vyberte **Zavřít**.

## <a name="manage-access-policies-for-a-blob-container"></a>Správa zásady přístupu pro kontejneru objektů blob

Následující kroky popisují, jak spravovat (přidání a odebrání) přístup k zásad pro objektů blob kontejner:

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte účet úložiště obsahující kontejneru objektů blob jehož zásady přístupu chcete spravovat.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Vyberte požadovanou objektů blob kontejner a - z místní nabídky – vyberte **Správa zásady přístupu**.

    ![Správa přístupu zásady místní nabídky][11]

1.  Dialogové okno **Zásady přístupu** zobrazí seznam všech zásady přístupu pro kontejneru vybraných objektů blob již vytvořili.

    ![Možnosti aplikace Access zásad][12]        

1.  Postupujte podle toho, úlohy správy zásady přístupu:

    - **Přidat nové zásady přístupu** – vyberte **Přidat**. Po vytvoření se zobrazí dialogové okno **Zásady přístupu** nově přidaný přístupu (ve výchozím nastavení).
    - **Úprava zásady přístupu** – proveďte požadované úpravy a vyberte **Uložit**.
    - **Odebrání zásad access** – vyberte **Odebrat** vedle zásady přístupu, kterou chcete odebrat.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Nastavit úroveň přístupu veřejnosti pro kontejneru objektů blob

Ve výchozím nastavení všech objektů blob kontejneru nastavenou na hodnotu "Bez přístupu veřejné".

Podle těchto kroků ukazují, jak určit úroveň přístupu veřejnosti kontejneru objektů blob.

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte účet úložiště obsahující kontejneru objektů blob jehož zásady přístupu chcete spravovat.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Vyberte požadovanou objektů blob kontejner a - z místní nabídky – vyberte **Nastavit úroveň přístupu veřejnosti**.

    ![Nastavení přístupu veřejnosti úrovně místní nabídky][13]

1.  V dialogovém okně **Nastavit úroveň přístupu veřejnosti kontejneru** zadejte úroveň požadovaný přístup.

    ![Nastavení možností přístupu veřejnosti úrovně][14]

1.  Klikněte na možnost **použít**.

## <a name="managing-blobs-in-a-blob-container"></a>Správa objektů BLOB v kontejneru objektů blob

Po vytvoření kontejneru objektů blob nahrát objektů blob, které obsahují objektů blob, si ho stáhnout objektů blob do místního počítače, můžete otevřít objektů blob na vašem počítači a různých dalších věcech.

Následující kroky popisují Správa objektů BLOB (a složky) v kontejneru objektů blob.

1.  Otevřete Průzkumníka úložiště (verze Preview).
1.  V levém podokně rozbalte úložiště účet obsahující kontejneru objektů blob, že chcete spravovat.
1.  Rozbalte účtu úložiště **Objektů Blob kontejnery**.
1.  Poklikejte na kontejner objektů blob, který chcete zobrazit.
1.  Hlavní podokno se zobrazí objektů blob kontejneru obsah.

    ![Zobrazení objektů blob kontejneru][3]

1.  Hlavní podokno se zobrazí objektů blob kontejneru obsah.

1.  Postupujte podle kroků v závislosti na úkol, který chcete provést:

    - **Nahrajte soubory do kontejneru objektů blob**

        1.  Na hlavním panelu nástrojů vyberte **Uložit**a pak **Nahrajte soubory** z rozevírací nabídky.

            ![Soubory nabídka odeslat][15]

        1.  V dialogovém okně **Nahrát soubory** vyberte tlačítko se třemi tečkami (****...) na pravé straně textového pole **souborů** vyberte soubory, které chcete nahrát.

            ![Nahrání souborů možnosti][16]

        1.  Určete typ **objektů Blob**. V článku [Začínáme s úložiště objektů Blob Azure pomocí .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) vysvětluje rozdíly mezi různé typy objektů blob.

        1.  Pokud chcete zadejte cílovou složku, do které vybrané soubory nahrané. Pokud je cílová složka neexistuje, bude vytvořena.

        1.  Vyberte **Odeslat**.

    - **Odeslání do složky do kontejneru objektů blob**

        1.  Na hlavním panelu nástrojů vyberte **Nahrát**a pak **Nahrajte složka** z rozevírací nabídky.

            ![Nahrání nabídka pro složky][17]

        1.  V dialogovém okně **Odeslat složku** vyberte tlačítko se třemi tečkami (****...) na pravé straně textového pole **složku** vyberte složku, jejíž obsah chcete nahrát.

            ![Nahrání Možnosti složky][18]

        1.  Určete typ **objektů Blob**. V článku [Začínáme s úložiště objektů Blob Azure pomocí .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) vysvětluje rozdíly mezi různé typy objektů blob.

        1.  Pokud chcete zadejte cílovou složku, do které obsah do vybrané složky nahrané. Pokud je cílová složka neexistuje, bude vytvořena.

        1.  Vyberte **Odeslat**.

    - **Stáhněte si objektů blob do místního počítače**

        1.  Výběr objektů blob, které chcete stáhnout.

        1.  Na hlavním panelu nástrojů zvolte **Stáhnout**.

        1.  V dialogovém okně **určení, kam chcete uložit stažený objektů blob** zadejte místo, kam chcete objektů blob stáhnout a název, který chcete, můžete si ji pojmenovat.  

        1.  Vyberte **Uložit**.

    - **Otevření objektů blob ve vašem počítači**

        1.  Výběr objektů blob, který chcete otevřít.

        1.  Na hlavním panelu nástrojů vyberte **Otevřít**.

        1.  Objektů blob bude stahovat a otevřít pomocí aplikace přidružená objektů blob základní typ souboru.

    - **Zkopírujte objektů blob do schránky**

        1.  Výběr objektů blob, které chcete kopírovat.

        1.  Na hlavním panelu nástrojů zvolte **Kopírovat**.

        1.  V levém podokně přejděte na jiný kontejner objektů blob a poklikáním ji zobrazíte v podokně hlavní.

        1.  Na hlavním panelu nástrojů vyberte **Vložit** a vytvořte kopii objektů blob.

    - **Odstranění objektů blob**

        1.  Výběr objektů blob, kterou chcete odstranit.

        1.  Na hlavním panelu nástrojů vyberte **Odstranit**.

        1.  Potvrzovacím dialogovém okně vyberte **Ano** .

## <a name="next-steps"></a>Další kroky

- Zobrazení [nejnovějších úložiště Explorer (verze Preview) poznámky k verzi a videa](http://www.storageexplorer.com).
- Zjistěte, jak [vytvořit aplikací pomocí objektů BLOB Azure, tabulek, dotazů a soubory](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png