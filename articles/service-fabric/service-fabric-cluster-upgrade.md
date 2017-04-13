<properties
   pageTitle="Upgrade struktury služby Azure clusteru | Microsoft Azure"
   description="Upgrade kód služby struktury a/nebo konfiguraci, která běží služba struktury clusteru, včetně nastavení režimu aktualizace clusteru upgradu certifikáty, přidání aplikací portů udělat s operačním systémem opravy a tak dále. Co můžete očekávat, když aktualizuje provádí?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Upgrade clusteru struktury služby Azure

> [AZURE.SELECTOR]
- [Azure obrázku](service-fabric-cluster-upgrade.md)
- [Samostatného obrázku](service-fabric-cluster-upgrade-windows-server.md)

Navrhování pro zdokonalovány moderní systém, je klíčová k dosažení dlouhodobé úspěšné produktu. Struktury služby Azure clusteru je zdroj, který jste vlastníkem, ale částečně spravuje Microsoft. Tento článek popisuje, co je spravováno automaticky a co můžete nakonfigurovat sami.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Určení verze struktury, která poběží na svůj Cluster

Můžete nastavit svůj cluster při vydání nová verze se zobrazí struktury automatické upgrady nebo vybrat verzi podporované struktury, která že se má svůj cluster na.

Udělejte toto nastavení konfigurace "upgradeMode" obrázku na portálu nebo pomocí Správce prostředků při vytváření nebo později živou obrázku 

