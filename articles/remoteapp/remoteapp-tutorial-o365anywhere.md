<properties
   pageTitle="Získat stejný prostředí Office 365 na zařízení s Azure RemoteApp | Microsoft Azure"
   description="Naučte se sdílet libovolné aplikaci Office 365 s jinými uživateli pomocí Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Získat stejný prostředí Office 365 na zařízení s Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Tento článek se zabývá nasazení Office 365 na libovolném zařízení ve vaší společnosti. Uživatele můžete získat stejné možnosti a možností uživatelského rozhraní pro Android, Apple a Windows.

Toho dosáhnete pomocí hostingu Office 365 na možnost měřítko virtuálních počítačích v Azure, které uživatelé mohli připojovat ke Azure RemoteApp. Tuto sadu virtuálních počítačích jsme volání "cloudu kolekce".

## <a name="create-a-cloud-collection"></a>Vytvoření kolekce cloudu

Nejdřív vytvořený účet Azure přejděte **RemoteApp** kliknutím na odkaz na levé straně.
![Zobrazování Azure RemoteApp na portálu Azure](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Pokračujte kliknutím na **Nový** na dolní a "snadné vytváření" kolekce. Zadejte název oblasti, předplatné, plánu a obrázek "Office Proffesional 2013", nabízíme.
![Dialogové okno vytvořit](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Po dokončení na formulář, který by měly začít s vytvářením kolekce. To může trvat až jednu hodinu, aby.

![Čekání](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Po dokončení procesu, bude vypadat takto. Pokud nám klikněte na **publikování** vidíme, že většina aplikací Office jste publikovali nám už.
![Vytvoření kolekce](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Publikované aplikace](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

V tomto okamžiku můžete také přidat další uživatele, kteří mají přístup k této kolekci tak, že kliknete na **Přístup uživatelů**.
![Nastavení uživatelského přístupu](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Teď Pojďme vyzkoušet si připojení k Office 365!

## <a name="connect-to-office-365"></a>Připojení ke službě Office 365

Budeme se sídlo přes [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), přejděte dolů a klikněte na **Stáhnout klienty** nainstalovat klientovi Azure RemoteApp na zařízení, které jste na. Snímky obrazovek níže jsou určené pro Windows.

Po spuštění aplikace budete vyzváni k přihlášení pomocí účtu Microsoft (dříve označovaného jako "Live ID"), použijte stejný jako ten jako účet Azure nyní. Když jste přihlášení v by měl zobrazit oznámení o nové pozvánky, klikněte na nějaké a měl zobrazit seznam jako to dole. Přijmete pozvání, která odpovídá e-mailu vlastníka účet Azure.

![Nové pozvání](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Jak to vypadá po nové pozvánky.

![Přijmout žádost](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Po přijetí pozvání byste měli vidět všechny aplikace Office v klientovi Azure RemoteApp.

![Seznam aplikací](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Po kliknutí na jeden z těchto kroků, které aplikace by měly začít na Azure virtuální počítač a budete by měl být všechny sady! Využijte!

![spuštění](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![aplikace PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
