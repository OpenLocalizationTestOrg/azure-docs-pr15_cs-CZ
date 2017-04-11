<properties
  pageTitle="Azure IoT sadu nejčastější dotazy týkající se | Microsoft Azure"
  description="Nejčastější dotazy IoT sadu"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Nejčastější dotazy IoT sadu

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Jaký je rozdíl mezi odstraněním skupina zdroje na portálu Azure a klepnutím na příkaz Odstranit na předkonfigurované řešení v azureiotsuite.com?

- Pokud odstraníte předkonfigurované řešení ve [azureiotsuite.com][lnk-azureiotsuite], odstraníte všechny zdroje, které poskytnuté při vytvoření předkonfigurované řešení; Pokud přidáte další zdroje informací ke skupině prostředků, tyto budou odstraněny také. 

- Pokud byste odstranili skupina zdroje [Azure portál][lnk-azure-portal], odstraníte pouze zdrojů v této skupině zdrojů; bude taky muset odstranění aplikace služby Azure Active Directory přidružené k předkonfigurované řešení [Azure klasické portál][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Kolik instancí IoT centrální můžete vytvořit v předplatné? 

Deset. Můžete vytvořit [Azure podporují lístek] [ link-azuresupportticket] na vysokoškolskou úroveň toto omezení, ale ve výchozím nastavení můžete jenom zřizujete deseti IoT rozbočovače jedno předplatné, jak je uvedeno v [Azure předplatné limity][link-azuresublimits]. V důsledku toho od každé předkonfigurované řešení ustanovení rozbočovači nové IoT, můžete jenom zřizujete maximálně deseti předkonfigurované řešení v dané předplatné. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Kolik instancí DocumentDB můžete vytvořit v předplatné?

50. Můžete vytvořit [Azure podporují lístek] [ link-azuresupportticket] na vysokoškolskou úroveň toto omezení, ale ve výchozím nastavení můžete jenom zřizujete 50 DocumentDB instance jedno předplatné. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Kolik bezplatné API mapy Bing můžete vytvořit v předplatné?

Dva. Pouze dvě interní transakce úrovně 1 mapy Bing pro plány Enterprise můžete vytvořit v Azure předplatného. Vzdálené monitorovací řešení máte k dispozici ve výchozím nastavení s plánem interní transakce úrovně 1. V důsledku toho můžete jenom zřizujete až dva vzdálené sledování řešení v předplatné beze změn.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Mám vzdálené sledování nasazení řešení s statickou mapu, jak přidat interaktivní mapu Bing? 
1. Získat vaše rozhraní API aplikace mapy Bing pro organizace QueryKey z [Azure portálu][lnk-azure-portal]: 
 1. Přejděte na skupina zdroje, kde je vaše rozhraní API mapy Bing pro podniky [Azure portál][lnk-azure-portal].
 2. Klikněte na všechna nastavení, pak klíčové správy. 
 3. Zjistíte, dva klíče: hlavního klíče a QueryKey. Zkopírujte hodnotu pro QueryKey.

     > [AZURE.NOTE] Nemáte rozhraní API aplikace mapy Bing pro účet organizace? Vytvořte si ho [Azure portál] [ lnk-azure-portal] podle klepnutím na + nový, vyhledávat API mapy Bing pro podniky a postupujte podle pokynů k vytvoření.

2. Rozbalovací nejnovější kód z [Azure IoT-vzdáleného – sledování][lnk-remote-monitoring-github].

3. Spuštění místního nebo cloudového nasazení sledovat příkazový řádek nasazování ve složce /docs/ v úložišti. 

4. Až se dostanete místního nebo cloudového nasazení, vyhledejte v kořenové složce pro *. user.config soubor vytvořený během nasazení. Otevřete tento soubor v textovém editoru. 

5. Změna obsahovat hodnotu, kterou jste zkopírovali z vaší QueryKey následující řádek: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Můžete vytvořit předkonfigurované řešení, pokud už nainstalovaný Microsoft Azure pro DreamSpark?
V současné době nejde vytvořit předkonfigurované řešení s [Microsoft Azure DreamSpark] [ lnk-dreamspark] účtu. Však můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] v jenom pár minut, které vám umožní vytvoříte předkonfigurované řešení.

### <a name="how-do-i-delete-an-aad-tenant"></a>Jak odstranit AAD klienta?

Přečtěte si [informace o odstraňování Tenanta Azure AD]příspěvek blogu Tomáš Golpe[lnk-delete-aad-tennant].

## <a name="next-steps"></a>Další kroky

Můžete také zkoumat některých dalších funkcí a možností řešení IoT sadu automaticky předem nakonfigurovaná:

- [Přehled prediktivní údržby automaticky předem nakonfigurovaná řešení][lnk-predictive-overview]
- [Zabezpečení IoT od základu nahoru][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
