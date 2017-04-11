<properties
    pageTitle="Azure AD připojení Agent stavu instalace | Microsoft Azure"
    description="Tohle je stránka stavu připojení Azure AD popisující instalací agent pro službu AD FS a synchronizace."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect instalaci agenta stavu

Instalace a konfigurace agenti stavu připojení Azure AD vás provede tohoto dokumentu. Zástupci si můžete stáhnout z [tady](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  <a name="requirements"></a>Požadavky
V následující tabulce je seznam požadavky pro použití stavu připojení Azure AD.

| Požadavek | Popis|
| ----------- | ---------- |
|Azure AD Premium| Stav připojení Azure AD je funkce Azure AD Premium a vyžaduje Azure AD Premium. </br></br>Další informace najdete v tématu [Začínáme s Azure AD Premium](active-directory-get-started-premium.md) </br>Spusťte bezplatnou 30denní zkušební verzi, najdete v článku [Start zkušební verzi.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Musíte být globální správce Azure AD začít pracovat s Azure AD stavu připojení|Ve výchozím nastavení jenom globální Správci instalace a konfigurace agentů stavu a začít pracovat, přístup k portálu provádět všechny operace ve stavu připojení Azure AD. Další informace najdete v tématu [Správa Azure AD adresář](active-directory-administer.md). <br><br> Pomocí řízení přístupu na základě rolí můžete povolit přístup k Azure AD připojení stavu ostatním uživatelům ve vaší organizaci. Další informace najdete v tématu [Role pro řízení přístupu pro stav připojení Azure AD.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Důležité:** Účet použili při instalaci agentů musí být pracovní nebo školní účet. Nesmí být svým účtem Microsoft. Další informace najdete v tématu [registraci Azure jako organizace](sign-up-organization.md)
|Agent stavu připojení Azure AD nainstalovaná na každém cílových serveru| Stav připojení Azure AD vyžaduje instalaci agenta cílových serverech k poskytnutí dat, které se zobrazí na portálu. </br></br>Například k načtení dat z místního infrastruktury služby AD FS, musí být nainstalovaný agent na servery služby AD FS, AD FS Proxy a proxy serveru webové aplikace. Podobně k načtení dat na svůj místní infrastruktury služby AD DS, agent musí být nainstalovaná na řadiče domény. </br></br>**Důležité:** Účet použili při instalaci agentů musí být pracovní nebo školní účet. Nesmí být svým účtem Microsoft. Další informace najdete v tématu [registraci Azure jako organizace](sign-up-organization.md)|
|Odchozí připojení k koncové body služby Azure|Během instalace a spuštění agent vyžadují připojení k koncové body služby Azure AD stavu připojení. Pokud je blokován odchozí připojení, zajistit, že následující koncové body se přidají do seznamu povolených: </br></br><li>& #42;. BLOB.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - portu: 5671 </li><li>https://Management.Azure.com </li><li>https://s1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.dc.AD.MSFT.NET/</li><li>https://Login.Windows.NET</li><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Portů brány firewall na serveru se službou agent.| Agent vyžaduje tyto porty brány firewall být otevřený v pořadí agenta komunikovat s koncové body služby Azure AD stavu.</br></br><li>TCP/UDP port 443</li><li>TCP/UDP portů 5671</li>
|Povolit následující weby, pokud je povoleno rozšířené zabezpečení aplikace Internet Explorer| Pokud je povoleno rozšířené zabezpečení aplikace Internet Explorer, musíte následujících webů povolené na serveru, který bude mít instalaci agenta.</br></br><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://Login.Windows.NET</li><li>Federačního serveru pro vaši organizaci důvěryhodný Azure Active Directory. Příklad: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Instalace Azure AD připojení Agent stavu služby AD FS
Spusťte instalaci agent, poklikejte na souboru .exe, který jste stáhli. Na první obrazovce klikněte na nainstalovat.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health-requirements/install1.png)

Po dokončení instalace klikněte na konfigurovat.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health-requirements/install2.png)

Příkazový řádek se spustí, následovaný některé Powershellu, který se spustí Register AzureADConnectHealthADFSAgent. Po zobrazení výzvy se přihlásit k Azure pokračovat a přihlaste se.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health-requirements/install3.png)


Po přihlášení, zůstanou v prostředí PowerShell. Po dokončení, zavřete prostředí PowerShell a konfigurace je dokončena.

V tomto okamžiku služby by měl být zahájen automaticky povolení agent shromažďování dat a sledovat. Pokud nejsou splněné všechny předpoklady uvedené v předchozí části, se zobrazí v okně prostředí PowerShell upozornění. Ujistěte se, dokončete [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) před instalací agent. Následující obrázek je příkladem tyto chyby.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health-requirements/install4.png)

Pokud chcete ověřit, zda že nainstaloval agent, hledejte následující služby na serveru. Pokud jste dokončili konfiguraci, by měly již být spuštěny. V opačném jsou ukončit až do dokončení konfigurace.

- Azure AD Connect stavu AD FS diagnostické služby
- Stav služby AD FS přehledy služby Azure AD Connect
- Azure AD Connect stavu AD FS sledování služby

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Instalace zástupce v systému Windows Server 2008 R2 servery

Kroky pro servery systému Windows Server 2008 R2:

1. Ujistěte se, že server běží ve Service Pack 1 nebo vyšší.
1. Vypnutí funkce IE ESC pro instalaci agenta:
1. Instalace Windows PowerShell 4.0 na všech serverů před instalací agent AD stavu. Instalace Windows PowerShell 4.0:
 - Nainstalujte [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) pomocí následující odkaz ke stažení offline instalačního programu.
 - Instalace prostředí PowerShell ISE (z funkce systému Windows)
 - Instalace [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Instalace aplikace Internet Explorer 10 nebo novější na serveru. (Musí službou stavu ověření, pomocí svých přihlašovacích údajů správce Azure).
1. Další informace o instalaci Windows PowerShell 4.0 v systému Windows Server 2008 R2, najdete v článku wikiwebu [tady](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Povolení auditování pro službu AD FS

> [AZURE.NOTE] Tato část platí jenom pro federační servery služby AD FS.

Aby funkci analýzy využití shromažďovat a analyzovat data musí agent stavu připojení Azure AD informace v protokolech auditování AD FS. Ve výchozím nastavení nejsou povolené tyto protokoly. Chcete-li povolit službu AD FS auditování a vyhledejte protokolů auditování služby AD FS na servery služby AD FS, použijte následující postupy.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Pokud chcete povolit auditování pro službu AD FS 2.0

1. Klikněte na tlačítko **Start**, přejděte na **programy**, přejděte na **Nástroje pro správu**a potom klikněte na **Místní zásady zabezpečení**.
2. Přejděte do složky, **Správa práv zásady\Přiřazení uživatelských zabezpečení\Místní zabezpečení** a potom poklikejte Generovat audity zabezpečení.
3. Na kartě **Místní nastavení zabezpečení** zkontrolujte, jestli je uvedená účet služby AD FS 2.0 služby. Pokud není je k dispozici, klikněte na **Přidat uživatele nebo skupinu** a přidejte do seznamu a klikněte na **OK**.
4. Pokud chcete povolit auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte tento příkaz:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zavřít místní zásady zabezpečení a potom otevřete modulu snap-in Správa. Otevřete modulu snap-in Správa, klikněte na tlačítko **Start**, přejděte na **programy**, přejděte na **Nástroje pro správu**a klikněte na AD FS 2.0 správy.
6. V podokně akcí klikněte na Upravit vlastnosti federace služby.
7. V dialogovém okně **Vlastnosti federace služby** klikněte na kartu **události** .
8. Zaškrtněte políčka **audity úspěšné** a **neúspěšné audity** .
9. Klikněte na **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Pokud chcete povolit auditování pro službu AD FS v systému Windows Server 2012 R2

1. Otevření **Místní zásady zabezpečení** otevřením **Správce serveru** na obrazovce Start nebo správce serveru na hlavním panelu na ploše a potom klikněte na **Nástroje a místní zásady zabezpečení**.
2. Přejděte do složky **Přiřazení zabezpečení zabezpečení\Místní uživatelských práv** a potom poklikejte na **vytvářet auditování zabezpečení**.
3. Na kartě **Místní nastavení zabezpečení** ověřte, jestli je uvedená jako účet služby AD FS. Pokud není je k dispozici, klikněte na **Přidat uživatele nebo skupinu** a přidejte do seznamu a klikněte na **OK**.
4. Pokud chcete povolit auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte tento příkaz:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zavřete **Místní zásady zabezpečení**a potom otevřete modulu snap-in **AD FS Management** (v správce serveru klikněte na nástroje a pak vyberte Správa AD FS).
6. V podokně akcí klikněte na **Upravit vlastnosti federaci služby**.
7. V dialogovém okně Vlastnosti federace služby klikněte na kartu **události** .
8. Zaškrtněte políčka **audity úspěšné a neúspěšné audity** a potom klikněte na **OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Vyhledejte auditování služby AD FS protokoly


1. Otevřete **Prohlížeč událostí**.
2. Přejděte na protokoly systému Windows a vyberte **zabezpečení**.
3. Na pravé straně klikněte na **Filtr aktuální protokoly**.
4. Ve skupinovém rámečku zdroj události vyberte **AD FS auditování**.

![AD FS protokolů auditování](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Pokud existuje zásad skupiny, který je zakázání AD FS auditování je Agent stavu připojení Azure AD nelze shromažďování informací. Ujistěte se, že nemáte zásad skupiny, který může být zakázání auditování.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Instalace agent stavu připojení Azure AD pro synchronizaci
Agent stavu připojení Azure AD pro synchronizaci se instaluje automaticky v posledním buildu Azure AD Connect. Pro účely Azure AD Connect synchronizovat, musíte si nejnovější verzi Azure AD Connect stáhnout a nainstalovat ho. Můžete si stáhnout nejnovější verzi [tady](http://www.microsoft.com/download/details.aspx?id=47594).

Pokud chcete ověřit, zda že nainstaloval agent, hledejte následující služby na serveru. Pokud jste dokončili konfiguraci, by měly již být spuštěny. V opačném jsou ukončit až do dokončení konfigurace.

- Stav synchronizace přehledy služby Azure AD Connect
- Stav synchronizace, sledování služby Azure AD Connect

![Ověření Azure AD Connect stav synchronizace](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Myslete na to, že použití stavu připojení Azure AD vyžaduje Azure AD Premium. Pokud nemáte Azure AD Premium, není možné dokončete konfiguraci Azure portálu. Další informace najdete v tématu [požadavky na stránku](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Ruční Azure AD stavu připojení pro synchronizaci registrace
Pokud Azure AD připojení stavu pro registraci agent synchronizace nepovede po úspěšném instalaci Azure AD Connect, můžete ručně zaregistrovat agent následujícího příkazu Powershellu.

>[AZURE.IMPORTANT] Pomocí tohoto příkazu Powershellu je jenom povinné, když zápis agent nezdaří po instalaci Azure AD Connect.

Následující Powershellu, ke kterému se příkaz povinné pouze při registraci agent stavu se nezdaří i po úspěšném instalace a konfigurace Azure AD Connect. Služby Azure AD stavu připojení začnou po agent byly úspěšně zaregistrovala.

Můžete ručně zaregistrovat agent stavu připojení Azure AD pro synchronizaci pomocí následujícího příkazu Powershellu:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Příkaz jen po parametry:

- AttributeFiltering: $true (výchozí nastavení) – Pokud Azure AD Connect není synchronizovaný atribut výchozí nastavení a přizpůsobený použít sadu filtrované atribut. $false jinak.
- StagingMode: $false (výchozí nastavení) – Pokud není server Azure AD Connect v přípravu režimu $true, pokud je server nakonfigurovaný být přípravu režimu.

Po zobrazení výzvy k ověření byste měli používat stejný účet globálního správce (například admin@domain.onmicrosoft.com) , který byl použit pro konfiguraci Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Instalace Azure AD připojení Agent stavu služby AD DS
Spusťte instalaci agent, poklikejte na souboru .exe, který jste stáhli. Na první obrazovce klikněte na nainstalovat.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Po dokončení instalace klikněte na konfigurovat.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Příkazový řádek se spustí, následovaný některé Powershellu, který se spustí Register AzureADConnectHealthADDSAgent. Po zobrazení výzvy se přihlásit k Azure pokračovat a přihlaste se.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Po přihlášení, zůstanou v prostředí PowerShell. Po dokončení, zavřete prostředí PowerShell a konfigurace je dokončena.

V tomto okamžiku služby by měl být zahájen automaticky povolení agent shromažďování dat a sledovat. Pokud nejsou splněné všechny předpoklady uvedené v předchozí části, se zobrazí v okně prostředí PowerShell upozornění. Ujistěte se, dokončete [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) před instalací agent. Následující obrázek je příkladem tyto chyby.

![Ověření Azure AD Connect stavu služby AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Pokud chcete ověřit, zda že nainstaloval agent, hledejte následující služby řadiče domény.

- Stav služby AD DS přehledy služby Azure AD Connect
- Azure AD Connect stavu AD DS sledování služby

Pokud jste dokončili konfiguraci, by měl už spuštěný těchto služeb. V opačném jsou ukončit až do dokončení konfigurace.

![Ověření Azure AD Connect stavu](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Instalace Azure AD připojení Agent stavu služby AD DS na Server Core.
Po instalaci souboru .exe, můžete dokončit proces registrace pomocí následujícího příkazu Powershellu:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Konfigurace Azure AD připojení stavu agentů Proxy pro HTTP
Můžete nakonfigurovat Azure AD připojení agenti stavu pro práci s Proxy pro HTTP.

>[AZURE.NOTE]
- Jako agent používá System.Net k vytvoření žádosti o web místo Microsoft Windows HTTP Services není podporované použití "Netsh WinHttp nastavit ProxyServerAddress".
- Nakonfigurované adresy Proxy pro Http se používá k předávací zašifrovaných zpráv Https.
- Nejsou podporovány ověřeným proxy servery (pomocí HTTPBasic).

### <a name="change-health-agent-proxy-configuration"></a>Konfigurace Proxy Agent změna stavu
Máte k dispozici následující možnosti pro nastavení Azure AD připojení Agent stavu můžete nastavit informace HTTP Proxy.

>[AZURE.NOTE]Všechny služby Azure AD připojení Agent stavu restartování, aby nastavení proxy mají být aktualizovány. Spusťte tento příkaz:<br>
Restart-Service AdHealth *

#### <a name="import-existing-proxy-settings"></a>Import existující proxy nastavení

##### <a name="import-from-internet-explorer"></a>Import z aplikace Internet Explorer
Nastavení Internet Exploreru HTTP proxy serveru můžete importovat pro použití v agenti stavu připojení Azure AD. Na všech serverech běží agent stavu spusťte následujícího příkazu Powershellu:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Import z WinHTTP
Nastavení serveru proxy WinHTTP lze importovat pro použití v agenti stavu připojení Azure AD. Na všech serverech běží agent stavu spusťte následujícího příkazu Powershellu:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ruční určení Proxy adresy
Ruční zadání proxy server na všech serverech běží Agent stavu spuštěním následujícího příkazu Powershellu:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Příklad: *vlastní server proxy Set AzureAdConnectHealthProxySettings - HttpsProxyAddress: 443*

- "address" může být název možností vyřešení serveru DNS nebo adresy IPv4
- "port" můžete vynechány. Pokud není zadáno pak vybrána jako výchozí port 443.

#### <a name="clear-existing-proxy-configuration"></a>Vymazat existující konfigurace proxy serveru
Spuštěním následujícího příkazu můžete zrušit existující konfigurace proxy serveru:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Přečtěte si aktuální nastavení proxy serveru
Nastavení konfigurovaných proxy si můžete přečíst spuštěním následujícího příkazu:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Test připojení k Azure AD připojení stavu služby
Je možné, že problémy by mohlo dojít způsobující agenta stavu připojení Azure AD ztratit spojení se službou Azure AD stavu připojení. Jedná se o problémům se sítí oprávnění problémy a různých dalších důvodů.

Pokud se agent nepodaří odeslat data do služby Azure AD stavu připojení déle než dvě hodiny, je označen se následující upozornění na portálu: "data stavu služby není aktuální." Můžete zkontrolovat, pokud problémového Azure AD Connect agent stavu je možné odeslání dat do služby Azure AD stavu připojení spuštěním následujícího příkazu Powershellu:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Parametr role aktuálně má následující hodnoty:

- SLUŽBY AD FS
- Synchronizace
- PŘIDÁ

Můžete použít příznak - ShowResults příkazu zobrazíte podrobné protokolování. Použijte v následujícím příkladu:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Pomocí nástroje připojení, musíte nejdřív dokončete registraci agent. Pokud si nejste schopni dokončete registraci agent, ujistěte se, že jsou splněny všechny [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) pro Azure AD stav připojení. Ve výchozím nastavení při registraci agent provedený test připojení.



## <a name="related-links"></a>Související odkazy

* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Azure AD Connect stavu operace](active-directory-aadconnect-health-operations.md)
* [Použití Azure AD stavu připojení se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Použití stavu připojení Azure AD pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Použití Azure AD stavu připojení se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect stavu časté otázky](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect historie verzí stavu](active-directory-aadconnect-health-version-history.md)
