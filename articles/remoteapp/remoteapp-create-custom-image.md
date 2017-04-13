<properties
    pageTitle="Postup vytvoření vlastní šablony pro Azure RemoteApp | Microsoft Azure"
    description="Naučte se vytvářet vlastní šablony pro Azure RemoteApp. Pomocí této šablony s hybridním nebo cloudového kolekcí."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Postup vytvoření vlastní šablony pro Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp používá obrázek šablony Windows serveru 2012 R2 hostovat všechny programy, které chcete sdílet s jinými uživateli. Pokud chcete vytvořit vlastní obrázek šablony RemoteApp, můžete začít s existujícím obrazem nebo vytvořte nový účet. 


> [AZURE.TIP] Věděli jste, že můžete vytvořit obrázek z OM Azure? True textu a snižuje dobu ho přijímá k importu obrázku. Podívejte se postup [tady](remoteapp-image-on-azurevm.md).

Požadavky na obrázek, který lze odeslat pomocí Azure RemoteApp jsou:


- Velikost obrázku by měl být násobkem MB. Pokud se pokusíte nahrát obrázek, který není přesný násobek, se nepovede nahrávání.
- Velikost obrázku musí být 127 GB nebo menší.
- Musí být v souboru virtuálního pevného disku (VHDX soubory [Hyper-V virtuální pevných discích] nepodporují aktuálně).
- Virtuální pevný disk nesmí být virtuálního počítače generování 2.
- Virtuální pevný disk může být pevnou velikostí nebo dynamicky se zvětšující. Dynamicky rozbaleného virtuálního pevného disku se nedoporučuje, protože ho zkracuje k nahrání na Azure než pevnou velikostí souboru virtuální pevný disk.
- Disk musí být spuštěn pomocí předlohy spouštěcí záznam (hlavní spouštěcí záznam) oddílů. Nepodporuje se stylem oddílů GUID (GPT).
- Virtuální pevný disk musí obsahovat jedna instalace systému Windows Server 2012 R2. Může obsahovat více svazky, ale pouze jeden, která obsahuje instalace systému Windows.
- Musí být nainstalovaný roli vzdálené plochy relace hostitele (RDSH) a funkci Možnosti práce s počítačem.
- Zprostředkovatel připojení k vzdálené plochy role musí *není* nainstalovat.
- Systém souborů EFS, musíte zakázat.
- Obrázek musí být SYSPREPed pomocí parametrů **/oobe / generalize/shutdown** (nesmí použít parametr **/mode:vm** ).
- Odeslání vašeho virtuální pevný disk z řetězce snímek není podporovaná.


**Než začnete**

Je potřeba udělat následující před vytvořením službu:

