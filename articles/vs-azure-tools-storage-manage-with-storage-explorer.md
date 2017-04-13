<properties
    pageTitle="Začínáme s aplikací úložiště Explorer (verze Preview) | Microsoft Azure"
    description="Přidávání a používání zdrojů Azure úložiště v Průzkumníkovi úložiště (verze Preview)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Začínáme s aplikací úložiště Explorer (verze Preview)

## <a name="overview"></a>Základní informace 

Microsoft Azure úložiště Explorer (verze Preview) je samostatné aplikace, který umožňuje snadno pracovat s daty Azure úložiště v systému Windows, OS X a Linux. V tomto článku se dozvíte různé způsoby připojení k a správě účtů Azure úložiště.

![Microsoft Azure úložiště Explorer (verze Preview)][15]

## <a name="prerequisites"></a>Zjistit předpoklady pro

- [Stažení a instalace úložiště Explorer (verze preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Připojení k úložišti účet nebo službu

Úložiště Explorer (verze Preview) lze používání velkého počtu způsoby připojení k účtům úložiště. Jedná se o připojení k úložišti účty přidružené Azure předplatných, připojení k účtům úložiště a služby sdílené z jiných předplatných Azure a dokonce i připojení k a řízení schůzky místní úložiště pomocí emulátoru úložiště Azure:

- [Připojení k předplatnému Azure](#connect-to-an-azure-subscription) – Správa úložiště zdroje, které patří k předplatnému Azure.
- [Práce s úložištěm místní rozvoj](#work-with-local-development-storage) - Správa místní úložiště pomocí emulátoru úložiště Azure. 
- [Připojení k externím úložišti](#attach-or-detach-an-external-storage-account) – přidávání a používání zdrojů úložiště patřící do jiného Azure předplatného pomocí účtu úložiště název účtu a klíče.
- [Úložiště účtu připojit pomocí přidružení zabezpečení](#attach-storage-account-using-sas) – přidávání a používání zdrojů úložiště patřící do jiného Azure předplatného pomocí přidružení zabezpečení.
- [Služba připojení pomocí přidružení zabezpečení](#attach-service-using-sas) – Správa konkrétní úložiště služby (objektů blob kontejneru, fronty nebo tabulky) patřící do jiného Azure předplatného pomocí přidružení zabezpečení.

## <a name="connect-to-an-azure-subscription"></a>Připojení k předplatné Azure

> [AZURE.NOTE] Pokud nemáte účet Azure, můžete [Registrace bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo [Aktivujte své výhody odběratele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. V Průzkumníku úložiště (verze Preview) zvolte **Nastavení účtu Azure**. 

    ![Nastavení účtu Azure][0]

1. V levém podokně se zobrazí všechny účty Microsoft, který jste přihlášeni. Připojení k jinému účtu, klikněte na **Přidat účet**a sledování dialogových oken Přihlaste se pomocí účtu Microsoft, který je spojený s alespoň jeden aktivní předplatné Azure.

    ![Přidání účtu][1]

1. Po úspěšném přihlášení pomocí účtu Microsoft, bude v levém podokně naplní Azure předplatná přidruženým k tomuto účtu. Vyberte Azure předplatná, se kterými chcete pracovat a pak vyberte **použít**. (Výběr **Všechna předplatná** přepíná vyberete všechny nebo žádný z uvedených Azure předplatných.)

    ![Vyberte Azure předplatná][3]

1. V levém podokně se zobrazí účty úložiště přidružené k vybrané Azure předplatná.

    ![Vybrané Azure předplatná][4]

## <a name="work-with-local-development-storage"></a>Práce s místní rozvoj úložiště

Úložiště Explorer (verze Preview) vám dává možnost pracovat s místní úložiště pomocí emulátoru úložiště Azure. Umožňuje kódu proti a testování úložiště bez nutně bez účtu úložiště nasazený na Azure (protože účtu úložiště je právě napodobit úložiště emulátorem Azure).

>[AZURE.NOTE] Úložiště emulátoru Azure je podporována pouze pro Windows. 

1. V levém podokně úložiště Explorer (verze Preview) rozbalte **(místní a připojit** > **Úložiště účty** > uzel**(vývoj)** .

    ![Místní rozvoj uzel][21]

1. Pokud jste ještě nenainstalovali emulátoru úložiště Azure, budete vyzváni k tomu nevyzve přes informační řádek. Pokud se zobrazí na informačním panelu, vyberte **Stáhnout nejnovější verzi**a nainstalujte emulátor. 

    ![Stáhněte si Azure úložiště emulátoru dotaz][22]

1. Po instalaci používat emulátor, budete mít možnost vytváření a práci s objekty BLOB místní, fronty a tabulky. Informace o práci s každý typ účtu úložiště, vyberte na příslušný odkaz:

    - [Přidávání a používání zdrojů úložiště objektů blob Azure](./vs-azure-tools-storage-explorer-blobs.md)
    - Přidávání a používání Azure soubor sdílet úložiště zdrojů – *už brzo*
    - Přidávání a používání zdrojů úložiště Azure fronty – *už brzo*
    - Přidávání a používání zdrojů úložiště tabulek Azure – *už brzo*

## <a name="attach-or-detach-an-external-storage-account"></a>Připojení nebo odpojení účtu externí úložiště

Úložiště Explorer (verze Preview) umožňuje připojit k účtům externí úložiště tak, aby úložiště účtům snadno se sdílí. Tato část popisuje, jak připojit k (a odpojit od) externí úložiště účty.

### <a name="get-the-storage-account-credentials"></a>Získání přihlašovací údaje účtu úložiště

Aby bylo možné sdílet účet externí úložiště, musí vlastník tohoto účtu nejprve získat pověření – název účtu a klíč – pro účet a sdílet informace s lidmi chtějí připojit k tomuto účtu (externím). Získání přihlašovací údaje účtu úložiště lze provést pomocí portálu Azure pomocí následujících kroků: 

1.  Přihlaste se k [portálu Azure](https://portal.azure.com).
1.  Vyberte **Procházet**.
1.  Vyberte **účty úložiště**.
1.  V zásuvné **Úložiště účty** vyberte požadované úložiště účet.
1.  V **Nastavení** zásuvné pro vybrané úložiště účet vyberte **přístupových kláves z verze**.

    ![Možnost přístupu klíče][5]
    
1.  V zásuvné **přístupových kláves z verze** zkopírujte hodnoty **Název účtu úložiště** a **klíč 1** pro použití při připojení k účtu úložiště. 

    ![Přístupových kláves][6]

### <a name="attach-to-an-external-storage-account"></a>Připojení k účtu externí úložiště
Pokud chcete připojit k účtu externí úložiště, musíte mít název účtu a klíče. V části *získat přihlašovací údaje účtu úložiště* vysvětluje, jak získat tyto hodnoty z portálu Microsoft Azure. Však Všimněte si, že na portálu klíč účtu se jmenuje "základní 1" takže pokud úložiště Explorer (verze Preview) požaduje klíč účtu, budete zadání (nebo vložení) hodnotu "základní 1". 
 
1.  V Průzkumníku úložiště (verze Preview) vyberte **připojit k základnímu úložišti Azure**.

    ![Připojení k možnost Azure úložiště][23]

1.  V dialogovém okně **připojení k úložišti Azure** zadejte klíč účtu ("základní 1" hodnotu z portálu Microsoft Azure) a potom vyberte **Další**.

    ![Připojení k dialogu Azure úložiště][24] 

1.  V dialogovém okně **Připojení externích úložiště** zadejte název účtu úložiště do pole **název účtu** , zadejte další požadovaná nastavení a vyberte **Další** po dokončení. 

    ![Připojení externího úložiště dialogového okna][8]

1.  V dialogovém okně **Souhrn připojení** ověřte informace. Pokud chcete provádět žádné změny, vyberte položku **zpět** a znovu zadejte požadované nastavení. Až budete hotovi, klikněte na **Připojit**.

1.  Po připojení, externí úložiště účtu se zobrazí s textem **(externím)** přidaným k název účtu úložiště. 

    ![Výsledek připojení k účtu externí úložiště][9]

### <a name="detach-from-an-external-storage-account"></a>Odpojení s klientem externí úložiště

1.  Klikněte pravým tlačítkem myši na externí úložiště účet, který chcete odpojit a - z místní nabídky – vyberte **Odpojit**.

    ![Odpojení od alternativy pro ukládání][10]

1.  Po zobrazení zprávy potvrzovacím okně vyberte **Ano** potvrďte uvolnění z účtu externí úložiště.

## <a name="attach-storage-account-using-sas"></a>Připojení účtu úložiště pomocí přidružení zabezpečení

[Přidružení zabezpečení (sdílené přístup podpis)](storage/storage-dotnet-shared-access-signature-part-1.md) vám bude radit správce předplatné Azure možnost udělit přístup k účtu úložiště dočasně aniž byste museli zadejte své přihlašovací údaje Azure předplatného. 

Pro znázornění je Řekněme, že Uživatel_a je správce Azure předplatného a Uživatel_a chce umožnit Uživateli_b pro přístup k účtu úložiště po omezenou dobu s určitá oprávnění:

1. Uživatel_a generuje přidružení zabezpečení (tvořené připojovací řetězec účtu úložiště) pro určitého časového období a s požadovaná oprávnění.
1. Uživatel_a sdílí s uživatelem chcete přístup k úložiště účtu - Uživateli_b, v našem příkladu přidružení zabezpečení.  
1. Uživateli_b využití úložiště Explorer (verze Preview) pro připojení k účtu patřící do Uživatel_a pomocí předaném přidružení zabezpečení. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Získání přidružení zabezpečení pro účet, který chcete sdílet

1.  V Průzkumníku úložiště (verze Preview) klikněte pravým tlačítkem myši na úložiště účet, který chcete sdílet a - z místní nabídky – vyberte **Získat sdílené přístup podpis**.

    ![Zobrazí možnost přidružení zabezpečení místní nabídky][13]

1. V dialogovém okně **Sdílené podpis Access** zadejte časový rámec a oprávnění, která chcete pro účet a vyberte **vytvořit**.

    ![Zobrazí dialogové okno přidružení zabezpečení][14]
 
1. Druhá dialogové okno **Sdílet podpisu aplikace Access** se zobrazí zobrazující přidružení zabezpečení. Zvolte **Kopírovat** vedle **Připojovací řetězec** ji zkopírujte do schránky. Vyberte možnost **Zavřít** zavřete dialogové okno.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Připojení ke sdílené účtu pomocí přidružení zabezpečení

1.  V Průzkumníku úložiště (verze Preview) vyberte **připojit k základnímu úložišti Azure**.

    ![Připojení k možnost Azure úložiště][23]

1.  V dialogovém okně **připojení k úložišti Azure** zadat připojovací řetězec a pak vyberte **Další**.

    ![Připojení k dialogu Azure úložiště][24] 

1.  V dialogovém okně **Souhrn připojení** ověřte informace. Pokud chcete provádět žádné změny, vyberte položku **zpět** a znovu zadejte požadované nastavení. Až budete hotovi, klikněte na **Připojit**.

1.  Po připojení, zobrazí se účtu úložiště s textem (přidružení zabezpečení) připojen k názvu účtu, který jste zadali.

    ![Výsledek připojené k účtu pomocí přidružení zabezpečení][17]

## <a name="attach-service-using-sas"></a>Připojení služby pomocí přidružení zabezpečení

V části [úložiště účtu připojit pomocí přidružení zabezpečení](#attach-storage-account-using-sas) ukazuje, jak správce Azure předplatné udělit dočasný přístup k účtu úložiště generování (a sdílení) přidružení účtu úložiště. Podobně přidružení zabezpečení můžete generovat pro konkrétní službu (objektů blob kontejneru, fronty nebo tabulky) v rámci účtu úložiště.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Vytvoření přidružení zabezpečení pro službu, kterou chcete sdílet

V tomto kontextu lze do služby objektů blob kontejneru, fronty nebo tabulku. Následující části popisují, jak vygenerovat přidružení uvedení služby:

- [Získat přidružení zabezpečení pro kontejneru objektů blob](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Získat přidružení zabezpečení pro sdílené – *už brzo*
- Získání přidružení zabezpečení fronty – *už brzo*
- Získat přidružení zabezpečení pro tabulky – *už brzo*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Připojení ke službě sdíleného účtu pomocí přidružení zabezpečení

1.  V Průzkumníku úložiště (verze Preview) vyberte **připojit k základnímu úložišti Azure**.

    ![Připojení k možnost Azure úložiště][23]

1.  V dialogovém okně **připojení k úložišti Azure** zadejte identifikátor URI přidružení zabezpečení a potom vyberte **Další**.

    ![Připojení k dialogu Azure úložiště][24] 

1.  V dialogovém okně **Souhrn připojení** ověřte informace. Pokud chcete provádět žádné změny, vyberte položku **zpět** a znovu zadejte požadované nastavení. Až budete hotovi, klikněte na **Připojit**.

1.  Po připojení, zobrazí se nově připojené služby uzlu **(Služby přidružení zabezpečení)** .

    ![Výsledek spojené s sdílených služeb pomocí přidružení zabezpečení][20]

## <a name="search-for-storage-accounts"></a>Vyhledejte úložiště účty

Pokud máte dlouhý seznam účtů úložiště, je rychlý způsob, jak najít konkrétní úložiště účet pomocí pole Hledat v horní části levého podokna. 

Při psaní do vyhledávacího pole, se zobrazí v levém podokně jen úložiště účty, které odpovídají hledanou hodnotu, kterou jste zadali až směřující. Následující snímek obrazovky znázorňuje příklad, kde můžu jste hledali všechny účty úložiště, kde název účtu úložiště s textem "tarcher".

![Hledání účtu úložiště][11]
    
Vyhledávání zrušit, klikněte na tlačítko **x** do vyhledávacího pole.

## <a name="next-steps"></a>Další kroky
- [Přidávání a používání zdrojů úložiště objektů blob Azure pomocí Průzkumníka úložišť (verze Preview)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
