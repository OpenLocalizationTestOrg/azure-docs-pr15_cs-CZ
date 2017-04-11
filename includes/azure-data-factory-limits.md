Data factory je služba více klienta, která obsahuje následující výchozí omezení na místě, abyste měli jistotu, že předplatné zákazníka zamknuté z ostatních úloh. V mnoha limity můžete snadno dosáhnout pro vaše předplatné až maximální limit kontaktováním podpory. 

**Zdroje** | **Výchozí Limit** | **Maximální Limit**
-------- | ------------- | -------------
zdroje dat v Azure předplatného | 50 | [Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
potrubí v rámci výroby dat | 2 500 | [Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
datové sady v rámci výroby dat | 5000 | [Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
souběžné výsečí za datové sady | 10 | 10
bajty objektu pro objekty kanálem k odesílání zpráv <sup>1</sup> | 200 ZNALOSTNÍ BÁZI KNOWLEDGE BASE | 2000 ZNALOSTNÍ BÁZI KNOWLEDGE BASE
bajty objektu pro datovou sadu a service propojené objekty <sup>1</sup> | 100 KB | 2000 ZNALOSTNÍ BÁZI KNOWLEDGE BASE
HDInsight obrázku na vyžádání jádra v rámci předplatného <sup>2</sup> | 48 | [Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Data pohyb jednotku <sup>3</sup> v cloudu | 8 | [Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Opakování pro spuštění aktivity kanálem k odesílání zpráv | 1 000 | MaxInt (32 bitů)

<sup>1</sup> kanálem k odesílání zpráv, datovou sadu a služby propojené objekty představují logické seskupení vaše pracovní zátěž. Limity pro tyto objekty se nevztahují k množství dat, přesunutí a zpracování se službou Azure Data Factory. Data výroby slouží k zvládnout petabytes data.

<sup>2</sup> na vyžádání HDInsight jádra přiřazených mimo předplatné, které obsahuje data výroby. Výše uvedené omezení jako výsledek je Data Factory nevynucují základní limit jádra HDInsight na vyžádání a se liší od limit core přidružený k předplatnému Azure.

<sup>3</sup> cloudu datové pohyb jednotky (DMU) používá v cloudu do cloudu kopírování. Je míra, která představuje power (kombinace procesoru, paměti a sítě přidělení zdrojů) jednu jednotku v Data Factory. Vyšší výkon kopírovat můžete dosáhnout využívání některých scénářích další DMUs. Podívejte se do [cloudu dat pohyb jednotky](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) části na stránce Podrobnosti.

**Zdroje** | **Výchozí dolní mez** | **Minimální omezení**
-------- | ------------------- | -------------
Plánování intervalu | 15 minut | 15 minut
Interval mezi pokusů | 1 sekunda | 1 sekunda
Opakovat hodnotu časového limitu | 1 sekunda | 1 sekunda


### <a name="web-service-call-limits"></a>Limity volání webové služby

Azure správce prostředků omezuje pro rozhraní API volání. Rozhraní API můžete volat sazbou v [Azure správce prostředků API omezení](../azure-subscription-service-limits.md#resource-group-limits). 


