<properties
    pageTitle="Co se stalo s projekt služby cloudu? | Microsoft Azure | Visual Studio připojené služby"
    description="Popisuje, co se stane v projektu cloud services po připojení k účtu Azure úložiště pomocí aplikace Visual Studio připojené služby"
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

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Co se stalo s projektových cloud services (služby Visual Studio Azure úložiště připojení)?

## <a name="references-added"></a>Přidání odkazů

Balíček Azure úložiště NuGet jsme přidali do aplikace Visual Studio projektu.  
Tento balíček přidá následující odkazy .NET:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro přidané úložiště Azure
Prvky jsou vytvořené pomocí připojovacího řetězce a klíč účtu vybrané úložiště. Změny provedené na následující soubory:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