- [Registrace](https://azure.microsoft.com/services/remoteapp/) pro RemoteApp.
- Vytvoření uživatelského účtu ve službě Active Directory pro použití účtu služby RemoteApp. Omezte oprávnění pro tento účet tak, že ho jenom účastnit se počítačích na doménu. Další informace najdete v tématu [Konfigurace Azure Active Directory pro RemoteApp](remoteapp-ad.md) .
- Shromážděte informace o vaší místní síti: IP adresa informace a podrobnosti o zařízení VPN.
- Nainstalujte modul [Azure Powershellu](../powershell-install-configure.md) .
- Shromážděte informace o uživatelích, které chcete udělit přístup k. To může být informace o účtu Microsoft nebo Active Directory pracovní účet informace pro uživatele.



## <a name="create-a-template-image"></a>Vytvořit obrázek šablony ##

Toto jsou základní kroky k vytvoření nové šablony od začátku:

1.  Vyhledejte obrázek Windows serveru 2012 R2 aktualizace DVD nebo ISO.
2.  Vytvoření souboru virtuální pevný disk.
4.  Instalace systému Windows Server 2012 R2.
5.  Nainstalujte roli vzdálené plochy relace hostitele (RDSH) a funkci Možnosti práce s počítačem.
6.  Nainstalujte doplňují funkce vyžadují aplikace.
7.  Nainstalujte a nakonfigurujte aplikace. Sdílení aplikací usnadnit přidáte aplikace nebo aplikace, které chcete sdílet do nabídky **Start** obrázku v **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs.
8.  Proveďte všechny další konfigurace Windows vyžadují aplikace.
9.  Zakázání šifrování systému souborů (EFS).
10. **REQUIRED:** Přejděte na Windows Update a nainstalujte všechny důležité aktualizace.
9.  SYSPREP obrázku.

Podrobný postup pro vytvoření nového obrázku je:

1.  Vyhledejte obrázek Windows serveru 2012 R2 aktualizace DVD nebo ISO.
2.  Vytvoření souboru virtuální pevný disk pomocí řízení disku.
    1.  Spusťte Správa disků (diskmgmt.msc).
    2.  Vytvoření dynamicky rozbaleného virtuálního pevného disku 40 GB nebo velikost. (Odhad požadovanou velikost prázdného místa potřebné pro Windows, aplikací a vlastní nastavení. Windows Server s RDSH rolí a nainstalovánu funkci Možnosti práce s počítačem budou vyžadovat asi 10 GB místa).
        1.  Klikněte na **Akce > vytvořit virtuální pevný disk**.
        2.  Zadejte umístění, velikost a formátu virtuální pevný disk. Vyberte **dynamicky rozbalování**a potom klikněte na **OK**.

            Tím se spustí několik sekund. Po dokončení vytváření virtuální pevný disk byste měli vidět nový disk bez jakékoli písmeno a v "Neinicializováno" stavu v konzole Správa disku.

        - Klikněte pravým tlačítkem myši na disku (ne volné místo) a potom klikněte na **Inicializace disku**. Vyberte **hlavní spouštěcí záznam** (hlavní spouštěcí záznam) jako styl oddílu a potom klikněte na **OK**.
        - Vytvoření nového svazku: klikněte pravým tlačítkem myši na volné místo a potom klikněte na **Nový jednoduchý multilicenční**. Přijměte výchozí hodnoty v průvodci, ale zkontrolujte, zda že přiřazení písmeno aby nedocházelo k problémům při nahrát obrázek šablony.
        - Klikněte pravým tlačítkem myši na disk a klikněte na **Odpojit virtuální pevný disk**.





1. Instalace systému Windows Server 2012 R2:
    1. Vytvoření nového virtuálního počítače. Pomocí Průvodce vytvořením virtuálního počítače v Hyper-V správce nebo klient Hyper-V.
        1. Na stránce zadejte generování zvolte **generování 1**.
        2. Na stránce připojit virtuální pevný Disk vyberte možnost **použít existující virtuální pevný disk**a přejděte do virtuálního pevného disku jste vytvořili v předchozím kroku.
        2. Na stránce Možnosti instalace vyberte **nainstalovat operační systém ze spouštěcího CD/DVD_ROM**a pak vyberte umístění systému Windows Server 2012 R2 instalačního média.
        3. Zvolte Další možnosti v Průvodci potřebné k instalaci systému Windows a aplikace. Dokončení průvodce.
    2.  Po dokončení Průvodce upravit nastavení virtuálního počítače a změňte potřebné k instalaci systému Windows a aplikace, například počet procesorů, virtuální a klikněte na **OK**.
    4.  Připojení k virtuální počítač a nainstalujte Windows serveru 2012 R2.
1. Nainstalujte roli vzdálené plochy relace hostitele (RDSH) a funkci Možnosti práce s počítačem:
    1. Spusťte správce serveru.
    2. Na úvodní obrazovce nebo v nabídce **Spravovat** klikněte na **Přidat role a funkce** .
    3. Na stránce než začnete, klikněte na tlačítko **Další** .
    4. Vyberte **instalace na základě rolí nebo založeného na funkcích**a klikněte na tlačítko **Další**.
    5. V seznamu vyberte místního počítače a klikněte na tlačítko **Další**.
    6. Vyberte **Vzdálená plocha**a potom na tlačítko **Další**.
    7. Rozbalte **uživatelským rozhraním a infrastrukturu** a vyberte **Možnosti práce s počítačem**.
    8. Klikněte na **Přidat funkce**a klikněte na tlačítko **Další**.
    9. Na stránce Vzdálená plocha na tlačítko **Další**.
    10. Klikněte na **hostitele relace vzdálené plochy**.
    11. Klikněte na **Přidat funkce**a klikněte na tlačítko **Další**.
    12. Na stránce Potvrdit instalace vybrané možnosti vyberte **automaticky v případě potřeby Restartujte cílový server**a potom klepněte na tlačítko **Ano restart upozornění** .
    13. Klikněte na **instalovat**. Restartuje počítač.
1.  Nainstalujte doplňují funkce vyžadují aplikace, například .NET Framework 3.5. Pokud chcete nainstalovat funkcí, spusťte Průvodce funkce a přidat role.
7.  Instalace a konfigurace aplikace a aplikace, které chcete publikovat prostřednictvím RemoteApp.

>[AZURE.IMPORTANT]
>
>Instalace RDSH role před instalací aplikací zajistit, že jsou před obrázek nahrané na RemoteApp zjištěny problémy s kompatibilitou aplikací.
>
>Zkontrolujte, že se zobrazí v nabídce **Start** pro všechny uživatele (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs) zástupce aplikace (soubor**lnk** ). Také zajistěte, aby byl na ikonu, kterou uvidíte v nabídce **Start** co má uživatelům zobrazit. V opačném případě ho změnit. (Není *mít* přidat aplikaci na začátek nabídky, ale jednodušší ho pro publikování aplikace v RemoteApp. Jinak budete muset zadat cestu instalace aplikace při publikování aplikace.)


8.  Proveďte všechny další konfigurace Windows vyžadují aplikace.
9.  Zakázání šifrování systému souborů (EFS). V okně zvýšenými příkaz spusťte tento příkaz:

        Fsutil behavior set disableencryption 1

    Můžete taky můžete nastavit nebo můžete přidat následující hodnotu DWORD v registru:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Pokud vytváříte obrázek uvnitř Azure virtuálního počítače, přejmenujte ** \%windir%\Panther\Unattend.xml** soubor, jak to bude blokovat skriptu nahrávání použít později fungovat. Změňte název tohoto souboru na Unattend.old tak, aby se pořád máte soubor v případě, že budete potřebovat obnovit nasazení.
10. Přejděte na Windows Update a nainstalujte všechny důležité aktualizace. Může být potřeba spustit Windows Update několikrát zobrazíte všechny aktualizace. (Někdy nainstalujte aktualizace a tuto aktualizaci samotné vyžaduje aktualizaci.)
10. SYSPREP obrázku. Na příkazovém řádku spusťte tento příkaz:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe / shutdown**

    **Poznámka:** Nepoužívejte přepínačem **/mode:vm** příkazu SYSPREP, i když je to je virtuální počítač.


## <a name="next-steps"></a>Další kroky ##
Teď, když máte vlastní šablony, budete muset nahrát tento obrázek do kolekce RemoteApp. Vytvořit kolekci pomocí informací v těchto článcích:


- [Jak vytvořit sadu hybridní RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Jak vytvořit sadu cloudu RemoteApp](remoteapp-create-cloud-deployment.md)
 