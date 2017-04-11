<properties
    pageTitle="Azure AD Connect synchronizace: technické koncepty | Microsoft Azure"
    description="Tento článek vysvětluje technické koncepcí Azure AD Connect synchronizace."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect synchronizace: technické koncepty
Tento článek je přehled v tématu [Principy architektury](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect synchronizace vychází z platformy plné metadirectory synchronizace.
V následujících částech Představte koncepty metadirectory synchronizace.
Vytváření po MIIS, nástroj ILM a FIM Azure Active Directory Sync Services poskytuje další platformu pro připojení ke zdrojům dat, synchronizace dat mezi zdroje dat, stejně jako zřizování a zrušení zajišťování identit.

![Technické koncepty](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

V následujících částech jsou podrobněji popsány následující aspekty FIM synchronizační službu:

- Spojnice
- Atribut toku
- Prostor konektoru
- Metaverse
- Zřízení

## <a name="connector"></a>Spojnice

Moduly kódu, které se používají pro komunikaci s adresářem a připojení se označují jako spojnice (dříve označované jako agenti management (MAs)).

Tyto nainstalovaných v počítači se službou Azure AD Connect synchronizace.
Spojnice umožňují agentless komunikaci pomocí vzdálený systém protokolů namísto nasazení specializované agentů. Znamená to, že snížení rizik a nasazení časy, zejména se týkají důležitých aplikací a systémy.

Na obrázku nahoře spojnice je synonymem prostor konektoru ale zahrnuje všechny komunikace s externímu systému.

Spojnice je zodpovědný za všechny import a export funkce do systému a uvolnění vývojáři z museli pochopit, jak se připojit k každý systém nativně při přizpůsobení transformace dat pomocí deklarativní zřizování.

Importy a export pouze dochází při plánování, umožňující další izolace před změnami vyskytující se v systému, protože změny nejsou automaticky šířit na zdroje dat připojeného. Vývojáři navíc může vytvářet vlastní spojnice připojit se k libovolných zdrojů dat.

## <a name="attribute-flow"></a>Atribut toku

Metaverse je konsolidované zobrazení všech spojených identit od sousedních spojnice mezery. Na obrázku nahoře je znázorněn atribut toku čarami pomocí šipek pro tok příchozích a odchozích. Atribut toku je proces kopírování nebo transformace dat z jednoho systému do jiného a všech atributů toků (příchozí nebo odchozí).

Atribut tok probíhá mezi prostor konektoru a metaverse bi obousměrně při plánování operace (celé nebo delta) synchronizace spustit.

Atribut toku pouze tehdy, tyto synchronizace se mají spustit. Atribut toků jsou definované v pravidlech synchronizace. Může se jednat příchozí (ISR na obrázku nahoře) nebo odchozí (OSR na obrázku nahoře).

## <a name="connected-system"></a>Připojení systému

Připojení systému (označovaná taky jako připojený adresář) je odkazující na vzdálený systém Azure AD Connect synchronizace připojený ke a čtení a zápis dat identity do a z.

## <a name="connector-space"></a>Prostor konektoru

Jednotlivé zdroje dat připojeného představuje filtrované podmnožinu objekty a atributy do pole spojnice.
To umožňuje synchronizační službu k ovládání místně bez nutnosti kontaktovat vzdálený systém při synchronizaci objekty a omezením interakce importy a exportuje pouze.

Zdroje dat a konektor obsahujících schopnost poskytovat na seznam změn (Importovat delta), potom provozní efektivity zvětšuje výrazně pouze změny od výměně poslední obrázku hlasování. Prostor konektoru insulates zdroji připojená data před změnami šíření automaticky vyžadováním, plán spojnice importuje a exportuje. Tento přidané pojištění uděluje míru paměti při testování, náhled nebo potvrzení příští aktualizace.

## <a name="metaverse"></a>Metaverse

Metaverse je konsolidované zobrazení všech spojených identit od sousedních spojnice mezery.

Propojovat identit a autorita přiřazen různé atributy prostřednictvím import toku mapování, objektu centrální metaverse začne souhrnné informace z více systémů. Z tohoto objektu atributu tok mapování údaj pro odchozí systémy.

Objekty se vytvářejí při systému autoritativní projekty je do metaverse. Jakmile všechna připojení odeberete, objektu metaverse se odstraní.

Objekty v metaverse nelze upravovat přímo. Až atribut toku musí přispěla všechna data v objektu. Metaverse udržuje trvalý spojnic pomocí každé spojnice mezery. Tato spojnice není nutné zadávat posouzení pro každou synchronizace. To znamená, že Azure AD Connect synchronizace nemá Najděte odpovídající vzdálené objekt pokaždé, když. To je zabráněno nutnosti nákladné agentů zákaz změn atributů, které by se normálně zodpovědný za korelace objekty.

Při zjišťování nových zdrojů dat, které mohou mít existující objekty, které je třeba spravovat, používá Azure AD Connect synchronizace proces s názvem pravidlo spojení k vyhodnocení možných kandidátů, se kterým lze vytvořit propojení.
Jakmile je vytvořeno odkaz toto hodnocení výskytu a normální atribut toku může dojít mezi zdroji vzdálené připojená data a metaverse.

## <a name="provisioning"></a>Zřízení

Když autoritativní zdroj projekty nového objektu do metaverse na nové místo objekt spojnice lze vytvořit v jiné spojnici představující zdroj podřízené připojená data.

To podstatě vytvoří propojení a atribut toku bi obousměrně pokračovat.

Pokaždé, když pravidla určuje, že na nové místo objekt spojnice musí být vytvořen, je označená jako zřizování. Však protože tuto operaci provedeno pouze prostoru spojnice, ho nepřenesou do zdroje dat připojeného, dokud se provádí export.

## <a name="additional-resources"></a>Další zdroje informací

* [Azure AD připojení synchronizace: Přizpůsobení možností synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
