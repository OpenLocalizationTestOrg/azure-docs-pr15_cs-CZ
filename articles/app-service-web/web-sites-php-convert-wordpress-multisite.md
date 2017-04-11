<properties 
    pageTitle="Převedení WordPress více pracovišť služby Azure aplikace" 
    description="Zjistěte, jak provádět existující WordPress webovou aplikaci vytvořený prostřednictvím Galerie v Azure a převést na více WordPress pracovišť" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Převedení WordPress více pracovišť služby Azure aplikace

## <a name="overview"></a>Základní informace

*Podle [Robert Lobaugh][ben-lobaugh], [Inc. technologií pro server Microsoft otevřít][ms-open-tech]*

V tomto kurzu se informace o trvat existující WordPress webovou aplikaci vytvořený prostřednictvím Galerie v Azure a převést na více WordPress pracovišť nainstalovat. Kromě toho se dozvíte, jak přiřazovat vlastní doménu pro každý podřízené weby v rámci vaší instalace.

Předpokládá se, jestli máte existující instalace WordPress. Pokud nemáte, postupujte podle pokynů v tématu [Vytvoření webu WordPress z Galerie v Azure][website-from-gallery].

Převod existujícího WordPress jednoho webu nainstalovat do více pracovišť je obvykle velmi jednoduché a spoustu tímto postupem počáteční pocházet přímo [Vytvořit síť] [ wordpress-codex-create-a-network] stránky na [WordPress Codex](http://codex.wordpress.org).

Pusťme se do práce.

## <a name="allow-multisite"></a>Povolit více pracovišť.

Nejdřív budete muset povolit více pracovišť prostřednictvím `wp-config.php` soubor s **WP\_povolit\_více PRACOVIŠŤ** konstanty. Upravovat soubory aplikace webu dvěma způsoby: první z nich je prostřednictvím FTP a druhý prostřednictvím libovolná. Pokud neznáte jak nastavit některý z těchto postupů, získáte následující kurzy:

* [PHP webový server MySQL a FTP][website-w-mysql-and-ftp-ftp-setup]

* [PHP webový server MySQL a libovolná][website-w-mysql-and-git-git-setup]

Otevřít `wp-config.php` soubor v editoru e a přidejte následující výše uvedených `/* That's all, stop editing! Happy blogging. */` řádku.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Nezapomeňte uložit soubor a uložit na server.

## <a name="network-setup"></a>Nastavení sítě

Přihlaste se v oblasti *wp správce* vaší webové aplikace a byste měli vidět nové položky v nabídce **Nástroje** s názvem **Nastavení sítě**. Klikněte na **Nastavení sítě** a zadejte údaje vaší síti.

![Obrazovka nastavení sítě][wordpress-network-setup]

Tento kurz používá schéma webu *podadresáře* proto, že by měla proběhnout a jsme budou nastavovat vlastní domény pro každý podřízený web dál v tomto kurzu. Však by měl být možné instalační program subdoménu nainstalovat pokud správně mapování doménu pomocí zástupných [Azure portálu](https://portal.azure.com) a nastavení DNS.

Další informace o dílčí domény podadresáře nastavení najdete v tématu [typy sítě s více servery] [ wordpress-codex-types-of-networks] článek o WordPress Codex.

## <a name="enable-the-network"></a>Povolení sítě

Sítě je teď nakonfigurované v databázi, ale jeden zbývající nikam povolit funkci sítě. Dokončení `wp-config.php` nastavení a zrušte zaškrtnutí `web.config` správně přesměrovává každého webu.


Po kliknutí na tlačítko **instalovat** na stránce *Nastavení sítě* , WordPress pokusí aktualizovat `wp-config.php` a `web.config` soubory. Měli byste vždy zkontrolovat soubory, které chcete zajistit, aby že byla aktualizace úspěšná. V opačném případě této obrazovce můžete potřebné aktualizacích prezentovat. Úpravy a ukládání souborů.


Po provedení těchto aktualizace, které budou muset odhlásit a znovu přihlásit, zpátky na řídicím panelu správce webové části.

Na stránce Správce panel s popisky **Osobních webů**by měl být teď další nabídku. V této nabídce umožňuje řídit novou síť prostřednictvím řídicím panelu **Správce sítě** .

## <a name="adding-custom-domains"></a>Přidání vlastní domény

[WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] modul plug-in díky kterému je uloženy přidání vlastní domény, které na jakémkoliv webu v síti. Aby modul plug-in fungoval správně musíte udělat některé další nastavení, na portálu i u svého doménového registrátora.

## <a name="enable-domain-mapping-to-the-web-app"></a>Povolit mapování domény na web appu

Režim plánu [Aplikace služby](http://go.microsoft.com/fwlink/?LinkId=529714) **Free** nepodporuje přidání vlastní domény, které do webových aplikací Web Apps. Potřebujete přepnout na **sdílené** nebo **standardním** režimu. Akce:

* Přihlaste se k portálu Azure a vyhledejte webovou aplikaci. 
* Klikněte na kartu **Měřítko** v **dialogovém okně Nastavení**.
* V části **Obecné**vyberte *SDÍLENÉ* nebo *Standardní*
* Klikněte na tlačítko **Uložit**

Zobrazí zprávu s žádostí o změnu ověřit a potvrdili, že váš web appu mohou být teď účtovány nákladů, v závislosti na použití a konfiguraci, kterou jste nastavili.

Trvá několik sekund, než zpracuje nové nastavení, tedy je dostatečném předstihu začít s nastavením domény.

## <a name="verify-your-domain"></a>Ověření vaší domény

Před Azure Web Apps vám umožní mapování domény na webu, musíte nejdřív zkontrolujte, jestli máte povolení namapovat domény. K tomu, musíte přidat nový CNAME záznam s položkou DNS.

* Přihlaste se k Správce DNS vaší domény
* Vytvořte nový CNAME *awverify*
* Přejděte na *awverify* *awverify. YOUR_DOMAIN.azurewebsites.NET*

Může trvat delší dobu změn DNS přejděte do plného, takže pokud podle těchto kroků okamžitě nefungují přejděte volání Šálek kávy, pak vrátit a zkuste to znovu.

## <a name="add-the-domain-to-the-web-app"></a>Přidání domény do web appu

Vraťte se do webové aplikace pomocí portálu Azure, klikněte na **Nastavení**a potom klikněte na **vlastní domény a SSL**.

Když *Nastavení SSL* se zobrazí, zobrazí se pole kde zajistíte všechny domény, které chcete přiřadit do webové aplikace. Pokud tady není uvedené doménu, nebude k dispozici pro mapování uvnitř WordPress, bez ohledu na to, jak je doména DNS nastavení.

![Správa dialogové okno vlastní domény][wordpress-manage-domains]

Po zadání svoji doménu do textového pole, bude Azure zkontrolujte záznam CNAME, kterou jste dříve vytvořili. Pokud DNS není plně rozšíří, se zobrazí červený indikátor. Jestliže byl úspěšný, zobrazí se zeleným zaškrtnutím. 

Tady je pár poznámek na IP adresu uvedenou v dolní části dialogového okna. Budete potřebovat, nastavit záznam pro vaši doménu.

## <a name="setup-the-domain-a-record"></a>Nastavení domény záznamu

Pokud ostatní kroky bylo úspěšné, může teď přiřadíte doménu Azure webovou aplikaci pomocí DNS záznam. 

Je důležité mít na paměti tady, že Azure webové aplikace přijmout CNAME a záznamy, ale *musí* pomocí záznamu a povolit mapování správné doméně. Záznam CNAME nejde předávají další CNAME, tedy co Azure vytvoří s YOUR_DOMAIN.azurewebsites.net.

Pomocí IP adresy v předchozím kroku, vraťte se do Správce DNS a nastavit záznam tak, aby ukazovaly na tuto IP.


## <a name="install-and-setup-the-plugin"></a>Instalace a nastavení modulu plug-in

Více WordPress pracovišť momentálně není k dispozici metodu předdefinované mapování vlastních domén. Je však modulu plug-in s názvem [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] , které za vás přidává funkci. Přihlaste se k části správce sítě vašeho webu a nainstalujte modul plug-in **WordPress MU domény mapování** .

Po instalaci a aktivaci modul plug-in, navštěvujte blog o **Nastavení** > **Mapování doménu** nakonfigurovat modul plug-in. První textového pole *IP adresa serveru*, zadejte IP adresu jste použili k nastavení záznamu A pro tuto doménu. Nastavte všechny *Možnosti domény* vyžadujete (výchozí nastavení, jsou často pořádku) a klikněte na **Uložit**.

## <a name="map-the-domain"></a>Mapa domény

Navštivte **řídicí panel** pro web, který chcete namapovat domény, kterou chcete. Klikněte na **Nástroje** > **Mapování domény** a do textového pole zadejte novou doménu a klikněte na **Přidat**.

Ve výchozím nastavení bude nutné seznamu nová doména přepsat vytvořeno automaticky domény webů. Pokud chcete mít všechny přenosy odeslané do seznamu nová doména, zaškrtněte políčko *pro tento blogu primární doménu* před uložením. Neomezený počet domény můžete přidat na web, ale může obsahovat jen jedno primární.

## <a name="do-it-again"></a>Provést akci

Azure Web Apps vám umožňují přidat neomezený počet domény pro web app. Chcete-li přidat další domény je nutné provést částech **tématu ověření domény** a **Nastavení domény záznam** pro každou doménu.  

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
