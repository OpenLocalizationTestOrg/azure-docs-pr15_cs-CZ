<properties 
    pageTitle="Jak implementovat havárie obnovení s použitím služby zálohování a obnovení správy rozhraní API Azure | Microsoft Azure" 
    description="Zjistěte, jak pomocí zálohování a obnovení obnovení havárie správy rozhraní API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Jak implementovat havárie obnovení s použitím služby zálohování a obnovení správy rozhraní API Azure

Vyberte publikovat a spravovat API přes Azure rozhraní API Management s využitím mnoho odolnost proti chybám a infrastruktury funkcí, které by jinak musíte návrh a implementace a spravovat. Azure platformu snižuje velkou část potenciální chyb na zlomek nákladů.

Obnovit dostupnost by měl být otázkách oblasti řízení vaší rozhraní API služba je hostitelem budete chtít znovuvytvoření služby v jiné oblasti kdykoli. Podle dostupnosti cíle a obnovení čas cíle můžete rezervovat záložní služby na jednu nebo více oblastí a pokusíte se udržovat synchronizace služby active jejich konfigurace a obsahu. Služba zálohování a obnovení funkce poskytuje potřebné stavební blok pro provádění strategie havárie obnovení.

Tato příručka ukazuje, jak ověřit žádosti správce prostředků Azure a zálohování a obnovení instancí služby správy rozhraní API.

>[AZURE.NOTE] Proces pro zálohování a obnovování instanci služby správy rozhraní API pro obnovení havárie lze také replikace instancí služby správy rozhraní API pro scénáře například pracovní.
>
>Všimněte si, že každé zálohování vyprší po 7 dní. Pokud budete chtít obnovení záložní po vypršení platnosti 7 den, obnovení se nezdaří s `Cannot restore: backup expired` zprávy.

## <a name="authenticating-azure-resource-manager-requests"></a>Požadavky ověřování správce prostředků Azure

