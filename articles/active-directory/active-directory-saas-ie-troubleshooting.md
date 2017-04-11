<properties
    pageTitle="Poradce při potížích s rozšíření panely aplikace Access pro aplikaci Internet Explorer | Microsoft Azure"
    description="Způsob použití zásad skupiny pro nasazení doplňku aplikace Internet Explorer pro portálu Moje aplikace."
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

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Poradce při potížích s rozšíření panely aplikace Access pro aplikaci Internet Explorer

Tento článek vám pomůže řešení těchto problémů:

- Nemůžete získat přístup k aplikace portálu Moje aplikace při používání aplikace Internet Explorer.
- Zobrazí se zpráva "Instalace softwaru" i když po instalaci softwaru.

Pokud jste správcem, viz také: [nasazení rozšíření panely aplikace Access pro Internet Explorer prostřednictvím zásad skupiny](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Spuštění nástroje pro diagnostiku

Diagnostikovat potíže při instalaci s příponou panely aplikace Access stahování a spuštěním diagnostický nástroj Panel přístup:

1. [Pokud si Pokud chcete stáhnout diagnostický nástroj, klikněte sem.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Otevřete soubor a na tlačítko **extrahovat všechny** .

    ![Stiskněte extrahovat všechny](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Klepněte na tlačítko **extrahovat** pokračovat.

    ![Stiskněte extrahovat](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Spuštění nástroje, klikněte pravým tlačítkem myši soubor s názvem **AccessPanelExtensionDiagnosticTool**a pak vyberte **Otevřít v aplikaci > Microsoft Windows na základě Script Host**.

    ![Otevřít v aplikaci > Microsoft Windows na základě Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Zobrazí se následující diagnostiky okně, které popisuje, co můžou být správné snažit se instalace.

    ![Ukázka diagnostiky okna](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Kliknutím na "**Ano**" nechte program řešení problémů, které nebyly nalezeny.

7. Tyto změny uložit, zavřete každého okna Internet Exploreru a pak znovu otevřete Internet Explorer.<br />Pokud ještě nemáte přístup ke aplikací, zkuste kroků.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Kontrola, zda je povolen rozšíření panely aplikace Access

Chcete-li povolte panelů přístup v aplikaci Internet Explorer:

1. V Internet Exploreru klikněte na **ikonu ozubeného kola** v pravém horním rohu okna. Klikněte na **Možnosti Internetu**.<br />(Ve starších verzích Internet Exploreru najdete ji do **Nástroje > Možnosti Internetu**.

    ![Přejděte na Nástroje > Možnosti Internetu](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Klikněte na kartu **programy** a potom klikněte na tlačítko **Spravovat doplňky** .

    ![Klikněte na Spravovat doplňky](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. V tomto dialogovém okně vyberte **Rozšíření panely aplikace Access** a klikněte na tlačítko **Povolit** .

    ![Klikněte na povolit](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Tyto změny uložit, Zavřít každého okna Internet Exploreru a pak znovu otevřete Internet Explorer.

##<a name="enable-extensions-for-inprivate-browsing"></a>Povolit rozšíření pro procházení InPrivate

Pokud používáte režim procházení InPrivate:

1. V Internet Exploreru klikněte na **ikonu ozubeného kola** v pravém horním rohu okna. Klikněte na **Možnosti Internetu**.<br />(Ve starších verzích Internet Exploreru najdete ji do **Nástroje > Možnosti Internetu**.

    ![Ukázka diagnostiky okna](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Přejděte na kartu **osobní údaje** , potom **zrušte zaškrtnutí políčka** zaškrtávací políčko **Zakázat panely nástrojů a rozšíření při procházení InPrivate spuštění**</p>

    ![Zrušte zaškrtnutí políčka Zakázat panely nástrojů a rozšíření při procházení InPrivate spuštění](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Tyto změny uložit, Zavřít každého okna Internet Exploreru a pak znovu otevřete Internet Explorer.

##<a name="uninstall-the-access-panel-extension"></a>Odinstalace rozšíření panely aplikace Access

Odinstalace aplikace Access panelů ze svého počítače:

1. Na klávesnici stiskněte **klávesy Windows** otevřete nabídku Start. Při otevření v nabídce napsat něco udělat hledání. Napište "Ovládací panely" a když se objeví ve výsledcích hledání otevřete **Ovládací panely** .

    ![Hledání pro ovládací Panel](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. V pravém horním rohu ovládací panely změňte možnosti **zobrazení tak, že** na **Velké ikony**. Pak najděte a klikněte na tlačítko **programy a funkce** .

    ![Chcete-li zobrazit velké ikony Chang zobrazení](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. V seznamu vyberte **Rozšíření panely aplikace Access**a klikněte na tlačítko **odinstalovat** .

    ![Klikněte na odinstalovat](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Můžete pak zkuste nainstalovat koncovku zjistíte, zda došlo k odstranění problému.

Pokud narazíte na problémy s odinstalací prodloužení, můžete ho nástrojem pro [Microsoft řešení ho](https://go.microsoft.com/?linkid=9779673) taky odebrat.

## <a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Nasazení rozšíření panely aplikace Access pro Internet Explorer prostřednictvím zásad skupiny](active-directory-saas-ie-group-policy.md)
