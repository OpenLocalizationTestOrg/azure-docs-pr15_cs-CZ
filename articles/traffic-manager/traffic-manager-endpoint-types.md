<properties
    pageTitle="Typy přenosů správce koncový bod | Microsoft Azure"
    description="Tento článek popisuje různé typy koncové body, které lze použít s Azure přenosy správce"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Koncové body přenosy správce

Správce Microsoft Azure přenosy umožňuje určit, jak v síti úměrně nasazení aplikace spuštěné v různých datacentrech. Konfigurace každý nasazení aplikace jako "koncový bod" ve Správci přenosy. Pokud přenosy Správce obdrží žádost o DNS, zvolí dostupné koncový bod vrátit v odpovědi DNS. Přenosy správce počítá výběr aktuální stav koncového bodu a metody přenosu směrování. Další informace najdete v tématu [Jak funguje přenosy správce](traffic-manager-how-traffic-manager-works.md).

Existují tři typy podporované správcem přenosy koncového bodu:

- **Azure koncové body** se používají pro službu hostované v Azure.
- **Externí koncové body** se používají pro službu hostované mimo Azure, buď místně, nebo pomocí jiného poskytovatele hostingu.
- **Koncové body vnořené** slouží ke kombinování profily přenosy správce k vytvoření flexibilnější směrování přenosu schémata podporuje potřeb větší, složitější nasazení.

Není žádná omezení jak kombinovat koncové body různých typů v jednom profilu přenosy správce. Každý profil může obsahovat libovolnou kombinaci typů koncového bodu.

Následující oddíly popisují každý typ koncového bodu podrobněji.

## <a name="azure-endpoints"></a>Azure koncové body

Azure koncové body používané služby založené na Azure ve Správci přenosy. Jsou podporovány následující typy Azure zdroje:

- Klasický"VMs IaaS a PaaS cloudových služeb.
- Web Apps
- PublicIPAddress zdroje (které jde připojit k VMs přímo nebo prostřednictvím Vyrovnávání zatížení Azure). PublicIpAddress musí mít přiřazené se nemusí používat v profil správce přenosy název DNS.

Správce prostředků Azure zdroje jsou PublicIPAddress zdroje. V klasickém nasazení modelu neexistují. Proto jsou pouze podporovaných v přenosy vedoucího správce prostředků Azure prostředí. Další koncový bod typy nejsou podporované přes správce zdrojů a modelu klasické nasazení.

