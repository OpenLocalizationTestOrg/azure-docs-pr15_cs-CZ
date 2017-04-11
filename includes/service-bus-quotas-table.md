Následující tabulka uvádí informace o kvótách specifické pro zasílání zpráv služby Bus. Limity rozbočovače událost jsou však započítávány v této tabulce, ale podrobnější informace o události rozbočovače, najdete v článku [Ceny rozbočovače události](https://azure.microsoft.com/pricing/details/event-hubs/). Informace o ceny a jiných kvóty pro službu Bus najdete v článku Přehled [Ceny Bus služby](https://azure.microsoft.com/pricing/details/service-bus/) .

|Název kvóty|Rozsah|Typ|Chování při překročení|Hodnota|
|---|---|---|---|---|
| Maximální počet obory názvů na jedno předplatné Azure|Namespace|Statická|Následující požadavky na další obory názvů odmítnuta tak, že na portálu.|100|
|Velikost fronty nebo tématu|Entita|Definované při vytvoření fronty nebo tématu.|Odmítnuty příchozích zpráv a výjimku obdrží volající kód.|1, 2, 3, 4 a 5 GB.<br /><br />Pokud je povoleno [oddílů](service-bus-partitioning.md) , maximální fronty/téma velikost je 80 GB.|
|Počet souběžné připojení na obor názvů|Namespace|Statická|Následující požadavky na další připojení budou odmítnuty a výjimku obdrží volající kód. ZBÝVAJÍCÍ operace nepočítají směrem souběžné připojení TCP.|NetMessaging: 1 000<br /><br />AMQP: 5 000|
|Počet souběžné připojení na entity fronty/téma/předplatného|Entita|Statická|Následující požadavky na další připojení budou odmítnuty a výjimku obdrží volající kód. ZBÝVAJÍCÍ operace nepočítají směrem souběžné připojení TCP.|Uzavřeny tak, že omezení souběžné připojení obor.|
|Počet souběžné příjem žádosti o entita fronty/téma/předplatného|Entita|Statická|Zobrazit další žádosti o bude odmítnuté a výjimku obdrží volající kód. Tato kvóta platí pro kombinované počet souběžné příjem operace u všech předplatných témat.|5 000|
|Počet témata/fronty na obor názvů služby|Systémové|Statická|Další požadavky pro vytvoření nového tématu nebo fronty na obor názvů služby bude odmítnuto. Jako výsledek Pokud nakonfigurován přes [Azure portál][], chybová zpráva se vygeneruje. Pokud se jmenuje z rozhraní API pro správu, výjimku získali při volání kód.|10 000<br /><br />Celkový počet témata plus fronty v oboru služba musí být menší nebo rovna hodnotě 10 000.<br/>Toto není k dispozici Premium jako oddíly všechny entity.|
|Počet oddílů témata/fronty na obor názvů služby|Systémové|Statická|Další požadavky pro vytvoření nového tématu oddílů nebo fronty na obor názvů služby bude odmítnuté. Jako výsledek Pokud nakonfigurován přes [Azure portál][], chybová zpráva se vygeneruje. Pokud se jmenuje z rozhraní API pro správu, výjimce **QuotaExceededException** získali při použití volající kód.|Základní a standardní úrovní - 100<br />Premium - 1 000<br/><br />Jednotlivé oddíly fronty nebo tématu spočítá směrem kvóty 10 000 entit na obor názvů.|
|Maximální velikosti doručovaných žádným zpráv entity: fronty nebo tématu|Entita|Statická|-|260 znaků|
|Maximální velikosti doručovaných všech zpráv název entity: názvů, předplatné, předplatné pravidla nebo centrum události|Entita|Statická|-|50 znaků|
|Maximální velikosti doručovaných události rozbočovače události|Systémové|Statická|-|256 ZNALOSTNÍ BÁZI KNOWLEDGE BASE|
|Velikost zprávy entity fronty/téma/předplatného|Systémové|Statická|Odmítnuty příchozích zpráv, které překročení kvóty a výjimku obdrží volajícího kódu.|Maximální velikost zprávy: 256KB ([Standardní osy](../articles/service-bus/service-bus-premium-messaging.md)) / 1 MB ([Premium osy](../articles/service-bus/service-bus-premium-messaging.md)). <br /><br />**Poznámka:** Vzhledem k systému režijních toto omezení je obvykle mírně menší.<br /><br />Velikost maximální záhlaví: 64KB<br /><br />Maximální počet záhlaví vlastností v kontejneru: **bajt/int. MaxValue**<br /><br />Maximální velikosti doručovaných vlastnost v kontejneru: neomezeno explicitní. Omezené velikost maximální záhlaví.|
|Vlastnost velikost zprávy entity fronty/téma/předplatného|Systémové|Statická|Vygeneruje se **SerializationException** výjimku.|Maximální vlastnost velikost zprávy pro každou vlastnost je 32 kB. Souhrnná velikost všech vlastností nesmí přesáhnout 64 kB. Týká se celý záhlaví [BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx), který má obě vlastností uživatele, jakož i vlastnosti systému (například [SequenceNumber](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx) [Popisek](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx), [MessageId](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)a tak dál).|
|Počet předplatná na téma|Systémové|Statická|Další požadavky pro vytváření další předplatná v tématu bude odmítnuto. Jako výsledek Pokud nakonfigurované pomocí portálu chybová zpráva zobrazí. Pokud s názvem z rozhraní API, volající kód obdrží výjimku pro správu.|2 000|
|Počet filtrů SQL na téma|Systémové|Statická|Odmítnuty následující požadavky na další filtry v tématu Vytvoření a výjimku obdrží volajícího kódu.|2 000|
|Počet filtrů korelace na téma|Systémové|Statická|Odmítnuty následující požadavky na další filtry v tématu Vytvoření a výjimku obdrží volajícího kódu.|100,000|
|Velikost SQL filtry/akce|Systémové|Statická|Následující požadavky pro vytvoření další filtry budou odmítnuty a výjimku obdrží volající kód.|Maximální délka řetězce podmínka filtr: 1 024 (1 kB).<br /><br />Maximální délka řetězce akce pravidla: 1 024 (1 kB).<br /><br />Maximální počet výrazy za akce pravidla: 32.|
|Počet [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) pravidel za obor názvů, fronty nebo tématu|Entitu, obor názvů|Statická|Následující požadavky pro vytváření pravidla nehledá budou odmítnuty a výjimku obdrží volající kód.|Maximální počet pravidel: 12. <br /><br /> Pravidla, která je nakonfigurovaný na obor služby Bus platí pro všechny fronty a témata v tomto názvů.

[Azure portálu]: https://portal.azure.com