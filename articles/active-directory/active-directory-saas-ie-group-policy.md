<properties
    pageTitle="Nasazení rozšíření panely aplikace Access pro Internet Explorer prostřednictvím zásad skupiny | Microsoft Azure"
    description="Způsob použití zásad skupiny zavedení doplňku aplikace Internet Explorer pro portálu Moje aplikace."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Nasazení rozšíření panely aplikace Access pro Internet Explorer prostřednictvím zásad skupiny

Tento kurz ukazuje, jak používat zásad skupiny pro vzdáleně instalaci panelů přístup pro aplikaci Internet Explorer na počítačích vašich uživatelů. Tuto linku je potřebný pro Internet Explorer uživatelé, kteří potřebují a přihlaste se do aplikace, které se konfigurují [na základě heslo jednotného přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Je vhodné správci automatizovat nasazení tuto linku. Jinak uživatelé budou muset stáhnout a nainstalovat rozšíření sami, který je k chybě uživatele a vyžaduje oprávnění správce. Tento kurz zahrnuje jednu metodu automatizace software nasazení pomocí zásad skupiny. [Další informace o zásad skupiny.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Přístup panelů je také k dispozici pro [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) a [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), ani z nich vyžadovat oprávnění správce a nainstalujte.

##<a name="prerequisites"></a>Zjistit předpoklady pro

- Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)a jste se připojili počítačích vašich uživatelů do vaší domény.
- Musíte mít oprávnění "Upravte nastavení" Chcete-li upravit objekty zásad skupiny (objekty zásad skupiny). Ve výchozím nastavení členů skupiny zabezpečení následující smíte tento: správci domén, podnikovým správcům a vlastníků poznámkové bloky pro školy zásad skupiny. [Víc se uč.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Krok 1: Vytvoření distribuční bod

Nejdřív musíte umístíte Instalační balíček do umístění v síti, můžete k nim získat přístup ze všech počítačů, které chcete vzdáleně nainstalovat rozšíření. K tomuto účelu postupujte takto:

1. Přihlaste se k serveru jako správce

2. V dialogovém okně **Správce serverů** přejděte na **soubory a úložiště služby**.

    ![Otevírat soubory a úložiště služby](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Přejděte na kartu **jejich počet** . Klikněte na **úkoly** > **Nové sdílet...**

    ![Otevírat soubory a úložiště služby](./media/active-directory-saas-ie-group-policy/shares.png)

4. Dokončení **Průvodce vytvořením sdílené položky** a nastavit oprávnění, která zajistí, že k němu z počítače vašich uživatelů. [Další informace o jejich počet.](https://technet.microsoft.com/library/cc753175.aspx)

5. Stáhněte si následující balíček Instalační služby systému Windows (souboru MSI): [Extension.msi panely aplikace Access](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Zkopírujte instalační balíček do požadovaného umístění ve sdílené složce.

    ![Kopie souboru .msi vám budete sdílet.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Ověřte, zda klientských počítačích mít přístup k instalační balíček ze sdílené složky. 

##<a name="step-2-create-the-group-policy-object"></a>Krok 2: Vytvoření objektu zásad skupiny

1. Přihlaste se k serveru, který je hostitelem vaší instalace Active Directory Domain Services (AD DS).

2. V správce serveru, přejděte na **Nástroje** > **Správa zásad skupiny**.

    ![Přejděte na Nástroje > Seskupit Managment zásad](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. V levém podokně okna **Správa zásad skupiny** zobrazit hierarchii organizační jednotce (OU) a zjistit, na které obor chcete použití zásad skupiny. Například můžete se rozhodnout vybrat malé OU nasadit několik uživatelů pro účely testování nebo vyberete nejvyšší úrovně OU nasazení pro celou organizaci.

    > [AZURE.NOTE] Pokud chcete vytvořit nebo upravit organizačních jednotek (OU), přepněte zpět na správce serveru a přejděte na **Nástroje** > **uživatele služby Active Directory a počítačů**.

4. Po výběru s organizační Jednotkou klikněte na něj pravým tlačítkem myši a vyberte **vytvořit objekt zásad skupiny v této doméně a propojit ji tady...**

    ![Vytvořit nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. Do řádku **Nový objekt zásad skupiny** zadejte název pro nový objekt zásad skupiny.

    ![Název nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Klikněte pravým tlačítkem myši na objekt zásad skupiny, kterou jste právě vytvořili a vyberte **Upravit**.

    ![Úprava nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Krok 3: Přiřazení instalační balíček

1. Zjistěte, zda chcete nasadit rozšíření podle **Konfigurace počítače** nebo **Konfigurace uživatele**. Při použití [počítače konfigurace](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)rozšíření nainstalovaným počítač bez ohledu na to, které přihlášení uživatele k ho. Na druhé straně [Konfigurace uživatele](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), uživatelé budou mít příponu nainstalovaný pro ně bez ohledu na kterých počítačích se přihlásí.

2. V levém podokně okna **Editoru správy zásad skupiny** přejděte na jednu z následujících cest složky, podle toho, jaký typ konfigurace jste se rozhodli:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Klikněte pravým tlačítkem myši na **Instalace softwaru**a potom vyberte **Nový** > **balíčku...**

    ![Vytvořit nový balíček instalace softwaru](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Přejděte na sdílenou složku, která obsahuje instalační balíček z [Krok 1: vytvoření distribuční bod](#step-1-create-the-distribution-point)vyberte soubor .msi a klikněte na tlačítko **Otevřít**.

    > [AZURE.IMPORTANT] Příkaz sdílet se nachází na stejný server, ověřte, zda přistupujete souboru MSI prostřednictvím cesta k souboru sítě, nikoli cesta místního soubor.

    ![Vyberte instalační balíček sdílenou složku.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. V řádku **Nasazení aplikace** vyberte **přiřazené** způsobu nasazení. Klikněte na **OK**.

    ![Vyberte přiřazeno klikněte na OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Rozšíření je teď nasazené na organizační jednotku, kterou jste vybrali. [Další informace o instalaci softwaru zásad skupiny.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Krok 4: Automatické povolení rozšíření pro aplikaci Internet Explorer 

Kromě spuštění instalačního programu, každý rozšíření pro aplikaci Internet Explorer musí být výslovně povolena před použitím. Postupujte podle pokynů a povolení přístupu panelů prostřednictvím zásad skupiny:

1. V okně **Editoru správy zásad skupiny** přejděte na jednu z následujících postupů podle toho, jaký typ konfigurace jste vybrali v [Krok 3: přiřazení instalační balíček](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Klikněte pravým tlačítkem myši na **Seznam doplňků**a vyberte **Upravit**.
    ![Upravte seznam doplňků.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. V okně **Seznam doplňků** vyberte **Povolit**. Pak v části **Možnosti** klikněte na **Zobrazit …**.

    ![Klikněte na povolit a potom klikněte na zobrazit...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. V okně **Zobrazit obsah** proveďte následující kroky:

    1. První sloupec ( **Hodnoty název** pole) zkopírujte a vložte následující ID třídy:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Pro druhém sloupci (poli **hodnota** ) zadejte následující hodnotu:`1`

    3. Klikněte na **OK** zavřete okno **Zobrazit obsah** .

    ![Vyplňte hodnoty výše uvedené.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Kliknutím na **OK** použijete provedené změny a zavřete okno **Seznam doplňků** .

Rozšíření nyní používat pro počítače do vybrané OU. [Další informace o použití zásad skupiny pro povolení nebo zakázání doplňků v aplikaci Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Krok 5 (nepovinné): zakázat "Zapamatovat heslo" výzva

Když uživatelé přihlašovat k webům pomocí panelů přístup, Internet Explorer může zobrazit následující dotaz s dotazem, "Chcete uložit heslo?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Pokud chcete uživatelům zabránit, aby výzva zobrazila, postupujte podle pokynů a zabránit automatického dokončování z pamatovat hesla:

1. V okně **Editoru správy zásad skupiny** přejděte na cestu vypsané dole. Všimněte si, že toto nastavení je k dispozici pouze v části **Konfigurace uživatele**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Najděte nastavení s názvem **zapněte funkci automatického dokončování pro uživatelská jména a hesla ve formulářích**.

    > [AZURE.NOTE] Předchozí verze služby Active Directory může seznam toto nastavení s názvem **nepovolují automatického dokončování ukládání hesel**. Konfigurace nastavení se liší od nastavení popsaných v tomto kurzu.

    ![Upozorňujeme, že to při hledání v části Nastavení uživatele.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Klikněte pravým tlačítkem na výše nastavení a vyberte **Upravit**.

4. V okně s názvem **zapněte funkci automatického dokončování pro uživatelská jména a hesla ve formulářích**vyberte **Zakázáno**.

    ![Vyberte zakázat](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Klikněte na **OK** použijete tyto změny a zavřete okno.

Uživatelé už nebudou moct ukládat své přihlašovací údaje nebo pomocí automatického dokončování pro přístup k dříve uložené přihlašovací údaje. Však této zásady umožňují dál používat automatického dokončování pro jiné typy poli formuláře, například vyhledávací pole.

> [AZURE.WARNING] Pokud je vybrán po uživatelé zvolili pro ukládání některých přihlašovací údaje, bude zásad *není* zrušte přihlašovací údaje, které již byly uloženy.

##<a name="step-6-testing-the-deployment"></a>Krok 6: Testování nasazení

Podle následujících kroků můžete ověřit, pokud byl úspěšný rozšíření nasazení:

1. Pokud jste nainstalovali pomocí **Konfigurace počítače**, přihlaste se k klientského počítače, který patří na organizační jednotku, kterou jste vybrali v [Krok 2: vytvoření objektu zásad skupiny](#step-2-create-the-group-policy-object). Pokud jste nainstalovali pomocí **Konfigurace uživatele**, zkontrolujte, že přihlášení jako uživatel, kterému patří tuto organizační jednotku.

2. Může to trvat pár sign in pro zásady skupiny se změní na plně aktualizovat s Tento počítač. Pokud chcete vynutit aktualizaci otevřete okno **příkazového** , spusťte tento příkaz:`gpupdate /force`

3. Bude potřeba restartovat počítač instalace proběhne. Spuštění může trvat výrazně déle než obvykle při rozšíření nainstaluje.

4. Po restartování počítače, spusťte **Aplikaci Internet Explorer**. V pravém horním rohu okna klikněte na **Nástroje** (ikona ozubeného kola) a potom vyberte **Spravovat doplňky**.

    ![Přejděte na Nástroje > Spravovat doplňky](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. V okně **Spravovat doplňky** ověřte nainstalované **Aplikace Access panelů** a jeho **Stav** je nastaveno tak **povoleno**.

    ![Ověřte, že přípona panelů přístup nainstalované a povolené.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Poradce při potížích s rozšíření panely aplikace Access pro aplikaci Internet Explorer](active-directory-saas-ie-troubleshooting.md)
