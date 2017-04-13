<properties
   pageTitle="Spusťte aplikaci systému Windows na libovolném zařízení s Azure RemoteApp | Microsoft Azure"
   description="Naučte se sdílet jakékoli aplikace pro Windows s jinými uživateli pomocí Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Spusťte aplikaci systému Windows na libovolném zařízení s Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Spuštěním aplikace pro systém Windows kdekoli na libovolném zařízení, právě teď vážně - jenom pomocí Azure RemoteApp. Ať už jde vlastní napsaného 10 lety nebo aplikaci Office, uživatelé již na určitý operační systém (jako je Windows XP) pro tyto několik aplikace.

S Azure RemoteApp vaši uživatelé můžete použít vlastní zařízeních s Androidem nebo Apple a získat stejné prostředí, které budou máte v systému Windows (nebo na telefony Windows Phone). To je provést hostingu aplikace Windows v kolekci virtuálních počítačích Windows na Azure - vaši uživatelé můžete k nim měli přístup odkudkoli, kde mají připojení k Internetu. 

Přečtěte si o příkladem celým postupem můžete to udělat.

V tomto článku ukážeme přístup nasdílet všechny naše uživatele. Však může použít jakékoli aplikace. Dokud aplikace můžete nainstalovat na počítači s Windows serveru 2012 R2, můžete s ním provedením následujících kroků. [Požadavky aplikace](remoteapp-appreqs.md) , abyste měli jistotu, že budou fungovat aplikace můžete zkontrolovat.

Dejte pozor, protože Access je databáze, a chceme databáze být užitečné, jsme bude by několik kroků navíc umožnit uživatelům přístup sdílení dat aplikace Access. Pokud vaše aplikace není databáze a nemusíte uživatelům umožnit přístup ke sdílené složce můžete přejít tyto kroky v tomto kurzu

> [AZURE.NOTE] <a name="note"></a>Musíte mít účet Azure tento kurz:
> - Můžete [Otevřít účet Azure zdarma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): získání přeplatky můžete vyzkoušet placené služby Azure a i po byli zvyklí až můžete ponechat účet a použití uvolnit Azure služby, jako je weby. Platební kartou nikdy strhne příslušná, pokud explicitně změňte nastavení a požádat o platit.
> - Je možné [Aktivovat výhod odběratele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): web MSDN pro vaše předplatné vám přeplatky každý měsíc, využívající služby placené Azure.


## <a name="create-a-collection-in-remoteapp"></a>Vytvoření kolekce v RemoteApp

Začněte tak, že vytvoříte kolekci. V kolekci slouží jako úložiště pro uživatele a aplikace. Jednotlivé kolekce je založená na obrázek – můžete vytvořit vlastní nebo použít jednu součástí vašeho předplatného. Pro účely tohoto návodu Pracujeme s programem obrázek zkušební verzi Office 2013 – s aplikací, který chcete sdílet.

1. Na portálu Azure přejděte dolů v levém navigačním stromu se zobrazila RemoteApp. Otevřete stránku.
2. Klikněte na **vytvořit kolekci RemoteApp**.
3. Klikněte na **vytvořit** a zadejte název pro svou kolekci.
4. Vyberte oblast, kterou chcete použít k vytvoření kolekci. Která zaručuje nejlepší možnosti vyberte oblast, které je nejblíž geograficky do umístění, kde bude uživatelům přístup k aplikaci. Například v tomto kurzu uživatele bude nacházet v Redmond, WA. Oblasti nejbližší Azure je **Západní USA**.
5. Vyberte fakturační plán, který chcete použít. Základní fakturační plán umístí 16 uživatelů na velké OM Azure během standardní fakturační plán má 10 uživatelů na velké OM Azure. Obecné příklad základní plán funguje výborně datový typ položky pracovního postupu. Aplikace produktivity, jako třeba Office je vhodné standardní plán.
6. Nakonec vyberte obrázek Office 2013 Professional. Tento obrázek obsahuje aplikace pro Office 2013. Jenom připomínku - tento obrázek je pouze vhodný k vyzkoušení kolekcí a POCs. Můžete "nelze použít tento obrázek v kolekci výroby.
7. Teď klikněte na **vytvořit RemoteApp kolekce**.

