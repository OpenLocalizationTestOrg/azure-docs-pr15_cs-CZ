
1. Ve Visual Studiu Průzkumníku klikněte pravým tlačítkem myši na projekt aplikace pro Windows Store, klikněte na **Store** > **Přidružení App Store …**.

    ![Přidružení aplikace pro Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. V průvodci klikněte na tlačítko **Další**, přihlaste se pomocí svého účtu Microsoft, do **rezervy nový název aplikace**zadejte název aplikace a potom klikněte na **Rezervovat**.

3. Po registraci aplikace úspěšně, vyberte nový název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přiřadit**. Tím přidáte do manifest aplikace požadované informace registrace pro Windows Store.

7. Opakujte kroky 1 a 3 pro projekt aplikace Windows Phone Storu pomocí stejného registrace, který jste dříve vytvořili pro Windows Store.  

7. Přejděte na [Centrum pro vývojáře Windows](https://dev.windows.com/en-us/overview)přihlásit pomocí svého účtu Microsoft, klikněte na novou registraci aplikace v **Moje aplikace**a potom rozbalte **služby** > **nabízená oznámení**.

8. Na stránce **nabízená oznámení** v části **Windows nabízená oznámení služby (WNS) a Microsoft Azure mobilní aplikace**klikněte na **Web služby Live** a poznamenejte si hodnoty **ID balíčku zabezpečení** a *aktuální* hodnotu v **Aplikaci tajná**. 

    ![Nastavení aplikace v Centru pro vývojáře](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Tajná aplikace a balíčku ID zabezpečení jsou důležité zabezpečovací přihlašovací údaje. Tyto hodnoty sdílet s kýmkoli nebo distribuce s aplikací.
