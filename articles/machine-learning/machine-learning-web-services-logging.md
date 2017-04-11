<properties 
    pageTitle="Protokolování webové služby strojového výukové | Microsoft Azure" 
    description="Zjistěte, jak povolit protokolování pro počítač výukové webové služby. Další informace můžou pomoct odstranit rozhraní API obsahuje protokolování." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Povolit protokolování pro počítač výukové webové služby  

Tento dokument obsahuje informace o funkci protokolování aplikace klasické webové služby. Povolení protokolování ve webových službách poskytující další informace, za právě číslo chyby a zprávy, která vám můžou pomoct řešit volání do rozhraní API výukové počítače.  

Povolení protokolování ve webových službách Azure klasické portálu:   

1.  Přihlaste se k [portálu Azure klasické](https://manage.windowsazure.com/)
2.  Ve sloupci funkce vlevo klikněte na **VÝUKOVÉ počítače**.
3.  Klikněte na pracovního prostoru, pak **Webové služby**.
4.  V seznamu webové služby klikněte na název webové služby.
5.  V seznamu koncové body klikněte na název koncového bodu.
6.  Klikněte na **KONFIGUROVAT**.
7.  Nastavit **Úroveň sledování DIAGNOSTIKY** *chyby* nebo *všechny*a klikněte na **Uložit**.

Chcete-li povolit protokolování v portálu Azure počítače výukové webové služby.

1. Přihlaste se k portálu [Azure počítače výukové webové služby](https://services.azureml.net) .
2. Klikněte na klasické webové služby.
3.  V seznamu webové služby klikněte na název webové služby.
4.  V seznamu koncové body klikněte na název koncového bodu.
5.  Klikněte na **Konfigurovat**.
6.  Nastavení **protokolování** *chyb* nebo *všechny*a klikněte na **Uložit**.

## <a name="the-effects-of-enabling-logging"></a>Efekty s povolením protokolování

Pokud protokolování zapnutá, všechny diagnostiky a chyby z vybraný koncový bod přihlášeni k účtu úložiště Azure spojené s pracovního prostoru uživatele. Zobrazí se tento účet úložiště na portálu Azure klasické zobrazení řídicího panelu (konec oddílu Snadné přehledně uspořádané) jejich pracovního prostoru.  

Protokoly můžete zobrazit pomocí žádného z několika nástrojů, které můžete prozkoumat účet Azure úložiště. Nejjednodušší může být jednoduše přejít k účtu úložiště na portálu Azure klasické a potom klikněte na **kontejnery**. By zobrazí kontejner **ml diagnostiky**. Tento kontejner obsahuje všechny diagnostické informace pro všechny webové služby koncové body pro všechny pracovní prostory přidružený k tomuto účtu úložiště. 
 
## <a name="log-blob-detail-information"></a>Protokol objektů blob podrobné informace

Každý objektů blob v kontejneru obsahuje tyto informace diagnostických nástrojů pro právě jeden z těchto věcí:

-   Spuštění metody dávkové zpracování  
-   Spuštění metody žádostí a odpovědí  
-   Inicializace kontejneru žádostí a odpovědí
  
Název každého objektů blob má předponu následující formulář: 

{Pracovního prostoru Id}-{webová služba Id}-{Id koncového bodu} / {protokolu zadejte}  

Kde je typ protokolu jednu z následujících hodnot:  

- dávkové  
- skóre/požadavky  
- skóre/inicializace  