>[AZURE.NOTE] Ujistěte se, aby svůj cluster vždy používat verzi podporované struktury. Jak a jsme oznámení nové verze služby struktury, v předchozí verzi je označen pro ukončení podpory nejméně 60 dní po. nové verze jsou oznámená [na blogu týmu struktury služby](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Je k dispozici a potom vyberte nová verze. 

14 dní před uplynutí vydání spuštěné svůj cluster událost nastavit jako stavu je generováno, které umístí svůj cluster do stavu upozornění. Clusteru zůstane ve stavu upozornění, dokud upgradu na verzi podporované struktury.


### <a name="setting-the-upgrade-mode-via-portal"></a>Nastavení upgradu režimu prostřednictvím portálu 

Obrázku můžete nastavit automatické nebo ruční při vytváření clusteru.

![Create_Manualmode][Create_Manualmode]

Obrázku můžete nastavit automatické nebo ruční v živé clusteru, pomocí možnosti spravovat. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Upgrade na novou verzi clusteru, která je nastavená jako ruční režimu prostřednictvím portálu.
 
Upgrade na novou verzi, musíte udělat stačí vyberte dostupné verzi z rozevíracího seznamu a uložit. Upgrade struktury získá vykazuje postavu automaticky. Zásady stavu obrázku (kombinace uzel zdraví a stavu všechny spuštěné v clusteru) jsou zachovány během upgradu.

Pokud není splněná zásady clusteru stavu upgradu je vrátit zpět. Posuňte se dolů tento dokument k další informace o tom, jak nastavit tyto zásady vlastní stavu. 

Jakmile máte pevná problémy, jejichž výsledkem vrácení zpět, budete muset zahajte upgrade znovu pomocí následujících stejným způsobem jako před.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Nastavení upgradu režimu pomocí šablony správce prostředků 

Přidání konfigurace "upgradeMode" definici Microsoft.ServiceFabric/clusters zdroje a nastavte "clusterCodeVersion" na jeden z podporovaných struktury verze, jak je ukázáno v následujícím příkladu a nasazení šablony. Platné hodnoty "upgradeMode" je "Ruční" nebo "Automatické"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Upgrade na novou verzi clusteru, která je nastavená jako ruční režimu pomocí šablony správce prostředků.
 
Pokud clusteru je ruční režimu, upgradovat na novou verzi přejděte podporovaná verze "clusterCodeVersion" a nasazení. Nasazení šablony, zejména kopance upgradů struktury získá vykazuje postavu automaticky. Zásady stavu obrázku (kombinace uzel zdraví a stavu všechny spuštěné v clusteru) jsou zachovány během upgradu.

Pokud není splněná zásady clusteru stavu upgradu je vrátit zpět. Posuňte se dolů tento dokument k další informace o tom, jak nastavit tyto zásady vlastní stavu. 

Jakmile máte pevná problémy, jejichž výsledkem vrácení zpět, budete muset zahajte upgrade znovu pomocí následujících stejným způsobem jako před.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Zobrazí seznam všech dostupných verzí na všechny prostředí pro dané předplatné

Spusťte tento příkaz a výstup by měla získat nějak takto.

Pokud říká "supportExpiryUtc" dané verzi blíží se konec platnosti nebo vypršela. Nejnovější verze nemá platná kalendářní data - má hodnotu "9999-12-31T23:59:59.9999999", která, znamená to, že datum vypršení platnosti ještě není nastavená.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Upgrade chování struktury po clusteru Upgrade režimu je automaticky

Společnost Microsoft udržuje kód struktury a konfigurace, které se spouští v Azure obrázku. Provedeme automatické upgrady sledované k softwaru na základě podle potřeby. Tato aktualizace může být kód, konfigurace nebo obojí. Abyste měli jistotu, že aplikace utrpí žádný vliv nebo minimální dopad kvůli tyto aktualizace, provedeme inovace v následující fáze:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fáze 1: Upgrade probíhá pomocí všechny zásady stavu obrázku

V této fázi inovace pokračovat jednu doménu upgrade po druhém a spuštěné v clusteru nadále pracovat bez jakékoli výpadek služeb. Zásady stavu obrázku (kombinace uzel zdraví a stavu všechny spuštěné v clusteru) jsou zachovány během upgradu.

Pokud není splněná zásady clusteru stavu upgradu je vrátit zpět. Klepněte na možnost vlastník předplatného se odešle e-mail. E-mail obsahuje následující informace:

- Oznámení, aby bylo vrátit zpět upgrade obrázku.
- Funkce navrhované akcí, pokud existuje.
- Počet dnů (n), dokud nemůžeme spustit fázi 2.

Pokusíme provést stejné upgrade několik krát v případě, že všechny aktualizace failed infrastruktury důvodů. Po n dnů od data, který byl odeslán e-mailu doporučujeme pokračujte fázi 2.

Pokud jsou splněny zásady stavu clusteru, upgrade považuje za úspěšné a označeny jako dokončené. K tomu může dojít během počáteční upgrade nebo některý inovací opakování v této fázi. Neexistuje žádná e-mailové potvrzení úspěšně spustit. Toto je Vyhněte se odesílání, že je příliš mnoho e-mailů – příjem e-mailu by měly považovat za výjimku na normální. Očekává většina clusteru aktualizace proběhla úspěšně bez vlivu na své dostupnosti aplikace.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fáze 2: Upgrade probíhá pomocí výchozího stavu pouze zásad

Zásady stavu v této fázi nastaveny tak, že počet aplikace, které byly správný začátkem upgrade zůstane po celou dobu procesu upgradu. Jako fázi 1 upgrady fázi 2 pokračovat jednu doménu upgrade po druhém a spuštěné v clusteru nadále pracovat bez jakékoli výpadek služeb. Zásady stavu obrázku (kombinace uzel zdraví a stavu všechny spuštěné v clusteru) jsou zachovány po celou dobu upgradu.

Pokud zásady stavu obrázku v podstatě splněná, upgrade je vrátit zpět. Klepněte na možnost vlastník předplatného se odešle e-mail. E-mail obsahuje následující informace:

- Oznámení, aby bylo vrátit zpět upgrade obrázku.
- Funkce navrhované akcí, pokud existuje.
- Počet dnů (n), dokud nemůžeme spustit fáze 3.

Pokusíme provést stejné upgrade několik krát v případě, že všechny aktualizace failed infrastruktury důvodů. Připomenutí e-mailu několik dny před naplno n dnů. Po n dnů od data, který byl odeslán e-mailu doporučujeme pokračujte fáze 3. E-mailů, které vám pošleme ve fázi 2 třeba vážně a třeba akcí.

Pokud jsou splněny zásady stavu clusteru, upgrade považuje za úspěšné a označeny jako dokončené. K tomu může dojít během počáteční upgrade nebo některý inovací opakování v této fázi. Neexistuje žádná e-mailové potvrzení úspěšně spustit.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fáze 3: Upgrade probíhá pomocí zásady agresivní stavu

Tyto zásady stavu v této fázi jsou zaměřené dokončování upgradu spíše než stavu aplikace. Hodně málo upgrady clusteru skončily v této fázi. Pokud svůj cluster bude této fázi, je velmi pravděpodobné, že aplikace změní chybná a/nebo dojít ke ztrátě dostupnosti.

Podobně jako jiné dvou fází upgrady fáze 3 pokračovat jednu doménu upgrade po druhém.

Pokud není splněná zásady clusteru stavu upgradu je vrátit zpět. Pokusíme provést stejné upgrade několik krát v případě, že všechny aktualizace failed infrastruktury důvodů. Potom se Připne clusteru, tak, aby se už poskytovat podporu a aktualizace.

E-mailu se tyto informace jsou odeslány vlastník předplatného, spolu s nápravné akce Očekáváme nejsou žádné clusterů získat do stavu místo, kam se nezdařila fáze 3.

Pokud jsou splněny zásady stavu clusteru, upgrade považuje za úspěšné a označeny jako dokončené. K tomu může dojít během počáteční upgrade nebo některý inovací opakování v této fázi. Neexistuje žádná e-mailové potvrzení úspěšně spustit.

## <a name="cluster-configurations-that-you-control"></a>Konfigurace clusterů, které ovládat

Kromě možnosti nastavení clusteru režim upgradu, tady je konfigurace, které můžete změnit živou clusteru.

### <a name="certificates"></a>Certifikáty

Můžete přidat novou nebo snadno odstranit certifikáty pro obrázku a klientů prostřednictvím portálu. Podívejte se na [Tento dokument podrobné pokyny](service-fabric-cluster-security-update-certs-azure.md)

![Snímek obrazovky zobrazující certifikát thumbprints Azure portálu.][CertificateUpgrade]


### <a name="application-ports"></a>Porty aplikací

Porty aplikací můžete změnit změnou vlastnosti Vyrovnávání zatížení zdroje, které jsou přidružené k typu uzlu. Můžete používat portál nebo můžete použít Správce prostředků Powershellu přímo.

Otevřete nový port na všechny VMs v typu uzel, postupujte takto:

1. Přidáte nové zkušební Vyrovnávání zatížení odpovídající.

    Nasazení svůj cluster pomocí portálu Vyrovnávání zatížení jsou s názvem "LB-název zdroje skupiny – NodeTypename", jedno pro každý typ uzlu. Protože názvy Vyrovnávání zatížení jsou jedinečné pouze v rámci skupiny zdrojů, je nejlepší hledání podle určité skupiny zdrojů.

    ![Snímek obrazovky zobrazující přidání zkušební Vyrovnávání zatížení na portálu.][AddingProbes]

2. Přidáte nové pravidlo pro vyrovnávání zatížení.

    Přidání nové pravidlo pro stejný Vyrovnávání zatížení pomocí zkušební, který jste vytvořili v předchozím kroku.

    ![Přidání nové pravidlo pro vyrovnávání zatížení na portálu.][AddingLBRules]


### <a name="placement-properties"></a>Vlastnosti umístění

Pro každý typ uzel můžete přidat vlastní umístění vlastnosti, které chcete použít v aplikacích. NodeType je ve výchozím nastavení využívající bez přidání explicitně.

>[AZURE.NOTE] Informace o použití umístění omezení, vlastnosti uzlu a jak je definovat naleznete v části "Umístění omezení a uzel vlastnosti" v správce prostředků clusteru služby struktury dokumentu [s popisem](service-fabric-cluster-resource-manager-cluster-description.md)clusteru.

### <a name="capacity-metrics"></a>Metriky kapacity

Pro každý typ uzel můžete přidat vlastní kapacita metriky, který chcete použít v aplikacích zatížení sestavy. Podrobné informace o použití metriky kapacitu k sestavě načíst, podívejte se na správce prostředků clusteru struktury služby dokumenty na [Popisující obrázku](service-fabric-cluster-resource-manager-cluster-description.md) a [metriky a načíst](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Nastavení upgradu struktury - zásad stavu

Můžete zadat vlastní stavu zásad pro upgrade struktury. Pokud nastavíte svůj cluster struktury automatické upgrady získat tyto zásady použít fázi 1 inovací automatické struktury.
Pokud jste nastavili svůj cluster pro ruční struktury upgradů, získat tyto zásady použije pokaždé, když vyberete novou verzi spuštění systému a spusťte tak upgrade struktury clusteru. Pokud není přepsat zásady, budou použity výchozí hodnoty.

Můžete zadat vlastní stavu zásad nebo zkontrolovat aktuální nastavení v části "struktury upgrade" zásuvné tak, že vyberete Upřesnit nastavení upgradu. Přečtěte si o tom, jak na následujícím obrázku. 

![Správa zásad vlastní stavu][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Přizpůsobení nastavení struktury pro svůj cluster

Podívejte se do [služby struktury clusteru struktury nastavení](service-fabric-cluster-fabric-settings.md) na co a jak je možné upravit.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>Operační systém opravy VMs tvořící clusteru

Tato možnost je plánován do budoucna jako funkci automatického. Ale aktuálně, jste zodpovědní k opravě vašeho VMs. Tento jeden OM najednou, musíte udělat tak, aby neprovedete dolů víc než jeden po druhém.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>O VMs tvořící clusteru inovace OS

Pokud je třeba upgradovat operační systém obrázek na virtuálních počítačích clusteru, musíte udělat ho OM jeden po druhém. Zodpovídáte pro tento upgrade – v současné době není žádný automatizaci pro tuto.

## <a name="next-steps"></a>Další kroky
- Zjistěte, jak upravit některá [Nastavení struktury clusteru struktury služby](service-fabric-cluster-fabric-settings.md)
- Zjistěte, jak zobrazit [svůj cluster nebo zmenšit](service-fabric-cluster-scale-up-down.md)
- Další informace o [aplikaci upgrady](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG