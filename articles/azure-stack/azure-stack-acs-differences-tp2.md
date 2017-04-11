
<properties
    pageTitle="Konzistentní Azure úložiště: rozdíly a co byste měli zvážit | Microsoft Azure"
    description="Princip z Azure úložiště a další Azure konzistentní nasazení aspektech úložiště rozdíly."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure konzistentní úložiště: rozdíly a co byste měli zvážit

Azure konzistentní úložiště je sada služby cloudové úložiště v aplikaci Microsoft Azure vrstvě. Azure konzistentní úložiště poskytuje objektů blob, tabulky, fronty a funkce správy účtu s Azure konzistentní sémantiku. Tento článek shrnuje známé Azure konzistentní úložiště rozdíly s úložištěm Azure. Shrnuje taky další aspekty mít na paměti při nasazení aktuálně veřejně dostupný náhled verzi Microsoft Azure vrstvě.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Známé rozdíly

Tuto verzi Technical Preview Azure konzistentní úložiště nemá dostupná funkce 100 % s Azure úložiště pro rozhraní API verze, které jsou podporovány. Známé rozdíly ve funkcích patřit následující úkoly:

-   Některé typy účtu úložiště ještě nejsou k dispozici, například standardní\_RAGRS a standardní\_GRS.

-   Premium\_LRS úložiště účtů můžete zřízení. Můžou ale nastat aktuálně bez omezení výkonu nebo záruky.

-   Funkce Azure soubory ještě není k dispozici.

-   API oblasti stránku získání nepodporuje načítání stránek, které se liší od snímky objektů BLOB stránky.

-   API oblasti stránku získání vrátí stránky, které mají 4 KB granularity.

-   Oddíl a klíče řádek v Azure konzistentní tabulky implementace úložiště jsou každý v omezené na 400 znaků (to znamená 800 bajtů) velikost.

-   Názvy objektů BLOB Azure konzistentní implementace služba úložiště objektů Blob jsou omezené 880 znaků (to znamená 1760 bajtů).

-   Implementace Azure konzistentní úložiště klienta úložiště o využití oznamovací poskytuje metry využití úložiště, které jsou stejné jako v Azure, což se stejnou sémantiku a jednotky. V současné době však měřiče využití úložiště transakce nezahrnuje IaaS transakce a použití měřiče přenosu dat nerozlišuje využití šířky pásma tak, že interní a externí sítě umožnění datových přenosů do oblasti Azure vrstvě.

-   V oboru funkce pro správu úložiště existují určité rozdíly. Například nelze změnit typ účtu nebo máte vlastní doménu. Kromě toho nabízených pouze konzistenci na úrovni rozhraní API pro Premium\_typ účtu úložiště LRS.

## <a name="deployment-considerations"></a>Důležité informace o nasazení

-   **Pouze test.** Nasazení Azure konzistentní úložiště v provozním prostředí, které používají aktuální verzi Microsoft Azure zásobníku Technical Preview. Tato verze je určen pouze k vyzkoušení v testovacím prostředí.

-   **Výkon**. Verze Microsoft Azure zásobníku Technical Preview Azure konzistentní úložiště není plně výkonu optimalizované.

-   **Škálovatelnost.** Verze Microsoft Azure zásobníku Technical Preview Azure konzistentní úložiště není optimalizovaná pro škálovatelnost.

## <a name="version-support-considerations"></a>Co byste měli zvážit verze podpory

V této verzi preview Azure konzistentní úložiště jsou podporovány následující verze:

> Knihovna Azure úložiště klienta: [Microsoft Azure úložiště 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Datové služby Azure úložiště: [2015 04 05 rozhraní REST API verze](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure úložiště správu přístupových: [2015 05 01 náhled](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015 06 15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Další kroky

-   [Úvod k základnímu úložišti konzistentní Azure](azure-stack-storage-overview.md)
