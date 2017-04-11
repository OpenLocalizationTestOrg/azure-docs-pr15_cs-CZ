<properties
    pageTitle="Přidání funkcí do prvního webové aplikace"
    description="Přidání zajímavých funkcí pro webovou aplikaci první za několik minut."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Přidání funkcí do prvního webové aplikace

V [nasazení první webovou aplikaci pro Azure pět minut](app-service-web-get-started.md)nasadili do ukázkové webových aplikací služby [Azure aplikace](../app-service/app-service-value-prop-what-is.md). V tomto článku se rychle přidáte některé skvělé funkce do nasazeném webové aplikace. Za několik minut udělejte toto:

- vynucení ověřování pro uživatele
- Změna velikosti aplikace automaticky
- příjem upozornění na výkon aplikace

Bez ohledu na to, které nasazenou v předchozí článek aplikaci vzorku můžete začít sledovat v tomto kurzu.

Tři aktivity v tomto kurzu je jenom několik příkladů řadu užitečných funkcí, které se zobrazí, když umístíte webovou aplikaci v aplikaci služby. Mnoho funkcí jsou k dispozici ve vrstvě **Free** (tedy webovou aplikaci první spuštění), a používáte zkušební přeplatky objevovat funkcí, které vyžadují vyšší ceny úrovní. Zbývající jistí, že webovou aplikaci zůstávají ve vrstvě **Uvolnit** , dokud ho explicitně změní na jinou ceny osy.

