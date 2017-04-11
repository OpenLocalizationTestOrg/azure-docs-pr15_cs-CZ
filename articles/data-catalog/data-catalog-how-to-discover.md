<properties
   pageTitle="Jak zjišťovat zdroje dat | Microsoft Azure"
   description="Článek s postupy zvýraznění způsob zjištění aktiv registrovaných dat pomocí katalogu dat Azure, včetně hledání a filtrování a použití přístupů zvýraznění funkcí katalogu dat Azure portálu."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Jak zjišťovat zdroje dat

## <a name="introduction"></a>Úvod
**Katalog dat Microsoft Azure** je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. **Katalog dat Azure** jinými slovy, je celé o tom pomáhá lidé zjišťovat, pochopit a získání další hodnoty z existujících dat pomocí zdrojů dat a nápovědu organizace. Jakmile registrované zdroje dat s **Katalogem dat Azure**, jeho metadata indexováno službou, tak, aby uživatelé můžete snadno vyhledat a objevovat dat, který potřebují.

## <a name="searching-and-filtering"></a>Hledání a filtrování

Vyhledávání v **Katalogu dat Azure** používá dva hlavní mechanismy: hledání a filtrování.

Hledání je navržen intuitivní a výkonných – ve výchozím nastavení, hledané termíny splnění proti libovolného vlastnosti v katalogu, včetně uživatelů – za předpokladu, že poznámky.

Filtrování navržen jako doplněk vyhledávání. Uživatele můžete vybrat konkrétní vlastností, jako je třeba odborníků zdroj dat, typ objektu a značky, chcete-li zobrazit pouze odpovídající majetku dat a chcete omezit výsledky hledání na odpovídající majetek i.

Pomocí kombinace hledání a filtrování uživatelů můžete rychle přejít zdroje dat, které byly registrovaný u **Katalog dat Azure** můžete zjišťovat zdroje dat, který potřebují.

## <a name="search-syntax"></a>Syntaxe vyhledávacího

Vyhledávání volných výchozí je jednoduchý a intuitivní, sice uživatelé mohou také pomocí syntaxe vyhledávacího **Katalog dat Azure**mít větší kontrolu nad výsledky hledání. Hledání v **Katalogu dat Azure** podporuje následující postupy:

| Postup                 | Použití                                                                                                                                     | Příklad                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Základní hledaný              | Základní hledaný pomocí jedné nebo více hledaných slov. Výsledky jsou prostředky, které splňují všechny vlastnost s jedním nebo víc podmínek definovaných. | data o prodeji                                                |
| Definování rozsahu vlastnost          | Pouze vrátíte zdroje dat, kde je spárované hledaný výraz s určitou vlastnost                                                   | Název: finance                                              |
| Logické operátory         | Rozšíření nebo zúžení vyhledávání pomocí logické operace                                                                                     | Finance není podnikové                                     |
| Seskupení s okrouhlá závorka | Použití závorek skupiny pohled na část dotazu k dosažení logické izolace, zejména v souvislosti s logické operátory              | Název: finance a (značky: Q1 nebo značky: Q2) |
| Relační operátory      | Porovnání než rovnosti určenou vlastnosti, které mají číselná a datum datových typů                                                | modifiedTime > "11/05/2014"                                 |

Další informace o hledání v **Katalogu dat Azure** najdete v článku [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Přístupů zvýraznění
Při prohlížení výsledků hledání, zvýrazní zobrazené vlastnosti, které splňují zadané hledané termíny – třeba dat materiálů název, popis a značky – můžete usnadnit její k identifikaci proč daného datového materiálů vrátil dané vyhledávání.

> [AZURE.NOTE] Uživatele můžete zapnout přístupů zvýrazňování vypnout, v případě potřeby přepínačem "Zvýraznění" na portálu **Katalog dat Azure** .

Při prohlížení výsledků hledání, nemusí být vždy zjevných proč materiálů dat je součástí, dokonce i přístupů zvýraznění povolené. Protože jsou ve výchozím nastavení prohledávat všechny vlastnosti materiálů dat může vrácená kvůli shodu na úrovni sloupce vlastnosti. A protože více uživatelů může pořizovat poznámky v registrovaná data majetku s vlastní značky a popisy, ne všechny metadat se může zobrazovat v seznamu výsledků hledání.

Ve výchozím zobrazení vedle sebe, jednotlivé dlaždice zobrazí ve výsledcích hledání budou obsahovat ikonu "odpovídá hledaný výraz se použije zobrazení", což umožňuje uživateli rychle zobrazit počet shody a jejich umístění a odkazů na ně v případě potřeby.

 ![Přístupů zvýraznění a jsou vraceny vyhledávání v katalogu dat Azure portálu](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Souhrn
Registrace zdroje dat s **Katalogem dat Azure** snazší daného zdroje dat zjišťovat a interpretaci, kopírování strukturální a popisný metadata ze zdroje dat do katalogu služby. Jakmile zaregistroval zdroj dat uživatelů můžete zjistit pomocí filtrování a hledání z v **Katalogu dat Azure** portálu.

## <a name="see-also"></a>Viz taky
- [Začínáme s katalogem dat Azure](data-catalog-get-started.md) kurz podrobné informace o tom, jak zjišťovat zdroje dat
