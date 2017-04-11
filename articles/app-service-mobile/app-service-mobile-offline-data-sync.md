<properties
    pageTitle="Synchronizace dat v režimu offline v Azure mobilních aplikací | Microsoft Azure"
    description="Koncepční odkaz a základní informace o funkci synchronizace offline dat k aplikacím Mobile Azure"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Synchronizace dat v režimu offline v Azure mobilní aplikace

## <a name="what-is-offline-data-sync"></a>Co je dat v režimu offline synchronizace?

Data v režimu offline synchronizace je klienta a serveru SDK funkce Azure mobilních aplikací, který usnadňuje vývojářům vytvářet aplikace, které jsou funkční bez připojení k síti.

Pokud aplikace v offline režimu, uživatelé pořád vytvářet a upravovat data, která budou uloženy na místním úložišti. Po návrat do online režimu aplikace ho místní změny synchronizovat s vaší back-end Azure mobilní aplikaci. Funkce podporuje také pro zjišťování konflikty při stejném záznamu se změní na klienta a back-end. Konflikty můžete přistupovat klikněte buď na serveru nebo klienta.

Offline synchronizace má několik výhod:

* Zlepšení rychlostí reakce aplikace pomocí ukládání do mezipaměti dat serveru místně na zařízení
* Vytváření robustních aplikací, které ještě zbývají užitečné, pokud existují problémům se sítí
* Povolit koncoví uživatelé vytvářet a upravovat i v případě žádné přístup k síti, podpora scénářů s malým nebo bez připojení data
* Synchronizace dat na víc zařízeních a zjistit konflikty při změně stejný záznam tak, že dvě zařízení
* Omezení použití sítě v sítích vysokou latencí nebo s měřením dat

Následující výukové programy pro zobrazení, jak lze přidat offline synchronizace pro mobilní klienty pomocí Azure mobilní aplikace:

* [Android: Povolení offline synchronizace]
* [iOS: Povolení offline synchronizace]
* [Xamarin iOS: Povolení offline synchronizace]
* [Xamarin Android: Povolení offline synchronizace]
* [Xamarin.Forms: Povolit offline synchronizace](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Univerzální platformu Windows už: Povolení offline synchronizace]

## <a name="what-is-a-sync-table"></a>Co je tabulka synchronizace?

Koncový bod "/ tabulky" zobrazíte Azure mobilních klientů SDK zajistit rozhraní, jako `IMobileServiceTable` (.NET klient SDK) nebo `MSTable` (iOS klienta). Tato rozhraní API připojit přímo do back-end Azure mobilní aplikaci a selže, pokud klientského zařízení nemá připojení k síti.

Podpora offline použití, aplikace místo používejte *synchronizace tabulky* rozhraní API, například `IMobileServiceSyncTable` (.NET klient SDK) nebo `MSSyncTable` (iOS klienta). Stejné CRUD operace (vytvoření, čtení, aktualizace a odstranění) nefungují před synchronizací tabulky rozhraní API, ale teď bude číst nebo psát do *místní úložiště přihlašovacích údajů* Předtím, než lze provést všechny operace synchronizace tabulky, musí být spuštěn v místním úložišti.

## <a name="what-is-a-local-store"></a>Co je místní úložiště?

Místní úložiště je vrstvy trvalé dat na klientského zařízení. Klient SDK Azure mobilní aplikace poskytují implementace výchozí místní úložiště přihlašovacích údajů. Ve Windows, Xamarin a Android je založena na SQLite; IOS je založeno na datech Core.

Použití implementaci SQLite založené na Windows Phone nebo Windows 8.1 úložiště, budete potřebovat k instalaci SQLite rozšíření. Další informace najdete v tématu [univerzální platformu Windows už: Povolení offline synchronizace]. IOS a Android dodat verzí SQLite v operačním systému zařízení, takže není nutné zadávat neodkazuje vlastní verzi SQLite.

Vývojáři implementovat vlastní místní úložiště přihlašovacích údajů. Například pokud chcete uložit data ve formátu šifrované na mobilních klientů, můžete definovat místní úložiště, které používá SQLCipher šifrování.

## <a name="what-is-a-sync-context"></a>Co je kontext synchronizace?

*Synchronizace kontextu* je přidružená k objektu mobilního klienta (například `IMobileServiceClient` nebo `MSClient`) a sleduje změny provedené v tabulkách synchronizace. Místní synchronizace udržuje *operace fronty* , který uchovává uspořádaných seznamů vytvoření operace (vytvoření, aktualizace, odstranění) se později odeslaných na server.

