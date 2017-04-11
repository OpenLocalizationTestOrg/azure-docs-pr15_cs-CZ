<properties
   pageTitle="Základní poznámky k verzi trezoru .NET 2.x API | Microsoft Azure"
   description=".NET bude vývojáři toto rozhraní API kódu pro trezoru klíč Azure"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure klíčové trezoru .NET 2.0 – poznámky k verzi a Průvodce migrací

Sledování poznámek a podle pokynů je pro vývojáře pracující s .NET Azure klíč trezoru a C# knihovny. V tématu Změna z verze 1.0 verze 2.0 počet aktualizace byly provedeny se bude vyžadovat migrace práce v kódu, aby vám těžit z funkční vylepšení a funkcí dodatky například klíč trezoru certifikáty podpory.

Podpora certifikáty klíč trezoru poskytuje pro správu vašeho x509 certifikáty a těchto situací:  

-   Umožňuje vlastníka certifikátu vytvořit certifikát prostřednictvím proces vytváření klíč trezoru nebo import stávající certifikát. Platí to i pro obě automatické přihlášení a certifikační autorita generovaného certifikáty.

- Umožňuje vlastníka certifikátu klíč trezoru implementace zabezpečeného úložiště a řízení X509 certifikáty bez interakce s privátní klíče.  

-   Umožňuje vlastníka certifikátu k vytvoření zásad, který směruje klíč trezoru Správa životního cyklu certifikát.  

-   Umožňuje certifikát vlastníci k poskytnutí kontaktních informací pro oznámení o událostech životního cyklu vypršení platnosti a obnovení certifikátu.  

-   Automatické prodlužování podporuje s vybranou vydavatelů - klíč trezoru partnera X509 certifikátů poskytovatelů / certifikát orgány.
    - Poznámka: než stala partnerem poskytovatelů/orgány také mohou, ale nebude podporovat funkce automatického obnovení.


## <a name="net-support"></a>Podpora .NET
- **.NET 4.0** nepodporuje verze 2.0 .NET Azure klíč trezoru a C# knihovny
- Podporuje **.NET základní** verze 2.0 .NET Azure klíč trezoru a C# knihovny

## <a name="namespaces"></a>Obory názvů
- Obor názvů pro **modely** se změnil z **Microsoft.Azure.KeyVault** **Microsoft.Azure.KeyVault.Models**.
- Obor názvů **Microsoft.Azure.KeyVault.Internal** nezobrazí.
- Obor názvů závislosti Azure SDK se změnil z **Hyak.Common** a **Hyak.Common.Internals** **Microsoft.Rest** a **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Typ změn
- *Tajná* změní na *SecretBundle*
- *Slovník* změnili *IDictionary*
- *Seznamu<T>, řetězec []* změnili *IList<T> *
- *NextList* změní na *NextPageLink*


## <a name="return-types"></a>Vrací typy
- Vrátí **KeyList** a **SecretList** *IPage<T> * místo *ListKeysResponseMessage*
- Generované **BackupKeyAsync** vrátí *BackupKeyResult* , která obsahuje *hodnotu* (blob zálohování). Před omotané metodu a vrácení pouze hodnoty.

## <a name="exceptions"></a>Výjimky
- *KeyVaultClientException* se změní na *KeyVaultErrorException*
- Chyba služby dojde ke změně z *výjimku. Chyba* *výjimky. Body.Error.Message*.
- Další informace o odebrali chybová zpráva pro **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktory
- Místo přijetí *HttpClient* jako argument konstruktoru přijímá konstruktoru pouze *HttpClientHandler* nebo *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Pokud chcete stažený balíčků  
Po klienta zpracovává závislost na klíč trezoru následující byly ke stažení
#### <a name="previous-package-list"></a>Předchozí seznam balíčku
- zabalení id="Hyak.Common" verze = "1.0.2" targetFramework = "net45"
- verze zabalení id="Microsoft.Azure.Common" = "verze 2.0.4" targetFramework = "net45"
- verze zabalení id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
- verze zabalení id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
- verze zabalení id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
- verze zabalení id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
- verze zabalení id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
- zabalení id="Microsoft.Net.Http" verze = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Aktuální seznam balíčku
- zabalení id="Microsoft.Azure.KeyVault" verze = "2.0.0-preview" targetFramework = "net45"
- verze zabalení id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
- verze zabalení id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Změny třídy

- Odebrali **UnixEpoch** třídy
- Přejmenování **Base64UrlConverter** třídy na **Base64UrlJsonConverter**

## <a name="other-changes"></a>Další změny

- Podpora pro konfiguraci KV operace opakovat zásad přechodná selhání přidala tuto verzi rozhraní API.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Pro operace, které vracejí *trezoru*je vráceným typem byl předmětu, který obsahoval vlastnost trezoru. Je vráceným typem je teď *trezoru*.
- *PermissionsToKeys* a *PermissionsToSecrets* jsou teď *Permissions.Keys* a *Permissions.Secrets*
- Některé změny návratové typy platí pro ovládací prvek rovině stejně.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Balíček je rozdělen **Microsoft.Azure.KeyVault.Extensions** a **Microsoft.Azure.KeyVault.Cryptography** pro operace šifrování.
