<properties
    pageTitle="Přístup k klíč trezoru za firewall | Microsoft Azure"
    description="Informace o přístupu trezoru klíč z aplikace za bránou firewall"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Přístup k klíč trezoru za brána firewall
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>Otázka: Můj klíčové trezoru klientské aplikaci musí být za bránou firewall, jaké porty/hosts a IP adresy mám otevřít povolení přístupu k klíčové trezoru?

Přístup k klíčové trezoru klientské aplikace klíčové trezoru musí být mít přístup k několika koncové pro různé funkce.

- Ověřování (přes Azure Active Directory)
- Správa klíč trezoru (jejichž součástí vytvořit, číst nebo aktualizace nebo odstranění a taky zásady přístupu nastavení) pomocí Správce prostředků Azure
- Přístup k a Správa objektů (klíče a tajemství) uložených v klíčové trezoru samotné prochází klíčové trezoru konkrétní koncového bodu (například [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

V závislosti na konfiguraci a prostředí existují některé varianty.   

## <a name="ports"></a>Porty

Všechny přenosy na klíčové trezoru u všech 3 funkcí (ověřování, Správa a rovině přístup k datům) se podíváme na HTTPS: Port 443. Ale pro CRL, bude občas přenosy protokolu HTTP (port 80). Klienti, které podporují OCSP neměli dosáhla CRL, ale může občas dosáhnout [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Ověřování

Klíč trezoru klientské aplikaci se potřebují dostat k koncové body Azure Active Directory pro ověřování. Koncový bod používá závisí na konfigurace klienta AAD a typ jistiny – UPN, služby jistinu a typ účtu, například Account Microsoft nebo ID organizace.  

| Hlavní typ | Koncový bod: port |
|----------------|---------------|
| Uživatelem, který používá Account Microsoft<br> (napříkladuser@hotmail.com) | **Globální:**<br> Login.microsoftonline.com:443<br><br> **Azure Čína:**<br> Login.chinacloudapi.CN:443<br><br>**Azure vládní organizace:**<br> přihlášení us.microsoftonline.com:443<br><br>**Azure Německo:**<br> Login.microsoftonline.de:443<br><br> a <br>Login.live.com:443   |
| ID organizace pomocí AAD (například jistinu uživatele a službyuser@contoso.com) | **Globální:**<br> Login.microsoftonline.com:443<br><br> **Azure Čína:**<br> Login.chinacloudapi.CN:443<br><br>**Azure vládní organizace:**<br> přihlášení us.microsoftonline.com:443<br><br>**Azure Německo:**<br> Login.microsoftonline.de:443 |
| Hlavní uživatele a služby (například pomocí organizace ID + ADFS nebo jiných federované koncový boduser@contoso.com) | Všechny výše uvedené koncové body služby AD FS a Org ID nebo jiných federované koncové body |

Existují další možné složité scénáře. Získáte [Azure Active Directory Authentication tok](/documentation/articles/active-directory-authentication-scenarios/) [Aplikací integrace se službou Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) a [Active Directory ověřovací protokoly](https://msdn.microsoft.com/library/azure/dn151124.aspx) doplňující informace.  

## <a name="key-vault-management"></a>Správa klíčových trezoru

Trezoru pro správu klíčů (CRUD a nastavením přístupu) klíčové trezoru klienta potřebuje přístup správce prostředků Azure koncového bodu.  

| Typ operace | Koncový bod: port |
|----------------|---------------|
| Operace klíč trezoru řízení plochy<br> pomocí Správce Azure prostředků | **Globální:**<br> Management.Azure.com:443<br><br> **Azure Čína:**<br> Management.chinacloudapi.CN:443<br><br> **Azure vládní organizace:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Německo:**<br> Management.microsoftazure.de:443 |
| Graf Azure Active Directory rozhraní API | **Globální:**<br> Graph.Windows.NET:443<br><br> **Azure Čína:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure vládní organizace:**<br> Graph.Windows.NET:443<br><br> **Azure Německo:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Klíčové trezoru operace

Správa klíčových trezoru objektu (klíče a tajemství) a cryptographic operace klíčové trezoru klienta musí pro přístup k koncový bod klíčové trezoru. V závislosti na umístění trezoru klíč přípona DNS koncový bod se liší. Koncový bod klíč trezoru je formátu: < trezoru název >. < oblast konkrétní--přípona dns > popsané v následující tabulce.  

| Typ operace | Koncový bod: port |
|----------------|---------------|
| Klíč trezoru operací, jako je cryptographic operace klíčů, vytvořeno, číst nebo aktualizace nebo odstranění klíče a tajemství, sadu/get značky a další atributy pro objekty klíčové trezoru (klíče/tajemství)     | **Globální:**<br> &lt;Název trezoru&gt;. vault.azure.net:443<br><br> **Azure Čína:**<br> &lt;Název trezoru&gt;. vault.azure.cn:443<br><br> **Azure vládní organizace:**<br> &lt;Název trezoru&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Německo:**<br> &lt;Název trezoru&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>Rozsahy IP adres

Klíč trezoru zase službou jiných Azure zdrojů, jako jsou PaaS infrastruktury, proto není možné zajistit určitý rozsah IP adres, které koncové body služby klíčové trezoru bude mít v daném okamžiku. Ale pokud brána firewall podporuje pouze IP adresu oblastí potom získáte dokumentu [Microsoft Azure Datacentra rozsahy IP adres](https://www.microsoft.com/download/details.aspx?id=41653) .   Ověřování a identity (Azure Active Directory) musí být aplikace připojit k koncové body podle [ověřování a identity adresy](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Další kroky

- Pokud máte dotazy k trezoru klíč, navštivte [Fóra trezoru klíč Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
