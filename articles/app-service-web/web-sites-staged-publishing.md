<properties
    pageTitle="Nastavení pracovní prostředí pro web apps v aplikaci služby Azure"
    description="Naučte se používat fázované publikování pro web apps v aplikaci služby Azure."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Nastavení pracovní prostředí pro web apps v aplikaci služby Azure
<a name="Overview"></a>

Při nasazení webovou aplikaci [Aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=529714)nástroje můžete nasazovat na samostatném nasazení pozici místo produkční úsek výchozí při spuštění v režimu plán **Standardní** nebo **Premium** aplikaci služby. Nasazení sloty jsou skutečně živou web apps s vlastní názvy hostitelů. Web app obsah a konfigurace prvky můžete vyměnit mezi dvěma sloty nasazení, včetně úsek výroby. Nasazení aplikace pro nasazení úsek má následující výhody:

- Ověřit web app změnami pracovní pozici nasazení před výměna s úsek výroby.

- S nasazováním ve web appu pozici první a výměna do provozu zaručuje, že všechny výskyty úsek jsou provozní teplotu před vyměnit do provozu. Díky prostoj při nasazení webovou aplikaci. Přesměrování přenosy je bezproblémové a důsledku vyměňovat operace se nezobrazí žádné požadavky. Celý pracovní postup můžete automatické pomocí konfigurace [Zaměnit automaticky](#configure-auto-swap-for-your-web-app) při ověření před vyměňovat není potřeba.

- Po odkládací úsek dříve fázované webovou aplikaci teď bude předchozí web appu výroby. Pokud jsou tyto změny vyměnit do úsek výrobní není podle očekávání, můžete provést stejné vyměňovat okamžitě zobrazíte "poslední známé dobré webu" zpět.

Každý plán režim aplikaci služby podporuje rozdílný počet sloty nasazení. Pokud chcete zjistit počet paticím webovou aplikaci režimu podporuje najdete v článku [Aplikace služby ceny](/pricing/details/app-service/).

- Po webovou aplikaci více sloty, není možné změnit režimu.

- Změna měřítka není k dispozici pro testovacím sloty.

- Správa propojených zdrojů není při testovacím sloty podporovaná. [Portál Azure](http://go.microsoft.com/fwlink/?LinkId=529715) pouze se můžete vyhnout tento potenciální dopad na výrobní úsek dočasně přesunutím úsek testovacím na jiný plán režim aplikaci služby. Všimněte si, že úsek testovacím ještě jednou sdílejí stejný režim se úsek výrobní před zaměnit dva sloty.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Přidání úsek nasazení do web appu ##

V režimu **Standardní** nebo **Premium** , aby o povolení více sloty nasazení musí běžet web appu.

1. Na [Portálu Azure](https://portal.azure.com/)otevřete webovou aplikaci zásuvné.
2. Klikněte na **Nastavení**a potom klikněte na **nasazení sloty**. **Nasazení sloty** zásuvné kliknutím na **Přidat úsek**.

    ![Přidání nového úsek nasazení][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Pokud web appu dosud v režimu **Standardní** nebo **Premium** , zobrazí se zpráva s oznámením podporované režimy povolení fázované publikování. Nyní máte možnost vyberte, **Upgrade** a přejděte na kartu **Měřítko** svojí webové aplikace před pokračováním.

2. V zásuvné **Přidat pozici** pojmenujte úsek a vyberte, zda chcete klonovat konfigurace webové aplikace z jiné existující úsek nasazení. Klikněte na značku zaškrtnutí pokračovat.

    ![Konfigurace zdroje][ConfigurationSource1]

    První přidáte pozici, pouze máte dvě možnosti: Konfigurace klonovat z výchozí úsek výrobní nebo vůbec.

    Po vytvoření několik sloty, budete moct klonovat konfigurace z úsek než ve výrobním:

    ![Konfigurace zdrojů][MultipleConfigurationSources]

5. V zásuvné **sloty nasazení** klikněte na úsek nasazení se sadu metriky a konfigurace stejně jako jakékoli jiné v prohlížeči otevřete zásuvné pro úsek. **your-Web-App-Name-Deployment-slot-Name** se zobrazí v horní části zásuvné s vysvětlením, že se nacházíte úsek nasazení.

    ![Název úsek nasazení][StagingTitle]

5. Klikněte na adresu URL aplikace v úsek zásuvné. Všimněte si úsek nasazení má vlastní hostname a také živou aplikace. Pokud chcete omezit přístup veřejné na pozici nasazení, najdete v článku [Aplikace služby Web App – blok web access sloty testovacím nasazení](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Po vytvoření úsek nasazení není žádný obsah. Můžete nasadit na pozici z jiné úložiště pobočky nebo úplně odebrat jiné úložiště. Můžete taky změnit úsek konfigurace. Použití Publikovat profil nebo nasazení jména a hesla k pozici nasazení obsahu aktualizace.  Můžete například [Publikovat na tento úsek s libovolná](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Konfigurace patice pro nasazení ##
Když klonovat konfigurace z jiného úsek nasazení klonovaný konfigurace je možné upravit. Navíc některé prvky konfigurace následovat obsah přes vyměňovat (ne úsek konkrétní) a další prvky konfigurace ve stejné úsek zůstanou po vyměňovat (úsek konkrétní). Následující seznamy zobrazit konfiguraci, která se změní, když zaměnit sloty.

**Nastavení, které jsou vyměnit**:

- Obecné nastavení – například framework verze, 32bitovou nebo 64bitovou verzi, Web sockets
- Nastavení aplikace (lze nakonfigurovat tak, aby se na pozici)
- Připojovací řetězec (lze nakonfigurovat tak, aby se na pozici)
- Obslužná rutina mapování
- Nastavení monitorování a diagnostiky
- WebJobs obsahu

**Nastavení, které nejsou vyměnit**:

- Publikování koncové body
- Vlastní názvy domén
- Certifikáty SSL a vazby
- Nastavení
- WebJobs plánovače

Abyste mohli nakonfigurovat aplikaci nastavení nebo připojení řetězci chcete, aby na pozici (ne vyměnit), přístup k **Nastavení aplikace** zásuvné pro konkrétní úsek a potom vyberte pole **Úsek nastavení** pro konfiguraci prvky, které by měl aby úsek. Všimněte si, že označení prvek konfigurace jako úsek konkrétní nemá vliv zřízení daný prvek jako není běhu přes všechny sloty nasazení přidružený k web appu.

![Nastavení úsek][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Chcete-li zaměnit sloty nasazení ##

>[AZURE.IMPORTANT] Před zaměnit do webových aplikací z úsek nasazení do provozu zkontrolujte, že všechny není úsek konkrétní nakonfigurováno přesně tak, jak chcete mít v cílové vyměňovat.

1. Záměna sloty nasazení, kliknutím na tlačítko **vyměňovat** na panelu příkazů ve web appu nebo na panelu příkazů úsek nasazení. Ujistěte se, že je správně nastavená vyměňovat zdrojové a cílové vyměňovat. Cíl vyměňovat obvykle, bude úsek výroby.  

    ![Tlačítko Zaměnit][SwapButtonBar]

3. Klikněte na **OK** dokončete operaci. Po dokončení operace mít byla vyměnit sloty nasazení.

## <a name="configure-auto-swap-for-your-web-app"></a>Nastavení automatického zaměnit pro webovou aplikaci ##

Odkládací automatického zjednodušuje DevOps scénáře místo, kam chcete nepřetržitě Nasaďte webovou aplikaci s nulovou studený start a nulové prostoje koncovým zákazníkům web appu. Při nasazení úsek nakonfigurovaný pro automatické vyměňovat do provozu, pokaždé, když nabízená aktualizace kódu na tuto pozici, aplikaci služby automaticky zaměnit web appu do provozu, když už má předehřívá v úsek.

>[AZURE.IMPORTANT] Pokud zapnete automatické zaměnit pro pozici, zkontrolujte, že konfigurace úsek je přesně konfigurace určené pro cílové úsek (obvykle úsek výrobní).

Konfigurace automatického zaměnit pro pozici je snadné. Postupujte následujícím způsobem:

1. V zásuvné **Nasazení sloty** vyberte testovacím úsek, klikněte na **Všechna nastavení** zásuvné tuto pozici.  

    ![][Autoswap1]

2. Klikněte na **Nastavení aplikace**. **Vyberte pro **Automatické zaměnit**** , vyberte požadovaný cílový úsek v **Automatického zaměnit úsek**a klepněte na tlačítko **Uložit** na panelu příkazů. Zkontrolujte, že konfigurace pro úsek je přesně konfigurace určené pro cílové úsek.

    Karta **oznámení** bude flash zelené **Úspěch** po dokončení operace.

    ![][Autoswap2]

    >[AZURE.NOTE] Testování automatického zaměnit pro webovou aplikaci, vyberte nejprve testovacím cílové úsek **Automatického zaměnit úsek** se seznamte s funkcí.  

3. Spusťte kód nabízených na tuto pozici nasazení. Odkládací automatického se stane po chvíli a aktualizace se projeví na úsek cílové adrese URL.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Použití více fází vyměňovat pro webovou aplikaci ##

Odkládací více fází neexistuje zjednodušit ověření v kontextu prvků konfigurace navržený chcete, aby na úsek třeba připojovací řetězec. V těchto případech je vhodné použít tyto prvky konfigurace z cílové vyměňovat ke zdroji vyměňovat a ověřit před vyměňovat skutečně projeví. Jakmile vyměňovat cílové konfigurace prvky zaevidují do odkládací zdroje dostupné akce jsou dokončení odkládací nebo návrat do původního umístění pro tento zdroj vyměňovat, který taky má za následek zrušení odkládací. Vzorky pro rutiny prostředí PowerShell Azure umožňující více fází vyměňovat jsou součástí rutiny prostředí PowerShell Azure pro nasazení sloty oddíl.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Při vracení výrobní aplikace po vyměňovat ##

Pokud jsou všechny chyby ve výrobním po vyměňovat úsek, vrátit sloty obnovit stav před vyměňovat výměna stejné dva sloty okamžitě.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Vlastní ohřátí před vyměňovat ##

Některé aplikace mohou vyžadují vlastní ohřátí akce. Konfigurace element applicationInitialization v web.config umožňuje určit vlastní inicializační akce, které se mají provést před přijetím žádost o. Operace vyměňovat počká pro tento vlastní ohřátí dokončete. Tady je web.config fragment výběru.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Chcete-li odstranit úsek nasazení##

V zásuvné pro nasazení úsek klikněte na panelu příkazů **Odstranit** .  

![Odstranění úsek nasazení][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure rutiny prostředí PowerShell patice pro nasazení

Azure Powershellu je moduly, které poskytuje rutiny pro správu Azure přes Windows PowerShell, včetně podpory pro správu webové aplikace nasazení slotů v aplikaci služby Azure.

- Informace o instalaci a konfigurace prostředí PowerShell Azure a v ověřování Azure PowerShell s předplatným Azure zjistěte, [Jak nainstalovat a konfigurace služeb Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Vytvoření webové aplikace

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Vytvoření úsek nasazení pro web app

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Zahájení více fází vyměňovat a jeho aplikování cílové úsek konfigurace úsek zdroje

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Vrátí první fázi vyměňovat více fáze a obnovit konfigurace úsek zdroje

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Záměna sloty nasazení

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Odstranění úsek nasazení

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku) příkazy pro nasazení

Azure rozhraní příkazového řádku obsahuje různé platformy příkazy pro práci s Azure, včetně podpory pro správu sloty nasazení Web Appu.

- Pokyny k instalaci a konfiguraci Azure rozhraní příkazového řádku, včetně informací o tom, jak připojit rozhraní příkazového řádku Azure k předplatnému Azure najdete v článku [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md).

-  Chcete-li seznam dostupných příkazů pro aplikaci služby Azure v Azure rozhraní příkazového řádku, zavolejte `azure site -h`.

----------
### <a name="azure-site-list"></a>seznam Azure webů
Informace o webových aplikacích aktuálního předplatného zavolejte na **seznam azure webů**jako v následujícím příkladu.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Vytvoření webu Azure
Pokud chcete vytvořit úsek nasazení, volání **azure webu vytvořit** a zadejte název existující web appu a název úsek vytvořit jako v následujícím příkladu.

`azure site create webappslotstest --slot staging`

Povolení ovládacího prvku zdroje pro nové úsek, pomocí možnosti **– Libovolná** jako v následujícím příkladu.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>odkládací Azure webu
Chcete-li aktualizované nasazení pozici aplikaci výrobní, použijte příkaz **Zaměnit azure webu** provádět operace vyměňovat jako v následujícím příkladu. Aplikaci výrobní nebude vyskytnout dolů čas ani podléhají studený start.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>odstranění webu Azure
Pokud chcete odstranit úsek nasazení, který již není potřeba, pomocí příkazu **azure webu odstranit** jako v následujícím příkladu.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="next-steps"></a>Další kroky ##
[Azure aplikaci služby Web App – web zablokování přístupu k sloty testovacím nasazení](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Bezplatná zkušební verze Microsoft Azure](/pricing/free-trial/)

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
