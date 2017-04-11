## <a name="setting-up-powershell-for-resource-manager-templates"></a>Nastavení prostředí PowerShell pro správce prostředků šablony

Než budete moct použít prostředí PowerShell Azure pomocí Správce prostředků, bude musíte mít na pravém prostředí Windows PowerShell a Azure PowerShell verze.

### <a name="verify-powershell-versions"></a>Ověření verze prostředí PowerShell

Zkontrolujte, jestli že máte prostředí Windows PowerShell verze 3.0 nebo 4.0. Verze Windows Powershellu najdete zadejte tento příkaz na příkazovém řádku prostředí Windows PowerShell.

    $PSVersionTable

Zobrazí se tento typ informací:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Zkontrolujte, jestli je hodnota **PSVersion** 3.0 nebo 4.0. Pokud není, přečtěte si téma [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Nastavit účet Azure a předplatného

Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

Otevřete příkazového prostředí PowerShell Azure a přihlaste se k Azure pomocí tohoto příkazu.

    Login-AzureRmAccount

Pokud máte víc předplatných Azure, můžete vytvořit seznam předplatné Azure pomocí tohoto příkazu.

    Get-AzureRmSubscription

Zobrazí se tento typ informací:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Nastavte aktuálního předplatného Azure spuštěním tyto příkazy na příkazovém řádku prostředí PowerShell Azure. Nahrazení všechno, co do uvozovek, včetně < a > znaky se správným názvem.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Další informace o účtu a Azure předplatná najdete v tématu [jak: připojení k vašemu předplatnému](powershell-install-configure.md#Connect).
