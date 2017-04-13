<properties
    pageTitle="Co se stalo s WebJob projektu (Visual Studio Azure úložiště připojení služby)? | Microsoft Azure"
    description="Popisuje, co se stalo v projektu Azure WebJob po připojení k úložišti účtu pomocí aplikace Visual Studio připojené služby"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Co se stalo s WebJob projektu (Visual Studio Azure úložiště připojení služby)?

## <a name="references-added"></a>Přidání odkazů

Balíček NuGet úložiště Azure byl přidán do nebo aktualizován ve Visual Studiu projektu.  
Tento balíček přidá následující odkazy .NET:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro přidané úložiště Azure
V souboru App.config projektu položky **AzureWebJobsStorage** a **AzureWebJobsDashboard** aktualizovaných s vybranou úložiště účtu připojovací řetězec a klíčem.

Další informace najdete v tématu [Azure WebJobs si přečtěte následující dokumentaci zdroje](http://go.microsoft.com/fwlink/?linkid=390226).