>[AZURE.NOTE] V prohlížeči, který jste vytvořili pomocí rozhraní příkazového řádku Azure běží **Free** osy, která umožňuje pouze jednou instancí OM sdílené s kvóty prostředků. Další informace o dosáhnout pomocí **Free** osy najdete v článku [limity aplikaci služby](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Ověřování uživatelů

Teď si ukážeme, jak je snadné přidat ověřování aplikace (Další materiály ke čtení v [Aplikaci služby ověřování/se tak mohli ověřovat](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. V portálu zásuvné aplikaci, kterou jste právě otevřeli, klikněte na **Nastavení** > **ověřování / se tak mohli ověřovat**.  
    ![Ověření – nastavení zásuvné](./media/app-service-web-get-started/aad-login-settings.png)

2. Klikněte **na** zapnout ověřování.  

4. V dialogovém okně **Zprostředkovatelů ověřování**klikněte na **Azure Active Directory**.  
    ![Ověření – vyberte Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. V **Nastavení služby Azure Active Directory** zásuvné **Express**klepnutím na tlačítko **OK**. Výchozí nastavení vytvořit novou aplikaci Azure AD v adresáři výchozí.  
 ![Ověření – express konfigurace](./media/app-service-web-get-started/aad-login-express.png)

6. Klikněte na **Uložit**.  
    ![Ověření - uložit konfiguraci](./media/app-service-web-get-started/aad-login-save.png)

    Po úspěšném změny uvidíte oznámení zvonu zapnutí zelená spolu s popisné zprávy.

7. Po návratu do portálu zásuvné aplikace klikněte na **adresu URL** (nebo **přejděte** v řádku nabídek). Odkaz se adresa HTTP.  
    ![Ověření – přejděte na adresu URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Ale jakmile ho na nové kartě otevře aplikace, adresu URL přesměrování několikrát a pole skončí aplikace s adresou HTTPS. Co se vám je, že už jste přihlášení k předplatnému Azure a automaticky se ověřeny v aplikaci.  
    ![Ověření - přihlášení](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Takže pokud teď otevřít relaci neověřených v jiný prohlížeč, zobrazí se přihlašovací obrazovka při přechodu na stejnou adresu URL.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Pokud jste nikdy napíšete nic se službou Azure Active Directory, výchozí adresář nemusí mít všichni uživatelé Azure AD. V takovém případě pravděpodobně jenom účtu v něm je účet Microsoft k předplatnému Azure. Proto nejsou automaticky přihlášeni do aplikace ve stejném prohlížeči dříve.
   Můžete této stejnému účtu Microsoft k přihlášení na tomto přihlašovací stránku.

Gratulujeme, ověřujete všechny přenosy na váš web appu.

Jste si všimli v **ověřování / se tak mohli ověřovat** zásuvné, který může provádět mnohem víc, jako je třeba:

- Povolení sociální přihlášení
- Povolení více možnosti přihlášení
- Změnit výchozí chování při prvním dostat do aplikace

Aplikace služby poskytuje že řešení zapnout klíč pro některé běžné ověřování vyžaduje nemuseli kvůli ověření logickou sami.
Další informace najdete v tématu [Aplikace služby ověřování/se tak mohli ověřovat](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Změna velikosti aplikace automaticky na základě na vyžádání

Další, Pojďme automatické měřítko aplikace tak, aby se automaticky přizpůsobí ho schopnost odpověď na požadavek uživatele (Další materiály ke čtení v [rozsahu nahoru vaši aplikaci distribuovali Azure](web-sites-scale.md) a [Měřítko počet instancí, ať už ručně nebo automaticky](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Stručně řečeno můžete změnit velikost webovou aplikaci dvěma způsoby:

- [Škálování](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Získejte další procesoru paměti, místo na disku a dalších funkcí, jako je vyhrazený VMs, vlastní domény a certifikátech, přípravu sloty neobsahovaly text a další. Škálování změnou ceny osy plán aplikace služeb, do kterých patří aplikace.
- [Měřítko,](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zvýšení počtu OM instance: spuštění aplikace.
Můžete změnit na maximálně 50 instance, v závislosti na ceny osy.

Bez dalších ado vytvoříme neobsahovaly text.

1. Nejdřív si škálování povolit neobsahovaly text. V portálu zásuvné aplikace klikněte na **Nastavení** > **Měřítko nahoru (plán služeb aplikací)**.  
    ![Škálování - zásuvné nastavení](./media/app-service-web-get-started/scale-up-settings.png)

2. Posuňte zobrazení a vyberte **S1 standardní** osy, nejnižší úrovně, která podporuje neobsahovaly text (kruhu v snímek), a pak klikněte na tlačítko **Vybrat**.  
    ![Škálování – zvolit osy](./media/app-service-web-get-started/scale-up-select.png)

    Dokončení změny velikosti nahoru.

    >[AZURE.IMPORTANT] Tento osy výdajů bezplatnou zkušební verzi přeplatky. Pokud máte účet platovou za použití poněkud poplatky ke svému účtu.

3. Dále si konfigurace neobsahovaly text. V portálu zásuvné aplikace klikněte na **Nastavení** > **Měřítko, (plán služeb aplikací)**.  
    ![Měřítko, - zásuvné nastavení](./media/app-service-web-get-started/scale-out-settings.png)

4. Změna **měřítka tak, že** na **Procento využití procesoru**. Posuvníky pod rozevíracím seznamu příslušně aktualizovat. Potom definujte oblast **instance** mezi **1** a **2** a **cílové oblast** mezi **40** a **80**. To udělejte tak, že zadáte do polí nebo přesunutím jezdců.  
 ![Rozšiřování – konfigurace neobsahovaly text](./media/app-service-web-get-started/scale-out-configure.png)

    Založené na této konfiguraci aplikace automaticky nastaví se při využití procesoru je vyšší než 80 % a upraví v při využití procesoru je menší než 40 %.

5. V řádku nabídek klikněte na **Uložit** .

Gratulujeme, je aplikace neobsahovaly text.

Jste si všimli v zásuvné **Měřítko** , který může provádět mnohem víc, jako je třeba:

- Změna velikosti na konkrétní počet výskytů ručně
- Zobrazit jako ostatní měřítka, například paměti procentuální hodnotu nebo disku fronty
- Přizpůsobení velikosti chování při aktivaci pravidla výkonu
- Automatické měřítko na plán
- Nastavení chování neobsahovaly text pro budoucí událost

Další informace o škálování aplikace najdete v článku [škálování vaši aplikaci distribuovali Azure](../app-service-web/web-sites-scale.md). Další informace o rozšiřování najdete v článku [Měřítko počet instancí, ať už ručně nebo automaticky](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Přijímání upozornění aplikace

Teď je aplikace neobsahovaly text, co se stane, když dosáhne instancí maximální počet (2) a sloupcem procesor je nad požadované využití (80 %)?
Upozornění (Další materiály ke čtení na [přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) můžete nastavit informující o této situace, můžete dál zmenšit nahoru/mimo aplikaci, třeba. Pojďme rychle nastavit upozornění pro tento scénář.

1. V portálu zásuvné aplikace klikněte na **Nástroje** > **upozornění**.  
    ![Výstrahy – nastavení zásuvné](./media/app-service-web-get-started/alert-settings.png)

2. Klikněte na **Přidat upozornění**. V dialogovém okně **zdroje** vyberte zdroj, který má na konci **(serverfarms)**. Je to váš plán služeb aplikací.  
    ![Výstrahy – přidání upozornění pro plán služeb aplikací](./media/app-service-web-get-started/alert-add.png)

3. Zadejte **název** jako `CPU Maxed`, **míru** jako **Procento využití procesoru**a **prahové hodnoty** jako `90`, vyberte **Vlastníci e-mailu, přispěvatelé a čtenáři**a klikněte na **OK**.   
 ![Výstrahy – Konfigurace upozornění](./media/app-service-web-get-started/alert-configure.png)

    Po dokončení Azure vytváření upozornění uvidíte v zásuvné **upozornění** .  
    ![Výstrahy – dokončení zobrazení](./media/app-service-web-get-started/alert-done.png)

Gratulujeme, se teď zobrazuje upozornění.

Toto upozornění nastavení zkontroluje využití procesoru každých pět minut. Pokud toto číslo dostane nad 90 %, budete dostávat e-mailovém upozornění, spolu s každý, kdo má oprávnění. Chcete-li zobrazit každý, kdo je oprávněn dostávat oznámení, vraťte se do portálu zásuvné aplikace a klikněte na tlačítko **přístup** .  
![V tématu kdo dostane oznámení](./media/app-service-web-get-started/alert-rbac.png)

Měli byste vidět **Správci předplatného** se již nacházejí **vlastník** aplikace. Tuto skupinu by obsahoval, pokud jste správce účet Azure předplatného (například zkušební předplatné). Další informace o řízení Azure přístupu na základě rolí v tématu [Řízení přístupu Azure Role-Based](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Pravidla výstrah je Azure funkce. Další informace najdete v tématu [přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Další kroky

Začít Konfigurace upozornění jste si všimli celá řada nástrojů v zásuvné **Nástroje** . Tady můžete vyřešit problémy a sledovat výkon test na chyby, přidávání a používání zdrojů, spolupracovat s konzole OM a přidat užitečné rozšíření. Pomozte klikněte na každý z nich tyto nástroje pro zjišťování nástroje jednoduchou a zároveň velmi užitečná při prstem tipy.

Zjistěte, jak můžete udělat víc s aplikací nasazena. Tady je jenom částečné seznam:

- [Koupit a konfigurace vaší vlastní doménou](custom-dns-web-site-buydomains-web-app.md) – nákup atraktivní domény pro web app místo *. azurewebsites.net domény. Nebo použití domény, kterou už máte.
- [Nastavení pracovní prostředí](web-sites-staged-publishing.md) – nasazení aplikace pracovní adresy URL před přepnutím do výroby. Aktualizujte živou webovou aplikaci spolehlivé. Nastavte si komplikovanou DevOps řešení s více paticemi nasazení.
- [Nastavení nepřetržitý nasazení](app-service-continuous-deployment.md) - integrace nasazení aplikace do vašeho systému řízení zdroje. Nasazení Azure s každé potvrzení.
- [Access místních zdrojů](web-sites-hybrid-connection-get-started.md) – Access existující místní databázi nebo systému CRM.
- [Obecnějším údajům aplikace](web-sites-backup.md) – nastavení zpět a obnovení pro web app. Příprava na neočekávaných chyb a obnovit z nich.
- [Povolení protokoly pro diagnostiku](web-sites-enable-diagnostic-log.md) - číst protokoly služby IIS z Azure nebo aplikace trasování. Čtení v toku, stáhněte si je nebo port přehledné [Aplikace](../application-insights/app-insights-overview.md) pro analýzu zapnout klíče.
- [Skenování aplikace pro chyby](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
skenování webovou aplikaci proti moderní hrozeb pomocí služby poskytovanou [Tinfoil zabezpečení](https://www.tinfoilsecurity.com/).
- [Spuštění úlohy pozadí](../azure-functions/functions-overview.md) – spuštění úlohy pro zpracování dat, vytváření sestav, apod.
- [Zjistěte, jak funguje aplikace služby](../app-service/app-service-how-works-readme.md)
