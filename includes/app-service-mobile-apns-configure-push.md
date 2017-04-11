

1. Na počítači Mac spusťte **Řetězce klíčů přístup**. Na panelu vlevo navigationn otevřete **Osobních certifikátů** ve skupinovém rámečku **kategorie** . Vyhledejte certifikát SSL, který jste stáhli v předchozí části a zveřejnit jeho obsah. Vyberte pouze certifikát (nevybírejte privátním klíčem) a [exportujte ho](https://support.apple.com/kb/PH20122?locale=en_US).

2. V [Azure portal](https://portal.azure.com/), klikněte na **Procházet vše** > **Aplikace služby** > mobilní aplikaci backendovou. V části **Nastavení** **Aplikace služby nabízená**, klepněte na klikněte na své jméno centrální oznámení. Přejděte na **Apple nabízená oznámení služby** > **Odeslat certifikát**. Nahrání souboru P12 výběr správné **režim** (podle toho, zda klientem SSL certifikát z dříve je výrobní nebo izolovaného prostoru). Uložte provedené změny.

Služba je nyní nakonfigurován pro práci s nabízená oznámení na iOS!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
