<properties
    pageTitle="Správa Azure rozhraní API nejčastější dotazy týkající se | Microsoft Azure"
    description="Přečtěte si odpovědi na běžné otázky, vzorků a osvědčené postupy správy rozhraní API Azure."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Nejčastější dotazy týkající se správy Azure rozhraní API

Projděte si odpovědi na běžné otázky, vzorků a osvědčené postupy pro správu rozhraní API Azure.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

-   [Jak lze můžu zeptejte se manažeři Microsoft Azure rozhraní API?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Co znamená po funkci ve verzi preview?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Jak lze zabezpečené připojení mezi bránu pro správu rozhraní API a Moje služby back-end?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Jak zkopírovat Moje instanci služby správy rozhraní API nové instance?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Můžu spravovat Moje rozhraní API správy instance programově?](#can-i-manage-my-api-management-instance-programmatically)
-   [Jak přidat uživatele do skupiny správců](#how-do-i-add-a-user-to-the-administrators-group)
-   [Proč je zásad, které chcete přidat k dispozici v editoru zásad?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Používání správy verzí rozhraní API správy rozhraní API](#how-do-i-use-api-versioning-in-api-management)
-   [Jak nastavím více prostředí do jediného rozhraní API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Dá se správou rozhraní API pomocí SOAP?](#can-i-use-soap-with-api-management)
-   [Je konstanta rozhraní API správy brány IP adresu? Můžete se používá v pravidlech brány firewall?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Můžete nakonfigurovat server OAuth 2.0 se tak mohli ověřovat pomocí služby AD FS zabezpečení?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [V nasazení více zeměpisné lokality směrování metoda používá rozhraní API správy?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Můžete použít šablonu správce prostředků Azure pro vytvoření instance služby správy rozhraní API?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Dá se pomocí certifikátu podepsaného svým držitelem protokol SSL pro back-end?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Proč se při pokusu o klonovat úložišti libovolná získat neúspěšné ověření?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Funguje rozhraní API správy Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Můžu přejít služby správy rozhraní API z předplatných na druhý?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Jak lze můžu zeptejte se manažeři Microsoft Azure rozhraní API?

Můžete nás kontaktovat pomocí jedné z těchto možností:

-   Pošlete svoje otázky v naší [fórum MSDN pro rozhraní API správy](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Odeslání e-mailu pro <apimgmt@microsoft.com>.
-   Pošlete nám žádost o funkci ve [fóru Azure názory](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Co znamená po funkci ve verzi preview?

Pokud funkce je v náhledu, znamená to, že jsme jste aktivně hledání názory na práci funkci za vás. Funkce náhledu je funkčně dokončen, ale je možné, že můžete vytočit jazycích změnit na základě reakcí zákazníků. Doporučujeme, aby si závisí na funkci, která se v náhledu v provozním prostředí. Pokud máte nějaké názory na funkce, dejte nám prosím vědět až jednu z možností kontaktů v [jak můžete můžu zeptejte se manažeři Microsoft Azure rozhraní API?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Jak lze zabezpečené připojení mezi bránu pro správu rozhraní API a Moje služby back-end?

Máte několik možností, zabezpečené připojení mezi bránu pro správu rozhraní API a back-end služby. Můžeš:

-   Používejte základní ověřování HTTP. Další informace najdete v tématu [Konfigurace rozhraní API nastavení](api-management-howto-create-apis.md#configure-api-settings).
- Použijte vzájemné ověřování protokolem SSL způsobem popsaným v [zabezpečení služeb back-end pomocí ověřování certifikátu klienta v části Správa rozhraní API Azure](api-management-howto-mutual-certificates.md).
- Použití povolení IP programů na back-end služby. Pokud máte standardní nebo Premium instance rozhraní API správy osy, konstantní IP adresu brány. Můžete nastavit povolených pro tento IP adresu povolit. Na řídicím panelu na portálu Azure můžete získat IP adresu instance rozhraní API správy.
- Rozhraní API správy instance připojení k síti virtuální Azure. Další informace najdete v tématu [jak nastavení sítě VPN správy rozhraní API Azure](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Jak zkopírovat Moje instanci služby správy rozhraní API nové instance?

Pokud chcete zkopírovat instanci rozhraní API správy nové instance máte několik možností. Můžeš:

-   Pomocí zálohování a obnovení funkce správy rozhraní API. Další informace najdete v článku [jak implementovat havárie obnovení pomocí služby zálohování a obnovení správy rozhraní API Azure](api-management-howto-disaster-recovery-backup-restore.md).
-   Vytvoření vlastního zálohování a obnovení funkce pomocí [Rozhraní API správy REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Pomocí rozhraní REST API pro uložení a obnovení entity z instanci služby, které chcete.
-   Stáhněte si konfiguraci služby pomocí libovolná a pak ho nahrajte do nové instance. Další informace najdete v tématu [jak uložit a konfiguraci vašeho řízení rozhraní API služeb pomocí libovolná](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Můžu spravovat Moje rozhraní API správy instance programově?

Ano, můžete spravovat rozhraní API správy programově pomocí:

-   [Rozhraní API správy rozhraní REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Knihovna správy ApiManagement služby Microsoft Azure SDK](http://aka.ms/apimsdk).
-   Rutiny pro [nasazení služby](https://msdn.microsoft.com/library/mt619282.aspx) a [služby správy](https://msdn.microsoft.com/library/mt613507.aspx) Powershellu.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Jak přidat uživatele do skupiny správců

Tady je způsob přidání uživatele do skupiny Administrators:

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Přechod ke skupině zdroje, které obsahuje rozhraní API správy instance, kterou chcete aktualizovat.
3. V části Správa rozhraní API uživateli přiřadíte role **Přispěvatele rozhraní Api správy** .

Nově přidaný Přispěvatel teď můžete pomocí prostředí PowerShell Azure [rutiny](https://msdn.microsoft.com/library/mt613507.aspx). Tady je postup, jak se přihlásit jako správce:

1. Použití `Login-AzureRmAccount` rutina přihlásit.
2. Nastavení kontextu předplatné, které je služba pomocí `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Získat adresu URL jednoho přihlašování pomocí `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Použijte adresu URL pro přístup k portálu pro správu.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Proč je zásad, které chcete přidat k dispozici v editoru zásad?

Pokud aktivní nebo stínované v editoru zásad, ujistěte se, že jste v správný rozsah zásady se zobrazí zásad, které chcete přidat. Každý prohlášení o zásadách slouží k použití v určité obory a zásady oddíly. Zásady oddíly a obory pro zásady naleznete v části Použití zásad do [zásad správy rozhraní API](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Používání správy verzí rozhraní API správy rozhraní API

Máte můžete používat k těmhle rozhraní API správy rozhraní API několika způsoby:

-   V části Správa rozhraní API můžete nakonfigurovat rozhraní API pro různé verze. Máte například dva různé rozhraní API, MyAPIv1 a MyAPIv2. Vývojář můžete zvolit verze, kterou vývojář chce používat.
-   Můžete taky nakonfigurovat vaše rozhraní API s adresou URL služby, které neobsahuje segment verze, například https://my.api. Nakonfigurujte segment verze na každé operace [Přepisu URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) šablony. Například můžete mít operaci [šablony adresu URL](api-management-howto-add-operations.md#url-template) s názvem/Resource a [Revize URL](api-management-howto-add-operations.md#rewrite-url-template) šablony s názvem /v1/Resource. Můžete změnit hodnotu verze segment zvlášť pro každou akci.
-   Pokud chcete zachovat segment "Výchozí" verze do pole Adresa URL rozhraní API služeb u vybraných operací, nastavte zásady, které používá [back-end službu nastavit](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) zásady ke změně cestu back-end žádost.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Jak nastavím více prostředí do jediného rozhraní API?

K nastavení prostředí s více, například testovacím prostředí a provozním prostředí jediného rozhraní API, máte dvě možnosti. Můžeš:

-   Host (hostitel) různých rozhraní API ve stejném klientovi.
-   Hostují stejný rozhraní API v různých klientů.

### <a name="can-i-use-soap-with-api-management"></a>Dá se správou rozhraní API pomocí SOAP?

Podpora [předávací protokol SOAP](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) je teď k dispozici. Správci můžou importovat WSDL své služby SOAP a správa rozhraní API Azure vytvoří front-end SOAP. Karta Vývojář v portálu přečtěte následující dokumentaci pro test konzoly, zásady a technologie pro analýzu se svými služby SOAP.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Je konstanta rozhraní API správy brány IP adresu? Můžete se používá v pravidlech brány firewall?

Na úrovní Standard a Premium veřejnou IP adresu (VIP) rozhraní API správy klienta je statické dobu trvání klienta, s určitými výjimkami. Změny adresy IP v těchto případech:

-   Služba je odstranit a pak znovu vytvořit.
-   Předplatné služby je pozastavené (například nonpayment) a potom obnoveny.
-   Přidání nebo odebrání Azure virtuální sítě (virtuální sítě lze použít pouze ve vrstvě Premium).

Nasazení více oblastí místní adresa změní Pokud vacated a potom obnoveny oblasti (nasazení více oblastí lze použít pouze ve vrstvě Premium).

Klienti osy Premium nakonfigurované pro nasazení více oblastí, mají přiřazenou veřejnou IP adres jednotlivých oblastech.

Na stránce klienta na portálu Azure se můžete přepnout IP adresy (nebo adresy ve více oblastech nasazení).

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Můžete nakonfigurovat server OAuth 2.0 se tak mohli ověřovat pomocí služby AD FS zabezpečení?

Informace o konfiguraci serveru se tak mohli ověřovat OAuth 2.0 se zabezpečením Active Directory Federation Services (AD FS) najdete v tématu [Pomocí služby AD FS v části Správa rozhraní API](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>V nasazení více zeměpisné lokality směrování metoda používá rozhraní API správy?

Rozhraní API správy používá [výkonu metodu směrování přenosu](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) v nasazení více zeměpisné lokality. Příchozích směrován k nejbližšímu rozhraní API bráně. V případě jednom regionu příchozích automaticky směrovat na další nejbližší bránu. Další informace o směrování metody v [přenosy Správce směrování metody](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Můžete použít šablonu správce prostředků Azure pro vytvoření instance služby správy rozhraní API?

Ano. V tématu [Správa služby Azure rozhraní API](http://aka.ms/apimtemplate) rychlý úvod šablony.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Dá se pomocí certifikátu podepsaného svým držitelem protokol SSL pro back-end?

Ano. Tady je postup, jak můžete certifikát podepsaný svým držitelem Secure (Sockets Layer SSL) u back-end:

1. Vytvořte entitu [back-end](https://msdn.microsoft.com/library/azure/dn935030.aspx) pomocí rozhraní API řízení.
2. Nastavte vlastnost **skipCertificateChainValidation** **logickou hodnotu PRAVDA**.
3. Pokud už nechcete umožní podepsaného certifikátů odstranit entity back-end nebo **skipCertificateChainValidation** vlastnost na **hodnotu false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Proč se při pokusu o klonovat úložišti libovolná získat neúspěšné ověření?

Pokud používáte libovolná přihlašovacích údajů správce nebo pokud se pokoušíte vytvořit kopii úložišti libovolná pomocí aplikace Visual Studio můžete narazit na známé problémy s dialogovým oknem přihlašovací údaje Windows. Dialogové okno omezuje délka hesla až 127 znaků a zkrátí heslo generované Microsoft. Pracujeme na zkrácení heslo. Nyní stiskněte klávesovou zkratku libovolná flám klonovat libovolná úložiště.

### <a name="does-api-management-work-with-azure-expressroute"></a>Funguje rozhraní API správy Azure ExpressRoute?

Ano. Správa rozhraní API pracuje s Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Můžu přejít služby správy rozhraní API z předplatných na druhý?

Ano. Další postup najdete v tématu [přesunutí zdrojů do nové skupiny prostředků nebo předplatného](../resource-group-move-resources.md).
