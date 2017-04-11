## <a name="download-and-understand-the-arm-template"></a>Stáhněte si a chápete šabloně ARM

Můžete stáhnout existující šablony ARM pro vytváření VNet a dvě podsítí z github, proveďte požadované změny může má a znovu použít. Postupujte podle následujících kroků.

1. Přejděte na [stránku ukázkové šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klikněte na **azuredeploy.json**a potom klikněte na **původní**.
3. Uložte soubor místní složky ve vašem počítači.
4. Pokud máte zkušenosti s ARM šablony, přejděte ke kroku 7.
5. Otevřete soubor, který jste právě uložili a podívejte se na obsah v **parametrů** v řádku 5. Parametry šablony ARM zadání zástupný symbol pro hodnoty, které lze vyplnit při nasazení.

    | Parametr | Popis |
    |---|---|
    | **umístění** | Azure oblasti, kde se bude vytvořena VNet |
    | **vnetName** | Název nové VNet |
    | **addressPrefix** | Adresní prostor pro VNet ve formátu CIDR |
    | **subnet1Name** | Název první VNet |
    | **subnet1Prefix** | Blok CIDR pro první podsítě |
    | **subnet2Name** | Název druhého VNet |
    | **subnet2Prefix** | Blok CIDR pro druhý podsítě |

    >[AZURE.IMPORTANT] Šablony ARM vedou v github můžete v průběhu času mění. Zkontrolujte, že zaškrtnete políčko šabloně než ho začnete používat.
    
6. Zkontrolovat obsah v seznamu **zdroje** a Všimněte si takto:

    - **Typ**. Typ zdroje vytvořené pomocí šablony. V tomto případě **Microsoft.Network/virtualNetworks**, které představují VNet.
    - **název**. Název zdroje. Všimněte si použití **[parameters('vnetName')]**, což znamená, že se název poskytovanou předávat na vstupu uživatele nebo souboru parametr během nasazení.
    - **Vlastnosti**. Seznam vlastností daného zdroje. Tato šablona používá vlastnosti adresu místa a podsítě při vytváření VNet.

7. Přejděte zpátky na [stránku ukázkové šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klikněte na **azuredeploy paremeters.json**a potom klikněte na **původní**.
9. Uložte soubor místní složky ve vašem počítači.
10. Otevřete soubor, který jste právě uložili a upravte hodnoty parametrů. Pomocí níže uvedených hodnot nasazení VNet popsané v našem případě.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Uložte soubor.
  