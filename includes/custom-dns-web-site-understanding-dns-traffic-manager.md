Systému DNS (Domain Name) slouží k vyhledání věci na Internetu. Třeba když zadáte adresu v prohlížeči nebo kliknout na odkaz na webovou stránku, používá DNS překladu domény na IP adresu. IP adresa vypadá jako typ poštovní adresy, ale není velmi lidské popisný. Je třeba mnohem jednodušší si uvědomit DNS název, například **contoso.com** , je zapamatovatelné adresy IP 192.168.1.88 ATP 2001:0:4137:1f67:24a2:3888:9cce:fea3.

V systému DNS vychází z *záznamy*. Záznamy určitý *název*, například **contoso.com**, přidružit IP adresa nebo jiný název serveru DNS. Pokud aplikaci, třeba ve webovém prohlížeči, vyhledává název v systému DNS, najde záznam a používá něco jiného, co odkazuje na adresu. Pokud je argument hodnota, která ukazuje na IP adresu, prohlížeč použije daná hodnota. Odkazuje na jiný název serveru DNS, aplikace má dělat rozlišení znovu. Nakonec překlad všechny skončí za k IP adrese.

Při vytváření webu Azure názvu DNS se automaticky přiřadí ke stránce. Tento název formu ** &lt;yoursitename&gt;. azurewebsites.net**. Při přidání webu jako koncový bod Azure přenosy správce váš web přístupný pak prostřednictvím ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domény.

> [AZURE.NOTE] Pokud váš web nakonfigurovaný jako koncový bod přenosy správce, použijte **. trafficmanager.net** adresy při vytváření záznamů DNS.

> Můžete použít jenom záznamy CNAME s správce dopravy

Existují také několik typů záznamů, oba objekty mají vlastní funkce a omezení, ale weby nakonfigurování jako koncové body přenosy správce jsme pouze starat o určitém; Záznamy *CNAME* .

###<a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias

Záznam CNAME mapuje *konkrétní* DNS názvu, například **mail.contoso.com** nebo **www.contoso.com**, na jiný název domény (kanonických). V případě webů Azure pomocí Správce přenosy v síti, je název kanonický domény ** &lt;aplikace >. trafficmanager.net** název domény pro svůj profil správce dopravy. Po vytvoření záznamy CNAME pro vytváří alias ** &lt;aplikace >. trafficmanager.net** název domény. Položka CNAME vyřeší na IP adresu vašeho ** &lt;aplikace >. trafficmanager.net** název domény automaticky, takže pokud změnou IP adresu webu, není třeba provádět žádné akce.

Po přenosy přijde na správce přenosy přesměrovává potom přenosy na svůj web pomocí metody, kterou je nakonfigurovaný pro vyrovnávání zatížení. Toto je úplně průhledné návštěvníci vašeho webu. Uvidí jenom vlastní název domény ve svém prohlížeči.

> [AZURE.NOTE] Některé doménových registrátorů umožňují mapování subdomény při použití záznam CNAME, například **www.contoso.com**a ne kořenové názvů, například **contoso.com**. Další informace o CNAME záznamy najdete v dokumentaci podle svého registrátora, <a href="http://en.wikipedia.org/wiki/CNAME_record">položce Wikipedie na záznam CNAME</a>nebo <a href="http://tools.ietf.org/html/rfc1035">Názvy domén IETF - implementaci a specifikace</a> dokument.
