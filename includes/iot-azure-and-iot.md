
# <a name="azure-and-internet-of-things"></a>Azure a Internet věcí

Vítá vás Microsoft Azure a Internet věcí (IoT). Tento článek představuje architekturu řešení IoT, která popisuje běžné vlastnosti IoT řešení, které můžete nasadit pomocí služby Azure. IoT řešení vyžaduje zabezpečené obousměrnou komunikaci mezi zařízeními, případně číslování milionů a back-end řešení. Back-end řešení může například použít automatizovaný, prediktivní analytiky odhalit poznatky z vašeho proudu událostí zařízení cloud.

Azure IoT rozbočovač je základní kámen při implementaci této architektury řešení IoT Azure služby. IoT Suite poskytuje kompletní, end-to-end, implementace této architektury pro konkrétní scénáře IoT. Například: 

- Řešení *vzdálené sledování* umožňuje sledovat stav zařízení, jako jsou prodejní automaty. 
- Řešení *prediktivní údržby* vám pomůže odhadnout potřeby údržby zařízení jako jsou čerpadla v vzdálené čerpacího stanoviště a k zamezení neplánované prostoje.

## <a name="iot-solution-architecture"></a>Architektura řešení IoT

Následující diagram ukazuje typické architektury řešení IoT. Diagram neobsahuje názvy všech zvláštních služeb Azure, ale popisuje klíčových prvků v obecné architektury řešení IoT. V této architektuře IoT zařízení shromažďovat údaje, které zasílají shluk brány. Brána cloudu zpřístupní data pro zpracování i ostatní služby back-end, z kterých se data doručena jiných řádku obchodních aplikací nebo lidské subjekty prostřednictvím řídicího panelu nebo jiné zařízení pro prezentaci.

![Architektura řešení IoT][img-solution-architecture]

> [AZURE.NOTE] Podrobný popis architektury IoT, naleznete v tématu [Microsoft Azure IoT referenční architektura][lnk-refarch].

### <a name="device-connectivity"></a>Připojení zařízení

V této architektuře řešení IoT zařízení odesílání telemetrické například senzor pro měření z čerpacího stanoviště, do koncového bodu cloud skladování a zpracování. Ve scénáři prediktivní údržby back-end může být datový proud dat senzor k určení, kdy konkrétní čerpadlo vyžaduje údržbu. Zařízení lze také přijímat a reagovat na příkazy cloudu zařízení čtení zpráv z koncového bodu cloud. Například v případě prediktivní údržby řešení back-end může odeslat příkazy jiných čerpadel čerpacího stanice zahájit přesměrování toků těsně před údržby je termín spuštění, ujistěte se, že inženýr údržby můžete zahájit, jestliže uživatel přijde.

Jedním z největších výzev, kterým čelí projekty IoT je jak spolehlivě a bezpečně připojit k back-end řešení zařízení. IoT zařízení mají různé vlastnosti v porovnání s jinými klienty prohlížeče a mobilní aplikace. IoT zařízení:

- Často jsou vložené systémy s žádný operátor.
- Mohou být nasazeny na vzdálených místech, kde je nákladné fyzický přístup.
- Může pouze být dosažitelné prostřednictvím řešení back-end. Existuje jiný způsob, jak komunikovat se zařízením.
- Mohou mít omezené, energie a prostředků zpracování.
- Může být přerušovaný, pomalá či nákladná síťová připojení.
- Mohou potřebovat speciální, vlastní nebo oborové aplikační protokoly.
- Lze vytvořit pomocí rozsáhlé sady populární hardwarových a softwarových platformách.

Kromě výše uvedených požadavků zaujme také jakékoli řešení IoT stupnice, zabezpečení a spolehlivost. Výsledná sada požadavků na připojení je pevný a časově náročné provádět pomocí tradičních technologií, jako jsou kontejnery webových a zasílání zpráv makléřů. Azure IoT Hub and SDK zařízení IoT usnadnit řešení, která splňují tyto požadavky.

Zařízení mohou přímo komunikovat s koncového bodu cloudu brány nebo pokud zařízení nemůže použít některý z komunikačních protokolů, které podporuje brány cloud, můžete připojit pomocí zprostředkující brány. Například [Azure IoT gateway protocol] [ lnk-protocol-gateway] můžete provést překlad protokolu v případě, že zařízení nelze použít žádný z protokolů, které podporuje IoT rozbočovač.

### <a name="data-processing-and-analytics"></a>Zpracování dat a analytics

V cloudu je IoT řešení zpět end, kde většinu zpracování dat dochází, jako například filtrování a seskupení telemetrické a směrování do jiných služeb. Řešení IoT zpět konec:

- Obdrží od zařízení telemetrické v měřítku a určuje, jak zpracovat a uložit tato data. 
- Umožňuje odeslat příkazy pro konkrétní zařízení z cloudu.
- Poskytuje možnosti registrace zařízení, které umožňují poskytování zařízení a ovládací zařízení, která jsou povoleny pro připojení k vaší infrastruktury.
- Umožňuje sledovat stav zařízení a sledovat jejich činnost.

V případě prediktivní údržby řešení back-end ukládá data historické telemetrické. Back-end lze tato data slouží ke zjištění charakteristiky, které označují, že údržba je splatná v určité čerpadla.

IoT řešení může obsahovat smyčky automatické zpětné vazby. Například modul analytics v back-end může identifikovat z telemetrické, který konkrétního zařízení je nad běžnou provozní úroveň. Řešení potom může odeslat příkaz k zařízení, pokynem přijmout nápravná opatření.

### <a name="presentation-and-business-connectivity"></a>Prezentační a obchodní spojení

Prezentační a obchodní vrstvu konektivity umožňuje koncovým uživatelům interakci s IoT řešení a zařízení. Umožňuje uživatelům zobrazit a analyzovat data shromážděná ze svých zařízení. Tato zobrazení může být ve formě řídicích panelů a sestav BI, které mohou zobrazit historická data nebo u dat v reálném čase. Například operátor můžete zkontrolovat stav konkrétní stanice čerpacího a zobrazit všechny výstrahy vyvolané systémem. Tato vrstva umožňuje také integrační IoT řešení back-end s existující řádek obchodní aplikace propojení do organizace obchodních procesů a pracovních postupů. Například řešení prediktivní údržby lze integrovat se systémem plánování knih inženýr k návštěvě čerpacího stanice při řešení identifikuje čerpadla v případě údržby.

![Řídicí panel řešení IoT][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
