<properties
   pageTitle="Jak zachovat konstantní virtuální IP adresu do cloudové služby | Microsoft Azure"
   description="Zjistěte, jak zajistit, že virtuální IP adresu (VIP) služby Azure cloudu nezmění."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Jak zachovat konstantní virtuální IP adresu do cloudové služby

Při aktualizaci do cloudové služby, který je umístěn v Azure může nutné ověřit, že virtuální IP adresu (VIP) služby nezmění. Mnoho domény správu přístupových použijte systému DNS (Domain Name) pro registraci názvy domén. DNS funguje jenom v případě, že VIP se nezmění. **Průvodce publikováním** v nástrojích Azure umožňuje zajistit, že VIP cloudové služby nezmění ho aktualizujete ho. Další informace o používání správy DNS domény pro cloudové služby najdete v článku [Konfigurace vlastního názvu domény pro službu Azure cloudu](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Publikování do cloudové služby beze změny jeho VIP

Virtuální do cloudové služby je přidělený při můžete nejdřív ho nasadit do Azure v konkrétním prostředí, například provozním prostředí. VIP nezmění, pokud odstraníte nasazení explicitně nebo je implicitně odstranil procesu nasazení aktualizace. Zachovat VIP, nesmí odstraníte nasazení a také ujistěte se, že aplikace Visual Studio automaticky neodstraníte nasazení. Řízení chování zadáním nastavení nasazení v **Publikovat Průvodce**, který podporuje několik možností nasazení. Můžete zadat svěže nasazení nebo instalace aktualizace, který může být přírůstková nebo souběžné, a oba typy nasazení aktualizaci zachovat VIP. Definice různých typů nasazení najdete v článku [Publikovat Průvodce aplikací Azure](vs-azure-tools-publish-azure-application-wizard.md).  Kromě toho můžete určit, zda se odstraní předchozí nasazení do cloudové služby, pokud dojde k chybě. Pokud tato možnost není správně nastavená se může neočekávaně změnit VIP.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Chcete-li aktualizovat do cloudové služby beze změny jeho VIP

1. Po aspoň jednou nasadíte cloudové služby, otevření místní nabídky pro uzel Azure projektu a potom vyberte **Publikovat**. Zobrazí se průvodce **Publikovat Azure aplikace** .

1. V seznamu předplatných vyberte tu, do kterého chcete nasadit a klikněte na tlačítko **Další** . Zobrazí se stránka **Nastavení** v průvodci.

1. Na kartě **Obecné nastavení** zkontrolujte, že všechny správně název cloudové služby, do kterého jste nasazení, **prostředí**, **Konfigurace vytváření**a **Konfigurace služby** .

1. Na kartě **Upřesnit nastavení** zkontrolujte, že účet úložiště a nápisem nasazení správně, zda je zrušeno zaškrtnutí políčka **Odstranit nasazení o chybě** a, že je zaškrtnuté políčko **nasazení** aktualizace. Zaškrtnutím políčka **nasazení** aktualizace zajistit nasazení se neodstraní a vaše VIP neztratí při opětovném aplikace. Zrušením okně pro potvrzení **Odstranění nasazení políčko selhání při**zajistíte, že vaše VIP neztratí Pokud dojde k chybě při nasazení.

1. Další určete, jak má role aktualizovat, klikněte na odkaz **Nastavení** vedle pole **Aktualizovat nasazení** a pak zvolte přírůstková aktualizace nebo možnost souběžné aktualizace v **nasazení aktualizovat** dialogovém okně nastavení. Pokud se rozhodnete přírůstková aktualizace, se aktualizuje pokaždé jeden po druhém, tak, aby aplikace je vždy dostupná. Pokud se rozhodnete současné aktualizaci, všechny výskyty aktualizovány ve stejnou dobu. Souběžné aktualizace je rychlejší, ale služby nemusí být k dispozici během procesu aktualizace.

1. Až budete spokojeni s nastavením, klikněte na tlačítko **Další** .

1. Na stránku **souhrnu** v průvodci zkontrolujte si nastavení a potom klikněte na tlačítko **Publikovat** .

  >[AZURE.WARNING] Pokud nasazení selže, by měl adresy důvod, proč se nezdařilo a přeinstalujte neprodleně, chcete-li předejít necháte cloudové služby v poškozeném stavu.

## <a name="next-steps"></a>Další kroky

Další informace o publikování na Azure z aplikace Visual Studio najdete v tématu [Průvodce aplikace publikovat Azure](vs-azure-tools-publish-azure-application-wizard.md).
