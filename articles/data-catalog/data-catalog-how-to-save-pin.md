<properties
   pageTitle="Jak uložit vyhledávání a připnout dat prostředky | Microsoft Azure"
   description="Článek s postupy zvýraznění funkce v katalogu dat Azure pro uložení zdroje dat a datové zdroje dat pro pozdější použití."
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
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Jak uložit vyhledávání a připnout prostředky dat

## <a name="introduction"></a>Úvod

Katalog dat Microsoft Azure poskytuje možnosti pro zjišťování zdroje. Uživatelé můžete rychle hledat a filtrování katalogu vyhledávat zdroje dat a interpretaci zamýšlený účel, usnadňuje vyhledání správná data pro práci.

Ale co v případě, že uživatelé potřebují pravidelné pracovat se stejnými daty? Co když uživatelé pravidelně přispívat své znalosti do stejné zdroje dat v katalogu? V těchto situacích museli opakovaně problémů stejné hledání může být neefektivní – to je uložená hledání a připnuté data, která můžou pomoct prostředky.

## <a name="saved-searches"></a>Uložená hledání

Uložené hledání v katalogu dat Azure je opakovaně použitelný, definice hledání jednotlivé uživatele. Jakmile vyhledávání – včetně hledaných slov, značky a další filtry – definované uživatelem si ho uložit pro pozdější použití. Definici uložené hledání může být spusťte znovu později vrátit případně zdroje dat, které splňují jeho kritéria vyhledávání.

### <a name="creating-a-saved-search"></a>Vytvoření uloženého hledání

Pokud chcete vytvořit uloženého hledání, nejdřív zadejte kritéria vyhledávání chcete znovu použít. Klikněte na odkaz "Uložit" v poli "Aktuálního hledání" na portálu katalog dat Azure.

 ![Vyberte "uložit" uložte aktuální nastavení vyhledávání](./media/data-catalog-how-to-save-pin/01-save-option.png)

Po zobrazení výzvy zadejte název uložené hledání. Vyberte název, který je přehlednější a popisný majetku dat, které budou výsledky hledání.

 ![Zadejte název pro uložené hledání](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Správa uložená hledání

Když uživatel uložil jeden nebo více hledání, možnost "– uložená hledání" se zobrazí na portálu katalog dat Azure pod polem "Aktuálního hledání". Rozbalený, zobrazí se seznam všech uloží vyhledávání.

 ![Seznam uložená hledání](./media/data-catalog-how-to-save-pin/03-list.png)

Výběr uloženého hledání v seznamu způsobí hledání tak, aby provádět.

Výběr v rozevírací nabídce se sadou možností správy:

 ![Možnosti správy uložená hledání](./media/data-catalog-how-to-save-pin/04-managing.png)

Výběr "Přejmenovat" se zobrazí výzvu k zadejte nový název pro uložené hledání. Definice hledání se nezmění.

Výběr "Odstranit" se zobrazí výzvu k potvrzení a pak odebere uložené hledání z uživatel v seznamu.

Výběr "Uložit jako výchozí" označí zvolené uložená hledání jako výchozí hledat uživatele. Pokud uživatel provede "prázdné" vyhledávání na domovské stránce katalog dat Azure, bude spouštět uživatele výchozí vyhledávání. Kromě toho hledání označen jako výchozí se zobrazí v horní části seznamu uložené hledání.

### <a name="organizational-saved-searches"></a>Organizační uložená hledání

Každý uživatel může uložit hledání pro vlastní potřebu. Správci katalogu dat lze také uložit vyhledávání pro všechny uživatele v organizaci. Při ukládání hledání, jsou správci nabídnuta možnost sdílet uložené hledání v rámci podniku. Pokud tato možnost vybrána, uložené hledání součástí seznam dostupných hledání pro všechny uživatele.

 ![Organizační uložená hledání](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Připnutý úkol dat prostředky

Uložená hledání umožňují uložení a opětovné použití definice vyhledávání. prostředky data vrácená vyhledávání v průběhu času jako obsah katalogu změnit mění. Připnutí prostředky dat umožňuje uživatelům explicitně označovat určitá data prostředky pro jejich usnadňují přístup k bez nutnosti použití hledání.

Připnutí materiálů dat je jednoduchá – uživatelů můžete jednoduše kliknout na ikonu "připnout" položka dat přidáte do svého připnutém seznamu. Tato ikona se zobrazí v dolním rohu dlaždice materiálů v zobrazení vedle sebe a ve sloupci úplně vlevo v zobrazení seznamu na portálu katalog dat Azure.

![Připnutí dat majetku](./media/data-catalog-how-to-save-pin/05-pinning.png)

Unpinning aktivum je rovnoměrně z vnější jednoduché – uživatelé jednoduše klikněte na ikonu "připnout" znovu k přepnutí nastavení pro vybraný majetek.

![Unpinning materiálů dat](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Aktiv"
Domovskou stránku portálu katalog dat Azure obsahuje oddíl "Moje aktiv", která se zobrazí prostředky potřebné k aktuálního uživatele. Tato část obsahuje obou připnuté prostředky a uložená hledání.

!["Moje aktiva" na domovské stránce](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Souhrn
Azure katalog dat obsahuje funkce, které usnadněte tak uživatelům zjišťovat zdroje dat, který potřebují, aby lidé tráví méně času hledáte data a pracovat s ním delší dobu. Uložená hledání a připnuté data, která prostředky vycházejí tyto základní funkce tak, aby uživatelé mohli snadno identifikovat zdroje dat, se kterými bude opakovaně fungovat.
