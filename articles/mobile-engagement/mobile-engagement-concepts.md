<properties
    pageTitle="Zapojení Mobile koncepty | Microsoft Azure"
    description="Azure Mobile zapojení koncepty"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile zapojení koncepty

Zapojení Mobile definuje několik koncepty běžné všechny podporované platformy. Tento článek popisuje stručně tyto koncepty.

Tento článek je vhodné spustit Pokud začínáte Mobile Engagement. Zkontrolujte taky dokumentaci specifické pro platformu, který používáte, jak bude upřesnění koncepty popisované v tomto článku se další informace a příklady i možné omezení.

## <a name="devices-and-users"></a>Zařízení a uživatelů
Zapojení Mobile identifikuje uživatele tak, že generování jedinečný identifikátor u každého zařízení. Tento identifikátor se nazývá identifikátor zařízení (nebo `deviceid`). Vygeneruje se tak, že všechny spuštěné aplikace stejného zařízení sdílet stejnou identifikátor zařízení.

Implicitně znamená, že Mobile zapojení byly použity jednoho zařízení patří k jednomu uživateli a tedy uživatelům a zařízením jsou odpovídající koncepty.

## <a name="sessions-and-activities"></a>Relace a aktivity
Relace je jedno použití aplikace provedly uživatele, od doby spuštění používat, dokud tabulátoru uživatele.

Aktivita je jedno použití dané dílčí součástí aplikace provedly jednomu uživateli (je obvykle na obrazovku, ale může být nic vhodnou aplikaci).

Uživatele můžete provádět pouze jedné najednou.

Aktivity je určen název (omezený až 64 znaků) a můžete volitelně vložit některé další data (limit 1 024 bajtů).

Relace se automaticky vypočítané z řady aktivit uživatelů. Relace, spustí se uživatel spustí jeho první činnosti a zastavíte mu dokončí svou činnost poslední. To znamená, že relaci nemusí explicitně spustit nebo zastavit. Aktivity místo toho explicitně spuštěním nebo zastavit. Jestliže žádná činnost, je uveden žádné relace.

## <a name="events"></a>Události
Události se používají k vykazování rychlé akce (třeba stisknutým tlačítkem nebo články, které nečte uživatelů).

Zvláštní události můžete související s aktuálním relace spuštěná úloha nebo může být událost nastavit jako samostatný.

Událost je určen název (omezený až 64 znaků) a můžete volitelně vložit některé další data (limit 1 024 bajtů).

## <a name="error"></a>Chyba
Chyby se používají k oznamujte problémy správně detekovaný aplikaci (třeba nesprávné uživatelské akce nebo selhání volání rozhraní API).

Chybu můžete související s aktuálním relace spuštěná úloha nebo může být na samostatný chybu.

Chyba je určen název (omezený až 64 znaků) a můžete volitelně vložit některé další data (limit 1 024 bajtů).

## <a name="job"></a>Úlohy
Práce se používají k vykazování akce s dobou trvání (třeba dobu trvání volání rozhraní API, zobrazí čas reklam, dobu trvání úlohy na pozadí nebo doby trvání akcí uživatele).

Úlohy nesouvisí s relaci, protože úkol lze provést na pozadí, bez zásahu uživatele.

Úlohy je určen název (omezený až 64 znaků) a můžete volitelně vložit některé další data (limit 1 024 bajtů).

## <a name="crash"></a>Dojde k chybě
Dojde k chybě se automaticky vystaven SDK zapojení Mobile vykazování chybách kde problémy nerozpozná aplikace usnadňují havárie.

## <a name="application-information"></a>Informace o aplikaci
Informace o aplikaci (nebo informace o aplikaci) slouží k označení uživatelů, tedy přidružíte některá data uživatelům aplikace (Toto je podobný soubory cookie na webu, s tím rozdílem, že aplikace informace jsou uložena na straně serveru platformy Azure Mobile zapojení).

Informace o aplikaci můžete registrované pomocí rozhraní API Mobile zapojení SDK nebo pomocí platformu zapojení mobilních zařízení API.

Informace o aplikaci je pár klíč/hodnota související s zařízení. Klíč stejný název aplikace info (omezené 64 ASCII písmena [a-zA-Z], [0 – 9] číslice a podtržítka [_]). Hodnota (omezený na 1 024 znaků) může být řetězec, celé číslo, datum (rrrr MM-dd) nebo logickou hodnotu (PRAVDA nebo NEPRAVDA).

Libovolný počet informace app může být přidružené k zařízení v určených mezích definované podmínky ceny Mobile zapojení. Pro dané jen jednu klávesu mobilní zapojení pouze uchovává informace o nejnovějších nastavená hodnota (bez historie). Nastavení nebo změna hodnoty informace o aplikaci vynutí Mobile zapojení znovu vyhodnotí cílové skupiny sadu kritérií na tyto informace o aplikaci (pokud existuje) znamená tyto informace aplikace mohou sloužit k aktivace posune reálném čase.

## <a name="extra-data"></a>Další data
Navíc dat (nebo doplňky) jsou některé libovolného data, která může být připojen k události, chyby, aktivity a úlohy.

Extra jsou strukturovány podobně JSON objektů: jejich vzniku stromové párů klíč/hodnota. Klávesy se omezuje 64 ASCII písmena [a-zA-Z], [0 – 9] čísel a podtržítka [_]) a celkovou velikost extra se omezí na 1 024 znaky (jednou zakódovaný JSON SDK zapojení mobilní telefon).

Celý strom párů klíč/hodnota je uložena jako objekt JSON. Však je rozložený přístupný přímo některé pokročilé funkce podobně jako segmenty pouze první úrovně klíčů/hodnot (například snadno se můžete definovat segment s názvem "SciFi ventilátory", které je tvořen všichni uživatelé s odeslané aspoň 10krát události s názvem "content_viewed" extra klíčové "hodnota content_type" nastavena na hodnotu "scifi" v minulý měsíc). Proto doporučujeme k odeslání, které pouze extra jednoduché seznamů párů klíč/hodnota skalární hodnotami (například řetězce, kalendářní data, celých čísel nebo logickou hodnotu).

## <a name="next-steps"></a>Další kroky

- [Přehled univerzální sady Windows SDK pro zapojení Mobile Azure](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight SDK přehled pro zapojení Mobile Azure](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK pro zapojení Mobile Azure](mobile-engagement-ios-sdk-overview.md)
- [Android SDK pro Azure mobilní zasunutí](mobile-engagement-android-sdk-overview.md)
