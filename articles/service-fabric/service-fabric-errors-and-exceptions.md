<properties
   pageTitle="Běžné FabricClient výjimky vyvolání | Microsoft Azure"
   description="Popisuje běžné výjimky a chyby, které můžou být rozhraní API FabricClient vyvolané při provádění aplikace clusteru operací a správy."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Běžné výjimky a chyby při práci s rozhraní API FabricClient
Rozhraní API [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) správcům obrázku a aplikace pro provádění úkolů správy na služby struktury aplikaci, služby nebo obrázku. Například nasazení aplikace, upgrade a odstranění se kontrola stavu clusteru, nebo testování třetí strana službu. Vývojáři aplikací a clusteru správci můžou pomocí rozhraní API FabricClient vyvíjet nástroje pro správu služby struktury obrázku a aplikace.

Existuje mnoha různých typů operacích, které lze provést pomocí FabricClient.  Obou metod může vyvolat výjimky pro chyby kvůli nesprávné vstupní, runtime chyb a problémů přechodná infrastruktury.  V dokumentaci k rozhraní API odkaz zobrazíte výjimek, které jsou vyvolané konkrétní způsob. Existuje několik výjimek však kterých můžete vyvolané mnoho různých rozhraní API [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) . V následující tabulce jsou uvedeny výjimek, které jsou běžné přes rozhraní API FabricClient.

|Výjimky| Kdy vyvolání|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Objekt [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) je ve skrytých stavu. Mít objektu [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) používáte a instanci nové [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objektu. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Operace vypršel časový limit. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) je vrácena, když operace trvá déle než MaxOperationTimeout dokončete.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Kontrola přístupu pro operaci se nezdařila. Vrátí se E_ACCESSDENIED.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Při provádění operace došlo k chybě runtime. Metod FabricClient potenciálně vyvolat [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), vlastnost [Kód chyby](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) označuje přesnou příčinu výjimku. Kódy chyb se definují výčet [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Operace se nezdařila kvůli přechodná chybovém stavu některé identifikovat. Třeba operace může selhat, protože replik je hlasování dočasně není dostupný. Přechodné výjimky odpovídají selhalo operacích, které můžete opakované.|

Některé běžné [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) chyby, které může být vrácen v [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx):

|Chyba| Podmínky|
|---------|:-----------|
|CommunicationError|Komunikace chyba způsobená operace nezdaří, opakujte.|
|InvalidCredentialType|Zadejte přihlašovací údaje je neplatný.|
|InvalidX509FindType|X509FindType není platný.|
|InvalidX509StoreLocation|X509 umístění úložiště je neplatný.|
|InvalidX509StoreName|X509 název úložiště je neplatný.|
|InvalidX509Thumbprint|X509 certifikát Miniatura řetězec je neplatný.|
|InvalidProtectionLevel|Úroveň ochrany není platný.|
|InvalidX509Store|X509 úložiště certifikátů nelze otevřít.|
|InvalidSubjectName|Název subjektu je neplatný.|
|InvalidAllowedCommonNameList|Formát běžné název seznamu řetězec není platný. Je třeba čárkou oddělený seznam.|
