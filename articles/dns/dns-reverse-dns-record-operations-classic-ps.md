<properties
   pageTitle="Správa obráceném záznamy DNS pro služby Azure (klasický) pomocí prostředí PowerShell | Microsoft Azure"
   description="Jak spravovat obráceném záznamy DNS nebo PTR záznamy služby Azure pomocí Powershellu v modelu klasické nasazení. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Jak spravovat obráceném záznamy DNS pro služby Azure (klasický) pomocí prostředí PowerShell Azure

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Ověření obráceném záznamů DNS
Zajistit, že třetí strany nelze vytvořit obráceném mapování na vaše domény DNS záznamy DNS, Azure jenom umožňuje vytváření obráceném záznam DNS, které jedním z následujících platí:

- Naopak DNS plně kvalifikovaný název domény je název cloudové služby, u kterého byla specifikována nebo názvů Cloudová služba ve stejném předplatném například je obráceném DNS "contosoapp1.cloudapp.net.".
- Obráceném DNS plně kvalifikovaný název domény vpřed převedena na jméno nebo IP cloudové služby, které ho není zadáno, nebo na kterýkoli název cloudové služby nebo IP ve stejném předplatném například obráceném DNS je "app1.contoso.com." tedy CName alias pro contosoapp1.cloudapp.net.

Ověření se provede pouze v případě obráceném vlastnost DNS pro do cloudové služby je nastavit nebo změnit. Pravidelné opětovné ověření neprovádí.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Přidání obráceném DNS ke stávající Cloudovým službám
Zpětné záznam DNS můžete přidat existující cloudové služby pomocí rutiny "Set-AzureService":

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Vytvoření do cloudové služby s obráceném DNS
Přidat nové Cloudová služba s zadaná obráceném DNS vlastnost pomocí rutiny "Set-AzureService":

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Zobrazení obráceném DNS pro existující Cloudovým službám
Nakonfigurovaná hodnota můžete zobrazit existující služby cloudu pomocí rutiny "Get-AzureService":

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Odebrání obráceném DNS z existující Cloudovým službám
Odebrání obráceném DNS vlastnosti z existující cloudové služby pomocí rutiny "Set-AzureService". Důvodem je nastavením hodnotu DNS obráceném prázdná:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
