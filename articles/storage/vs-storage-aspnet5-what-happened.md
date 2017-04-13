<properties
    pageTitle="Co se stalo s projekt ASP.NET 5 (Visual Studio připojení služby) | Úložiště Microsoft Azure"
    description="Popisuje, co se stane po připojení k účtu Azure úložiště ve Visual Studiu ASP.NET 5 projektu pomocí aplikace Visual Studio připojené služby"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Co se stalo s projekt ASP.NET 5 (Visual Studio Azure úložiště připojení služby)?

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

Navíc jsme přidali balíček NuGet **Microsoft.Framework.Configuration.Json** .

## <a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro přidané úložiště Azure
V souboru config.json projektu prvku, který byl vytvořený pomocí připojovacího řetězce a klíč účtu vybrané úložiště.

Další informace najdete v tématu [ASP.NET 5](http://www.asp.net/vnext).
