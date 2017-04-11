Azure zákazníci odemknout 25 000 bezplatné e-mailů každý měsíc. Tyto 25 000 bezplatné měsíční e-mailů bude umožňují přístup k rozšířené vytváření sestav a technologie pro analýzu a [všechna rozhraní API][] (Web, SMTP, události, analýzy a další). Informace o dalších službách poskytovanou SendGrid najdete v tématu stránce [SendGrid funkce][] .

### <a name="to-sign-up-for-a-sendgrid-account"></a>Abyste si zaregistrovali SendGrid účtu

1. Přihlaste se k [portálu Správa Azure][].

2. V dolním podokně portálu pro správu klikněte na **Nový**.

    ![nové příkaz panelu][command-bar-new]

3. Na položku **Marketplacea**.

    ![sendgrid úložiště][sendgrid-store]

4. V dialogovém okně **Zvolit aplikace a přihlašovacích údajů** vyberte **SendGrid** a klikněte na šipku doprava.

5. V dialogovém okně **Upravit aplikace a přihlašovacích údajů** vyberte **SendGrid** plán, který chcete přihlásit k.

6. Zadejte název pro identifikaci služby **SendGrid** v nastavení Azure nebo použijte výchozí hodnotu **SendGridEmailDelivery.Simplified.SMTPWebAPI**. Názvy musí být v rozsahu 1 až 100 znaků a obsahovat pouze alfanumerické znaky, pomlčky, tečky a podtržítka. Název musí být jedinečný ve vašem seznamu odebíraných položek úložiště Azure.

    ![úložiště obrazovky-2][store-screen-2]

7. Zvolí hodnotu pro oblast; například západní US.

8. Klikněte na šipku doprava.

9. Na kartě **Revize nákupní** zkontrolujte plán a ceny informace a zkontrolujte právní podmínek. Pokud jste souhlasili s podmínkami, klikněte na značku zaškrtnutí. Po kliknutí na značku zaškrtnutí účtu SendGrid začne [SendGrid zřizovací obrázku].

    ![úložiště obrazovky-3][store-screen-3]

10. Po potvrzení nákup budete přesměrováni na řídicím panelu doplňky a zobrazí se zpráva **Zakoupení SendGrid**.

    ![sendgrid zakoupení zprávy][sendgrid-purchasing-message]

    Ihned zřízení účtu SendGrid a zobrazí se zpráva **úspěšně koupili SendGrid doplňku**. Váš účet a přihlašovací údaje jsou vytvořena. Jste připravení odeslat e-mailů v tomto okamžiku. 

    Změna vašeho předplatného plánu nebo najdete v článku SendGrid, obraťte se na nastavení, klikněte na název služby SendGrid otevřete řídicím panelu SendGrid Marketplace. 

    ![sendgrid – přidání na-řídicího panelu][sendgrid-add-on-dashboard]

    Pokud chcete poslat e-mailů pomocí SendGrid, je nutné zadat svoje přihlašovací údaje účtu (uživatelské jméno a heslo).

### <a name="to-find-your-sendgrid-credentials"></a>Najít svoje přihlašovací údaje SendGrid ###

1. Klikněte na **informace o připojení**.

    ![sendgrid připojení informace tlačítka][sendgrid-connection-info-button]

2. V dialogovém okně *informace o připojení* zkopírujte **heslo** a uživatelské jméno, můžete dál v tomto kurzu.

    ![informace o sendgrid připojení][sendgrid-connection-info]

    Nastavení e-mailu deliverability, klikněte na tlačítko **Spravovat** . To vás přesměruje na váš SendGrid ovládací panely. 

    ![sendgrid ovládací panely][sendgrid-control-panel]

    Další informace o začínáte pracovat s SendGrid přečtěte si článek [Začínáme SendGrid][].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[Funkce SendGrid]: http://sendgrid.com/features
[Portál Správa Azure]: https://manage.windowsazure.com
[SendGrid Začínáme]: http://sendgrid.com/docs
[Proces vytváření SendGrid]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[všechna rozhraní API]: https://sendgrid.com/docs/API_Reference/index.html

