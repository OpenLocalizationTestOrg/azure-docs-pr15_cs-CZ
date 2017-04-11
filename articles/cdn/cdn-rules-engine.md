<properties
    pageTitle="Přepíše výchozí nastavení HTTP v Azure CDN pomocí modul pravidla | Microsoft Azure"
    description="Modul pravidla umožňuje přizpůsobit, jak jsou požadavků HTTP uskutečněných jednotlivými Azure CDN, například blokování doručení určité typy obsahu, definovat zásadu ukládání do mezipaměti a změnit HTTP záhlaví."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Přepsat výchozí chování HTTP pomocí modul pravidel

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Základní informace

Modul pravidla je možné přizpůsobit způsob zpracování žádostí o HTTP, například blokování doručení určité typy obsahu, definování zásad ukládání do mezipaměti a úpravy záhlaví HTTP.  Tento kurz se demonstrují vytvoření pravidla, které se změní chování mezipaměti CDN prostředky.  Existuje také obsahu videa k dispozici v části ",[Viz také](#see-also)".

## <a name="tutorial"></a>Kurz

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-rules-engine/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Klikněte na kartu **HTTP velký** , následovaný **Stroj pravidel**.

    Zobrazí se možnosti pro nové pravidlo.

    ![CDN nové pravidlo – možnosti](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] Pořadí, ve kterém jsou uvedeny více pravidel vliv na způsob zpracování. Následující pravidla může přepsat akcí zadaných z předchozí pravidla.
    
3. Zadejte název do pole **název a popis** textové pole.

4. Určení typu požadavky, které se u pravidla.  Ve výchozím nastavení je zaškrtnuté podmínce **vždy** POZVYHLEDAT.  Budete používat **vždy** pro účely tohoto návodu, ponechejte proto vybraný.

    ![Podmínka CDN POZVYHLEDAT](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Podmínky dostupné v rozevíracím seznamu existuje mnoho typů POZVYHLEDAT.  Po kliknutí na modrou informační ikonu vlevo od podmínka shoda vysvětluje aktuálně vybranou podmínku podrobně.
    >
    >Úplný seznam podmínek shody podrobně najdete v článku [pravidla Engine splňují podmínku a funkce podrobností](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Klikněte **+** tlačítko vedle **funkce** přidáte novou funkci.  V rozevíracím seznamu na levé straně vyberte **Interní Max-stáří platnost**.  Do textového pole, která se zobrazí zadejte **300**.  Nechte zbývající výchozí hodnoty.

    ![Funkce CDN](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Jako s podmínkami shoda, kliknutím na modré informační ikonu vlevo od nové funkce se zobrazí podrobnosti o této funkci.  V případě **Platnost interní Max-Age**jsme při uzel CDN okraj aktualizuje majetek z původ naléhavé majetku **Mezipaměti řízení** a **platnost vyprší** záhlaví na ovládací prvek.  Náš příklad 300 sekund znamená, že na uzel okraj CDN bude ukládat do mezipaměti aktiva pro 5 minut před obnovením materiálů proti původní.
    >
    >Úplný seznam funkcí podrobně najdete v článku [pravidla Engine POZVYHLEDAT podmínku a funkce podrobností](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Klikněte na tlačítko **Přidat** nové pravidlo uložte.  Nové pravidlo teď čeká schválení. Jakmile ho schválený, stav změní **Na zjištění stavu úkolů XML** **Aktivní XML**.

    >[AZURE.IMPORTANT] Pravidla změny může trvat až 90 se rozšíří prostřednictvím CDN.

## <a name="see-also"></a>Viz taky
* [Azure pátek: Azure CDN výkonné nové prémiových funkcí](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)
* [Pravidla Engine shoda podmínku a funkce podrobností](https://msdn.microsoft.com/library/mt757336.aspx)
