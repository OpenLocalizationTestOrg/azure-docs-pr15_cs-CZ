<properties
   pageTitle="Správa rolí v Azure cloud services projektů s Visual Studio | Microsoft Azure"
   description="Zjistěte, jak přidat nové role projektu služby Azure cloudu nebo odebrat role stávajících z něho pomocí aplikace Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Správa rolí v projekty služby Azure cloudu pomocí aplikace Visual Studio

Po vytvoření projektu služby Azure cloudu, můžete do ní přidávat nové role nebo odebrat role stávajících z něho. Můžete taky importovat existujícího projektu a převést na roli. Můžete třeba importovat webové aplikace technologie ASP.NET a označit jako roli web.

## <a name="adding-or-removing-roles"></a>Přidávání nebo odebírání rolí

**Chcete-li přidat role**

V **Okně Průzkumník**otevřete místní nabídky pro uzel **role** v projektu služby cloudu a zvolte **Přidat**. Můžete vybrat existující web nebo pracovní role z aktuálního řešení nebo vytvořte nový projekt webu nebo pracovního role. Nebo vyberte příslušné projekt, například projekt aplikace web technologie ASP.NET a přidružit role projektu.

**Odebrání role přidružení**

V uzel **role** projekt služby cloudu v okně Průzkumník otevřete místní nabídky pro roli chcete odebrat a zvolte **Odebrat**.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Odstranění a přidání role v cloudové službě

Je-li odebrat roli z vašeho projektu cloudové služby, ale později rozhodnete přidat roli zpět do projektu, se přidají jenom role deklarace a základní atributy, jako jsou například koncové body a diagnostice informace. Žádná další zdroje informací nebo odkazy se přidají do souboru ServiceDefinition.csdef nebo k souboru ServiceConfiguration.cscfg. Pokud chcete přidat tyto informace, budete muset ručně přidat zpátky do těchto souborů.

Například může odebrat roli webové služby a později rozhodnete přidat tuto roli zpátky do vašeho řešení. Když to uděláte, dojde k chybě. Nechcete, aby tato chyba, budete muset přidat `<LocalResources>` prvek vidět v následujícím XML zpět do souboru ServiceDefinition.csdef. Nesou název této role webové služby, které jste přidali zpět do projektu v rámci název atributu **<LocalStorage>** prvek. V tomto příkladu je název role webové služby **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Další kroky

Informace o tom, jak nakonfigurovat role ve Visual Studiu [Konfigurovat role pro službu Azure cloudu pomocí aplikace Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)pro čtení.
