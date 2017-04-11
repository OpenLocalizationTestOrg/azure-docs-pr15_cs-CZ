<properties
    pageTitle="Vytvoření Azure BizTalk služby Azure portálu | Microsoft Azure"
    description="Zjistěte, jak zřídit nebo vytvořte Azure BizTalk služby Azure portálu; MABS WABS"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Vytvoření BizTalk služby Azure portálu

Vytvoření Azure BizTalk služby Azure portálu.

> [AZURE.TIP] Pokud chcete přihlásit k portálu Azure, musíte Azure předplatného a účet Azure. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební objevit během pár minut. V tématu [Azure bezplatnou zkušební verzi](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Vytvoření BizTalk služby
Podle toho, které zvolíte Edition ne všechny služba BizTalk nastavení může být k dispozici.

1. Přihlaste se k [portálu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V navigačním podokně dolní vyberte **Nový**:  
![Klepnutím na tlačítko Nový][NEWButton]

3. Vyberte **aplikaci služby** > **Služba BIZTALK** > **vlastní vytvořit**:  
![Vyberte službu BizTalk a vyberte možnost vytvořit vlastní][NewBizTalkService]

4. Zadejte nastavení BizTalk služby:

    <table border="1">
    <tr>
    <td><strong>Název BizTalk služby</strong></td>
    <td>Můžete zadat libovolný název, ale je konkrétní. Několik příkladů:<br/><br/>
    <em>Společnost</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>Mojeaplikace</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" automaticky přidají i do názvu můžete zadat. Vytvoří adresu URL, která se používá k přístupu ke službě BizTalk, třeba <strong>https://<em>Mojeaplikace</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Pokud jste ve fázi testování/vývoj, zvolte <strong>Vývojář</strong>. Pokud jste ve fázi výroby, použít <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk služby: edice grafu</a> zjistíte <strong>Premium</strong> <strong>Standardní</strong>nebo <strong>základní</strong> správná volba pro obchodní scénář.
    </td>
    </tr>
    <tr>
    <td><strong>Oblast</strong></td>
    <td>Vyberte požadovanou zeměpisná oblast hostovat BizTalk služby.</td>
    </tr>
    <tr>
    <td><strong>Adresu URL domény</strong></td>
    <td><strong>Volitelné</strong>. Ve výchozím nastavení je adresu URL domény <em>YourBizTalkServiceName</em>. biztalk.windows.net. Můžete také zadat vlastní doménu. Například pokud je vaše doména <em>contoso</em>, můžete zadat: <br/><br/>
    <em>Společnost</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>Mojeaplikace</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Vyberte šipku Další.

5. Zadejte úložiště a databáze nastavení:

    <table border="1">
    <tr>
    <td><strong>Monitorování a archivace úložiště účtu</strong></td>
    <td>Vyberte existující účet úložiště nebo vytvořte nový účet úložiště. <br/><br/>Pokud vytvoříte nový účet úložiště, zadejte <strong>Název účtu úložiště</strong>.</td>
    </tr>
    <tr>
    <td><strong>Sledování databáze</strong></td>
    <td>Pokud používáte existující databáze SQL Azure, nelze použít jinou službou BizTalk. Potřebujete přihlašovací jméno a heslo zadané při, aby byl vytvořen serveru databáze SQL Azure.<br/><br/><strong>Tip:</strong> Vytvoření databáze sledování a monitorování a archivace úložiště účtu ve stejné oblasti jako služba BizTalk.</td>
    </tr>
    </table>
Vyberte šipku Další.

6. Zadejte nastavení databáze:

    <table border="1">
    <tr>
    <td><strong>Jméno</strong></td>
    <td>K dispozici vyberete-li <strong>vytvořit novou instanci SQL databáze</strong> je na předchozí obrazovku.
    <br/><br/>
   Zadejte název databáze SQL pro použití v BizTalk služby.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>K dispozici vyberete-li <strong>vytvořit novou instanci SQL databáze</strong> je na předchozí obrazovku.
    <br/><br/>
   Vyberte stávající Server SQL databáze nebo vytvořte novou databázi SQL serveru.</td>
    </tr>
    <tr>
    <td><strong>Název serveru přihlášení</strong></td>
    <td>Zadejte přihlašovací jméno uživatele.</td>
    </tr>
    <tr>
    <td><strong>Server přihlašovacího hesla</strong></td>
    <td>Zadejte heslo databáze.</td>
    </tr>
    <tr>
    <td><strong>Oblast</strong></td>
    <td>K dispozici, je-li vybrán <strong>vytvořit novou instanci SQL databáze</strong> . Vyberte požadovanou zeměpisná oblast hostovat databázi SQL.</td>
    </tr>
    </table>

Vyberte značku zaškrtnutí dokončete průvodce. Zobrazí se ikona průběh:  
![Až budete hotovi, zobrazí se ikona průběhu][ProgressComplete]

Až budete hotovi, služba Azure BizTalk jsou vytvořené a jste připraveni na aplikace. Výchozí nastavení je dostatečně. Pokud chcete změnit výchozí nastavení, vyberte **BIZTALK služby** v levém navigačním podokně a vyberte BizTalk služby. Další nastavení se zobrazují na [řídicí panel, sledování a měřítko karet](biztalk-dashboard-monitor-scale-tabs.md) nahoře.

V závislosti na stav služby BizTalk existují některé operacích, které nelze dokončit. Seznam tyto operace přejděte na [BizTalk grafu stav služby](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Po zřizovací kroky

-  [Instalace certifikátu do místního počítače](#InstallCert)
-  [Přidání certifikátu výrobní připravených](#AddCert)
-  [Získání oboru řízení přístupu](#ACS)

#### <a name="InstallCert"></a>Instalace certifikátu do místního počítače
V rámci služby BizTalk zřizování certifikátu podepsaného svým držitelem vytvořili a přidružený k předplatnému BizTalk služby. Musíte stáhnout tento certifikát a nainstalovat ho na počítačích, ze kterých můžete nasazení aplikace služeb BizTalk nebo zprávy odešlete koncový bod služby BizTalk.

1. Přihlaste se k [portálu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Vyberte **BIZTALK služby** v levém navigačním podokně a pak vyberte předplatné BizTalk služby.
3. Vyberte kartu **řídicího panelu** .
4. Zaškrtněte políčko **stahovat certifikát SSL**:  
![Změnit certifikát SSL][QuickGlance]
5. Poklepejte na certifikát a spusťte Průvodce instalace certifikátu. Ujistěte se, že musíte nainstalovat certifikát v úložišti **Důvěryhodné kořenové certifikační autority** .

#### <a name="AddCert"></a>Přidání certifikátu výrobní připravených
Certifikát podepsaný se automaticky vytvoří při vytváření BizTalk služeb jsou určeny pro použití v pouze vývojové prostředí. Scénáře výrobní ho nahraďte certifikát připravené na výroby.

1. Na kartě **řídicího panelu** vyberte **Certifikát SSL aktualizace**.
2. Přejděte na vaše soukromé certifikát SSL (*CertificateName*.pfx), který obsahuje vaše jméno služba BizTalk, zadejte heslo a klepněte na značku zaškrtnutí.

#### <a name="ACS"></a>Získání oboru řízení přístupu

1. Přihlaste se k [portálu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Vyberte **BIZTALK služby** v levém navigačním podokně a potom vyberte BizTalk službu.
3. V hlavním panelu vyberte **Informace o připojení**:  
![Vyberte informace o připojení][ACSConnectInfo]

4. Zkopírujte hodnoty řízení přístupu.

Při nasazení služba BizTalk projektu z aplikace Visual Studio zadejte tento obor názvů řízení přístupu. Obor názvů řízení přístupu se automaticky vytvoří službě BizTalk.

Řízení přístupu hodnot lze použít u jakékoli aplikace. Po vytvoření služby Azure BizTalk tento obor názvů řízení přístupu mezery nastavuje ověřování s nasazením BizTalk služby. Pokud chcete změnit předplatné nebo Správa názvů, vyberte **Služby ACTIVE DIRECTORY** v levém navigačním podokně a pak vyberte obor názvů. Na hlavním panelu seznam možností.

Kliknutím na tlačítko **Spravovat** otevřete portálu pro správu řízení přístupu. Na portálu správy řízení přístupu využívá služba BizTalk **identitami služby**:  
![Služba ACS identit v dialogu přístup určit portálu pro správu][ACSServiceIdentities]

Řízení přístupu služby identit je sadu přihlašovacích údajů, které umožňují aplikací nebo klienti ověřit přímo pomocí řízení přístupu a přijímat token.

> [AZURE.IMPORTANT]Služba BizTalk používá **vlastník** pro služby identit výchozí a hodnotu **heslo** . Pokud používáte symetrickou klíč hodnota místo hodnotu heslo k následující chybě může dojít.<br/><br/>*Nelze se připojit k účtu služby správy řízení přístupu s zadaná pověření*

[Správa ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) jsou uvedené některé pokyny a doporučení.

## <a name="requirements-explained"></a>Požadavky na vysvětlení

Tyto požadavky, nebudou mít vliv na bezplatné verzi.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Co byste měli</strong></td>
        <td><strong>Proč je nutné</strong></td>
</tr>
<tr>
<td>Azure předplatného</td>
<td>Předplatné Určuje, kdo se přihlásit k portálu Azure. Majitele účtu vytvoří předplatné v <a HREF="https://account.windowsazure.com/Subscriptions">Azure předplatná</a>.
<br/><br/>
Účet Azure můžete mít víc předplatných a dá se ovládat každý, kdo je povolen. Například váš účet Azure držitelem vytvoří předplatné s názvem <em>BizTalkServiceSubscription</em> a poskytuje správcům BizTalk ve vaší společnosti (například ContosoBTSAdmins@live.com) přístup k tomuto předplatnému. V tomto scénáři správci BizTalk přihlásit k portálu Azure a mít úplné práva správce hostované služby v rámci předplatného, včetně Azure BizTalk Services. Správci BizTalk nejsou na účet Azure majitele a tedy nemají přístup k fakturačních údajů.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Správa předplatného a úložiště účtů na portálu Azure</a> s dalšími informacemi.
</td>
</tr>
<tr>
<td>Databáze Azure SQL</td>
<td>Ukládá tabulek, zobrazení a uložené procedury použita službou BizTalk, včetně data sledování.
<br/><br/>
Při vytváření služba BizTalk můžete použít existující Azure SQL serveru, databázi SQL Azure nebo automaticky vytvoří nový Server nebo databáze.
<br/><br/>
Měřítko databáze SQL nakonfigurovaný automaticky. Výchozí měřítko obvykle, stačí služby BizTalk. Změna měřítka ovlivní ceny. Přečtěte si téma <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">účty a fakturace v databázi Azure SQL</a>
<br/><br/>
<strong>Poznámky</strong>
<br/>
<ul>
<li> Při vytváření nového Azure SQL Server a databázi služby Azure automaticky zapnutá. Služba BizTalk vyžaduje službu Azure povolit.</li>
<li>Pokud vytvoříte novou databázi SQL Azure na existující Server SQL Azure, brány firewall pravidla serveru se nezmění. Výsledkem je možné že další služby Azure nejsou povolené přístup do databáze serveru.</li>
</ul>
</td>
</tr>
<tr>
<td>Obor názvů Azure řízení přístupu</td>
<td>Ověří služby Azure BizTalk. Při nasazení služba BizTalk projektu z aplikace Visual Studio zadejte tento obor názvů řízení přístupu. Při vytváření služba BizTalk se automaticky vytvoří oboru řízení přístupu.</td>
</tr>

<tr>
<td>Účet Azure úložiště</td>
<td>Umožňuje přístup k tabulkám, objekty BLOB a službou BizTalk slouží k uložení následující:

<ul>
<li>Protokoly, které sledují služba BizTalk. Sledování výstup se také zobrazí na kartě **Sledování** na portálu Azure.</li>
<li>Při vytváření X12 nebo AS2 smlouvu mezi partnerů, můžete povolit funkci archivace k ukládání vlastností zprávy. Tato data se uloží do účtu úložiště.</li>
</ul>
<br/>
Při vytváření služba BizTalk můžete použít existující účet úložiště nebo automaticky vytvořit nový účet úložiště.
<br/><br/>
Výchozí nastavení ukládání jsou dostatečně služby BizTalk.
<br/><br/>
Při vytváření účtu úložiště jsou automaticky vytvoří primární klíč a vedlejší klíč. Klávesy řízení přístupu k účtu úložiště. Služba BizTalk automaticky používá primárního klíče.
<br/><br/>
Další informace najdete v tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">úložiště</a> .
</td>
</tr>

<tr>
<td>Soukromé certifikát SSL</td>
<td>
Po vytvoření služby Azure BizTalk URL HTTPS, který obsahuje vaše jméno služba BizTalk také vytvořit. Tato adresa URL je nastaven jako pomocí certifikátu podepsaného svým držitelem jen vývoj. Ve výrobním třeba soukromé certifikát SSL.
<br/><br/>
<strong>Informace o certifikátu SSL důležité</strong>

<ul>
<li>Datum vypršení platnosti certifikátu musí být menší než 5 let.</li>
<li>Soukromé certifikátů heslo. Znáte heslo a jako doporučený postup nasdílet toto heslo správce.</li>
<li>Používání podepsaného certifikátů v test/vývojové prostředí. Při použití podepsaného certifikáty, importujte certifikátu do úložišti osobních certifikátů a úložiště certifikátů důvěryhodných kořenových certifikátů.</li>
</ul>
<br/>Při odesílání žádost o certifikátu výrobní certifikační autority, poskytovat následující vlastnosti certifikát:
<br/>

<ul>
<li><strong>Rozšířené použití klíče</strong>: minimálně Azure BizTalk Services vyžaduje ověřování serveru.</li>
<li><strong>Běžné název</strong>: zadejte plně kvalifikovaný název domény (FQDN) o svoji adresu URL Azure BizTalk služby. V tomto článku najdete v článku <a HREF="#BizTalk">vytvoření BizTalk služby</a> .</li>
</ul>
<br/>
Po vytvoření služba BizTalk lze přidat nový nebo jiný certifikát.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybridní připojení

Při vytváření služby Azure BizTalk je dostupná Záložka **Hybridní připojení** :

![Kartu hybridní připojení][HybridConnectionTab]

Hybridní připojení se používá pro připojení k všechny místní prostředek, který používá statických TCP portu, třeba SQL Server MySQL, HTTP webového rozhraní API, mobilní služby a většina vlastní webové služby Azure web nebo Azure mobilních služeb.  Hybridní připojení a služba adaptér BizTalk se liší. Služba adaptér BizTalk se používá pro připojení služby Azure BizTalk do místního systému řádek Business (LOB).

 V tématu [Hybridní připojení](integration-hybrid-connection-overview.md) informace, včetně vytváření a Správa připojení hybridní.


## <a name="next-steps"></a>Další kroky

Teď, když se vytvoří BizTalk služby, seznamte se s různými [BizTalk služby: karty řídicích panelů, sledování a měřítko](biztalk-dashboard-monitor-scale-tabs.md). Služba BizTalk je připravený na aplikace. Zahájení vytváření aplikací, přejděte na [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Viz taky
- [BizTalk služby: Graf edice](biztalk-editions-feature-chart.md)<br/>
- [BizTalk služby: Diagram stavu](biztalk-service-state-chart.md)<br/>
- [BizTalk služby: Zálohování a obnovení](biztalk-backup-restore.md)<br/>
- [Služby BizTalk: omezení](biztalk-throttling-thresholds.md)<br/>
- [BizTalk služby: Název vydavatele a Vystavitel klíč](biztalk-issuer-name-issuer-key.md)<br/>
- [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybridní připojení](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
