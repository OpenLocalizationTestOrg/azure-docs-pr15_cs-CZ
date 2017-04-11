<properties
   pageTitle="Porovnání úložiště jezera dat Azure s Azure úložiště objektů Blob | Microsoft Azure"
   description="Porovnání úložiště jezera dat Azure s Azure úložiště objektů Blob"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Porovnání úložiště jezera dat Azure a úložišti objektů Blob Azure

Tabulky v tomto článku shrnuje rozdíly mezi úložiště jezera dat Azure a úložišti objektů Blob Azure podél některé klíčové aspekty velký zpracování dat. Úložiště objektů Blob Azure je univerzální úložiště scalable objekt, který je určený pro celou řadu úložiště scénáře. Úložiště jezera dat Azure slouží jako úložiště hyper měřítko, která je optimalizovaná pro velký dat analýzy úloh.

|    | Úložiště jezera dat Azure  | Úložiště objektů Blob Azure  |
|----|-----------------------|--------------------|
| Účel  | Optimalizované úložiště pro velký dat analýzy úloh                                                                          | Univerzální objekt uložení pro celou řadu scénáře úložiště                                                                                |
| Případy použití                                   | Dávka, interaktivní, streamování technologie pro analýzu a data výukové počítače jako soubory, IoT dat protokolu, klikněte na tlačítko datové proudy velkých sad dat | Libovolný text nebo binární dat, například aplikace zpět ukončit, zálohování dat, médií úložiště pro streamování a hlavní účel dat |
| Klíčové koncepty                                | Účet úložiště jezera dat obsahuje složky, která zase obsahuje data uložená jako soubory | Úložiště má účet kontejnerů, která zase má datům ve formuláři objektů BLOB |
| Struktura | Systém souborů HFS                                                                                                    | Uložení objektu s ploché obor názvů  |
| ROZHRANÍ API   | Rozhraní REST API přes protokol HTTPS | Rozhraní REST API prostřednictvím protokolu HTTP/HTTPS                                                                                                                            |
| Rozhraní API straně serveru                             | [Kompatibilní s prohlížečem WebHDFS REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Úložiště objektů Blob Azure rozhraní REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Soubor systému Hadoop systém klienta                   | Ano                                                                                                                         | Ano                                                                                                                                                 |
| Operace s daty – ověření            | Založené na [Azure Active Directory identity](../active-directory/active-directory-authentication-scenarios.md) | Podle sdílené tajemství - [Přístupových kláves z verze účtu](../storage/storage-create-storage-account.md#manage-your-storage-account) a [Sdílené přístupových kláves podpis](../storage/storage-dotnet-shared-access-signature-part-1.md).                                                                       |
| Operace s daty – ověření Protocol (protokol)     | OAuth 2.0. Volání musí obsahovat platné (JSON Web tokenu) JWT vydán Azure Active Directory                     | Ověřovací kód zprávy na základě hash (HMAC). Volání musí obsahovat kódováním Base 64 SHA-256 hash nad část žádost HTTP. |
| Operace s daty – ověření               | POSIX seznamy řízení přístupu (ACL).  ACL založené na Azure Active Directory identit můžete nastavit úroveň souborů a složek. | Povolení úroveň účtu – použití [Přístupových kláves z verze účtu](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Pro účet, kontejneru nebo se tak mohli ověřovat objektů blob – použití [Sdílených přístupových kláves podpisu](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Operace s daty – auditování                    | K dispozici. Najdete [tady](data-lake-store-diagnostic-logs.md) informace.                                                                                                                   | K dispozici                                                                                                                                           |
| Šifrování data na ostatních                     | Průhledné na straně serveru (už brzo)<ul><li>S klávesami služby Správa přístupových práv</li><li>S klávesami zákazníka Správa přístupových práv v Azure KeyVault</li></ul>| <ul><li>Průhledné na straně serveru</li> <ul><li>S klávesami služby Správa přístupových práv</li><li>S klávesami zákazníka Správa přístupových práv v Azure KeyVault (už brzo)</li></ul><li>Šifrování na straně klienta</li></ul> |
| Správa operace (například účet vytvoření) | [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovanou Azure pro správu účtů                                                                       | [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovanou Azure pro správu účtů                                                                                               |
| Karta Vývojář SDK                              | Node.js .NET, Java                                                                                                         | .NET, Java, Python Node.js, C++, skutečné                                                                                                              |
| Technologie pro analýzu pracovní zátěž výkonu              | Optimalizaci výkonu pro paralelní analýzy úloh. Vysoký výkon a procesorů.                                           | Nejsou optimalizované pro analýzy úloh                                                                                                               |
| Omezení velikosti                                 | Bez omezení velikostí účtu, velikosti souborů nebo počet souborů                                                                   | Omezení dokumentovaného [tady](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| GEO redundance                              | Místně nadbytečné (více kopií dat v jednom regionu Azure)                                                             | Místně nadbytečné (LRS), globálně nadbytečné (GRS), globálně nadbytečné přístup pro čtení (Vzdálená pomoc-GRS). Najdete [tady](../storage/storage-redundancy.md) Další informace |
| Stav služby                               | Veřejné náhledu                                                                                                              | Obecně k dispozici                                                                                                                                 |
| Místní dostupnosti  | Najdete [tady](https://azure.microsoft.com/regions/#services)| Najdete [tady](https://azure.microsoft.com/regions/#services) |
| Price                                       |     V tématu [ceny](https://azure.microsoft.com/pricing/details/data-lake-store/)| V tématu [ceny](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Další kroky

- [Základní informace o úložiště jezera dat Azure](data-lake-store-overview.md)
- [Začínáme s jezera úložiště dat](data-lake-store-get-started-portal.md)



