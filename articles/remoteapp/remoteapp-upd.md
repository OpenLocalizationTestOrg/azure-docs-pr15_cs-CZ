
<properties 
    pageTitle="Jak Azure RemoteApp ukládá uživatelských dat a nastavení? | Microsoft Azure"
    description="Zjistěte, jak Azure RemoteApp ukládá uživatelská data pomocí disku profilu uživatele."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Jak Azure RemoteApp ukládá uživatelských dat a nastavení?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp ukládá identity uživatele a přizpůsobení různých zařízeních a relace. Tento uživatel data uložena v disk za kolekce uživatelská jmenoval disku profilu uživatele (UDP). Disk následuje uživatele a zajišťuje, že má uživatel konzistentní setkat i v případě, bez ohledu na to, kde se přihlásit. slouží k uložení 

Disků profilu uživatele jsou úplně průhledné uživateli – uživatelé ukládat dokumenty do složky Dokumenty (zdánlivě na místní disk) a změnit jejich nastavení aplikace jako obvykle. Ve stejnou dobu uchovávat všechny osobní nastavení při připojování k Azure RemoteApp z libovolného zařízení. Uživateli se zobrazí stačí svých dat na stejném místě.

Každý UDP má 50GB trvalý skladování a obsahuje obou dat a aplikací nastavení. 

Přečtěte si o zvláštnosti na dat profilů uživatelů.

>[AZURE.NOTE] Potřebujete zakázat UDP? Můžete udělat, které teď - pokladna Pavithra na příspěvek na blog, [Zakázat uživatelského profilu disků (UPDs) Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/)podrobnosti.


## <a name="how-can-an-admin-get-to-the-data"></a>Jak lze získat správce k datům?

Pokud potřebujete přístup k datům pro jednoho nebo více uživatelů (pro obnovení havárie nebo pokud uživatel opustí společnosti), kontaktujte podporu Azure a zadejte informace o předplatném pro kolekci a identita uživatele. Tým Azure RemoteApp zadejte její adresu URL do virtuální pevný disk. Stáhněte si tento virtuální pevný disk a načíst všechny dokumenty nebo soubory, které potřebujete. Všimněte si, že virtuální pevný disk 50GB, bude trvat tento bit pro její stažení.


## <a name="is-the-data-backed-up"></a>Vytvoření zálohy data?

Ano, doporučujeme uložit záložní kopii uživatelská data podle geografické polohy. Data jsou jen pro čtení a můžete k nim získat přístup stejně jako běžný dat bude (kontakt Azure RemoteApp získání), je-li primární datovém centru dolů. Kopírování dat v reálném čase do umístění zálohy a jsme Neudržovat kopií různých verzí. Ano na poškození dat, můžeme nepůjde obnovit dřív známou správnou verzi, ale -li primární datovém centru dolů, bude moct získat uživatelská data z jiného umístění.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Jak uživatelům se zobrazí UDP na straně serveru?

Každý uživatel bude mít vlastní adresář na serveru, který odpovídá jejich UDP: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Co je nejlepší způsob, jak pomocí aplikace Outlook a UDP?

Azure RemoteApp ukládá stav aplikace Outlook (poštovní schránky, PST) mezi jednotlivými relacemi. Aby, potřebujeme PST se uloží do dat profilů uživatelů (c:\users\<uživatelské jméno >). Toto je výchozí umístění pro uložení dat, takže, dokud nezměníte umístění, data se zachovají relace.

Doporučujeme také v režimu "režim cached" v aplikaci Outlook a v režimu "server/online" při hledání.

Podívejte se [v tomto článku](remoteapp-outlook.md) najdete další informace o použití aplikace Outlook a Azure RemoteApp.

## <a name="what-about-redirection"></a>Co dávat pozor přesměrování?
Můžete nakonfigurovat Azure RemoteApp umožníte uživatelům přístup k místním zařízením nastavením [přesměrování](remoteapp-redirection.md). Místní zařízení pak bude mít přístup k datům v UDP.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Můžete použít Moje UDP jako sdílené síťové složky?
Ne. UPDs nelze použít jako sdílené síťové složky. UDP je k dispozici pro uživatele pouze když uživatel aktivně připojení k Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Pokud se uživatel odstranit z kolekce, odstraněna jejich UDP?

Ne, odstraníte uživatele, jsme neodstraňujte automaticky UDP: místo toho jsme ukládání dat dokud neodstraníte kolekci. 90 dní po odstranění v kolekci jsme odstranit všechny UPDs. 

Pokud potřebujete odstranit z kolekce UDP, obraťte se na Azure RemoteApp - jsme odstranit UDP z naší strany.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Můžete získat přístup k UPDs mých uživatelů (aktuální nebo odstraněné uživatele)?

Ano, pokud budete kontaktovat [Azure RemoteApp](mailto:remoteappforum@microsoft.com), jsme můžete nastavit můžete s adresou URL pro přístup k datům. Budete muset asi 10 hodin stahování dat nebo souborů UDP před vypršením platnosti přístup.

## <a name="are-upds-available-offline"></a>UPDs dostupné offline?

Přímo teď můžeme nenabízejí offline přístup k UPDs, za okna aplikace access 10 hodinu jsme je popsali výše. To znamená, že jsme zrovna nemáte způsob, jak můžete poskytnout přístup dlouhé dost lze provést složitější úkoly, jako je spuštěný antivirový software na UPDs nebo získání přístupu k datům auditu.

