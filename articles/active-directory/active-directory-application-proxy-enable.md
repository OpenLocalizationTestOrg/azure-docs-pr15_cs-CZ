<properties
    pageTitle="Povolení proxy server Azure AD aplikace | Microsoft Azure"
    description="Zapnutí Proxy aplikace v portálu Azure klasické a nainstalujte spojnice pro reverzního proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Povolení aplikace Proxy na portálu Azure

Tento článek vás provede kroky povolení Proxy aplikace Microsoft Azure AD pro adresáři cloudu v Azure AD.

Pokud nevíte jaké aplikace Proxy můžete udělat, dozvíte, [jak poskytnout zabezpečené vzdálený přístup k místní aplikací](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Požadavky na Proxy aplikací
Před můžete povolit a používat aplikace Proxy služby, musíte mít:

- [Microsoft Azure AD základní nebo premium předplatného](active-directory-editions.md) a adresáře služby Azure AD pro kterou jste globální správce.
- Server systém Windows serveru 2012 R2 nebo Windows 8.1 nebo vyšší, ve kterém je nainstalován konektoru aplikace proxy serveru. Server odešle žádosti o služby Proxy aplikace v cloudu a potřebuje HTTP nebo HTTPS připojení k aplikacím, které publikujete.

    - Pro jednotné přihlašování publikovaných aplikací tento počítač by měl být doméně v tu samou doménu AD jako aplikace, které publikujete.

- Pokud je cestu bránu firewall, ujistěte se, se otevřít tak, aby spojnice můžete požadavky HTTPS (TCP) k aplikaci Proxy. Spojnice používá tyto porty společně s poddomény, které jsou součástí msappproxy.net uceleném domén a servicebus.windows.net. Zkontrolujte, že otevřete tyto porty pro **odchozí** přenosy:

  	| Číslo portu | Popis |
  	| --- | --- |
  	| 80 | Povolte odchozí přenosy protokolu HTTP ověřovacích zabezpečení. |
  	| 443 | Povolení ověření uživatele před Azure AD (povinné jenom pro procesu registrace spojnice) |
  	| 10100 – 10120 | Povolení LOB HTTP odpovědí odeslaných zpátky na proxy serveru |
  	| 9352, 5671 | Povolte komunikaci mezi konektoru směrem k Azure služby pro příchozí žádosti. |
  	| 9350 | Volitelné, chcete-li povolit lepší výkon pro příchozí žádosti |
  	| 8080 | Povolení zavádění posloupnosti spojnici a automatické aktualizace spojnice |
  	| 9090 | Povolení registrace spojnice (povinné jenom pro procesu registrace spojnice) |
  	| 9091 | Povolení automatického obnovení certifikátu zabezpečení spojnice |

    Pokud brána firewall vynucuje přenosy podle předpisů rozhraní pocházejících z uživatelé, otevřete tyto porty přenosů pocházejících z Windows služby jako služba sítě. Navíc nezapomeňte povolit port 8080 pro NT Authority\System.

- Pokud vaše organizace používá proxy servery pro připojení k Internetu funguje, přejděte prosím podívejte se na příspěvku na blogu [práce s stávající servery proxy místní](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) podrobné informace o tom, jak nakonfigurovat.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Krok 1: Povolte Proxy aplikace v Azure AD
1. Přihlaste se jako správce [Azure klasické portálu](https://manage.windowsazure.com/).
2. Přejděte ke službě Active Directory a vyberte složku, ve kterém chcete povolit aplikaci Proxy.

    ![Služba Active Directory - ikonu](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Vyberte **Konfigurovat** ze stránky služby directory a posuňte se dolů na **Proxy aplikace**.
4. Přepnout **Povolit služeb Proxy aplikací pro tento adresář** **povolené**.

    ![Povolení aplikace Proxy](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Vyberte **Stáhnout**. Tím přejdete **Azure AD aplikace Proxy Connector stáhnout**. Přečtěte si a přijměte licenční podmínky a kliknutím na tlačítko Uložit soubor Instalační služby systému Windows (.exe) konektoru **Stáhnout** .

## <a name="step-2-install-and-register-the-connector"></a>Krok 2: Instalovat a registrovat spojnice
1. **AADApplicationProxyConnectorInstaller.exe** se spouštějí na serveru, který jste připraveni podle požadavky.
2. Postupujte podle pokynů v průvodci a nainstalujte.
3. V průběhu instalace se dozvíte, jak se zobrazí výzva k registraci konektoru aplikace proxy server Azure AD klienta.

  - Zadejte svoje přihlašovací údaje Azure AD globálního správce. Globální správce klienta se může lišit od přihlašovacích údajů Microsoft Azure.
  - Zkontrolujte, jestli správce, kteří zaregistruje spojnice je ve stejném adresáři místo, kam jste povolili služba Proxy aplikací. Například, pokud je doména klientovi contoso.com, Správce by měl být admin@contoso.com nebo jiných alias na tuto doménu.
  - Pokud **Rozšířeného zabezpečení aplikace Internet Explorer** je nastavený **na** na serveru Pokud instalujete na spojnici, mohou být blokovány registrační obrazovku. Postupujte podle pokynů v chybové zprávě přístupu. Ujistěte se, že rozšířené zabezpečení aplikace Internet Explorer je vypnout.
  - Pokud spojnice registrace neproběhne úspěšně, přečtěte si článek [Poradce při potížích s Proxy aplikace](active-directory-application-proxy-troubleshoot.md).  

4. Po dokončení instalace dva nové služby se přidají do vašeho serveru:

    - **Microsoft AAD aplikace Proxy Connector** umožňuje připojení
    - **Microsoft AAD aplikace Proxy Connector Updater** je automatickou aktualizaci služba, která pravidelně kontroluje pro nové verze spojnici a podle potřeby se aktualizuje spojnice.

    ![Aplikace Proxy konektor služby – snímek](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Klikněte na tlačítko **Dokončit** v okně instalace.

Pro účely dostupnost nasadí aspoň dva spojnice. Abyste mohli nasadit další spojnic, opakujte kroky 2 a 3 nad. Každou spojnici musí být registrován samostatně.

Pokud chcete odinstalovat spojnice, odinstalujte službu konektoru a služba Updater. Restartujte počítač úplně odebrat službu.


## <a name="next-steps"></a>Další kroky

Teď jste připraveni [Publikovat](active-directory-application-proxy-publish.md)aplikací s Proxy aplikace.

Pokud máte aplikace, které jsou na jednotlivé sítě nebo na různých místech, spojnice skupiny slouží k uspořádání jiný spojnicím do logické jednotky. Další informace o [práci s Proxy aplikace spojnice](active-directory-application-proxy-connectors.md).
