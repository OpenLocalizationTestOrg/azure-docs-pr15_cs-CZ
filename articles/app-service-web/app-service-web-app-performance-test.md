<properties
   pageTitle="Testování Azure webovou aplikaci výkonu | Microsoft Azure"
   description="Spusťte Azure webové aplikace výkonu testy ke kontrole zpracování zatížení uživatele aplikace. Měření doby odezvy a najděte chyby, ke kterým může označovat problémy."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Výkon otestujte Azure webovou aplikaci zatížení

Zaškrtněte webovou aplikaci výkon než ji spustit a nasazení aktualizace výroby. Tímto způsobem můžete lépe vyhodnotit zda je připraven verzi aplikace. Jistotu, že víc, můžete aplikaci při používání ve špičce, nebo na další marketingové nabízených zpracování přenosů.

Během veřejných zobrazení náhledu můžete test výkonu aplikace zdarma na portálu Azure.
Tyto testy simulovat uživatele zatížení aplikace v určitém časovém období a změřte vaše aplikace odpověď. Třeba výsledky testů zobrazit, jak rychle aplikace odpovídá určitý počet uživatelů. Také znázorňují, kolik požadavků selže, které může označovat problémy s aplikací.      

![Zjišťování problémů podle výkonu ve web appu](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Než začnete

* Pokud už nemáte budete potřebovat [Azure předplatného](https://account.windowsazure.com/subscriptions). Zjistěte, jak můžete [Otevřít účet Azure zdarma](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Musíte mít účet [Visual Studio týmovou](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) zachovat historie test výkonu. Vhodné účtu se vytvoří automaticky při nastavení test výkonu. Nebo můžete vytvořit nový účet nebo použít existující účet, pokud jste vlastníkem účtu. 

* Nasaďte aplikaci pro testování v testovacím prostředí. Máte aplikaci použijte některý z plánů aplikaci služby než použitého ve výrobním plánu. Tímto způsobem není ovlivnit stávajících zákazníků nebo zpomalit vaši aplikaci distribuovali výrobní. 

## <a name="set-up-and-run-your-performance-test"></a>Nastavení a spusťte test výkonu

0.  Přihlaste se k [portálu Azure](https://portal.azure.com). Postup použití účtu Visual Studio týmovou, kterou vlastníte, přihlaste se jako vlastník účtu.

0.  Přejděte na web appu.

    ![Přejděte na Procházet vše, Web Apps webovou aplikaci](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Přejděte na **Test výkonu**.

    ![Přejděte na nástroje, zkouška](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Teď budete propojit účet [Visual Studio týmovou](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) zachovat historie test výkonu.

    Pokud máte účet týmovou používat, vyberte tento účet. Pokud nemáte, vytvořte nový účet.

    ![Vyberte stávající týmovou účet nebo vytvořte nový účet](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Vytvořte zkušební výkonu. Nastavte podrobnosti a spusťte test. 

V průběhu test můžete sledovat výsledky v reálném čase.

Předpokládejme například, že máme aplikace, která zobrazila se kupóny při prodeji svátků loňského roku. Tato událost činí 15 minut se zatížením Špička 100 souběžné zákazníků. Chceme poklikáním počet zákazníků, tento rok. Chceme zlepšit spokojenost zákazníků tím, že zkrácení doby načítání stránek z 5 sekund na 2 sekund. Ano budete testování výkonu naše aktualizované aplikace s 250 uživatelů 15 minut.

Budeme se generování virtuální uživatelé (Customers zákazníci) simulovat zatížení na naše aplikace kdo webu naše ve stejnou dobu. Zobrazí nám nedaří nebo reagovat pomalu žádosti o kolik.

  ![Vytvoření, nastavení a spusťte test výkonu](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Výchozí web appu adresa URL se automaticky přidá. 
   Můžete změnit adresu URL otestovat další stránky (požadavků HTTP GET pouze).

   *  Simulovat místní podmínky a zmenšení latence, vyberte umístění nejblíže k vaši uživatelé pro generování načíst.

  Tady je test probíhá. Během prvního minuty načtení pomaleji, než si naše stránky.

  ![Zkouška probíhá data v reálném čase](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Po dokončení testu jsme informace rychleji načtení stránky za první minuty. Díky identifikaci, pokud nám chcete začít řešení potíží.

  ![Dokončené zkouška zobrazuje výsledky, včetně se nepodařilo požadavky](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testování více adres URL

Můžete taky spustit testy výkonu zahrnující více adresy URL, které představují scénář – koncový uživatel uložením souboru Visual Studio Web Test. Některé způsoby můžete vytvořit Visual Test Web Studio soubor:

* [Zachyťte přenosu pomocí Fiddler a export do souboru Visual Studio Web Test](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Vytvoření souboru test zatížení ve Visual Studiu](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Nahrání a spusťte soubor Visual Studio Web Test:
 
0. Pomocí výše uvedených kroků otevřete zásuvné **otestujte nový výkonu** .
   V tomto zásuvné Odsouhlasíte OTESTUJTE pomocí CONFIGFURE otevřete zásuvné **Konfigurovat testovací pomocí** .  

    ![Otevření testování zásuvné konfigurovat zatížení](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Zkontrolujte, jestli testovací typ je nastavený na **Visual Studio Web Test** a vyberte soubor protokolu HTTP archivu.
    Použití ikony "složky" Chcete-li otevřít dialogové okno Výběr souboru.

    ![Nahrávání souboru více URL Visual Studio Web Test](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Po nahrání souboru se zobrazí seznam adres URL otestuje v části Adresa URL podrobnosti.
 
0. Zadejte načtení uživatele a otestujte dobu trvání a pak vyberte **Spustit test**.

    ![Výběrem uživatele načítání a doba trvání](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Po dokončení testu, uvidíte výsledky ve dvou podoken. V levém podokně informacemi performnace jako řady graf s dílčími pruhy.

    ![Podokno výsledků výkonu](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    V pravém podokně zobrazí se seznam Nezdařené požadavky, s typu chyby a toho, kolikrát se výskytu.

    ![V podokně selhání požadavek](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Spusťte test výběrem ikony **znovu spustit** nahoře v podokně vpravo.

    ![Opětovného spuštění test](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Služba Q & A

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>Otázka: Existuje limit na jak dlouho mám spusťte test? 

**A**: Ano, můžete v portálu Azure spustit test až jednu hodinu.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>Otázka: jak dlouho se dostanu ke spuštění testy výkonu? 

**A**: po veřejné preview dostanete 20 000 virtuální uživatele minuty (VUMs) bezplatné každý měsíc pomocí svého účtu aplikace Visual Studio týmovou. VUM je počet uživatelů, kteří virtuální vynásobena počet minut podmínku. Pokud vašim potřebám přesáhne limit bezplatného, můžete zakoupit delší dobu a platí pouze pro můžete použít.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>Otázka: kde můžete zkontrolovat kolik VUMs můžu vypotřebovali zatím?

**A**: tuto hodnotu na portálu Azure můžete zkontrolovat.

![Přejděte ke svému účtu týmovou](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Kontrola VUMs používá](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>Otázka: Jaký je výchozí možnost a jsou ovlivněny svůj stávající testů?

**A**: výchozí možnost, aby výkon načítání testů je ruční test - stejně jako před více URL otestujte možnost byl přidán k portálu.
Existující testů dál používat nakonfigurované adresy URL a budou funkční jako předtím.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>Otázka: co funkce nejsou podporované ve Visual Studio Web testovací soubor?

**A**: V současné době tuto funkci nelze použít zadanou Web Test moduly plug-in zdroje dat a extrahování pravidla. Je třeba upravit svůj Web testovací soubor na tyto prvky odeberte. Jsme ať přidat podporu pro tyto funkce v budoucích aktualizacích.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>Otázka: podporuje všechny další formáty souborů Web Test?
  
**A**: při prezentování pouze Visual Studio Web Test jsou podporované soubory ve formátu.
Jsme by potěšením od vás něco dozvědět v případě potřeby podpora pro další formáty souborů. E-mailové adrese [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>Otázka: Co dalšího můžete dělat s účtem Visual Studio týmovou?

**A**: najít vašemu novému účtu, přejděte na ```https://{accountname}.visualstudio.com```. Sdílení kódu, vytvořit, otestovat, sledují práci a příjemce software – vše v cloudu pomocí nástroje nebo jazyk. Další informace o tom, jak funkce [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) a služby pomoct váš tým snadněji spolupracovat a nasazení nepřetržitě.

## <a name="see-also"></a>Viz taky

* [Spuštění testů výkonu jednoduché cloudu](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Spuštění testů Apache Jmeter výkonu](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Nahrání a přehrání testů cloudové zatížení](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Výkon testování aplikace v cloudu](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
