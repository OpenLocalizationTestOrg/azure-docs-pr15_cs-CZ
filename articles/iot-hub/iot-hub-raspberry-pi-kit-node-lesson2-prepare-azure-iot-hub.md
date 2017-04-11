<properties
 pageTitle="Vytvořit centrum IoT a zaregistrovat malin pí 3 | Microsoft Azure"
 description="Vytvoření skupiny prostředků, vytvořte rozbočovači Azure IoT a zaregistrujte svůj pí v centru Azure IoT pomocí rozhraní příkazového řádku Azure."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 vytvořit rozbočovače IoT a zaregistrovat malin pí 3

## <a name="221-what-you-will-do"></a>2.2.1 co bude dělat

- Vytvoření skupiny prostředků.
- Vytvoření rozbočovače Azure IoT ve skupině prostředků.
- Přidejte pí 3 malin centru Azure IoT pomocí rozhraní příkazového řádku Azure.

Při použití rozhraní příkazového řádku Azure přidáte svůj pí rozbočovače IoT službu vygeneruje klíč pro pí ověření se službou. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="222-what-you-will-learn"></a>2.2.2 co jste se naučili

- Postup vytvoření rozbočovači IoT Azure pomocí rozhraní příkazového řádku Azure.
- Jak vytvořit zařízení identity pro Pi v rozbočovače IoT Azure.

## <a name="223-what-you-need"></a>2.2.3 co byste měli

- Účet Azure
- Macu nebo počítači s Windows s nainstalovanou rozhraní příkazového řádku Azure

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 vytvořit rozbočovače Azure IoT

Azure centrální IoT umožňuje připojit, sledování a správa miliony IoT prostředky. Vytvoření rozbočovače Azure IoT, postupujte takto:

1. Přihlaste se k účtu Azure tohoto příkazu:

    ```bash
    az login
    ```

    Po úspěšném přihlášení jsou uvedené všechny dostupné Azure předplatné.

2. Nastavení výchozího Azure předplatné, které chcete použít spuštěním následujícího příkazu:

    ```bash
    az account set -n {subscription id or name}
    ```

    ID předplatného nebo název najdete v výstup `az login`.

3. Zaregistrujte zprostředkovatele spuštěním následujícího příkazu:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Zprostředkovatel zaregistrujte před nasazením Azure prostředek, který obsahuje zprostředkovatele.

    > [AZURE.NOTE] Většina poskytovatelů jsou registrované automaticky pomocí portálu Azure nebo Azure rozhraní příkazového řádku, který používáte, ale ne vždy. Další informace o poskytovateli najdete v článku [Poradce při běžných chyb ve Azure nasazení pomocí Správce prostředků Azure](../resource-manager-common-deployment-errors.md)

4. Vytvoření skupiny zdroje s názvem iot vzorku v oblastí západní USA spuštěním následujícího příkazu:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Vytvoření rozbočovači IoT ve skupině iot ukázkové zdroje spuštěním následujícího příkazu:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    Výchozí verzi centrální IoT vytvořená je **F1**, která je **zadarmo**. Další informace najdete v tématu [Centrum IoT Azure ceny](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] Název rozbočovače IoT musí být jedinečné.
>
> Jedinou **F1** verzi Azure IoT centrální můžete vytvořit v rámci předplatného Azure.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 registrovat váš pí v centrální IoT

Každé zařízení, které odesílá/obdrží zprávy od rozbočovače Azure IoT registrovaný u jedinečné ID.

Registrace vaší Pi v Azure IoT centrální spuštěné následující příkaz:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 souhrn

Po vytvoření rozbočovači Azure IoT a zaregistrovanými vaší pí zařízení identit v centrální Azure IoT. Jste připraveni přejít na další lekcí se dozvíte, jak k odesílání zpráv od vašeho pí rozbočovače IoT.

## <a name="next-steps"></a>Další kroky

Teď jste připraveni začít [vytvářet aplikace Azure funkce a účet Azure úložiště pro zpracování a ukládání zpráv centrální IoT](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)lekci 3, který začíná.