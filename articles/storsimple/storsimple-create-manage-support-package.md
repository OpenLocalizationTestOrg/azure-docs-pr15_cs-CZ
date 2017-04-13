<properties
   pageTitle="Vytvoření balíčku podpory StorSimple | Microsoft Azure"
   description="Naučte se vytvořit, dešifrování a upravovat balíčku podporu pro zařízení s StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Vytváření a Správa balíčku StorSimple podpory

## <a name="overview"></a>Základní informace

Podpora balíček StorSimple je mechanismus snadno použitelné, který shromáždí všechny relevantní protokoly pomáhat Microsoft Support k řešení problémů s všechny StorSimple zařízení. Shromážděny protokoly jsou zašifrované a komprimovány.

Tento kurz obsahuje podrobné pokyny k vytváření a správě balíček podpory.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Vytvoření a uložení balíčku podpory Azure klasické portálu

Můžete vytvořit a nahrát balíček podporu na web Microsoft Support prostřednictvím stránky **údržby** služby Azure klasické portálu.

> [AZURE.NOTE] Nahrávání vyžaduje klíč podpory. Podpora technik by měl obsahovat to pro vás v e-mailu.

Podpora zašifrované a mu tuhle zkomprimovanou balíčku (soubor CAB) vytvořené a nahrát na web podpory. Pracovníka podpory poté můžete získat balíček z webu Podpora pro řešení potíží.

Proveďte následující kroky v portálu klasické můžete vytvořit balíček a podpora.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Vytvoření balíčku podpory Azure klasické portálu

1. Vyberte **zařízení** > **údržbu**.

2. V části **Podpora balíčku** vyberte **vytvořit a nahrát balíček podpory**.

3. V dialogovém okně **vytvořit a nahrát balíčku podpory** postupujte takto:

    ![Vytvoření balíčku podpory](./media/storsimple-create-manage-support-package/IC740923.png)

    - Do textového pole **Podpory klíč** zadejte klíč. Pracovníka podpory společnosti Microsoft má odeslat tento klíč pro vás v e-mailu.

    - Zaškrtněte políčko zajistit souhlas automaticky nahrát balíček podporu na web Microsoft Support.

    - Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Ruční vytvoření balíčku podpory

V některých případech bude nutné ručně vytvořit balíček podpory přes Windows PowerShell pro StorSimple. Příklad:

- Pokud potřebujete odebrat citlivých informací ze souboru protokolu před sdílením s Microsoft Support.

- Pokud máte potíže s nahrání balíčku kvůli problémům s připojením k.

Ručně vygenerovaných podpory balíčku můžete sdílet s Microsoft Support prostřednictvím e-mailu. Takto můžete vytvořit balíček a podporu v prostředí Windows PowerShell pro StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Vytvoření balíčku podporu v prostředí Windows PowerShell pro StorSimple

1. Spustit jako správce ve vzdáleném počítači, který se používá pro připojení do zařízení StorSimple relaci prostředí Windows PowerShell, zadejte tento příkaz:

    `Start PowerShell`