## <a name="do-registry-key-settings-persist"></a>Nastavení klíče registru přetrvávají?
Ano, něco zapisovat HKEY_Current_User je součástí UDP.

## <a name="can-i-disable-upds-for-a-collection"></a>Můžete zakázat UPDs kolekce?

Ano, můžete požádat o Azure RemoteApp deaktivaci UPDs pro předplatné, ale nemůžete dělat, které sami. To znamená, že UPDs přestane pro všechny kolekce v předplatného.

Můžete chtít zakázat UPDs v některém z následujících situací: 

- Dokončení potřebovat přístup a řízení uživatelských dat (pro auditování a zkontrolujte účely například finanční instituce).
- Máte 3rd výrobců uživatelský profil Správa řešení místní a chcete dál používat ve vaší doméně Azure RemoteApp nasazení. Bude vyžadovat agenta profil načte do gold obrázek. 
- Nemusíte všechny místní úložiště dat nebo zobrazit všechna data v cloudu nebo ve sdílené složce a chcete ovládací prvek ukládání dat místně pomocí Azure RemoteApp.

Další informace najdete v tématu [Zakázat uživatelského profilu disků (UPDs) Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Můžou omezit uživatelů z ukládání dat na systémovou jednotku?

Ano, ale budete muset nastavení, která šablona obrázku před vytvořením kolekci. Zablokování přístupu na systémovou jednotku pomocí následujících kroků:

1. Spusťte **gpedit.msc** na obrázek šablony.
2. Přejděte na **Konfigurace uživatele > šablony pro správu > součásti > Explorer**.
3. Vyberte následující možnosti:
    - **Skrýt tyto jednotky ve složce Tento počítač**
    - **Zakázat přístup k jednotky z tohoto počítače:**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Můžete inokula UPDs? Chci umístit některá data, která je dostupná uživatel přihlásí poprvé UDP.

Nastavení Ano znamená, když vytvoříte obrázek šablony, můžete přidat informace do výchozího profilu. Tyto informace přibude klikněte UDP.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Můžete změnit velikost UDP v závislosti na množství zpracovávaných dat chci uložit?

Ne, všechny UPDs máte 50 GB úložiště. Pokud chcete ukládat různých množství dat, zkuste toto:

1. Zakázání UPDs kolekce.
2. Nastavení sdílení souborů pro uživatele k přístupu.
3. Načtení sdílené složky pomocí skriptu spuštění. Podrobnosti najdete v části pod na spouštěcí skripty v Azure RemoteApp.
4. Požádejte uživatele, aby uložit všechna data do sdílené složky.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Spouštění skript při spuštění v Azure RemoteApp

Pokud chcete spustit spuštění skript, začněte tím, že vytváření naplánovaný úkol v obrázek šablony, které budete používat pro kolekci. (Tento *před* spuštěním nástroje sysprep.) 

![Vytvoření úkolu systému](./media/remoteapp-upd/upd1.png)

![Vytvoření úkolu systému, které se spouští při přihlášení uživatele](./media/remoteapp-upd/upd2.png)

Na kartě **Obecné** nezapomeňte změna **Uživatelského účtu** v části zabezpečení a "BUILTIN\Users."

![Změna účtu uživatele do skupiny](./media/remoteapp-upd/upd4.png)

Naplánovaný úkol, spustí se skriptem při spuštění pomocí přihlašovací údaje uživatele. Plánování spuštění každé čas může uživatel přihlásí.

![Nastavení aktivační události pro daný úkol jako "při přihlášení"](./media/remoteapp-upd/upd3.png)

Můžete taky použít [zásadami skupiny spouštěcího](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Na co dávat umístění spouštěcího skriptu do nabídky Start? Které fungují?

Jinými slovy lze vytvořit bat spouštějící skript okno Konfigurace a uložte ho do složky Menu\Programs\StartUp c:\ProgramData\Microsoft\Windows\Start a pak je potřeba spustit pokaždé, když uživatel spustí relaci RemoteApp skript?

Ne, který není podporován s Azure RemoteApp, který využívá RDSH, který taky nepodporuje spouštěcího v nabídce Start.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Dá se pomocí mstsc.exe (vzdálené plochy programů) konfigurace přihlašovacích skriptů?

Nope není podporována Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Můžete ukládat data na OM místně?

Data uložená kdekoli na OM jinak než v UDP Ne, budou ztraceny. Je velmi pravděpodobné uživatel nebude získat stejný OM dalšího čas, který se přihlaste se k Azure RemoteApp. Jsme nepracují trvalé uživatele OM, uživatel nebude přihlásit do stejné OM, takže data budou ztraceny. Navíc při aktualizaci kolekci, existující VMs nahrazeny novou sadu VMs -, to znamená, že všech dat uložených na OM samotné bude ztracen. Doporučujeme uložit data UDP sdílené úložiště třeba Azure soubory, souborovém serveru uvnitř VNET nebo v cloudu pomocí systému cloudové úložiště jako Dropboxu.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Jak připojit Azure sdílené složky na OM pomocí prostředí PowerShell

Můžete použít rutinu čistého PSDrive připojení jednotku, následujícím způsobem:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Publikujte své přihlašovací údaje spuštěním následujícího:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Která umožňuje přeskočit - pověření parametru v rutinu New-PSDrive.
