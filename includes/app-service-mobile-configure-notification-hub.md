Aplikace služby mobilních aplikací používá k odeslání [Oznámení rozbočovače] posune, takže budete konfigurovat rozbočovači oznámení pro mobilní aplikaci.

1. Na [Portálu Azure], přejděte na **Procházet** > **Aplikace služby**, klikněte na serverové části aplikace. V části **Nastavení**klikněte na **Aplikaci služby nabízená**.

2. Klikněte na **Konfigurovat** konfigurace rozbočovači oznámení. Můžete vytvořit rozbočovači nebo připojení k existující úrovně.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Teď jste připojeni rozbočovači oznámení backendovou mobilní aplikaci. Později nakonfigurujete tento rozbočovač oznámení pro připojení k systému oznámení platformu (PNS) jestli chcete použít pro zařízení.

[Azure portálu]: https://portal.azure.com/
[Oznámení o rozbočovače]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/