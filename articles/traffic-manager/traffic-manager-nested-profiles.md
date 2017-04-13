<properties
    pageTitle="Vnořené profily přenosy správce | Microsoft Azure"
    description="Tento článek vysvětluje funkce vnořené profily Správce dopravy Azure"
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

# <a name="nested-traffic-manager-profiles"></a>Vnořené profily přenosy správce

Přenosy správce obsahuje velké množství směrování přenosu metody, které umožňují řídit, jak přenosy správce vybírá, který koncový bod mají dostávat přenosy z každé koncového uživatele. Další informace najdete v článku [směrování přenosu metod přenosy správce](traffic-manager-routing-methods.md).

Každý profil správce přenosy Určuje jednu metodu směrování přenosu. Můžou ale nastat scénáře, které vyžadují složitější směrování přenosu než směrování poskytovanou jeden profil přenosy správce. Můžete do sebe vnořit profily přenosy správce umožňují kombinovat výhody více než jednu metodu směrování přenosu. Vnořené profily umožňují změnit výchozí chování přenosy správce podpora větších a složitější nasazení aplikace.

Následující příklady ukazují, jak používat vnořené profily přenosy správce v různých situacích.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Příklad 1: Sloučení "Výkon" a "Vážená" směrování přenosu

Předpokládejme nasazení aplikace v těchto oblastech Azure: západní USA, západní Evropy a jihovýchodní Asie. Použijte metodu směrování přenosu "Výkonu" přenosy správce k distribuci umožnění datových přenosů do oblasti nejblíže k uživatele.

![Správce přenosy jednoduchým profilu][1]

Předpokládejme, že chcete otestovat aktualizaci služby před široce postupných informace. Chcete-li použijte metodu "Vážený" směrování přenosu směrování malé procento umožnění datových přenosů do zkušební nasazení. Nastavit zkušební nasazení společně s existujícím provozním nasazením v Evropě západní.

Nejde kombinovat obou vážená a "výkonu přenosy směrovací v jednom profilu. K podpoře tento scénář, vytvoříte profil správce přenosy pomocí dvou koncové body západní Evropy a metodu směrování přenosu "Vážená". Pak přidáte tento profil "dětmi" jako koncový bod profilu "nadřazené". Profil nadřazené pořád používá metodu směrování přenosu výkonu a obsahuje ostatní globální nasazení jako koncové body.

Následující diagram ukazuje tento příklad:

![Vnořené profily přenosy správce][2]

V této konfiguraci data prostřednictvím profilu nadřazené rozdělí přenosy oblastí běžným způsobem. V rámci západní Evropy distribuuje vnořené profilu umožnění datových přenosů do výrobní a otestujte koncové body podle gramáží přiřazené.

