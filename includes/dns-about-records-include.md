## <a name="about-records"></a>Záznamy

Každý záznam DNS má název a typ. Záznamy jsou uspořádány do různých typů podle data, která obsahují. Nejběžnější typ je "Na" záznam, který odpovídá názvu IPv4 adresu. Jiný typ je "MX" záznam, který odpovídá názvu e-mailového serveru.

Azure DNS podporuje všechny společné typům záznamů DNS, včetně A, AAAA, CNAME, MX, NS, PTR, SOA, SRV a TXT. Všimněte si, že:
- SOA sady záznamů jsou vytvářeny automaticky jednotlivé zóny, nedá se vytvořit samostatně.
- Záznamy SPF by měl vytvořený s využitím typ záznamu TXT. Další informace najdete v tématu [tuto stránku](http://tools.ietf.org/html/rfc7208#section-3.1).

Do pole Server Azure DNS záznamy jsou určeny pomocí relativní názvy. "Plně kvalifikovaný" název domény (FQDN) obsahují název zóny, že "relativní" název nemá. Například relativní název záznamu "www" v zóně "contoso.com" vám bude radit www.contoso.com plně kvalifikovaný název záznamu.

## <a name="about-record-sets"></a>Informace o sady záznamů

Někdy potřebujete k vytvoření více než jeden záznam DNS s křestní jméno a zadejte. Předpokládejme například, že na webu "www.contoso.com" je hostovaný ve dvou různých IP adres. Na webu vyžaduje dva různé záznamy, jeden pro každou IP adresu. Toto je příklad sada záznamů:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS záznamy DNS spravuje pomocí sady záznamů. Sada záznamů je v kolekci záznamy DNS v oblasti, které mají stejný název a jsou stejného typu. Většina sady záznamů obsahovat jeden záznam, ale příklady tohoto typu, ve kterém sada záznamů obsahuje více než jeden záznam, nejsou běžné.

SOA a CNAME záznamu sady jsou výjimky. Standardy DNS nepovolují více záznamů se stejným názvem k těmto typům.

Hodnota time to live nebo TTL určuje, jak dlouho každý záznam je do mezipaměti klienty před znovu dotazovaném. V tomto příkladu je hodnota TTL 3600 sekund nebo 1 hodinu. Hodnota TTL není určen sady záznamů není pro každý záznam, aby stejnou hodnotu používal pro všechny záznamy v této sadě záznamů.

#### <a name="wildcard-record-sets"></a>Zástupný znak sady záznamů

Azure DNS podporuje [zástupných záznamy](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Toto jsou vráceny pro každý dotaz s odpovídajícím názvem (pokud existuje bližší shoda ze sady záznamů-zástupných znaků). Zástupný znak sady záznamů jsou podporovány pro všechny typy záznamů názvového serveru a SOA.  

K vytvoření záznamu sady zástupných znaků, pomocí názvu sada záznamů "\*". Nebo použití názvu s popiskem "\*", například"\*xyz".

#### <a name="cname-record-sets"></a>Sady záznamů CNAME

Sady záznamů CNAME nemohou být nainstalovány s další sady záznamů se stejným názvem. Například nemůžete vytvoříte sada se relativní název "www" CNAME záznam a záznamu s relativní název "www" ve stejnou dobu. Protože vrcholu pásma (název = ‘@’) vždy obsahovat názvového serveru a SOA záznamu sady, které byly nastavené při zóně, nelze vytvořit záznam CNAME rozmístěné po vrcholu pásma. Tato omezení vzniknou DNS standardy a nejsou omezení Azure DNS.