Pokud chcete použít Azure koncové body, přenosy správce rozpozná OM IaaS "Klasické", cloudové služby nebo Web App zastaven a začít. Tenhle stav se projeví v stav koncového bodu. Podrobnosti najdete v části [Sledování koncový bod přenosy správce](traffic-manager-monitoring.md#endpoint-and-profile-status) . Po zastavení služby základní přenosy správce neprovádí kontroly stavu koncový bod nebo přímé přenosy na koncový bod. Žádné přenosy správce fakturace událostem přestal instance. Restartování služby fakturační životopisů a koncový bod při přijímat přenosy. Toto zjišťování, nebudou mít vliv na PublicIpAddress koncové body.

## <a name="external-endpoints"></a>Externí koncové body

Externí koncové body se používají pro služby mimo Azure. Například službu hostované místní nebo pomocí jiného poskytovatele. Externí koncové body lze použít jednotlivě nebo společně s Azure koncové body v stejný jako profil přenosy správce. Kombinování Azure koncové body s externím koncové body umožňuje různých situacích:

- V obou aktivní nebo aktivní pasivní převzetí modelu umožňuje Azure poskytnout lepší redundance existujícího místní aplikace.
- Snížit latence aplikací pro uživatele ve světě, rozšiřte existující místní aplikaci pro další zeměpisné lokality v Azure. Další informace najdete v tématu [přenosy správce "Výkonu" směrování přenosu](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Použití Azure k poskytují další kapacitu existujícího místní aplikace průběžně nebo jako "požadavků cloudu" řešení, které splňují zásobníku v služba.

V některých případech je vhodné používat externí koncové body neodkazuje služby Azure (příklady naleznete v tématu [Nejčastější dotazy týkající se](#faq)). V tomto případě kontroly stavu se vám účtovat poplatky sazbou Azure koncové body, ne sazba externí koncové body. Však na rozdíl od Azure koncové body, pokud zastavit nebo odstranění služby základní kontrolu stavu fakturace pokračuje, dokud zakázání nebo odstranění koncového bodu ve Správci přenosy.

## <a name="nested-endpoints"></a>Vnořené koncové body

Vnořené koncové body sloučení více profily přenosy správce vytvářet flexibilní směrování přenosu schémata a podporu potřeb větší, složitých nasazení. S koncové body vnořené "dětmi" profilu se přidá ve formě koncový bod "nadřazené" profilu. Podřízené a nadřazené profil může obsahovat další koncové body libovolný typ, včetně dalších vnořené profily. Další informace najdete v tématu [vnořené profily přenosy správce](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Webové aplikace jako koncové body

Při konfiguraci webové aplikace jako koncové body ve Správci přenosy některé další aspekty, které platí:

1. Pouze Web Apps na standardní"SKU nebo vyšší se dá použít pro použití s přenosy správce. Pokusí přidat do webových aplikací nižší SKU fungovat. Přechodu SKU existující Web Appu výsledky v přenosy správce už odesílání přenosy na tento Web Appu.

2. Koncový bod obdrží žádost HTTP, použije záhlaví "host" v žádosti o rozhodnout, kterou webovou aplikaci by měl služby žádost. Záhlaví hostitele obsahuje název DNS zahajte pozvánce na schůzku, například contosoapp.azurewebsites.net. Pokud chcete použít jiný název DNS webovou aplikaci, musí být název DNS registrován jako vlastního názvu domény pro aplikaci. Při přidávání koncového bodu v prohlížeči jako koncový bod Azure, správce přenosy Název_profilu DNS automaticky registrované pro aplikaci. Tato registrace se automaticky odeberou při odstranění koncového bodu.

3. Každý profil přenosy správce může obsahovat maximálně jeden Web App koncový bod z každé Azure oblasti. Informace k alternativním řešením pro toto omezení, můžete nakonfigurovat do webových aplikací jako externí koncový bod. Další informace najdete v tématu [Časté otázky](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Povolení nebo zakázání koncové body

Zakázání koncový bod ve Správci přenosy může být užitečné dočasně přenosy odeberete koncový bod, který je v režimu údržby nebo opětovném nasazení. Po opětovném spuštění koncový bod může být znovu povolit.

Koncové body může povolit nebo zakázat prostřednictvím portálu správce přenosy Powershellu, rozhraní příkazového řádku nebo rozhraní REST API, které podporuje správce zdrojů a modelu klasické nasazení.

>[AZURE.NOTE] Zakázání Azure koncový bod nesouvisí s stavu nasazení v Azure. Služby Azure (například OM nebo Web App zůstane pracovního a nedostáváte přenosy i v případě, že zakázané ve Správci přenosy. Přenosy lze vyřešit, přímo na instance služby a ne prostřednictvím přenosy správce Název_profilu DNS. Další informace najdete v tématu [Princip přenosy správce](traffic-manager-how-traffic-manager-works.md).

Aktuální oprávněnost každý koncový bod pro příjem vysílání závisí na následující faktory:

- Stav profilu (povolit nebo zakázat)
- Stav koncového bodu (povolit nebo zakázat)
- Výsledky stavu hledá tohoto koncového bodu

Další informace najdete v tématu [Sledování koncový bod přenosy správce](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Protože správce přenosy pracuje na úrovni DNS, se nepodařilo ovlivnit existující připojení všechny koncový bod. Při koncový bod není k dispozici, správce provoz směrovat tak, aby nové připojení k jiné koncový bod k dispozici. Hostiteli za koncový bod zakázané nebo chybná však může nadále přijímat přenosy prostřednictvím existující připojení až do ukončení tyto relace. Aplikace měli omezit dobu trvání relace přenosy poté, co z existující připojení.

Pokud jsou zakázány všechny koncové body v profilu nebo zakázané samotném profilu, správce přenosy odešle odpověď "Kódem NXDOMAIN" nový dotaz DNS.

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Dá se koncové body z víc předplatných pomocí přenosy správce?

Použití koncové body z víc předplatných není možné s Azure Web Apps. Azure Web Apps vyžaduje, aby všechny vlastní název domény použít s Web Apps se používá pouze v rámci jedné předplatné. Není možné použít Web Apps z více předplatného se stejným názvem domény.

Pro jiné typy koncový bod je možné používat přenosy správce s koncové body z více než jedno předplatné. Jak nakonfigurovat přenosy správce závisí na tom, jestli používáte modelu klasické nasazení nebo prostředí správce prostředků.

- Ve Správci zdrojů koncové body z jakéhokoli předplatného můžete přidat ke Správci přenosy té osoby konfigurace profilu přenosy správce má přístup pro čtení na koncový bod. Tato oprávnění můžete k ní mít udělený pomocí [řízení přístupu na základě rolí správce prostředků Azure (RBAC)](../active-directory/role-based-access-control-configure.md).
- V rozhraní klasické nasazení modelu přenosy správce vyžaduje, že Cloudovým službám nebo Web Apps nakonfigurování jako Azure koncové body nacházejí v rámci stejného předplatného jako správce dopravy profil. Koncové body služby cloudu v jiných předplatných můžete přidat ke Správci přenosy jako "externí" koncové body. Tyto externí koncové body je faktura jako Azure koncové body, nikoli externí sazba.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>S sloty "přípravu cloudové služby, používat přenosy správce?

Ano. Cloudová služba přípravu sloty je možné konfigurovat ve Správci přenosy jako externí koncové body. Kontroly stavu bude účtovaná pořád sazbou koncové body Azure. Protože typ externího koncového bodu používá, změny služby základní nejsou převzít automaticky. S externím koncové body přenosy správce nelze zjistit cloudové služby je zastaveno nebo odstranit. Správce přenosy proto pokračuje fakturace za kontroly stavu, dokud deaktivovaným nebo odstraněným koncový bod.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Podporuje správce přenosy protokolu IPv6 koncové body?

Přenosy správce neposkytuje aktuálně IPv6 addressible názvové servery. Však přenosy správce může dál používat IPv6 připojení klientů k koncové body IPv6. Klient abyste požadavky DNS přímo přenosy správci. Místo toho klient používá rekurzivní služby DNS. Pouze protokol IPv6 klient odešle žádosti o službu DNS rekurzivní prostřednictvím protokolu IPv6. Potom službu rekurzivní soubory byste měli kontaktovat správce přenosy názvové servery pomocí IPv4.

Přenosy správce odpoví DNS název koncového bodu. Podporuje protokol IPv6 koncový bod, musí existovat DNS AAAA záznam DNS název koncového bodu přejdete na adresu IPv6. Kontroly stavu Správce přenosy podporují pouze adresy IPv4 pro. Služba, musí vystavit koncového IPv4 na stejný název DNS.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Můžete použít Správce přenosy s více než jeden Web App ve stejné oblasti?

Přenosy správce obvykle používá k směrovaly do nasazené v různých oblastech. Však jej lze také mají víc než jedno nasazení aplikace ve stejné oblasti. Koncové body přenosy správce Azure Nepovolit víc než jeden koncový bod v prohlížeči ze stejného Azure oblasti, kterou chcete přidat stejný jako profil přenosy správce.

Řešení: Toto omezení v následujících krocích:

1. Zkontrolujte, zda jsou vaše koncové body v různých webových aplikací "jednotkách". Název domény musí namapujte jednoho webu v jednotce dané měřítko. Dva Web Apps ve stejné měřítko jednotce nemůžete proto sdílet profil správce přenosy.
2. Přidejte název domény marnivost jako vlastní hostname pro každý Web Appu. Každý Web Appu musí být v jiné měřítko jednotku. Všechny webové aplikace musí patřit do stejného předplatného.
3. Přidání jednoho (a pouze jeden) koncového bodu v prohlížeči do vlastního profilu přenosy správce jako koncový bod Azure.
4. Přidání každý další koncový bod Web Appu do vlastního profilu přenosy správce jako externí koncový bod. Externí koncové body můžete přidat jenom pomocí modelu nasazení Správce prostředků.
5. Vytvořit záznam DNS CNAME ve vaší doméně marnivost, který odkazuje na název profilu DNS přenosy správce (<> …. trafficmanager.net).
6. Přístup k webu prostřednictvím název domény marnivost není přenosy Správce DNS profil.

## <a name="next-steps"></a>Další kroky

- Zjistěte, [Jak funguje přenosy správce](traffic-manager-how-traffic-manager-works.md).
- Informace o přenosu správce [Sledování koncového bodu a automatické převzetí](traffic-manager-monitoring.md).
- Informace o přenosu správce [metody směrování přenosu](traffic-manager-routing-methods.md).
