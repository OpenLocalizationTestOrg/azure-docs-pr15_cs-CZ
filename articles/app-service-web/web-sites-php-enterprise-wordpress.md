<properties
    pageTitle="Podnikových WordPress na Azure aplikaci služby | Microsoft Azure"
    description="Naučte se hostitelem webu WordPress podnikových aplikací služby Azure"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Podnikových WordPress Azure aplikace služby

Azure aplikaci služby nabízí prostředí scalable zabezpečené a snadno se použije pro velice důležité, velké měřítko [WordPress] [ wordpress] weby. Microsoft sám spouští podnikových webů například [Office] [ officeblog] a [Bing] [ bingblog] blogů. Tento dokument se dozvíte, jak můžete Azure aplikace služby Web Apps k vytvoření a správě rozsáhlých, cloudové WordPress web, který lze zpracovat velké množství návštěvníci.

## <a name="architecture-and-planning"></a>Architektura poznámek a plánování

Základní instalace WordPress obsahuje pouze dva požadavky.

* **Databáze MySQL** - dostupné prostřednictvím [ClearDB v Azure Marketplace][cdbnstore], nebo můžete spravovat vlastní instalace MySQL na virtuálních počítačích Azure pomocí některého z [Windows] [ mysqlwindows] nebo [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB obsahuje několik konfigurací MySQL s vlastnostmi různých výkonu pro každou konfiguraci. V tématu [Azure úložiště] [ cdbnstore] informace o nabídkách k dispozici prostřednictvím úložišti Azure nebo přímo, jak je vidět na [ClearDB webu](http://www.cleardb.com/pricing.view).

* **PHP 5.2.4 nebo vyšší** -aplikaci služby Azure aktuálně poskytují [PHP verze 5.4, 5,5 a 5.6][phpwebsite].

    > [AZURE.NOTE] Doporučujeme spouštět vždycky nejnovější verzi PHP ověřit, jestli že máte nejnovější bezpečnostní opravy.

### <a name="basic-deployment"></a>Základní informace o nasazení

Použití pouze základní požadavky, můžete vytvořit základní řešení Azure oblasti.

![Azure web app a databáze MySQL hostované v jedné oblasti Azure][basic-diagram]

Když budete moci škálování aplikace vytvořením několika instancích Web Apps webu, všechno, co je umístěn na datacentrech v konkrétní zeměpisnou oblast. Návštěvníci z mimo této oblasti možná uvidíte pomalé odezvy při použití na web, a jestli datacentrech v této oblasti přejděte dolů, to znamená aplikace.


### <a name="multi-region-deployment"></a>Nasazení více oblastí

V Azure [Správce dopravy][trafficmanager], je možné zobrazit webu WordPress napříč několika zeměpisné oblastí zároveň pouze jednu adresu URL pro návštěvníky. Všechny návštěvníci přichází pomocí Správce přenosy a nedefinujete do oblasti podle konfigurace vyrovnávání zatížení.

![Azure webovou aplikaci, hostované ve více oblastech pomocí CDBR dostupnost směrovači směrovat MySQL různých oblastí][multi-region-diagram]

V každé oblasti webu WordPress by pořád zachován napříč několika instancích spuštěných Web Apps, ale tato změna velikosti je oblast konkrétní; vysoký provoz oblastí je možné měřítko jinak než zhoršeným přenosy z nich.

Replikace a směrování k více databázím MySQL lze provést pomocí společnosti ClearDB [CDBR vysoké dostupnost směrovači] [ cleardbscale] (viz vlevo) nebo [MySQL clusteru CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Nasazení více oblastech pomocí médií úložiště a ukládání do mezipaměti
Pokud webu přijme nahrávání a hostitele multimediální soubory, použijte úložiště objektů Blob Azure. Pokud potřebujete ukládání do mezipaměti, zvažte [Redis mezipaměti][rediscache], [Memcache cloudu](http://azure.microsoft.com/gallery/store/garantiadata/memcached/) [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)a jeden z jiných mezipaměti nabídkách v [Azure úložiště](http://azure.microsoft.com/gallery/store/).

![Azure webovou aplikaci, hostované ve více oblastech pomocí směrovači CDBR dostupnost pro MySQL s spravovaných mezipaměti, úložiště objektů Blob a CDN][performance-diagram]

Úložiště objektů blob je geo rozvržena oblastí ve výchozím nastavení, abyste pokaždé nemuseli starat o to replikace soubory na všech webech. Můžete taky povolit Azure [Network obsahu rozdělení (CDN)] [ cdn] k úložišti objektů Blob, které distribuuje soubory ukončit uzly blíž k návštěvníci.

### <a name="planning"></a>Plánování

#### <a name="additional-requirements"></a>Další požadavky

Můžete to udělat... | Pomocí tohoto příkazu...
------------------------|-----------
**Uložit nebo uložit velkých souborů** | [Modul plug-in WordPress pro používání úložiště objektů Blob][storageplugin]
**Odeslání e-mailu** | [SendGrid] [ storesendgrid] [WordPress modul plug-in pro používání SendGrid] a[sendgridplugin]
**Vlastní názvy domén** | [Konfigurace vaší vlastní doménou v aplikaci služby Azure][customdomain]
**PROTOKOL HTTPS** | [Povolení HTTPS pro web app v aplikaci služby Azure][httpscustomdomain]
**Ověření před výrobní** | [Nastavení pracovní prostředí pro web apps v aplikaci služby Azure][staging] <p>Přechod do webových aplikací z pracovní výroby taky slouží k přesunutí konfigurace WordPress. Ujistěte se, že jsou všechna nastavení aktualizuje tak, aby požadavky aplikace výrobní před změnou fázované aplikace do provozu.</p>
**Sledování a odstraňování potíží** | [Povolení protokolování diagnostiky pro web apps v aplikaci služby Azure] [ log] a [Sledování aplikací webové aplikace služby Azure][monitor]
**Nasazení webu** | [Nasazení aplikace webové aplikace služby Azure][deploy]

#### <a name="availability-and-disaster-recovery"></a>Obnovení dostupnost a havárie

Můžete to udělat... | Pomocí tohoto příkazu...
------------------------|-----------
**Načtení zůstatek weby** nebo **geo-distribuce weby** | [Směrování přenosu s Azure přenosy správce][trafficmanager]
**Zálohování a obnovení** | [Zálohování web app v aplikaci služby Azure] [ backup] a [obnovení do webových aplikací v aplikaci služby Azure][restore]

#### <a name="performance"></a>Výkon

Výkon v cloudu nedosáhnete především pomocí ukládání do mezipaměti a škálování; ale paměť, šířka pásma a další atributy webové aplikace hostingu považovat za.

Můžete to udělat... | Pomocí tohoto příkazu...
------------------------|-----------
**Porozumění možnostem instancí aplikace služby** | [Podrobnosti o cenách, včetně možností aplikaci služby úrovní][websitepricing]
**Prostředky mezipaměti** | [Redis mezipaměti][rediscache], [Memcache cloudu](/gallery/store/garantiadata/memcached/) [MemCachier](/gallery/store/memcachier/memcachier/)a jeden z dalších mezipaměti nabídky v [Azure úložiště](/gallery/store/)
**Změna velikosti aplikace** | [Změnit velikost do webových aplikací v aplikaci služby Azure] [ websitescale] a [Vysoký směrování dostupnost ClearDB][cleardbscale]. Pokud zvolíte možnost hostovat a správa instalací MySQL, měli byste zvážit [MySQL clusteru CGE] [ cge] pro škálování

#### <a name="migration"></a>Migrace

Migrace stávajícího webu WordPress aplikace služby Azure dvěma způsoby.

* ** [WordPress exportovat] [ export] ** – to exportuje obsah blogu, který lze importovat do nového webu WordPress v aplikaci služby Azure pomocí modulu [Plug-in pro import WordPress][import].

    > [AZURE.NOTE] Během tohoto procesu umožňuje migrace obsahu, nemigrují moduly plug-in pro všechny, motivy nebo další úpravy. Toto je třeba nainstalovat znovu ručně.

* **Ruční migrace** - [obecnějším údajům webu] [ wordpressbackup] a [databáze][wordpressdbbackup], pak ručně obnovíte do webových aplikací v aplikaci služby Azure a přidružené databáze MySQL migrace vysoce přizpůsobený weby a vyhnout se můžete ručně instalace moduly plug-in, motivy a další úpravy.

## <a name="step-by-step-instructions"></a>Podrobné pokyny

### <a name="create-a-wordpress-site"></a>Vytvoření webu WordPress

1. Použití [Webu Azure Marketplace] [ cdbnstore] k vytvoření databáze MySQL velikosti uvedené v části [Architektura poznámek a plánování](#planning) v oblasti, že bude hostovat svůj web.

2. Postupujte podle pokynů v tématu [Vytvoření WordPress web app v aplikaci služby Azure] [ createwordpress] při vytváření WordPress webové aplikace. Při vytváření web appu, vyberte možnost **použít existující databáze MySQL** a vyberte databázi vytvořili v kroku 1.

Při migraci stávajícího webu WordPress, projděte si téma [migrace stávajícího webu WordPress na Azure](#Migrate-an-existing-WordPress-site-to-Azure) po vytvoření nové webové aplikace.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migrace stávajícího webu WordPress Azure

Jak je uvedeno v části [Architektura poznámek a plánování](#planning) , migraci WordPress webu dvěma způsoby.

* **export a import** - webů bez rozsah vlastních úprav nebo místo, kam chcete přesunout obsah.

* **zálohování a obnovení** pro weby s velkým množstvím přizpůsobení místo, kam chcete přesunout všechny položky.

Použijte jeden z těchto částí migrace webu.

#### <a name="the-export-and-import-method"></a>Způsob exportu a importu

1. Použít [WordPress export] [ export] export existující web.

2. Vytvoření webové aplikace pomocí postupu v části [Vytvoření webu WordPress](#Create-a-new-WordPress-site) .

3. Přihlaste se k vašemu webu WordPress na Web Apps a klikněte na **moduly plug-in** -> **Přidat nový**. Vyhledejte a nainstalujte modul plug-in **WordPress import** .

4. Po instalaci modulu plug-in import, klikněte na **Nástroje** -> **importovat**a vyberte **WordPress** používat modul plug-in WordPress import.

5. Na stránce **Import WordPress** klikněte na **Zvolit soubor**. Přejděte na soubor WXR vyexportovaný z existující web WordPress a potom zvolte **Nahrát soubor a importovat**.

6. Klikněte na **Odeslat**. Zobrazí se výzva, že import byl úspěšný.

8. Po dokončení těchto kroků restartujte webu z jeho web app zásuvné [Azure portál][mgmtportal].

Po importu webu, budete muset provedení následujících kroků můžete povolit nastavení není obsažen importovaného souboru.

Pokud jste používali toto... | Udělejte toto …
------------------ | ----------
**Trvalých** | Na řídicím panelu WordPress nový web klikněte na **Nastavení** -> **trvalých** a pak aktualizace struktuře trvalých
**odkazy na obrázek a média** | O aktualizaci propojení do nového umístění pomocí [modul plug-in paroží modré aktualizovat URL][velvet], nástroji hledání a nahrazení nebo ručně v databázi
**Motivy** | Přejděte na **Vzhled** -> **motiv** a aktualizace motiv webu, v případě potřeby
**Nabídky** | Pokud motiv podporuje nabídek, odkazů na domovskou stránku pravděpodobně stále staré podadresáře vložené do nich. Přejděte na **Vzhled** -> **nabídky** a aktualizovat

#### <a name="the-backup-and-restore-method"></a>Metoda zálohování a obnovení

1. Zálohování existující WordPress webu pomocí informací v [WordPress zálohy][wordpressbackup].

2. Zálohování existující databáze podle pokynů na [Zálohování databáze][wordpressdbbackup].

3. Vytvoření databáze a obnovení zálohy.

    1. Koupit nové databáze z [Azure Marketplace][cdbnstore], nebo nastavení databáze MySQL ve [Windows] [ mysqlwindows] nebo [Linux] [ mysqllinux] OM.

    2. Pomocí klienta MySQL jako [MySQL Workbench][workbench], připojte k nové databáze a import WordPress databáze.

    3. Aktualizujte databáze, kterou chcete změnit položky doménu pro vaši novou doménu Azure aplikaci služby. Například mywordpress.azurewebsites.net. Použijte možnosti [pro hledání a nahrazení WordPress databází skriptu] [ searchandreplace] bezpečně změnit všechny instance.

4. Vytvoření webové aplikace na portálu Azure a publikovat WordPress zálohování.

    1. Vytvořit web appu [Azure portál] [ mgmtportal] s databází pomocí **Nový** -> **Web + Mobile** -> **Azure Marketplace** -> **Web Apps** -> **v prohlížeči + SQL** (nebo **v prohlížeči + MySQL**) -> **vytvořit**. Nastavení všech požadovaných vytvořit prázdnou webovou aplikaci.

    2. Do zálohování WordPress vyhledejte soubor **wp config.php** a otevřete ho v editoru. Informace o nové databáze MySQL nahraďte následující položky.

        * **DB_NAME** – uživatelské jméno databáze

        * **DB_USER** – uživatelské jméno použité pro přístup k databázi

        * **DB_PASSWORD** - uživatelského hesla

        Po změně tyto položky, uložte a zavřete soubor **wp config.php** .

    3. Použití [nasadit do webových aplikací v aplikaci služby Azure] [ deploy] informace, které umožňují metody nasazení chcete použít a nainstalujte ji zálohování WordPress do webové aplikace v aplikaci služby Azure.

5. Jakmile byly nasazeny WordPress webu, je třeba mít přístup k nový web (jako webovou aplikaci aplikaci služby) pomocí *. azurewebsite.net adresy URL pro daný web.

### <a name="configure-your-site"></a>Konfigurace vašeho webu

Po WordPress webu byla vytvořená nebo migrovaná, použijte tyto informace pro zvýšení výkonu nebo povolení další funkce.

Můžete to udělat... | Pomocí tohoto příkazu...
------------- | -----------
**Nastavení režimu plánu aplikace služby, velikost a povolit měřítka** | [Změnit velikost do webových aplikací v aplikaci služby Azure][websitescale]
**Povolit připojení trvalý databáze** | Ve výchozím nastavení WordPress nepoužívá trvalý databáze připojení, která tuto chybu může způsobovat připojení k databázi stanou omezena po více připojení. Povolit připojení persistent, nainstalujte (trvalá připojení adaptér modul plug-in) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Zvýšení výkonu** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Zakázání souborů cookie ARR</a> – dosáhnout zvýšení výkonu při spuštění WordPress v několika instancích spuštěných Web Apps</p></li><li><p>Povolte ukládání do mezipaměti. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis mezipaměti</a> (verze preview) můžete používat se <a href="https://wordpress.org/plugins/redis-object-cache/">Redis objekt mezipaměti WordPress modul plug-in</a>a použijte některou z dalších mezipaměti nabídky z <a href="/gallery/store/">Azure úložiště</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Jak vytvořit WordPress rychleji s Wincache</a> - Wincache aktivované ve výchozím nastavení pro Web Apps</p></li><li><p><a href="../web-sites-scale/">Měřítko web app v aplikaci služby Azure</a> a jejich použití <a href="http://www.cleardb.com/developers/cdbr/introduction">Směrování ClearDB vysoké dostupnost</a> nebo <a href="http://www.mysql.com/products/cluster/">CGE clusteru MySQL</a></p></li></ul>
**Určenou úložiště objektů BLOB** | <ol><li><p><a href="../storage-create-storage-account/">Vytvořte účet Azure úložiště</a></p></li><li><p>Zjistěte, jak na článek <a href="../cdn-how-to-use/">použití sítě rozdělení obsahu (CDN)</a> geo-distribuci dat uložených v objektů BLOB.</p></li><li><p>Nainstalujte a nakonfigurujte <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure úložiště u WordPress modulu plug-in</a>.</p><p>Podrobné nastavení a informace o konfiguraci pro modul plug-in najdete v tématu <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">Průvodce uživatelů</a>.</p> </li></ol>
**Povolit e-mailu** | Povolte <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> v úložišti Azure. Nainstalujte <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">modul plug-in SendGrid</a> WordPress.
**Konfigurace vaší vlastní doménou** | [Konfigurace vaší vlastní doménou v aplikaci služby Azure][customdomain]
**Povolení HTTPS pro vaší vlastní doménou** | [Povolení HTTPS pro web app v aplikaci služby Azure][httpscustomdomain]
**Načtení rozložení nebo geo-distribuce webu** | [Směrovat přenosy v síti s Azure přenosy správce][trafficmanager]. Pokud používáte vlastní doménu, přečtěte si téma [konfigurace vaší vlastní doménou v aplikaci služby Azure] [ customdomain] informace o použití správce dopravy s vlastní názvy domén
**Povolení automatického zálohování** | [Obecnějším údajům web app v aplikaci služby Azure][backup]
**Povolení protokolování diagnostiky** | [Povolení protokolování diagnostiky pro web apps v aplikaci služby Azure][log]

## <a name="next-steps"></a>Další kroky

* [Optimalizace WordPress](http://codex.wordpress.org/WordPress_Optimization)

* [Převedení WordPress více pracovišť Azure aplikace služby](web-sites-php-convert-wordpress-multisite.md)

* [Průvodce pro Azure ClearDB upgradu](http://www.cleardb.com/store/azure/upgrade)

* [Hostingu WordPress v podsložce webovou aplikaci v aplikaci služby Azure](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Podrobné pokyny: Vytvoření webu WordPress pomocí Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Hostovat blogu existující WordPress Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Povolení přidávání nějakých trvalých v WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Migrace a spuštění WordPress blog o aplikaci služby Azure](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Postup při spuštění WordPress pro aplikaci služby Azure zdarma](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress na Azure v dvou minut a méně](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Přesunutí blogu WordPress Azure – část 1: vytvoření blogu WordPress Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Přesunutí do blogu WordPress Azure – část 2: převod obsahu](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Přesunutí blogu WordPress Azure – část 3: nastavení vlastní domény](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Přesunutí blogu WordPress Azure – část 4: schůzek vás v podstatě trvalých a adresu URL revize pravidla](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Přesunutí blogu WordPress Azure – část 5: Přesunutí z podsložky do kořenového adresáře](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Jak nastavit WordPress web app v účtu Azure](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping nahoru WordPress na Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Tipy pro WordPress na Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
