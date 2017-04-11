<properties
   pageTitle="Výchozí velikost složky TEMP je příliš malá pro roli | Microsoft Azure"
   description="Role služby cloudu má omezené množství místa pro složku TEMP. Tento článek obsahuje několik návrhů, jak chcete-li předejít nedostatku prostoru."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Výchozí velikost složky TEMP je moc malé na webu nebo pracovního roli služby cloudu

Dočasné adresáři výchozí role pracovníka nebo webové služby cloudu obsahuje maximální velikosti doručovaných 100 MB, což se může stát celou nastane okamžik. Tento článek popisuje, jak chcete-li předejít dostatek místa pro dočasný adresář.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Proč mám docházet prostor?

Standardní proměnné prostředí Windows TEMP a technického jsou dostupné pro kód, který běží v aplikaci. TEMP a technického přejděte na jeden adresář, který obsahuje maximální velikosti doručovaných 100 MB. Všechna data, která je uložená v této složce není zachován přes životním cyklu cloudové služby, Pokud jsou z koše instance role v do cloudové služby, adresář vyčistit.

## <a name="suggestion-to-fix-the-problem"></a>Návrhy k řešení problému

Implementace jednu z následujících možností:

- Konfigurace místního úložiště zdroje a přístup k přímo místo použití TEMP nebo technického. Přístup k prostředku Místní úložiště z kód, který běží v rámci aplikace, zavolejte na metodu [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) . 

- Konfigurace místní uložení zdrojů a přejděte adresářů TEMP a technického tak, aby ukazovaly na cestu místní úložiště zdroje. Tato změna se provádí v rámci metody [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

Následující příklad ukazuje, jak upravit cílovou adresářů pro TEMP a technického z v rámci metody při spuštění:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Další kroky

Přečtěte si blog, který popisuje, [jak chcete zvětšit Azure Web Role ASP.NET dočasné složky](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Zobrazte další [Poradce při potížích s články](/?tag=top-support-issue&product=cloud-services) při cloudovým službám.

Další řešení problémů s cloudové služby rolí pomocí Azure PaaS počítačových diagnostiky dat, zobrazení [Jan Williamson řada](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