Místní úložiště souvisí s místní synchronizace pomocí metody inicializace jako `IMobileServicesSyncContext.InitializeAsync(localstore)` v [.NET klienta SDK].

## <a name="how-sync-works"></a>Jak v režimu offline synchronizace funguje

Při používání synchronizační tabulky, ovládací prvky kód klienta při místní změny bude se synchronizovat s backendovou Azure mobilní aplikaci. Nic se pošle back-end až volání *nabízených* místní změny. Podobně v místním úložišti se zobrazí nová data jenom v případě, že volání *vyžádat* data.

* **Nabízená**: nabízených je operaci místní synchronizace a odešle všechny změny vytvoření od posledního připínáčku. Všimněte si, že není možné odeslat jenom jednotlivé tabulky změny, protože jinak operace může být mimo pořadí. Nabízená provede řadu ZBÝVAJÍCÍ volání na váš back-end Azure mobilní aplikaci, která zase změní databázovém serveru.

* **Vyžádat**: vyžádané proběhne v jednotlivých tabulek a lze jej přizpůsobit pomocí dotazu k načtení jen podmnožinu dat serveru. Azure mobilních klientů SDK vložte Výsledná data do v místním úložišti.

* **Implicitní posune**: Pokud vložit je spouštět oproti tabulku obsahující čekající místní aktualizace, Vložit nejprve spustit push na místní synchronizace. Díky minimalizovat konfliktu mezi změnami, které jsou již uloženy ve frontě a nová data ze serveru.

* **Přírůstková synchronizace**: první parametr operace je *název dotazu* , který se používá pouze na straně klienta. Pokud používáte název dotazu jinou hodnotu než null, provede Azure Mobile SDK *Přírůstková synchronizace*.
  Pokaždé, když operace vyžádané vrátí sadu výsledků, nejnovější `updatedAt` časové razítko ze sady tento výsledek je uložená v tabulkách SDK místní systém. Operace následné vložit pouze načte záznamy po razítek.

  Před použitím Přírůstková synchronizace musí vrátit serveru smysluplné `updatedAt` hodnoty a musí také podporují řazení podle tohoto pole. Však od SDK přidá vlastní řazení na základě pole updatedAt, nelze použít vyžádané dotaz, který obsahuje vlastní `$orderBy$` klauzule.

  Název dotazu může být jakýkoli řetězec, které zvolíte, ale musí být jedinečné pro každý logické dotazu v aplikaci.
  V opačném různých vyžádané operace může přepsat stejné časové razítko Přírůstková synchronizace a dotazech můžete vrátit nesprávné výsledky.

  Pokud dotaz má parametr, je jedním způsobů, jak vytvořit jedinečný dotazu zahrnutí hodnotu parametru.
  Například pokud filtrujete na ID uživatele, název dotazu může být následujícím způsobem (v jazyce C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Pokud budete chtít vyjádření výslovného nesouhlasu Přírůstková synchronizace, předejte `null` jako ID dotazu. V tomto případě se při každém volání na načíst všechny záznamy `PullAsync`, což je potenciálně neefektivní.

* **Purging**: můžete vymazat obsah místním úložišti pomocí `IMobileServiceSyncTable.PurgeAsync`.
  Může být nutné máte zastaralá data v klientské databázi nebo pokud se chcete zahodit všechny změny čekající na vyřízení.

  Vymazat Vymaže tabulky z v místním úložišti. Pokud jsou operace čeká na vyřízení synchronizace s databází serveru, vyprázdnit výjimku pokud je parametr *platnost vymazat* .

  Jako příklad zastaralá data na straně klienta Předpokládejme v příkladu "úkol seznam" Device1 použije pouze položky, které nejsou dokončit. Potom todoitem "Nákup mléčné" je označený udělat na serveru jiné zařízení. Však Device1 budete mít stále todoitem "Nákup mléčné" v místním úložišti protože táhne jenom položky, které nejsou označeny jako dokončené. Vymazat Vymaže této zastaralé položky.

## <a name="next-steps"></a>Další kroky

* [iOS: Povolení offline synchronizace]
* [Xamarin iOS: Povolení offline synchronizace]
* [Xamarin Android: Povolení offline synchronizace]
* [Univerzální platformu Windows už: Povolení offline synchronizace]

<!-- Links -->
[.NET klienta SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Povolení offline synchronizace]: app-service-mobile-android-get-started-offline-data.md
[iOS: Povolení offline synchronizace]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Povolení offline synchronizace]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Povolení offline synchronizace]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Univerzální platformu Windows už: Povolení offline synchronizace]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
