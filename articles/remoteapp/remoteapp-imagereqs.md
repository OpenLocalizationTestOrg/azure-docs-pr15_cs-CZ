
<properties
    pageTitle="Požadavky na Azure obrázek RemoteApp | Microsoft Azure"
    description="Další informace o požadavcích pro vytváření obrázky pro použití se Azure RemoteApp"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Požadavky pro obrázky s Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp používá Windows serveru 2012 R2 obrázky hostovat všechny programy, které chcete sdílet s jinými uživateli. Pokud chcete vytvořit vlastní obrázek, můžete začít s existující obrázek nebo [vytvořte nový účet](remoteapp-create-custom-image.md).

> [AZURE.TIP] Věděli jste, že předplatné Azure RemoteApp vám umožňuje přístup k Windows serveru 2012 R2 obrázek v galerii Azure OM, který slouží k vytvoření vlastní šablony? [Zkontrolujte, jestli je mimo](remoteapp-image-on-azurevm.md).  


Požadavky na obrázek, který lze odeslat pomocí Azure RemoteApp jsou:


- Vlastní aplikace neukládejte data místně na obrázek. Tyto obrázky jsou příslušnosti a by měl obsahovat pouze aplikací.
- Obrázek neobsahuje data, která může dojít ke ztrátě.
- Velikost obrázku by měl být násobkem MB. Pokud se pokusíte nahrát obrázek, který není přesný násobek, se nepovede nahrávání.
- Velikost obrázku musí být 127 GB nebo menší.
- Musí být v souboru virtuálního pevného disku (soubory VHDX nepodporují aktuálně).
- Virtuální pevný disk nesmí být virtuálního počítače generování 2.
- Virtuální pevný disk může být pevnou velikostí nebo dynamicky se zvětšující. Dynamicky rozbaleného virtuálního pevného disku se nedoporučuje, protože ho zkracuje k nahrání na Azure než pevnou velikostí souboru virtuální pevný disk.
- Disk musí být spuštěn pomocí předlohy spouštěcí záznam (hlavní spouštěcí záznam) oddílů. Nepodporuje se stylem oddílů GUID (GPT).
- Virtuální pevný disk musí obsahovat jedna instalace systému Windows Server 2012 R2. Může obsahovat více svazky, ale pouze jeden, která obsahuje instalace systému Windows.
- Musí být nainstalovaný roli vzdálené plochy relace hostitele (RDSH) a funkci Možnosti práce s počítačem.
- Zprostředkovatel připojení k vzdálené plochy role musí *není* nainstalovat.
- Systém souborů EFS, musíte zakázat.
- Obrázek musí být SYSPREPed pomocí parametrů **/oobe / generalize/shutdown** (nesmí použít parametr **/mode:vm** ).
- Odeslání vašeho virtuální pevný disk z řetězce snímek není podporovaná.

Další informace o vytváření obrázků Azure RemoteApp najdete v článku [Vytvoření Azure RemoteApp obrázek](remoteapp-imageoptions.md) .
