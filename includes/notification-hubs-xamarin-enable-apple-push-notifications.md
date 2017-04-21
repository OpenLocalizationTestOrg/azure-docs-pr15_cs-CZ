

Zaregistrovat aplikaci nabízených oznámení přes Apple nabízená oznámení služby (APNS), musíte vytvořit nový certifikát nabízených, ID aplikace a zřizovací profil projektu na portál společnosti Apple vývojář. ID aplikace bude obsahovat nastavení konfigurace umožňující aplikace pro odesílání a příjem nabízených oznámení. Toto nastavení bude obsahovat certifikát nabízená oznámení potřeba k ověření s Apple nabízená oznámení služby (APNS) při odesílání a příjem nabízených oznámení. Další informace o těchto koncepty v dokumentaci oficiální [Apple nabízená oznámení služby](http://go.microsoft.com/fwlink/p/?LinkId=272584) .


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Vytvoření souboru žádost o podepisování certifikátu nabízených certifikátu

Tento postup vás provede jednotlivými vytváření žádost o podepisování certifikátu. Tím se použijí k vygenerování certifikátu nabízených pro použití se APNS.

1. Na počítači Mac spusťte nástroj řetězce klíčů přístup. Můžete otevřít ze složky **Nástroje** nebo **jinou** složku na panelu Snadné spuštění.

2. Klikněte **Řetězce klíčů přístup**, rozbalte položku **Pomocník pro funkci certifikát**a pak klikněte na **požádat o certifikát od certifikační autority...**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Vyberte **E-mailovou adresu uživatele** a **Běžných název** , ujistěte se, že je vybraná **uložili na disk** a pak klikněte na **pokračovat**. Nechte pole **CA e-mailovou adresu** prázdné není potřeba.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Na stránce **Uložit jako**zadejte název souboru certifikát Podepisování požádat o oddělení služeb zákazníkům, vyberte umístění, ve **kterém**a pak klikněte na tlačítko **Uložit**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Tím uložíte soubor CSR do vybraného umístění. Výchozí umístění je v plochy. Nezapomeňte místo pro tento soubor.


####<a name="register-your-app-for-push-notifications"></a>Registrace aplikace nabízených oznámení

Vytvoření nové explicitní aplikace ID pro aplikaci s Apple a taky nakonfigurovat nabízených oznámení.  

1. Přejděte na [iOS Provisioning portál](http://go.microsoft.com/fwlink/p/?LinkId=272456) v Centru pro vývojáře Apple, přihlaste se pomocí Apple ID, klikněte na **identifikátory**klikněte **ID aplikace**a nakonec klikněte na **+** Přihlaste se k registraci nové aplikace.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Aktualizujte tato tři pole pro novou aplikaci a pak klikněte na **pokračovat**:

    * **Název**: do pole **název** v části **Popis ID aplikace** zadejte popisný název aplikace.

    * **Identifikátor URI příruček**: v části **Explicitní ID aplikace** zadejte **Identifikátor příruček** ve formuláři `<Organization Identifier>.<Product Name>` uvedené v [Aplikaci rozdělení Průvodce](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Tím se musí shodovat také k čemu slouží v aplikaci project XCode, Xamarin nebo Cordova aplikace.

    * **Nabízená oznámení**: zaškrtnout **Nabízená oznámení** v části **Aplikace služby** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Na stránce Potvrďte obrazovce ID aplikace zkontrolujte nastavení tohoto režimu a po ověření je klikněte na tlačítko **Odeslat**

4.  Po odeslání nové ID aplikace, zobrazí se obrazovce **Registrace dokončeno** . Klikněte na tlačítko **Hotovo**.

5. V Centru pro vývojáře, ve skupinovém rámečku ID aplikace vyhledejte ID aplikace, který jste právě vytvořili a klikněte na jeho řádek. Po kliknutí na řádku ID aplikace se zobrazí podrobnosti aplikace. Klikněte na tlačítko **Upravit** dole.

6. Přejděte do dolní části obrazovky a klikněte na tlačítko **Vytvořit certifikát** části **Vývoj nabízená certifikát SSL**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Zobrazí se pomocníka "Přidat iOS certifikát".

    > [AZURE.NOTE] Tento kurz používá vývoj certifikát. Stejný postup se používá při registraci výrobní certifikát. Ujistěte se, se doporučuje používat stejný certifikát typ odesílání oznámení.

7. Klikněte na **Zvolit soubor**, přejděte do umístění pro uložení zástupce pro push certificate. Klikněte na **Generovat**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Certifikát je vytvořený portálem, klikněte na tlačítko **Stáhnout** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Ke stažení podpisového certifikátu a uloží jej s vaším počítačem ve složce stažení.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Ve výchozím nastavení stažený soubor certifikát vývoj názvem **aps_development.cer**.

9. Poklikejte na certifikát stažený nabízených **aps_development.cer**. Nainstalujete nový certifikát v řetězci klíčů, jak je ukázáno v následujícím příkladu:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] Název vašeho certifikátu se může lišit, ale bude předponou **iOS vývoj Apple Push služby:**.

10. V aplikaci Access řetězce klíčů klikněte pravým tlačítkem Nový nabízených certifikát, který jste právě vytvořili v kategorii **certifikáty** . Klikněte na **Exportovat**, zadejte název souboru, vyberte požadovaný formát **p12** a klikněte na **Uložit**.

    Mějte na paměti název a umístění souboru exportovaného P12 nabízených certifikátu. Se použijí k ověření s APNS tím, že jej odešlete na portálu klasické Azure.



####<a name="create-a-provisioning-profile-for-the-app"></a>Vytvoření zřizovací profilu aplikace

1. Zpátky v <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning portál</a>vyberte **Zřizování profily**, vyberte **všechny**a klikněte **+** s cílem vytvořit nový profil. Spustí se průvodce **Přidat iOS Provisiong profilu**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Vyberte **iOS vývoj aplikací** ve **vývoji** jako typ provisiong profilu a klikněte na **pokračovat**.


3. Potom vyberte aplikaci ID jste právě vytvořili z rozevíracího seznamu **ID aplikace** a klepněte na tlačítko **pokračovat**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. V okně **Vyberte certifikáty** vyberte certifikát vývoj používaných pro podpis kódu a klikněte na **pokračovat**. Toto je certifikátu s automatickým podpisem, ne nabízených certifikát, který jste právě vytvořili.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Potom vyberte **zařízení** , která chcete použít pro testování a klikněte na **pokračovat**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Nakonec vyberte název profilu na **Profil**, klikněte na **Generovat**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