2. V relaci prostředí Windows PowerShell připojení konzoly SSAdmin zařízení:

    - Na příkazovém řádku zadejte:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. V dialogovém okně, které se otevře zadejte heslo správce zařízení. Výchozí heslo je:

        `Password1`

        ![Dialogové okno pověření prostředí PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Klikněte na **OK**.
    1. Na příkazovém řádku zadejte:

        `Enter-PSSession $MS`

3. V relaci, která se otevře zadejte příslušný příkaz.

    - Akcie sítě, které jsou chráněné heslem zadejte:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Zobrazí se výzva k zadání hesla, cesty a sdílené síťové složce šifrování heslo (z toho důvodu musí být zašifrovaný balíček podpora). Podpora balíčku se pak vytvoří do zadané složky.

    - Pro sdílené složky, které nejsou chráněný heslem, není nutné `-Credential` parametr. Zadejte následující údaje:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Vytvoření balíčku podpory pro oba řadiče ve sdílené složce určité sítě. Je šifrované, komprimovaná soubor, který můžou posílat Microsoftu Support návody na řešení. Další informace najdete v tématu [Kontaktovat podporu Microsoftu](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Export HcsSupportPackage rutina parametry
Pomocí následujících parametrů s rutinu HcsSupportPackage exportovat.

| Parametr            | Povinné a nepovinné | Popis                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Povinné          | Umožňuje zadat umístění sdílené síťové složce ve kterém je umístěn balíček podpory.                                                                 |
| `-EncryptionPassphrase` | Povinné          | Umožňuje zadat přístupové heslo k šifrování balíček podpory.                                                                                                        |
| `-Credential`           | Volitelné          | Slouží k zadání přihlašovacích údajů aplikace access pro sdílené síťové složce.                                                                                        |
| `-Force`                | Volitelné          | Slouží k šifrování heslo potvrzení krok vynechat.                                                                                                                |
| `-PackageTag`           | Volitelné          | Slouží k určení v adresáři v části *cestu* , ve kterém je umístěn balíček podpory. Výchozí hodnota je [název]-[aktuální datum a time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Volitelné          | Zadejte jako **obrázku** (výchozí) můžete vytvořit balíček a podpora pro oba řadiče. Pokud chcete vytvořit balíček pouze pro aktuální zařízení, určete **řadiče domény**. |


## <a name="edit-a-support-package"></a>Úprava balíčku podpory

Po vygenerování balíčku podpory může být nutné upravit balíček odebrat citlivé informace. To může zahrnovat hlasitost jména, adresy IP zařízení a záložní jména ze souboru protokolu.

> [AZURE.IMPORTANT] Můžete upravovat jenom podpory balíčku, který byl vytvořený prostřednictvím prostředí Windows PowerShell pro StorSimple. Nemůžete upravovat balíčku vytvořeného v portálu Azure klasické službou StorSimple správce.

Úprava balíčku podpory před nahráním na webu Microsoft Support nejdřív dešifrování balíček podpory, upravovat soubory a potom ho znovu zašifrovat. Proveďte následující kroky.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Chcete-li upravit balíčku podporu v prostředí Windows PowerShell pro StorSimple

1. Podpora balíčku generovat způsobem popsaným v tématu [Vytvoření balíčku podporu v prostředí Windows PowerShell pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Stáhněte si skript](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na svému klientovi.

3. Importujte modulu Windows PowerShell. Zadejte cestu do místní složky, ve které jste si stáhli skript. K importu modulu, zadejte:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Všechny soubory jsou *.aes* soubory, které jsou komprimované a šifrovaná. Rozbalte a dešifrování soubory, zadejte:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Všimněte si, že skutečné přípony teď zobrazí pro všechny soubory.

    ![Úprava podpory balíčku](./media/storsimple-create-manage-support-package/IC750706.png)

5. Po zobrazení výzvy k šifrování heslo, zadejte heslo, které jste použili při vytvoření balíčku podpory.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Přejděte do složky obsahující soubor protokolu. Protože jsou teď dekomprimoványt a dešifrovat souborů protokolu, tyto bude mít původní příponu souboru. Úprava těchto souborů, které chcete odebrat všechny informace o zákazníkovi například hlasitost jména a adresy IP zařízení a soubory uložte.

7. Zavřete soubory, které chcete komprimovat s gzip a zašifrovat pomocí AES 256. Jde o rychlosti a zabezpečení s přenosem zpráv balíček podpora v síti. Komprese a šifrování soubory, zadejte tento příkaz:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Úprava podpory balíčku](./media/storsimple-create-manage-support-package/IC750707.png)

8. Po zobrazení výzvy zadejte heslo šifrování balíčku změněné podpory.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Poznamenejte si nové heslo, takže ji můžete sdílet s Microsoft Support při požadavku.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Příklad: Úpravy souborů v balíčku podpory ve sdílené složce chráněné heslem

Následující příklad ukazuje, jak dešifrovat, upravit a znovu zašifrovat balíčku podpory.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Další kroky

- Informace o [použití podpory balíčků a zařízení protokoly řešit problémy s nasazením zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
