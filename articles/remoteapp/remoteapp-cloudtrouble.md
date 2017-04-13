
<properties
    pageTitle="Poradce při potížích s RemoteApp cloudu kolekce – vytvoření | Microsoft Azure"
    description="Zjistěte, jak řešit problémy s chyby vytváření kolekce cloudové RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Poradce při potížích s vytvářením kolekce cloudu RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Pokud máte potíže vytváření kolekce cloudu, přečtěte si následující informace.

## <a name="your-image-is-invalid"></a>Obrázek nejsou platná: ##
Pokud při čekáte Azure zřízení kolekci uvidíte zprávu podobnou "GoldImageInvalid", znamená to, že obrázek šablony nesplňuje [definované požadavky na obrázek](remoteapp-imagereqs.md). Přejděte Ano, přečtěte si tyto [požadavky](remoteapp-imagereqs.md), opravíte obrázek a vytvořte kolekci znovu.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Běžné chyby zobrazené na portálu Správa Azure

    DNS server could not be reached
    ProvisioningTimeout

Často cloudu kolekce selhání při vytváření kvůli můžete používáte vlastní obrázky.  Pokud se zobrazí jedna z výše uvedených chyb a používáte vlastní obrázek vytvoříte kolekci, zkontrolujte následující věci:

- Ujistěte se, že vlastní obrázek, který jste nahráli splňuje požadavky na obrázek.
- Nejčastěji používané běžné je to, že obrázek není správně syspreped.  
- Ověření obrázku můžete spustit v rámci Hyper-V nebo zkuste vytvořit OM IAAS přímo do svého předplatného Azure pomocí obrázku. Pokud OM nepovede spustit a nespustí, obvykle označuje, že vlastní obrázek nebyl připraven správně.  Ověření vlastní obrázek tvořily původní po stránce jak vytvořit vlastní šablony pro RemoteApp

Pokud používáte některou z Microsoft obrázky součástí vašeho předplatného, zkuste vytvořit kolekci znovu. Pokud potíže potrvají kontaktujte podporu společnosti Microsoft.

    PlatformImageTrialModeOnly

Pokud se zobrazí tato chyba většinou to znamená, které nebyly upgradované na placené účet, ale chcete použijte dodaný společností Microsoft obrázek, který je platný pouze během zkušební režim služby. V tomto případě se pokusíte vytvořit kolekci cloudové znovu, ale je nutné zadat správné obrázek.
