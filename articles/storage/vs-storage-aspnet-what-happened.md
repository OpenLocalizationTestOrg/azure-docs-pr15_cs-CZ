<properties
    pageTitle="Co se stalo s projekt ASP.NET | Microsoft Azure | Visual Studio připojené služby"
    description="Popisuje, co se stane po přidání úložiště Azure ASP.NET projektu pomocí aplikace Visual Studio připojené služby"
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

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Co se stalo s projekt ASP.NET (Visual Studio Azure úložiště připojení služby)?

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

##<a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro přidané úložiště Azure
V souboru web.config projektu prvku, který byl vytvořený pomocí připojovacího řetězce a klíč účtu vybrané úložiště.

Další informace najdete v tématu [ASP.NET](http://www.asp.net).
