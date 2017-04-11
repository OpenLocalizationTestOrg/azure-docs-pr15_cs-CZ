<properties 
   pageTitle="Zóny DNS a záznamy | Microsoft Azure" 
   description="Základní informace o podporu pro hostování zóny DNS a záznamy ve službě Microsoft Azure DNS." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>Zóny DNS a záznamy

Tato stránka vysvětluje klíčové pojmy domén, zóny DNS záznamy DNS a sady záznamů, a jak jsou podporovány u Azure DNS.

## <a name="domain-names"></a>Názvy domén
 
Domain Name System je hierarchie domén. Hierarchie začíná "kořenovou" doménu, jejichž název je jednoduše**.**.  Pod to jsou domény nejvyšší úrovně, jako třeba "com", "čistý", "organizace", "Kanada" nebo "CF".  Pod to jsou domény druhé úrovně, jako třeba "org.uk" nebo "co.jp". Domén v hierarchii DNS globálně rozloženy, hostitelem názvové servery DNS ve světě.

Doménového registrátora je organizace, která umožňuje koupit název domény, například "contoso.com".  Zakoupení název domény vám dá právo provádět hierarchii DNS v části tohoto názvu, například umožňuje odkázat název "www.contoso.com," na webu společnosti. Registrátora na hostitelském domény v jeho vlastní názvové servery vaším jménem nebo můžete taky určit alternativní názvové servery.

Azure DNS poskytuje infrastrukturu serveru globálně distribuované, vysoké dostupnosti jméno, které můžete použít k hostitelem vaší domény. Uložením vaše domény u Azure DNS můžete spravovat záznamy DNS s stejné přihlašovací údaje, API, nástroje, fakturace a podporu jako jiných Azure služeb.

Azure DNS aktuálně nepodporuje zakoupení s názvy domén. Pokud budete chtít koupit název domény, musíte použít doménového registrátora třetích stran. Registrátorovi se obvykle poplatek malých ročních plateb. Domény můžete pak hostované v Azure DNS ke správě záznamů DNS. Další informace najdete v článku [delegáta Domain Azure DNS](dns-domain-delegation.md) .

## <a name="dns-zones"></a>Zóny DNS 

Zóny DNS slouží k hostovat záznamy DNS pro určité domény. Začít, který je hostitelem vaší domény v Azure DNS, je potřeba vytvořit zóny DNS u tohoto názvu domény. Každý záznam DNS pro svoji doménu se pak vytvoří v této zóně DNS. 

Například domény "contoso.com" může obsahovat několika záznamech DNS, například "mail.contoso.com" (k poštovnímu serveru) a "www.contoso.com" (pro web).

Při vytváření zóny DNS v Azure DNS, název zóny musí být jedinečný v rámci skupiny zdrojů. Stejný název zóny můžete znovu použít v jiné skupině zdrojů nebo jiné Azure předplatné. Pokud více pásem sdílet stejný název, pokaždé přiřazena jiné názvové servery. U doménového registrátora název je možné konfigurovat pouze jednu sadu adresy.

>[AZURE.NOTE] Nemáte vlastní název domény k vytvoření zóny DNS pomocí tohoto názvu domény v Azure DNS. Ale potřebujete majiteli domény, kterou chcete konfigurovat názvové servery Azure DNS jako správné názvové servery pro název domény u doménového registrátora.

Další informace najdete v tématu [delegáta domain Azure DNS](dns-domain-delegation.md).

## <a name="dns-records"></a>Záznamy DNS

### <a name="record-types"></a>Typy záznamů

Každý záznam DNS má název a typ. Záznamy jsou uspořádány do různých typů podle data, která obsahují. Nejběžnější typ je "A" záznam, který odpovídá názvu adresy IPv4. Jiný typ běžné je "MX záznam, který odpovídá názvu e-mailového serveru.

Azure DNS podporuje všechny běžných typů záznamů DNS: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV a TXT.

### <a name="record-names"></a>Záznam názvů

