## <a name="peering-across-subscriptions"></a>Prozkoumávání u předplatných

V tomto scénáři vytvoříte prozkoumávání mezi dvěma VNets patřící do různých předplatných.

![scénář křížového sub](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Prozkoumávání VNet závisí na řízení přístupu na základě rolí (RBAC) pro povolení. Pro více předplatných scénář musíte nejdřív udělit dostatečná oprávnění uživatelům, kteří vytvoří peering odkaz:

> [AZURE.NOTE] Pokud uživatel stejné oprávnění přes obou předplatná, můžete přejít krok 1-4 při instalaci dole.
