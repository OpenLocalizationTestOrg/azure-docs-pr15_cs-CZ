<properties
    pageTitle="Odstranění koncového bodu Azure CDN | Microsoft Azure"
    description="Naučte se z koncového bodu CDN vymazat všechny obsah v mezipaměti."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Odstranění koncového bodu Azure CDN

## <a name="overview"></a>Základní informace

Azure uzly okraj CDN bude ukládat do mezipaměti prostředky do vypršení platnosti majetku time to live (TTL).  Když je majetek TTL vyprší jejich platnost, pokud klient požaduje majetku z uzel hrany, uzel hrany načte aktualizované novou kopii materiálů a bude předávat požadavek klienta a úložiště aktualizovat mezipaměti.

Může někdy budete chtít vymazání mezipaměti obsah ze všech uzlů okraj a vynutit uspořádat načíst nový aktualizované prostředky.  Může to být kvůli aktualizace vaší webové aplikaci a rychle aktualizace prostředky, které obsahují nějaké informace nejsou správné.

> [AZURE.TIP] Všimněte si, že vymazání pouze slouží k vymazání mezipaměti obsahu na serverech CDN okraj.  Všechny podřízené mezipaměti, jako jsou proxy servery a místním prohlížeči mezipaměti, může pořád podržte režim cached kopii souboru.  Je důležité si zapamatovat, pokud nastavíte do souboru time-to-live.  Můžete vynutit podřízené klienta požádat o nejnovější verzi souboru tím, že ho jedinečný název pokaždé, když ho aktualizujete nebo využijete [ukládání do mezipaměti řetězce dotazu](cdn-query-string.md).  

Tento kurz vás provede vymazání prostředky ze všech uzlů okraj koncového bodu.

## <a name="walkthrough"></a>Návod

1. Na [Portálu Azure](https://portal.azure.com)procházením profilu CDN obsahující koncový bod, který chcete odstranit.

2. Z profilu zásuvné CDN klikněte na tlačítko Vymazat.

    ![Zásuvné CDN profilu](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Vymazání zásuvné otevře.

    ![Vymazání zásuvné CDN](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Na zásuvné vymazání vyberte adresy služby, kterou chcete odstranit z rozevíracího seznamu Adresa URL.

    ![Vymazání formuláře](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Můžete taky dostali na zásuvné vymazat kliknutím na tlačítko **Vymazat** na zásuvné CDN koncového bodu.  V takovém případě do pole **Adresa URL** bude předem vyplněné s adresou služby této konkrétní koncového bodu.

4. Vyberte jaké prostředky chcete vymazat ze strany uzlů.  Pokud chcete vymazat všechny prostředky, zaškrtněte políčko **Vymazat vše** .  Jinak, zadejte úplnou cestu každého majetku chcete vymazat (například `/pictures/kitten.png`) v textovém poli **cestu** .

    > [AZURE.TIP] Další **cestu** textová pole se zobrazí po zadejte text, který umožňuje sestavit seznam více prostředky.  Prostředky můžete odstranit ze seznamu kliknutím na tlačítko se třemi tečkami (...).
    >
    > Cesty musí být relativní adresy URL, která přizpůsobit následující [regulárních výrazů](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  **Azure CDN z Verizon** (Standard a Premium) hvězdička (\*) mohou sloužit jako zástupný znak (například `/music/*`).  Zástupné znaky a **Vymazat všechny** nejsou povoleny s **Azure CDN z Akamai**.
    
5. Klikněte na tlačítko **Odstranit** .

    ![Tlačítko Vymazat](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Odstranění žádosti o převzetí přibližně 2 – 3 minuty zpracuje s **Azure CDN z Verizon** (Standard a Premium) a přibližně 7 minut s **Azure CDN z Akamai**.  Azure CDN může obsahovat maximálně 50 souběžné odstranění žádosti o v daném okamžiku. 

## <a name="see-also"></a>Viz taky
- [Předem načíst prostředky na koncový bod Azure CDN](cdn-preload-endpoint.md)
- [Azure CDN REST API odkaz – vymazání nebo předem načtení koncový bod](https://msdn.microsoft.com/library/mt634451.aspx)