V Azure DNS záznamy určená pomocí relativní názvů. *Plně kvalifikovaný* název domény (FQDN) obsahuje název zóny, že *relativní* název nemá. Například název relativní záznam "www" v zóně "contoso.com" vám bude radit záznam plně kvalifikovaný název "www.contoso.com".

Záznam *vrcholu* je záznam DNS na kořenové (nebo *vrcholu*) zóny DNS. Například v zóně DNS "contoso.com" záznam vrcholu má také plně kvalifikovaný název contoso.com (Toto je někdy se jí říká *otevřeným* domény).  Podle názvů, relativní název '@' slouží k vytváření záznamů vrcholu.

### <a name="record-sets"></a>Sady záznamů
Někdy potřebujete k vytvoření více než jeden záznam DNS s křestní jméno a zadejte. Předpokládejme například, že na webu "www.contoso.com" je hostovaný ve dvou různých IP adres. Na webu vyžaduje dva různé záznamy, jeden pro každou IP adresu. Toto je příklad sada záznamů:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS spravuje záznamy DNS s použitím *sady záznamů*. Sada záznamů (označovaná taky jako *zdroje* sada záznamů) je v kolekci záznamy DNS v oblasti, které mají stejný název a jsou stejného typu. Většina sady záznamů obsahovat jeden záznam, ale příklady tohoto typu, ve kterém sada záznamů obsahuje více než jeden záznam, nejsou běžné.

Například předpokládejme, že jste již vytvořili záznam "www" v zóně "contoso.com", přejdete IP adresu "134.170.185.46" (první záznam výše).  K vytvoření druhého záznamu můžete by přidání záznamu do existujícího záznamu sady namísto vytvoření nové sady záznamů.

Typy záznamů SOA a CNAME jsou výjimky. Standardy DNS nepovolují více záznamů se stejným názvem k těmto typům, což může obsahovat tyto sady záznamů pouze jeden záznam.

### <a name="time-to-live"></a>Time-to-live

Hodnota time to live nebo TTL určuje, jak dlouho každý záznam je do mezipaměti klienty před znovu dotazovaném. Ve výše uvedeném příkladu je hodnota TTL 3600 sekund nebo 1 hodinu.

Do pole Server Azure DNS hodnota TTL není určen sady záznamů není pro každý záznam tak, aby se stejnou hodnotou se používal pro všechny záznamy v této sadě záznamů.  Můžete použít libovolná hodnota TTL od 1 do 2 147 483 647 sekund.

### <a name="wildcard-records"></a>Zástupný znak záznamů

