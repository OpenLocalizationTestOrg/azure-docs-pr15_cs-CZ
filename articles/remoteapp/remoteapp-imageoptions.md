<properties
    pageTitle="Vytvořit obrázek Azure RemoteApp | Microsoft Azure"
    description="Zjistit, jaké možnosti k dispozici pro tvorbu obrázky služby Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="create-an-azure-remoteapp-image"></a>Vytvořit obrázek Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp používá obrázky k ukládání aplikace, které sdílíte s jinými uživateli. (Jsme prohlédněte obrázek a použít k vytvoření VMs – to je přístup uživatelů, když se odhlásíte do Azure RemoteApp.) Vytvoříte kolekci Azure RemoteApp s volbou aplikací, ať už jde cloudu nebo hybridní začnete vytvořením obrázek s těmito aplikacemi nainstalovaný. Vytvořte kolekci, která používá tento obrázek, přiřazení uživatelů do kolekce a publikovat aplikace těmto uživatelům.

Máte několik možností pro vytváření a používání obrázků. Základní [požadavek](remoteapp-imagereqs.md) na obrázek je, že ho Windows serveru 2012 R2 a máte nainstalovanou rolí vzdálené plochy relace hostitele (RDSH). Jak získat, která je kde získat zajímavých věcí.

Když přijde na obrázky máte následující možnosti:

- Můžete importovat a používat [Obrázek založené na Azure virtuálního počítače](remoteapp-image-on-azurevm.md). Toto je vhodné pro řádek obchodních aplikací, které vyžadují vlastní nastavení. Můžete přizpůsobit obrázek pro práci s aplikací.
- Můžete [vytvořit a uložit vlastní obrázek](remoteapp-create-custom-image.md). Toto je vhodné, pokud už máte obrázek, který použijete pro místního nasazení služby Vzdálená plocha.
- Můžete použít jeden z těchto [obrázky šablon](remoteapp-images.md) součástí vašeho předplatného RemoteApp. Tyto obrázky se vytvářejí a spravován RemoteApp týmu a obsahují některé standardní aplikace (třeba sadu Office), které můžete zpřístupnit uživatelům. Všimněte si, že jenom obrázky Office 365 Pro Plus lze použít v nastavení výroby.

Bez ohledu na to, kde se zobrazí obrázek nebo jak ho vytvořit je vhodné, aby zkontrolovala, jestli že víte, [požadavky aplikace](remoteapp-appreqs.md) zajistit, že aplikace funguje taky v RemoteApp. Potom dalším krokem je vytvoření kolekce [cloudu](remoteapp-create-cloud-deployment.md) nebo [hybridní](remoteapp-create-hybrid-deployment.md) .
