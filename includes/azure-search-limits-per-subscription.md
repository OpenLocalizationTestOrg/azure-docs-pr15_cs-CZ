Můžete vytvořit více služeb v rámci předplatného, každý z nich zřízení na konkrétní osy omezená jen podle čísla služeb povolených u každé vrstvy. Můžete například vytvořit až 12 na základní osy a jiné 12 služby ve vrstvě ve stejném předplatném S1. Další informace o úrovní najdete v tématu [zvolit jinou SKU nebo osy mají být vyhledávány Azure](../articles/search/search-sku-tier.md).

Maximální služby omezení můžete umocněné na žádost. Pokud potřebujete další služby v rámci stejného předplatného, kontaktujte podporu Azure.

Zdroje|Uvolnit|Základní|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Maximální služby |1 |12 |12  |6 |6 |6 
Maximální velkém měřítku v SH <sup>2</sup>|Není k dispozici <sup>3</sup>|3 dílčí <sup>4</sup> |36 SH|36 SH|36 SH|12 SH 3 dílčí <sup>5</sup>

<sup>1</sup> S3 HD nepodporuje v současné době [indexování](../articles/search/search-indexer-overview.md) . 

<sup>2</sup> hledání jednotky (SH) jsou fakturaci jednotek na služby přidělit jako *otevřené* či *oddíl*. Potřebujete oba zdroje pro úložiště indexování a operací dotazu. Zobrazíte další informace o platné kombinace, které zůstávají v části maximální limity najdete v článku [úrovně zdroje měřítko pro dotaz a index úloh](../articles/search/search-capacity-planning.md). 

<sup>3</sup> Free vychází z sdílených používá více účastníky. Na této osy se žádné vyhrazené zdroje pro jednotlivého účastníka. Z toho důvodu je maximální měřítko označen jako není k dispozici.

<sup>4</sup> basic obsahuje jeden pevné oddíl. V této osy další SUs slouží k přidělování další repliky pro lepší dotazu úloh.

<sup>5</sup> S3 HD obsahuje strukturu jiné rozdělení z hlediska povolenou kombinace. Replik může mít maximální 12. Pro oddíly maximální hodnota je 3.




