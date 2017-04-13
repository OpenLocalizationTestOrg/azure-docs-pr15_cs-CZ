<properties 
    pageTitle="Upgrade agenta PhoneFactor server Azure Vícefaktorové ověřování"
    description="Tento dokument popisuje, jak začít s Azure MFA serveru a jak upgradovat z agenta starší phonefactor."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Upgrade agenta PhoneFactor server Azure Vícefaktorové ověřování

Upgrade z agenta PhoneFactor v5.x nebo starší server Azure Multi-Factor Authentication vyžaduje PhoneFactor Agent a přidružených komponenty k odinstalaci před Multi-Factor ověřování serveru a přidružených součásti bude možné nainstalovat.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Upgrade agenta PhoneFactor Azure Multi-Factor Authentication Server
<ol>
<li>Nejdřív obecnějším údajům PhoneFactor datového souboru. Výchozí umístění instalace je C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Pokud je nainstalovaná na portálu uživatele:</li>
<ol>
<li>Přejděte do složky nainstalovat a obecnějším údajům nastavení(Web.config)). Výchozí umístění instalace je C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Pokud přidáte vlastní motivy na portál obecnějším údajům vlastní složky pod adresáři C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Odinstalujte portálu uživatelského agenta PhoneFactor (dostupné jenom nainstalovaného na stejný server jako PhoneFactor Agent) prostřednictvím nebo Windows programy a funkce.</li></ol>




<li>Pokud je nainstalovaná mobilní aplikace webové služby:
<ol>
<li>Přejděte do složky nainstalovat a obecnějším údajům nastavení(Web.config)). Výchozí umístění instalace je C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Odinstalace mobilní aplikace webové služby prostřednictvím Windows programy a funkce.</li></ol>

<li>Pokud je nainstalovaný SDK webové služby, odinstalujte agenta PhoneFactor prostřednictvím nebo Windows programy a funkce.

<li>Odinstalace agenta PhoneFactor přes Windows programy a funkce.

<li>Nainstalujte Vícefaktorové ověřování serveru. Všimněte si, že Instalační cesta je vybraných kontakty z registru z předchozích agenta PhoneFactor instalace tak, aby měli nainstalovat ve stejném umístění (například C:\Program Files\PhoneFactor). Instalace nové bude mít jiné výchozí instalační cesta (například C:\Program Files\Multi faktorem ověřování serveru). Datový soubor zanechané předchozích agenta PhoneFactor měli upgradovat během instalace tak uživatelů a nastavení ještě měli není po instalaci nového ověřování serveru Multi-Factor.

<li>Po zobrazení výzvy se aktivace Multi-Factor ověřování serveru a zajistěte, aby že přidělený skupiny správné replikace.

<li>Pokud SDK webové služby byla dříve nainstalována, nainstalujte nový Web služby SDK prostřednictvím uživatelského rozhraní Multi-Factor ověřování serveru. Všimněte si, že výchozí název virtuálního adresáře teď "MultiFactorAuthWebServiceSdk" místo "PhoneFactorWebServiceSdk". Pokud chcete použít předchozí název, musíte změnit název virtuální adresáře během instalace. Jinak Pokud povolíte nainstalovat a použít nové výchozí název, budete muset změnit adresu URL v všechny aplikace, které odkazují na SDK webové služby jako je portál uživatele a mobilní aplikace webová služba tak, aby ukazovaly na správné místo.

<li>Pokud portálu uživatele byla dříve nainstalována na serveru Agent PhoneFactor, nainstalujte nové Multi-Factor Authentication uživatele portálu prostřednictvím uživatelského rozhraní Multi-Factor ověřování serveru. Všimněte si, že výchozí název virtuálního adresáře teď "MultiFactorAuth" místo "PhoneFactor". Pokud chcete použít předchozí název, musíte změnit název virtuální adresáře během instalace. Jinak Pokud povolíte nainstalovat a použít nové výchozí název, se mají klikněte na ikonu portál uživatele na serveru Multi-Factor Authentication a aktualizují adresu URL portálu uživatele na kartě nastavení.

<li>Pokud uživatel portál a/nebo mobilní aplikaci webová služba dříve nainstalovaný na jiném serveru z agenta PhoneFactor:
<ol>
<li>Přejděte do umístění instalace (například C:\Program Files\PhoneFactor) a zkopírujte odpovídající installer(s) na server. Existuje 32bitové a 64bitové systémy Instalační služby pro uživatele portálem a mobilní aplikace webové služby. Jsou nazývají MultiFactorAuthenticationUserPortalSetupXX.msi a MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi v tomto pořadí.</li>
<li>Pokud chcete nainstalovat portálu uživatele na webový server, otevřete okno příkazového řádku jako správce a spusťte MultiFactorAuthenticationUserPortalSetupXX.msi. Všimněte si, že výchozí název virtuálního adresáře teď "MultiFactorAuth" místo "PhoneFactor". Pokud chcete použít předchozí název, musíte změnit název virtuální adresáře během instalace. Jinak Pokud povolíte nainstalovat a použít nové výchozí název, se mají klikněte na ikonu portál uživatele na serveru Multi-Factor Authentication a aktualizují adresu URL portálu uživatele na kartě nastavení. Stávajícím uživatelům bude potřeba informovat, uveďte nové adresy URL.</li>
<li>Přejděte na portál uživatele instalace umístění (například C:\inetpub\wwwroot\MultiFactorAuth) a upravte nastavení(Web.config)). Zkopírujte hodnoty v části appSettings a applicationSettings z původního souboru web.config do nového nastavení(Web.config)) zálohovaný před upgradem. Pokud nové výchozí název virtuálního adresáře byl k dispozici při instalaci SDK webové služby, změňte adresu URL v části applicationSettings tak, aby ukazovaly na správném místě. Pokud v předchozí nastavení(Web.config)) byly změněny jiné výchozí hodnoty, platí pro nové nastavení(Web.config)) změnám stejné.</li>
<li>Instalace mobilních aplikací webové služby na webovém serveru, otevřete okno příkazového řádku jako správce a spusťte MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Všimněte si, že výchozí název virtuálního adresáře teď "MultiFactorAuthMobileAppWebService" místo "PhoneFactorPhoneAppWebService". Pokud chcete použít předchozí název, musíte změnit název virtuální adresáře během instalace. Je vhodné zvolit kratší název pro snadnou koncovým uživatelům psaním na mobilních zařízeních. V opačném povolíte nainstalovat a použít nové výchozí název, můžete by měl klikněte na ikonu mobilních aplikací na serveru Multi-Factor Authentication a aktualizace adresu URL mobilní aplikace webové služby.</li>
<li>Přejděte mobilní aplikace webové službě instalace umístění (například C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) a upravte nastavení(Web.config)). Zkopírujte hodnoty v části appSettings a applicationSettings z původního souboru web.config do nového nastavení(Web.config)) zálohovaný před upgradem. Pokud nové výchozí název virtuálního adresáře byl k dispozici při instalaci SDK webové služby, změňte adresu URL v části applicationSettings tak, aby ukazovaly na správném místě. Pokud v předchozí nastavení(Web.config)) byly změněny jiné výchozí hodnoty, platí pro nové nastavení(Web.config)) tyto změny.</li></ol>
