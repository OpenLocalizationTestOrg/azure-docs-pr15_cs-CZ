<properties
    pageTitle="Webová aplikace na Azure zásobníku – známé problémy a řešení problémů s | Microsoft Azure"
    description="Podrobné pokyny pro nasazení Web Apps ve vrstvě Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Webové aplikace zdroje poskytovatele - známých problémů a řešení potíží

> [AZURE.NOTE] Tyto informace platí jenom pro nasazení TP1 zásobníku Azure.

Pokud máte problémy při nasazení nebo práci se Web Apps na Azure zásobníku naleznete v pokynech pod.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure zásobníku webové aplikace není k dispozici na portálu

![Azure zásobníku Web Apps vytvořte novou webovou aplikaci][1]

Po [registraci poskytovatele Azure zásobníku webových aplikací](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) jste dokončili, pokud se nemůžete najít Web + mobilní zásuvné, a potom proveďte následující akci:
* **Odhlášení z portálu** a **zavřete okno prohlížeče** a potom do **nové prohlížeče okno přihlášení k portálu** a opakujte.

Pokud se pořád nezobrazuje na webu a mobilní zásuvné proveďte následující akci:

1.  Na **Hostitelském počítači Azure zásobníku** ve **Správci Hyper-V** vyhledejte **xRPVM** a **Připojit** v angličtině.
2.  Otevřete **Příkazový řádek jako správce** a spusťte **IISRESET**
3.  Vraťte se do **ClientVM** a otevřete **nové okno prohlížeče**, **Přihlaste se k portálu** a zkuste to znovu.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Při vytváření do webových aplikací do nové skupiny prostředků dochází k selhání nasazení

V okně první náhled z webové aplikace musí být **Všechny webové aplikace** vytvořeny ve stejné skupině zdroje jako poskytovatele zdroje webových aplikací (**Místní AppService**).  Toto je známý problém a bude v budoucích náhled vyřešit.

## <a name="other-issues"></a>Jiné problémy

Pokud narazíte na jiné problémy s Web Apps na Azure zásobníku můžete vystavit [fóru zásobníku Azure] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) kde členové náš tým sledujete příspěvky.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
