<properties
   pageTitle="Technické požadavky pro tvorbu obrázku virtuální počítač služby Azure Marketplace | Microsoft Azure"
   description="Porozumět tomu, že splníte požadavky pro vytvoření a nasazení obrázku virtuálního počítače k webu Azure Marketplace pro jiné uživatele k nákupu."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technické požadavky pro tvorbu obrázku virtuální počítač služby Azure Marketplace
Přečtěte si procesu důkladně před zahájením a vysvětlení kdy a proč je provedena každého kroku. Co nejvíce můžete měli připravit údajů o vaší společnosti a dalších dat, stáhněte si potřebné nástroje nebo vytvořit technické součásti před zahájením proces vytváření nabídky. Tyto položky by měl být vymazat z revizí v tomto článku.  

## <a name="download-needed-tools--applications"></a>Stáhněte si potřebné nástroje a aplikací
Byste si měli jste připravení před zahájením procesu následující položky:

- V závislosti na operačním systému směrujete, nainstalujte [rutiny prostředí PowerShell Azure](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) nebo [Linux rozhraní příkazového řádku nástroj](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) ze stránky [Souborů ke stažení Azure](https://azure.microsoft.com/downloads/) .
- Instalace aplikace Explorer Azure úložiště na webu CodePlex.
- Stáhněte a nainstalujte nástroj Certifikační Test pro Azure certifikované:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Potřebujete počítače se systémem Windows spustit nástroj Certifikační. Pokud nemáte počítač dostupné serveru s Windows, můžete spustit nástroj pomocí OM serveru s Windows Azure.

## <a name="platforms-supported"></a>Podporované platformy
Můžete vyvíjet založené na Azure VMs ve Windows a v Linuxu. Některé prvky procesu publikování – například vytvoření kompatibilní s prohlížečem Azure virtuálního pevného disku (virtuální pevný disk) – použití různých nástrojů a v závislosti na operačním systému používáte kroky:  

- Pokud používáte Linux, naleznete v části "Vytvořit kompatibilní s prohlížečem Azure virtuálního pevného disku (založené na Linux)" [virtuálního počítače obraz publikování průvodce](marketplace-publishing-vm-image-creation.md).
- Pokud používáte Windows, naleznete v části "Vytvořit kompatibilní s prohlížečem Azure virtuálního pevného disku (serveru s Windows)" [virtuálního počítače obraz publikování průvodce](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] Je nutné použít počítač serveru s Windows:
- Spusťte nástroj Certifikační ověření.
- Vytvoření přístup virtuální pevný disk sdílené podpis adresy URL pro odeslání certifikační virtuální pevný disk.

## <a name="develop-your-vhd"></a>Můžete vyvíjet vaší virtuální pevný disk
Můžete vyvíjet Azure VHD v cloudu a místní:

- Cloudové vývoj znamená, že všechny kroky vývoj provádí vzdáleně na virtuálního pevného disku držen v Azure.
- Místní vývoj vyžaduje stahování virtuálního pevného disku a jeho používání místního infrastruktury vývoji. I když je to možné, ale nedoporučujeme ho. Nezapomeňte, že vývoji pro Windows nebo SQL místní vyžaduje abyste měli odpovídající místního licenční klíče. Můžete zahrnout nebo nainstalovat SQL Server po vytvoření virtuálního počítače. Vaši nabídku musíte taky vytvořit na základě schválené obrázek z portálu Microsoft Azure SQL. Pokud se rozhodnete se dají místní, je nutné provést několik dalších kroků jinak než pokud byly vývoj v cloudu. Důležité informace najdete v tématu [Vytvoření místní OM obrázek](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Další kroky
Teď prohlédnete požadavky a dokončené potřebné úkoly, můžete se Přechod vpřed s vytvářením vaši nabídku obrázek virtuálního počítače jako podrobný [Průvodce publikování obrázek virtuálního počítače](marketplace-publishing-vm-image-creation.md).

## <a name="see-also"></a>Viz taky
- [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
- [Vytvoření virtuálního počítače s Windows Azure náhled portálu](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
