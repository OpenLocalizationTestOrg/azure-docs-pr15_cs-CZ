####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Konfigurace iOS projektu v Xamarin Studio

1. V Xamarin.Studio otevřete **Info.plist**a aktualizujte **Příruček identifikátor** příruček ID, který jste vytvořili pomocí svého ID nové aplikace.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Posuňte se dolů na **Pozadí režimy** a zaškrtněte políčko **Povolit režimy pozadí** a **vzdálené oznámení** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Poklikejte na projektu v panelu řešení otevřete dialogové okno **Možnosti aplikace Project**.

4.  Zvolte **iOS balíku podepisování** v části **vytvořit**a vyberte odpovídající **identit** a **Provisioning profilu** právě jste vytvořili pro tento projekt. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Zajistíte tak, že projektu podepsání kódu pomocí nového profilu. Oficiální Xamarin zařízení si přečtěte následující dokumentaci pro zřízení najdete v tématu [Příprava Xamarin zařízení].

####<a name="configuring-the-ios-project-in-visual-studio"></a>Konfigurace iOS projektu ve Visual Studiu

1. Ve Visual Studiu klikněte pravým tlačítkem projekt a potom klikněte na **Vlastnosti**.

2. V vlastností stránky klikněte na kartu **iOS aplikace** a aktualizovat **identifikátor** s ID, který jste vytvořili.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Na kartě **iOS příruček podepisování** vyberte odpovídající **identit** a **Provisioning profilu** právě jste vytvořili pro tento projekt. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Zajistíte tak, že projektu podepsání kódu pomocí nového profilu. Oficiální Xamarin zařízení si přečtěte následující dokumentaci pro zřízení najdete v tématu [Příprava Xamarin zařízení].

4. Poklikejte na Info.plist otevřít ho a pak ji povolit **RemoteNotifications** v režimech pozadí. 



[Zařízení Xamarin zřízení]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/