<properties
   pageTitle="Použití obrázků klienta Windows pro vývojáře nebo zkoušení scénáře | Microsoft Azure"
   description="Jak používat výhody předplatného Visual Studio nasazení systému Windows 7 a 8/10 v Azure pro vývojáře nebo zkoušení scénáře"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Pomocí klienta Windows v Azure pro vývojáře nebo zkoušení scénáře

Používáte Windows 7, Windows 8 nebo Windows 10 v Azure pro vývojáře nebo zkoušení scénáře, pokud že máte předplatné odpovídající Visual Studio (dřív to bylo MSDN). Tento článek popisuje požadavky na oprávněnost k pracovního klienta Windows Azure a použitím Azure Galerie obrázků.


## <a name="subscription-eligibility"></a>Oprávněnost předplatného
Aktivní Visual Studio předplatitele (lidí, kteří jste získali předplatitelskou licenci Visual Studio) můžete použít Windows klienta pro účely vývoje a testování. Windows klienta mohou sloužit na hardware a Azure virtuálních počítačích spuštěné v jakémkoli typu Azure předplatného. Klienta Windows nemusí používaný nebo používaný v Azure pro použití v normálním provozním nebo používané uživatelům, kteří nejsou aktivní Visual Studio účastníky.

Pohodlný jsme jste zpřístupnili pro určité Windows 10 obrázky z Galerie Azure v rámci [měl zařízením nebo zkoušení nabízí](#eligible-offers). Visual Studio předplatitele v rámci libovolný typ nabídku můžete taky [dostatečně připravit a vytvoření](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64bitová verze Windows 7, Windows 8 nebo Windows 10 obrázek a pak [nahrajte do Azure](virtual-machines-windows-upload-image.md). Použití zůstane omezena pro vývojáře nebo zkoušení aktivní Visual Studio účastníky.


## <a name="eligible-offers"></a>Vhodné nabídky
Následující tabulka uvádí nabídky ID, které mají nárok na nasazení systému Windows 10 až galerii Azure. Na některých obrázcích Windows 10 viditelné pouze následující nabídky. Visual Studio účastníky, kteří potřeba spustit Windows klienta v různých nabídka typ je nutné, abyste [dostatečně připravit a vytvořte](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64bitová verze Windows 7, Windows 8 nebo Windows 10 obrázek a [pak nahrajte Azure](virtual-machines-windows-upload-image.md).

| Nabídka název | Nabídka číslo | K dispozici klienta obrázky |
|:-----------|:------------:|:-----------------------:|
| [Přislíbený zařízením nebo zkoušení](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Předplatitelé Visual Studio Enterprise (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Visual Studio Professional účastníky](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Visual Studio Test Professional účastníky](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium s MSDN (výhoda)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Visual Studio Enterprise účastníky](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Předplatitelé Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Pole organizace zařízením nebo zkoušení](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Kontrola předplatného Azure
Pokud neznáte nabídky ID, můžete ji získat prostřednictvím portálu Azure nebo portálu účtu.

ID předplatného nabídky je uvedeno na zásuvné "Předplatná" v rámci Azure portálu:

![Podrobnosti ID nabídky z portálu Microsoft Azure](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Můžete taky zobrazit ID nabídky v systému ["Předplatná" kartu](http://account.windowsazure.com/Subscriptions) portálu účet Azure:

![Podrobnosti ID nabídky z portálu Microsoft Azure účtu](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Další kroky
Nyní můžete nasadit vaše VMs pomocí [prostředí PowerShell](virtual-machines-windows-ps-create.md), [Správce prostředků šablon](virtual-machines-windows-ps-template.md)nebo [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
