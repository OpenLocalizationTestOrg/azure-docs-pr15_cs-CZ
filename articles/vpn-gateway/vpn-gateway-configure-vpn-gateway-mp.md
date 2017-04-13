<properties 
   pageTitle="Konfigurace brány VPN na portálu Azure klasické | Microsoft Azure"
   description="Tento článek vás konfigurace virtuální sítě VPN brány a změny brány směrování typ VPN provede."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Konfigurace VPN brány pro klasické nasazení modelu


Pokud chcete vytvořit spojení zabezpečené křížově místní mezi Azure a místních umístění, budete muset konfigurace brány připojení VPN. V klasickém nasazení modelu, může být brány dvou typů VPN směrování: statického nebo dynamického. Typ, které zvolíte, závisí na plánu návrh sítě a zařízení místní virtuální privátní sítě, který chcete použít. 

Některé možnosti připojení, například čárky webu připojení, například vyžadovat dynamické směrování brány. Pokud budete chtít konfigurace brány pro podporu čárky webu (P2S) připojení a připojení k webu (S2S), budete muset konfigurace dynamické směrování brány, i když je na webu můžete pomocí některého z brány směrování typ VPN nakonfigurovat. 

Kromě toho Ujistěte se, že zařízení, které chcete použít pro připojení podporuje směrování typ VPN, který chcete vytvořit. Přečtěte si [téma VPN zařízení](vpn-gateway-about-vpn-devices.md).


**O tomto článku** 