Pokud profilu nadřazeného používá metodu směrování přenosu "Výkonu", musí mít každý koncový bod přiřazené umístění. Umístění se přiřadí při konfiguraci koncový bod. Výběr oblasti Azure nejblíže k nasazení. Azure oblastí jsou hodnoty umístění nepodporuje tabulce latence Internet. Další informace najdete v tématu [přenosy správce "Výkonu" metody směrování přenosu](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Příklad 2: Koncový bod sledování vnořené profily

Přenosy správce aktivně sleduje stav každé koncový bod služby. Při chybná koncový bod správce provoz směrovat tak, aby uživatelům alternativní koncové body zachovat dostupnost služby. Toto chování sledování a převzetí koncový bod platí pro všechny přenosy směrování způsoby. Další informace najdete v tématu [Sledování koncový bod přenosy správce](traffic-manager-monitoring.md). Sledování koncový bod vyhovovat jinak vnořené profily. S vnořené profily profilu nadřazené nechová kontroly stavu na podsložky přímo. Místo toho stavu koncové body profilu podřízené slouží k výpočtu celkový stav podřízené profilu. Tyto informace stavu proběhne hierarchii vnořené profilu. Profil nadřazené to agregované stavu k určení, zda budou přímé umožnění datových přenosů do profilu podřízené. Naleznete v části [Nejčastější dotazy týkající se](#faq) tohoto článku úplné informace o stavu sledování vnořené profilů.

Vrací jako v předchozím příkladu, Předpokládejme, že dochází k selhání nasazení výrobní v Evropě západní. Ve výchozím nastavení profilu "dětmi" směrovat tak, aby všechny přenosy na zkušební nasazení. Když se zkušební nasazení také nepovede, profilu nadřazené určí, jakým, neměli profilu podřízené vzhledem k tomu, že jsou všechny koncové body podřízené chybná dostávat přenosy. Potom profilu nadřazené distribuuje umožnění datových přenosů do jiných oblastí.

![Vnořené profilu převzetí (výchozí nastavení)][3]

Je možné s tohoto ujednání. Nebo může být, že všechny přenosy pro západní Europe teď bude zkušební nasazení místo přenosy omezené podmnožina. Bez ohledu na stav zkušební nasazení Chcete-li převzít do jiných oblastí, selhání nasazení výrobní v Evropě západní. Povolit tento překlopení, můžete použít parametr "MinChildEndpoints" při konfiguraci profilu podřízené jako koncový bod v nadřazené profilu. Parametr určuje minimální počet dostupných koncové body v podřízené profilu. Výchozí hodnota je "1". V tomto scénáři nastavte hodnotu MinChildEndpoints 2. Pod tento mezní profilu nadřazené za celou podřízené profil je k dispozici a umožnění datových přenosů do jiných koncové body směrovat tak, aby.

Následující obrázek znázorňuje tento konfigurace:

![Vnořené převzetí profil s "MinChildEndpoints" = 2][4]

>[AZURE.NOTE]
>Metoda směrování přenosu "Priorita" distribuuje všechny přenosy na jeden koncový bod. V nastavení MinChildEndpoints než '1' tedy není malé účel podřízené profilu.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Příklad 3: Přednostně převzetí oblastí v směrování přenosu "výkon.

Výchozí chování metodu směrování přenosu "Výkonu" slouží povolená načítání další nejbližší koncového bodu a jak příčinou CSS řadu k chybám. Pokud koncový bod selže, všechna data, která by vás k této koncový bod rovnoměrně úměrně jiných koncové body ve všech oblastech.

![Směrování s výchozí převzetí přenosy "výkon.][5]

Předpokládejme však radši převzetí přenosy západní Europe západní USA a pouze směrovaly do jiných oblastí když oba koncové body nejsou k dispozici. Můžete vytvořit toto řešení s profilem podřízené směrování přenosu metodou "Priorita".

![Směrování s preferenčních převzetí přenosy "výkon.][6]

Protože koncový bod západní Europe musí mít vyšší prioritu než koncový bod západní USA, všechny přenosy jsou odeslány koncový bod západní Europe jsou oba koncové body online Když západní Europe nepovede, je jeho přenos směrovány západní USA. Vnořené profilu přenosy přesměrován jihovýchodní Asie pouze když západní Evropa a západní USA.

Opakujte tento vzor pro všechny oblasti. Nahraďte všechny tři koncové body v nadřazené profilu tři podřízené profily každý poskytující posloupnost uspořádaný překlopení.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Příklad 4: Řízení 'Výkon' směrování přenosu mezi více koncové body ve stejné oblasti

Předpokládejme "Výkonu" směrování přenosu metoda slouží profil, který obsahuje více než jeden koncový bod v určité oblasti. Ve výchozím nastavení je ve všech dostupných koncové body v dané oblasti rovnoměrně přenosy přesměrují do této oblasti.

!["Výkonu" provoz směrování přenosu v oblasti rozdělení (výchozí nastavení)][7]

Namísto přidávání víc koncové body v Evropě západní, jsou tyto koncové body uzavřeny v samostatné podřízené profilu. Profil podřízené je přidán do nadřazeného jako pouze koncového bodu v Evropě západní. Nastavení v podřízené profilu můžete určit přenosy rozdělení západní Europe povolením podle priority nebo vážené přenosy směrování v rámci dané oblasti.

![Směrování rozdělení vlastní přenosy v oblasti přenosy "výkon.][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Příklad 5: Nastavení sledování na koncový bod

Předpokládejme, že používáte přenosy správce hladce migrace adres odkazem místní webu novou verzi cloudové hostované v Azure. Starší verze lokality chcete použít na domovskou stránku URI sledování stavu webů. Pro nové verze cloudové jsou implementace vlastního sledování stránky, ale (cestu "/ monitor.aspx"), který bude zahrnovat další kontroly.

![Koncový bod přenosy správce sledování (výchozí nastavení)][9]

Nastavení sledování v profilu přenosy správce platí pro všechny koncové body v rámci jednoho profilu. Vnořené profily můžete pomocí různých podřízené profilu na jeden web s nastavení různých sledování.

![Koncový bod přenosy správce sledování s nastavením za koncový bod][10]

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

### <a name="how-do-i-configure-nested-profiles"></a>Konfiguraci vnořené profilů

Vnořené profilů přenosy Manager je možné konfigurovat pomocí Správce prostředků Azure i klasické Azure REST API, Azure PowerShell a různé platformy rozhraní příkazového řádku Azure příkazy. Jsou taky podporované prostřednictvím portálu nové Azure. Nejsou podporovány v portálu klasické.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Počet úrovní vnoření slouží přenosy správce pro školy?

Můžete do sebe vnořit profily až 10 úrovní vnoření. "Cykly nejsou povoleny.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Můžete kombinovat dalších typů koncového bodu s vnořené podřízené profilů v stejný jako profil přenosy správce?

Ano. Neplatí žádná omezení jak kombinovat koncové body různých typů v profilu.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Jak platí fakturační modelu profilů vnořené?

Je už záporné ceny dopad používání vnořených profilů.

Přenosy správce fakturace má dvě součásti: kontroly stavu koncového bodu a miliony DNS dotazů

- Kontroly stavu koncového bodu: je bezplatná podřízené profilu po nakonfigurování jako koncový bod v nadřazené profilu. Sledování koncové body v podřízené profilu se vám účtovat poplatky obvyklým způsobem.
- Dotazy DNS: obou dotazů se počítá jenom jednou. Dotaz na nadřazené profil, který vrátí koncový bod z podřízené profilu se počítá proti na nadřazené profil.

Úplné podrobnosti najdete v tématu [přenos správce ceny stránka](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Existuje dopad na výkon vnořené profilů?

Ne. Neexistuje žádný dopad na výkon při používání vnořených profilů.

Přenosy správce názvové servery procházet hierarchii profilu interně při zpracování obou dotazů DNS. Dotaz DNS do nadřazené profilu dostávat odpovědi DNS ke koncovému z podřízené profilu. Jestli používáte jeden profil nebo vnořené profily se používá jeden záznam CNAME. Je potřeba vytvořit záznam CNAME pro každého profilu v hierarchii.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Jak přenosy správce výpočet stavu vnořené koncového bodu v nadřazené profilu?

Profil nadřazené nechová kontroly stavu na podsložky přímo. Místo toho stavu koncové body profilu podřízené slouží k výpočtu celkový stav podřízené profilu. Tyto informace se rozšíří hierarchii vnořené profilu stav vnořené koncového bodu. Profil nadřazené používá tento agregované zdraví a zjistit, zda lze provoz směrovat podsložky.

Následující tabulka popisuje chování z provozu správce stavu hledá vnořené koncového bodu.

|Podřízené profilu sledování stavu|Nadřazené koncový bod sledování stavu|Poznámky|
|---|---|---|
|Zakázané. Profil podřízené byl zakázán.|Přerušili|Stav koncového bodu nadřazené zastaven, není zakázané. Vypnutí stavu je rezervován pro označující, že jste zakázali koncového bodu v nadřazené profilu.|
|Sníženou kvalitu. Koncový bod profilu alespoň jeden podřízené je ve stavu snížený.| Online: počet Online koncové body v profilu podřízené je minimální hodnota MinChildEndpoints.<BR>CheckingEndpoint: počet Online plus CheckingEndpoint koncové body v profilu podřízené je minimální hodnota MinChildEndpoints.<BR>Sníženou kvalitu: jinak.|Přenosy směrován k koncový bod stav CheckingEndpoint. Pokud MinChildEndpoints je nastavený moc vysoké, koncový bod vždy sníženou kvalitu.|
|Online. Koncový bod profilu alespoň jeden podřízené je Online stavu. Žádný koncový bod je ve stavu snížený.|Viz výše.||
|CheckingEndpoints. Koncový bod profilu alespoň jeden podřízené je "CheckingEndpoint". Žádné koncové body jsou "Online" nebo "Sníženou kvalitu."|Stejně jako nahoře.||
|Neaktivní. Všechny koncové body podřízené profilu jsou zakázané nebo zastaveno nebo profil obsahuje koncové body.|Přerušili||


## <a name="next-steps"></a>Další kroky

Další informace o tom, [Jak funguje přenosy správce](traffic-manager-how-traffic-manager-works.md)

Zjistěte, jak [vytvořit profil správce dopravy](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