![Vytvoření kolekce cloudu v RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Spustí se vytváření kolekci, ale to může trvat až jednu hodinu.

Teď jste připraveni přidat uživatele.

## <a name="share-the-app-with-users"></a>Sdílet s uživateli aplikace

Po úspěšném vytvoření kolekci, je čas publikovat přístupu pro uživatele a přidat uživatele, kteří mají mít přístup k němu.

Pokud přecházíte od uzel Azure RemoteApp při vytváření kolekci, začněte tím, že pohybovat na jiné místo na domovské stránce Azure.

2. Klikněte na kolekce jste vytvořili dříve, můžete přejít na další možnosti a nakonfigurovat kolekci.
![Novou kolekci cloudu RemoteApp](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Na kartě **publikování** klikněte na **Publikovat** v dolní části obrazovky a potom klikněte na **programy v nabídce Start publikovat**.
![Publikování aplikace RemoteApp](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Vyberte aplikace, které chcete publikovat ze seznamu. Pro naše účel zvolili jsme přístup. Klikněte na **dokončení**. Počkejte aplikace dokončete publikování.
![Publikování přístupu v RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Jakmile aplikace proběhla publikování, vedoucí myši na kartu **Přístup uživatelů** k přidání všech uživatelů, kteří potřebují mít přístup ke svým aplikacím. Zadejte uživatelská jména (e-mailová adresa) pro uživatele a potom klikněte na **Uložit**.

![Přidání uživatelů do RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Teď je čas na informování uživatelů o těchto nových aplikací a jak se k nim měli přístup. K tomuto účelu vaší uživatelům poslat e-mailu je přejdete na adresu URL pro připojení ke vzdálené ploše klienta stažení.
![Klient stažení adresy URL pro RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Konfigurace aplikace access do aplikace Access

Některé aplikace vyžaduje další nastavení po nasazení prostřednictvím RemoteApp. Zejména pro Access, ukážeme, můžete vytvářet sdílené složky na Azure, k níž všichni uživatelé přístup. (Pokud nechcete to udělat, můžete vytvořit [hybridní kolekce](remoteapp-create-hybrid-deployment.md) [místo naše cloudu kolekce], které umožní uživatelům přístup k souborům a informace v místní síti.) Potom bude potřeba dát uživatelích namapujte místním disku v počítači na systém Azure souborů.

První část prováděným jako správce. Potom máme některé kroky pro uživatele.

1. Začněte tím, že publikování rozhraní příkazového řádku (cmd.exe). Na kartě **publikování** vyberte **cmd**a klikněte na **Publikovat > publikovat program pomocí cesty**.
2. Zadejte název aplikace a cestu. Pro naše účel použijte jako cestu "Průzkumníka souborů" jako název a "% SYSTEMDRIVE%\windows\explorer.exe".
![Soubor cmd.exe publikujte.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Teď je potřeba vytvořit Azure [účtu úložiště](../storage/storage-create-storage-account.md). Pojmenovali jsme ho náš "accessstorage," takže vyberte název, který je smysluplný. (K misquote Highlander, může existovat pouze jeden "accessstorage.") ![Účtu úložiště náš Azure](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Teď přejděte zpět do řídícího tak, abyste se cestu k úložišti (koncový bod umístění). Budete slouží v trochu, proto se ujistěte, že ho zkopírujete jinam, postupujte.
![Cesta účtu úložiště](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Dále po vytvoření účtu úložiště je třeba primární přístupové klávesy. Klikněte na **Správa přístupových kláves z verze**a zkopírujte primární přístupové klávesy.
6. Teď nastavte kontextu účtu úložiště a vytvořte nové sdílené složky přístup pomocí protokolu. V okně aplikace zvýšenými prostředí Windows PowerShell, spusťte tyto rutiny:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Tak, aby pro naše sdílet tyto rutiny, jsme spuštění:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Nyní je zapnout uživatele. Nejdřív máte vaši uživatelé nainstalovat [RemoteApp klienta](remoteapp-clients.md). Pak uživatelé muset namapovat jednotku z svůj účet do této Azure sdílené složky, které jste vytvořili a přidejte své přístup k souborům. Jedná se to udělat takto:

1. V klientovi RemoteApp access publikované aplikace. Spusťte aplikaci cmd.exe.
2. Spusťte tento příkaz mapovat jednotku z počítače ke sdílené složce:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Pokud jste nastavili parametr **/ trvalý** na hodnotu Ano, bude trvat UNC relacích.
1. Teď spusťte aplikaci Průzkumníka souborů z RemoteApp. Zkopírujte přístup k souborům, které chcete použít ve sdílené aplikace do sdílené složky.
![Vložení souborů aplikace Access Azure sdílet](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Nakonec spusťte Access a otevřete databázi, kterou sdílíte. Měli byste vidět dat v aplikaci Access spuštěné v cloudu.
![Skutečné databáze Access spuštěné z cloudu](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Teď můžete přístup na všech svých zařízeních – stačí zkontrolujte, že nainstalujete RemoteApp klienta.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Teď, když jste zvládli vytváření kolekce, zkuste vytvořit [kolekce používající Office 365](remoteapp-tutorial-o365anywhere.md). Nebo můžete vytvořit [hybridní kolekce ](remoteapp-create-hybrid-deployment.md)dostanete místní síti.

<!--Image references-->
 
