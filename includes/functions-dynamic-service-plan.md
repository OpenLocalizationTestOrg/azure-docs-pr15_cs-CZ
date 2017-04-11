## <a name="dynamic-service-plan"></a>Plán dynamické služeb

V dynamické služby plánu funkce aplikace přiřazené instanci aplikace (funkce). Pokud potřeby další instance se přidá dynamicky.
Tato instance může zahrnovat napříč několika výpočetních prostředků, jak nejlépe využít dostupné Azure infrastruktury. Kromě toho funkce spustí paralelně minimalizace celkovou dobu potřebnou k zpracovávat požadavky. Čas spuštění jednotlivých funkcí sečtená v sekundách a agregovat v aplikaci obsahující (funkce). S náklady řízený úsilím počtem instance jejich velikosti paměti a celkový čas spuštění jako v sekundách GB. Toto je pracovníků možnost, pokud jsou vaše potřeby výpočetním chybovému nebo pracovních hodin jsou obvykle velmi krátké jako umožňuje zaplatit jenom pro zdroje výpočetním okamžiku, kdy je skutečně používá.   

### <a name="memory-tier"></a>Vrstvy paměti

V závislosti na vaší funkce musí můžete vybrat paměti potřebné ke spuštění v aplikaci funkce (container funkcích).
Možnosti velikosti paměti se liší od **128 MB 1536 MB**. Velikost vybrané paměti odpovídá pracovní sada potřeby tak, že všechny funkce, které jsou součástí aplikace (funkce). Pokud váš kód vyžaduje více paměti, než je vybrané velikost, instance aplikace funkce vypnout kvůli nedostatku paměti.

### <a name="scaling"></a>Změna měřítka

Platformu Azure funkce vyhodnotí přenosy potřeb, podle nakonfigurovaného aktivačních událostí se rozhodnout, kdy se má zobrazit nahoru nebo dolů. Rozlišení měřítku je funkce aplikace. Škálování v tomto případě znamená, že přidáte další instance aplikace (funkce). Nepřímo přenosy přejde, funkce instancemi aplikace jsou zakázané-postupně zmenšení na nulovou hodnotu když jsou spuštěné žádné.  

### <a name="resource-consumption-and-billing"></a>Využití prostředků a fakturace

V dynamické přidělení zdrojů režimu dělají jinak než standardní plán služeb aplikací, model spotřebu tedy i jiné, umožňují modelu "platovou za použití". Spotřeba bude vykázaného za funkce aplikace pouze dobu, kdy je prováděn kód.  
Výpočet je vynásobením velikosti paměti (v GB) tak, že celkové množství času spuštění (v sekundách) pro všechny funkce běží uvnitř, které funkce aplikace. Jednotky spotřeby budou **GB-ne (gigabajtů sekundy)**.