Azure DNS podporuje [zástupných záznamy](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Zástupné záznamy budou vráceny jako odpověď na dotaz s odpovídajícím názvem (pokud existuje bližší shoda ze sady záznamů-zástupných znaků). Pro všechny typy záznamů názvového serveru a SOA jsou podporovány zástupný znak sady záznamů.  

K vytvoření záznamu sady zástupných znaků, pomocí názvu sada záznamů "\*". Můžete taky můžete také jméno obsahující "\*"jako úplně vlevo popiskem, například"\*xyz".

### <a name="cname-records"></a>Záznamy CNAME

Sady záznamů CNAME nemohou být nainstalovány s další sady záznamů se stejným názvem. Například nemůžete vytvoříte sada se relativní název "www" CNAME záznam a záznamu s relativní název "www" ve stejnou dobu.

Protože vrcholu zóny (název = ‘@’) vždy obsahovat názvového serveru a SOA záznamu sady, které byly nastavené při zóně, nelze vytvořit záznam CNAME rozmístěné po vrcholu zóny.

Tato omezení vzniknou DNS standardy a ostatní vás omezení Azure DNS.

### <a name="ns-records"></a>Záznamy názvového serveru

Sada záznamů názvového serveru se automaticky vytvoří ve vrcholu každou zónu (název = ‘@’), a odstraní se automaticky při odstranění zóny (její odstranění není možné samostatně).  Změna TTL záznamu nastavenou, ale nemůžete změnit záznamy, které jsou předkonfigurovaná odkázat na názvové servery Azure DNS přiřazené k zóně.

Můžete vytvářet a odstraňovat další záznamy NS uvnitř pásma, nikoli na vrcholu zóny.  Umožňuje provádět konfigurace podřízených zón (viz [delegování dílčích domén v Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns).)

### <a name="soa-records"></a>SOA záznamů

Sada záznamů SOA se vytvoří automaticky ve vrcholu každou zónu (název = ‘@’), a odstraní se automaticky při odstranění zóny.  Záznamy SOA nelze vytvořit nebo odstranit samostatně. 

Můžete upravit všechny vlastnosti záznamu SOA s výjimkou vlastnost "host", která je předkonfigurovaná a odkaz na primární název název serveru poskytovanou Azure DNS.

### <a name="spf-records"></a>Záznamy SPF

Záznamy odesílatele zásad Framework (SPF) slouží k určení které e-mailové servery jsou povoleny jak poslat email za názvu domény zadaného.  Správné konfigurace záznamy SPF je důležité nechcete, aby příjemci označení e-mailů jako "nevyžádané".

RFC DNS původně zavádí nový typ záznamu "SPF" podporuje tento scénář. K podpoře starší názvové servery, budou taky povoleno použití typu záznamu TXT určit záznamů SPF.  Tento nejednoznačnosti vedly k zmatky, který byl vyřešit [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  To je uvedeno, že by měl být vytvořit pouze pomocí typ záznamu TXT záznamy SPF a změněny typ záznamu SPF. 

**Záznamy SPF podporovaných Azure DNS a by měl být vytvořené pomocí typ záznamu TXT.** Typ zastaralého záznamu SPF nepodporuje. Při [importu DNS zónu soubor](dns-import-export.md), všechny záznamy SPF pomocí typ záznamu SPF převedou na typ záznamu TXT. 

### <a name="srv-records"></a>Záznamy SRV

[Záznamy SRV](https://en.wikipedia.org/wiki/SRV_record) používají různé služby určit umístění serveru. Při zadávání SRV záznam v Azure DNS:

- Tuto *službu* a *Protocol (protokol)* musí být zadán jako součást názvu sada záznamů s předponou podtržítka.  Například "\_sip. \_tcp.name ".  Záznamu ve vrcholu pásma, je potřeba zadat '@' v názvu záznamu jednoduše použít tuto službu a Protocol (protokol), například "\_sip. \_tcp ".
- *Priority (priorita)*, *weight (váha)*, *portů*a *cílové* jsou zadány jako parametry každý záznam v sadě záznamů. 

## <a name="tags-and-metadata"></a>Značky a metadata

### <a name="tags"></a>Značky
Značky jsou seznam párů název hodnota a používají Azure správcem na popisek zdroje.  Azure správce prostředků povolit filtrovaných zobrazeních faktuře Azure pomocí značek a můžete také nastavit zásady, na které jsou potřeba značky. Další informace o značky najdete v článku [použití značek k uspořádání Azure zdroje](../resource-group-using-tags.md).

Azure DNS podporuje pomocí Správce prostředků Azure značek na DNS zone zdroje.  Nepodporuje značky na sady záznamů DNS.

### <a name="metadata"></a>Metadata

Jako alternativu k sadě záznamů značky Azure DNS podporuje přidávání poznámek pomocí "metadat" sady záznamů.  Podobně jako značky metadat umožňuje přidružit každého záznamu nastavit dvojice název hodnota.  To může být užitečné, například k záznamu účel každé sadě záznamů.  Na rozdíl od značky metadata se nedají použít k poskytnutí filtrované zobrazení faktuře Azure a nemůže být zadán v zásad správce prostředků Azure.

## <a name="limits"></a>Omezení

Platí následující omezení výchozí při použití Azure DNS:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Další kroky

- Začít používat služby Azure DNS, zjistěte, jak [vytvořit zóny DNS](dns-getstarted-create-dnszone-portal.md) a [Vytvoření záznamů DNS](dns-getstarted-create-recordset-portal.md).
- Migrace stávajícího zóny DNS, zjistěte, jak [importovat a soubor zóny DNS](dns-import-export.md).
