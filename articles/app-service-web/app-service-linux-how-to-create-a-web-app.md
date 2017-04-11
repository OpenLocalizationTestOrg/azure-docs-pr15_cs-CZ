<properties 
    pageTitle="Jak vytvořit Web Appu s aplikaci služby na Linux | Microsoft Azure" 
    description="Web pracovního postupu aplikace vytvoření aplikace služby na Linux." 
    keywords="Služba Azure aplikací v prohlížeči, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>Vytvořit Web Appu se službou aplikace na Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Vytvoření webové aplikace pomocí portálu pro správu
Vytvoření webové aplikace na Linux z [portálu pro správu](https://portal.azure.com) , jak je znázorněno na následujícím obrázku můžete začít.

![][1]

Jakmile vyberete možnost níže, zobrazí se zásuvné vytvořit jak je znázorněno na následujícím obrázku. 

![][2]

-   Zadejte název svojí webové aplikace.
-   Zvolte existující skupiny zdrojů nebo vytvořte nový účet. (Viz oblasti k dispozici v [části omezení](./app-service-linux-intro.md)).
-   Vyberte existující plán služeb aplikací nebo vytvořte novou jednu (viz aplikace služby plán poznámky v [části omezení](./app-service-linux-intro.md)). 
-   Zvolte možnosti balíček aplikace, kterou chcete použít. Bude si můžete vybrat mezi více verzích Node.js a PHP. 

Až budete mít aplikaci vytvořili, můžete změnit balíčku aplikace z aplikace nastavení jak je znázorněno na následujícím obrázku.

![][3]

## <a name="deploying-your-web-app"></a>Nasazení aplikace pro web

Výběr "možnosti nasazení" z portálu Správa poskytuje možnost umožňuje místním úložišti libovolná nebo GitHub nasazení aplikace. Pokynů poté mají podobně-Linux web appu a podle pokynů v naší [místní nasazení libovolná](./app-service-deploy-local-git.md) nebo naší [průběžné nasazení](./app-service-continuous-deployment.md) Base pro GitHub.

Můžete taky FTP nahrát aplikace na web. Koncový bod FTP pro webovou aplikaci získáte v části protokolování diagnostiky jak je znázorněno na následujícím obrázku.

![][4]


## <a name="next-steps"></a>Další kroky ##

* [Co je aplikace služby na Linux?](./app-service-linux-intro.md)
* [Pomocí konfigurace PM2 pro Node.js ve webových aplikacích na Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