>[AZURE.IMPORTANT] Rozhraní REST API pro zálohování a obnovení používá správce prostředků Azure a má jiný mechanismus ověřování než rozhraní REST API pro správu rozhraní API správy entity. Postup v této části popisují, jak ověřit žádosti správce prostředků Azure. Další informace najdete v tématu [požadavky ověřování správce prostředků Azure](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Všechny úkoly, které můžete provádět na zdrojů pomocí Správce prostředků Azure musí být ověřeny službou Azure Active Directory pomocí následujících kroků.

-   Přidání aplikace do klienta služby Azure Active Directory.
-   Nastavení oprávnění pro aplikaci, kterou jste přidali.
-   Získání tokenu pro ověřování žádosti o Správci zdrojů Azure.

Cílem prvního kroku je vytvořit aplikaci služby Azure Active Directory. Přihlaste se k [Portálu klasické Azure](http://manage.windowsazure.com/) pomocí předplatné, které obsahuje instanci služby správy rozhraní API a přejděte na kartu **aplikace** pro výchozí Azure Active Directory.

>[AZURE.NOTE] Pokud výchozí adresář Azure Active Directory není zobrazená ke svému účtu, kontaktujte správce Azure předplatné udělit potřebná oprávnění ke svému účtu. Informace o umístění výchozí adresář najdete v tématu [Vyhledání výchozí adresář](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Vytvoření aplikace služby Azure Active Directory][api-management-add-aad-application]

Klikněte na tlačítko **Přidat** **Přidat aplikaci, které vyvíjí naší organizace**a zvolte **nativní klientské aplikaci**. Zadejte popisný název a klikněte na Další šipky. Zadejte adresu URL zástupného symbolu, třeba `http://resources` pro **Přesměrování URI**ZobrazitFormulář.aspx pole je povinné, ale hodnota není použit později. Zaškrtněte políčko Uložit aplikace.

Po uložení aplikace klikněte na **Konfigurovat**, přejděte dolů do části **oprávnění na jiné aplikace** a klikněte na **Přidat aplikaci**.

![Přidat oprávnění][api-management-aad-permissions-add]

Vyberte **Windows** **Azure služby správy API** a klepnutím na zaškrtávací políčko Přidat aplikaci.

![Přidat oprávnění][api-management-aad-permissions]

Klepnutím na možnost **Delegovat oprávnění** vedle nově přidaný webu **Windows** **Azure služby správy rozhraní API** aplikace, zaškrtněte políčko pro **Správu služby Azure přístupu (verze preview)**a klikněte na tlačítko **Uložit**.

![Přidat oprávnění][api-management-aad-delegated-permissions]

Před vyvolání rozhraní API, která generovat zálohování a obnovení, je nutné získat token. Balíček nuget [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) načítat tokenu v následujícím příkladu.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Nahrazení `{tentand id}`, `{application id}`, a `{redirect uri}` postupujte podle následujících pokynů.

Nahrazení `{tenant id}` s id klienta služby Azure Active Directory aplikace jste právě vytvořili. Id můžete přejít kliknutím **koncové body zobrazení**.

![Koncové body][api-management-aad-default-directory]

![Koncové body][api-management-endpoint]

Nahrazení `{application id}` a `{redirect uri}` pomocí **Id klienta** a adresu URL z oddílu **Přesměrovat URI** z karty **nakonfigurovat** aplikaci služby Azure Active Directory. 

![Zdroje informací][api-management-aad-resources]

Jakmile hodnoty jsou zadány, příklad kódu, měly vrátit token podobně jako v následujícím příkladu.

![Tokenu][api-management-arm-token]

Před voláním zálohování a obnovení operace popsané v následující části, nastavte záhlaví se tak mohli ověřovat žádost pro ZBÝVAJÍCÍ volání.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Zálohování služby správy rozhraní API
Zálohování žádost správy rozhraní API služeb problém následující HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

kde je:

* `subscriptionId`– id předplatného obsahující službu pro správu rozhraní API pokoušíte k zálohování
* `resourceGroupName`-řetězec ve tvaru "Rozhraní Api - výchozí-{služby oblasti}" kde `service-region` identifikuje Azure oblasti, kde správy rozhraní API služby pokoušíte je hostovaný zálohování, například`North-Central-US`
* `serviceName`– Název službu pro správu rozhraní API vytváříte záložní kopii zadané v době jeho vytvoření
* `api-version`-nahradit`2014-02-14`

V textu žádosti o zadejte název účtu úložiště Azure cílové, přístupové klávesy, jméno container objektů blob a název zálohy:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Nastavte hodnotu `Content-Type` žádost o záhlaví `application/json`.

Zálohování bylo dlouhodobé operaci, která může trvat několik minut.  Pokud podaří žádosti a proces zálohování vznikla dostanete `202 Accepted` odpověď stavový kód s `Location` záhlaví.  Zkontrolujte získat požadavky na adresu URL v `Location` záhlaví zjistěte stav operace. Probíhá zálohování vám budou zasílané zobrazí stavový kód "202 přijaté". Kód odpověď `200 OK` výskyt znamená úspěšném dokončení zálohování.

**Poznámka**:

- **Kontejner** uvedené v žádosti o textu, **musí existovat**.
* Probíhá zálohování jste **neměli všechny operace Správa služeb** třeba SKU upgrade nebo omezit, změna názvu domény, atd. 
* Obnovení **záložní zaručuje pouze 7 dní** od okamžiku jeho vytvoření. 
* **Použití zásad správy informací** pro vytváření analýz sestavy **není součástí** v zálohování. Pomocí [Rozhraní API Azure správy rozhraní REST API][] pravidelně načíst sestavy analytics pro bezpečné uchování.
* Četnost, se kterým jste zálohování služby ovlivní vaším cílem bodu obnovení. Minimalizovat ho, doporučujeme provádění pravidelného zálohování i po provedení změn důležité ke službě Správa rozhraní API zálohování na vyžádání.
* **Změny** provedené v konfiguraci služby (například rozhraní API zásad, karta Vývojář v portálu vzhled) při zálohování je proces **nemusí být součástí zálohování a proto se ztratí**.

## <a name="step2"> </a>Obnovit služby správy rozhraní API
Obnovit správy rozhraní API služeb ze zálohy dříve vytvořené zkontrolujte následující žádost HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

kde je:

* `subscriptionId`– id předplatného obsahující službu pro správu rozhraní API obnovujete záložní do
* `resourceGroupName`-řetězec ve tvaru "Rozhraní Api - výchozí-{služby oblasti}" kde `service-region` identifikuje oblasti Azure hostitelem služby správy rozhraní API obnovujete zálohy do, například`North-Central-US`
* `serviceName`– Název správy rozhraní API služeb obnovována do zadané v době jeho vytvoření
* `api-version`-nahradit`2014-02-14`

V těle žádost zadejte umístění záložní soubor, tedy název účtu úložiště Azure přístupové klávesy, jméno container objektů blob a název zálohy:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Nastavte hodnotu `Content-Type` žádost o záhlaví `application/json`.

Obnovení je dlouhodobé operaci, která může trvat až 30 nebo víc minut dokončete.  Pokud požadavek byl úspěšný a obnovování vznikla dostanete `202 Accepted` odpověď stavový kód s `Location` záhlaví.  Zkontrolujte získat požadavky na adresu URL v `Location` záhlaví zjistěte stav operace. Probíhá obnovení vám budou zasílané přijímání "202 přijaté" stavový kód. Kód odpověď `200 OK` výskyt znamená úspěšném dokončení obnovení.

>[AZURE.IMPORTANT] **SKU** služby se obnoví do **se musí shodovat** SKU zálohované služba obnovena.
>
>**Změny** provedené v konfiguraci služby (například rozhraní API zásad, karta Vývojář v portálu vzhled) při obnovení je v průběhu **může dojít k přepsání**.

## <a name="next-steps"></a>Další kroky
Podívejte se na následující blogy Microsoft pro dvě různé návody procesu zálohování a obnovení.

-   [Replikovat Azure rozhraní API Správa účtů](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Děkujeme vám Gisela pro svůj příspěvek v tomto článku.
-   [Správa Azure rozhraní API: Zálohování a obnovování konfigurace](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Přístup podrobné Stuart neodpovídá oficiální pokyny, ale je velmi zajímavé.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Správa Azure rozhraní API rozhraní REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
