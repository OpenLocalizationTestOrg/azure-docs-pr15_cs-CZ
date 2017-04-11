<properties
    pageTitle="Časté otázky týkající se použití | Microsoft Azure"
    description="Seznamu Azure zásobníku metry porovnání Azure použití rozhraní API, časového využití a vykázaného čas kódy chyb."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="azure-stack-usage-api-faqs"></a>Nejčastější dotazy týkající se použití rozhraní API Azure zásobníku
Tento článek odpovídá na některé časté otázky k rozhraní API využití vrstvě Azure.

## <a name="what-meter-ids-can-i-see"></a>K čemu metr ID se budou zobrazovat?

Použití je v současné době vykázaného za sítě, ukládání a zprostředkovatelé výpočetním zdroje.

| **Zprostředkovatel zdroje** | **ID měřiče** |**Název metr** | **Jednotky** | **Další informace** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Sítě** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Použití veřejnou IP adresu | IP adresa |                    
| **Úložiště**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*hodin | Celkovou kapacitu využívané tabulek |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*hodin | Celkovou kapacitu využívané objektů BLOB stránky |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*hodin  | Celkovou kapacitu využívané fronty |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*hodin  | Celkovou kapacitu využívané blok objekty BLOB |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Žádost o počet za 10,000s   | Žádost o službu tabulky (v 10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Průniku dat v GB | Tabulku služba dat průniku v GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress v GB | Výstupní data service tabulky v GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Požadavky na počet v 10,000s | Žádost o službu objektů BLOB (v 10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Průniku dat v GB          | Kulatý průniku dat služby v GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress v GB              | Výstupní data service objektů BLOB v GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Požadavky na počet v 10,000s   | Fronty žádost o službu (10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Průniku dat v GB         | Fronta služby dat průniku v GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress v GB  | Výstupní data service fronty v GB 
| **Výpočet** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | OM velikost hodin | Velikost virtuálního počítače |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Jak rozhraní API využití vrstvě Azure srovnání k [Rozhraní API použití Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (aktuálně ve veřejných verze preview)?

-   Rozhraní API použití klienta je v souladu s rozhraní API Azure s jednou výjimkou: Příznak *showDetails* aktuálně nepodporuje Azure vrstvě.

-   Rozhraní API použití poskytovatele platí jenom pro Azure vrstvě.

-   Rozhraní [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) , která je dostupná v Azure v současné době není k dispozici ve vrstvě Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Jaký je rozdíl mezi časem použití a vykázaného?

Data sestavy využití mají dva hlavní časových hodnot:

-   **Vykázaného čas**. Při použití akce zadaný systému použití čas

-   **Použití čas**. Čas, kdy byla spotřebované množství zdrojů Azure zásobníku

Může se zobrazit rozdíl hodnoty pro použití a času vykázaného pro konkrétní použití událost. Zpoždění může být dokud více hodiny v jakémkoli prostředí.

V současné době můžete dotaz *pouze podle času vykázaného*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Co znamenají tyto kódy chyb použití rozhraní API?

| **Nastavit informace HTTP stavový kód** | **Kód chyby** | **Popis** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400/Chybný požadavek        | *NoApiVersion*     | Parametr dotazu *verze rozhraní api* nebyl nalezen.
| 400/Chybný požadavek        | *InvalidProperty*  | Vlastnost chybí nebo byl neplatnou hodnotu. Zpráva v kódu chyby v těle odpovědi identifikuje chybějící vlastnosti.
| 400/Chybný požadavek        | *RequestEndTimeIsInFuture*  | *ReportedEndTime* hodnotu v budoucnu. Pro tento argument nejsou povolené hodnoty v budoucnu.
| 400/Chybný požadavek        | *SubscriberIdIsNotDirectTenant*    | Zprostředkovatele rozhraní API volají použít ID předplatného, který není platný klienta volající.
| 400/Chybný požadavek        | *SubscriptionIdMissingInRequest*   | Chybí předplatné ID volajícího.
| 400/Chybný požadavek        | *InvalidAggregationGranularity*   | Neplatné agregace granularity bylo požadováno. Platné hodnoty jsou denní a každou hodinu.
| 503                    | *ServiceUnavailable*   | Protože teď na něčem pracuje služba nebo je právě omezena hovor došlo k chybě opakovatelná. |

## <a name="next-steps"></a>Další kroky
[Fakturace se zákazníky a zpětné ve vrstvě Azure](azure-stack-billing-and-chargeback.md)

[Používání zdrojů poskytovatelem rozhraní API](azure-stack-provider-resource-api.md)

[Klient využití prostředků rozhraní API](azure-stack-tenant-resource-usage-api.md)