Tento článek je určen pro klasické nasazení modelu pomocí [klasické portál](https://manage.windowsazure.com) (ne Azure portálu). 

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Přehled konfigurace

Podle těchto kroků vás provede jednotlivými konfigurace bránu VPN Azure klasické portálu. Tento postup platí pro bran virtuálních sítí, které jsou vytvořené pomocí klasické nasazení modelu. V současné době není všechna nastavení konfigurace brány jsou dostupné v portálu Azure. Jakmile budou, vytvoříte novou sadu pokynů, které se vztahují k portálu Azure.


1. [Vytvoření brány VPN pro vaše VNet](#create-a-vpn-gateway)

1. [Shromažďování informací o konfiguraci zařízení VPN](#gather-information-for-your-vpn-device-configuration)

1. [Konfigurace zařízení VPN](#configure-your-vpn-device)

1. [Ověřte svoje místní síti oblastí a VPN brány IP adresa](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace brány, musíte nejdřív vytvořit virtuální sítě. Postup vytvoření virtuální síť pro více místní připojení najdete v článku [Konfigurace virtuální sítě s připojení VPN k webu](vpn-gateway-site-to-site-create.md)nebo [Konfigurovat virtuální sítě s připojení VPN čárky webu](vpn-gateway-point-to-site-create.md). Potom postupujte podle těchto kroků ke konfiguraci brány sítě VPN a shromáždění informací potřebných pro konfiguraci vašeho zařízení VPN. 

Pokud už máte brány VPN a chcete změnit typ směrování VPN, naleznete v článku [Změna typu směrování VPN brány](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Vytvoření brány VPN

1. Na [portálu Azure klasické](https://manage.windowsazure.com), na stránce **sítě** zkontrolujte, že sloupec Stav sítě virtuální **vytvořili**.

1. Ve sloupci **název** klikněte na název svojí virtuální sítě.

1. Na stránce **Dashboard** Všimněte si, že tento VNet nemá brány nakonfigurovány ještě. Tenhle stav se zobrazuje jako projděte kroky pro nastavení brány.

![Brána není vytvořený](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Pak v dolní části stránky klikněte na **Vytvořit brány**. Můžete vybrat buď *Statické směrování* nebo *Dynamické směrování*. Typ směrování VPN, který jste vybrali závisí na několika faktorech. Například podporuje zařízení VPN a jestli potřebujete podporu čárky webu připojení. Zkontrolujte [Informace o zařízení VPN virtuální připojení k síti](vpn-gateway-about-vpn-devices.md) a ověřte směrování typ VPN, které potřebujete. Po vytvoření brány není možné změnit mezi brány VPN směrování typy bez odstraníte a znovu vytvořit brány. Až systému vás vyzve k potvrzení, že má brána vytvořili, klikněte na **Ano**.

![Typ směrování VPN brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Při vytváření brána Všimněte si grafiku brány na stránce se změní na žlutá a *Vytvoření brány*se objeví. Může trvat až 45 minut u brány na vytvořit. Počkejte, dokud brána je dokončeno před přechodem vpřed s další nastavení.

![Vytvoření brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Když brány změní *připojení*, můžete získat informace, které budete potřebovat pro zařízení s VPN.

![Připojení brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Shromažďování informací o konfiguraci zařízení VPN

Po vytvoření brány shromažďování informací o konfiguraci VPN zařízení. Tyto informace se nachází na stránku **řídicího panelu** virtuální sítě:

1. **Adresa brány-** IP adresy se nachází na stránky **řídicího panelu** . Nebude moct vidět až po brána je hotový.

1. **Sdílené klíč-** Klikněte na **Spravovat klíč** v dolní části obrazovky. Klikněte na ikonu vedle klávesu zkopírování do schránky a potom vložte a jejich ukládání klávesu. Kliknutím na toto tlačítko jde použít pouze po jedné tunelem S2S VPN. Pokud máte víc tunelů S2S VPN, stiskněte klávesovou zkratku rutiny prostředí PowerShell nebo *Získat virtuální sítě sdílené klíče brány* rozhraní API.

![Správa klíč](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Konfigurace zařízení VPN

Po dokončení předchozích kroků, je váš správce sítě nebo třeba nakonfigurovat zařízení VPN k vytvoření připojení. Další informace o VPN zařízení najdete v článku [O zařízení VPN virtuální připojení k síti](vpn-gateway-about-vpn-devices.md) .

Po nakonfigurování sítě VPN zařízení můžete zobrazovat informace o aktualizované připojení na stránky řídicího panelu pro vaše VNet.

Můžete taky spustit jednu z následujících příkazů otestovat připojení:

|                      | Cisco ASA             | ISR Cisco/automatickým obnovením systému         | Juniper SSG/ISG | Juniper tomto dokumentu se týkají/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Kontrola přidružení**  | Zobrazit šifrování isakmp správce systému | Zobrazit šifrování isakmp správce systému | Získání souborů cookie ike  | Zobrazit zabezpečení přidružení zabezpečení ike   |
| **Kontrola přidružení zabezpečení v rychlém režimu** | Zobrazit šifrování ipsec správce systému  | Zobrazit šifrování ipsec správce systému  | získání správce systému          | Zobrazit spojení se zabezpečením ipsec pro zabezpečení |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Ověřte svoje místní síti oblastí a VPN brány IP adresa

### <a name="verify-your-vpn-gateway-ip-address"></a>Ověřte svoji IP adresu brány VPN

Brány připojení správně musí být IP adresa VPN zařízení správně nakonfigurované pro místní síti, které jste zadali při konfiguraci křížově místních poštovních. Obvykle to nakonfigurovaný během procesu konfigurace webů pro web. Ale pokud jste dříve použili místní síti s jiné zařízení nebo na IP adresu změnila pro místní síti, upravte nastavení nastavit správné brány IP adresu.

1. Ověřte svoji IP adresu brány kliknutím na **sítí** v levém podokně portálu a pak vyberte **Místní sítě** v horní části stránky. Zobrazí se adresa brány VPN pro každou místní síť, kterou jste vytvořili. Upravit na IP adresu, vyberte VNet a klikněte na **Upravit** v dolní části stránky.

1. Na stránce **Zadejte podrobnosti místní síti** upravit IP adresa a potom klikněte na Další šipky v dolní části stránky.

1. Na stránce **zadejte do adresního prostoru** klikněte na zaškrtnutí v pravé dolní svá nastavení uložíte.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Ověření rozsahy adres pro místní sítě

Pro správné komunikační tok přes bránu místního pracoviště budete muset ověřte, že je určen každý rozsah IP adres. Každou oblast, musí být vedená v konfiguraci Azure **Místní sítě** . V závislosti na konfiguraci sítě místního pracoviště může to být trochu velké úkolu. Přenos, který je svázán pro IP adresu, která je obsažená v uvedené oblasti odešle prostřednictvím virtuální sítě VPN brány. Rozsahy v seznamu nemusí být privátní oblastí, i když budete chtít ověřte, že konfiguraci místního přijímat příchozí data.

Přidání nebo úpravy oblastí pro místní síti, pomocí následujících kroků.

1. Upravit rozsahy IP adres pro místní síti, klikněte v levém podokně portálu **sítí** a vyberte **Místní sítě** v horní části stránky. Na portálu je nejjednodušší způsob, jak zobrazit oblasti, které jste uvedli na stránce **Upravit** . Zobrazit oblasti, vyberte VNet a klikněte na **Upravit** v dolní části stránky.

1. Na stránce **Zadejte podrobnosti místní síti** není proveďte požadované změny. Klikněte na Další šipky v dolní části stránky.

1. Na stránce **zadejte do adresního prostoru** měnit vaší sítě adresu místa. Klikněte na políčko Uložit konfiguraci.

## <a name="how-to-view-gateway-traffic"></a>Jak zobrazit provoz brány

Ze stránky virtuální sítě **řídicí panel** můžete zobrazit brány a provoz brány.

Na stránce **řídicí panel** můžete zobrazit takto:

- Velikost dat, která je prochází brány, data a data se.

- Názvy serverů DNS určených pro virtuální sítě.

- Spojení mezi brány a zařízení VPN.

- Sdílené klíč, který se používá ke konfiguraci brány připojíte se k zařízení VPN.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Jak změnit typ směrování VPN pro brány

Protože některé konfigurace připojení jsou dostupné jenom pro určité typy směrování brány, můžete zjistit, že potřebujete změnit brány směrování typ existující brány virtuální privátní sítě VPN. Chcete například přidat čárky webu připojení již existujícího připojení k webu, který má statické brány. Bod webu připojení vyžadují dynamické brány. Znamená to, že konfigurace připojení P2S, musíte změnit brána směrování typ VPN z statické dynamické.

Pokud potřebujete změnit brány směrování typ VPN, budete existující bránu odstranit a pak vytvořte novou bránu novému směrování typu. Není potřeba odstranit celý virtuální sítě Změna typu směrování brány.

Před změnou brány směrování typ VPN, je potřeba ověřit, že zařízení VPN podporuje směrování typ, který chcete použít. Stáhnout nové směrování konfigurace ukázky a zkontrolujte požadavky na zařízení VPN, najdete v článku [O zařízení VPN virtuální připojení k síti](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Když odstraníte brány virtuální sítě VPN, VIP přiřazená brány vydání. Jestliže obnovit bránu nové VIP se přiřadí k němu.

1. **Odstranění existující Brána VPN.**

    Na stránce **řídicí panel** pro síť virtuální přejděte do dolní části stránky a klikněte na **Odstranit bránu**. Čekat na oznámení, že byl odstraněn brány. Jakmile se zobrazí upozornění na obrazovce odstranil brány, můžete vytvořit novou bránu.

1. **Vytvořte novou bránu VPN.**

    Vytvořit novou bránu pomocí postupu v horní části stránky: [Vytvoření brány VPN](#create-a-vpn-gateway).


## <a name="next-steps"></a>Další kroky

Virtuálních počítačích můžete přidat k síti virtuální. Zjistěte, [jak vytvořit vlastní virtuálního počítače](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Pokud budete chtít konfigurace připojení VPN čárky webu, přečtěte si téma [Konfigurace připojení VPN čárky webu](vpn-gateway-point-to-site-create.md).

 
