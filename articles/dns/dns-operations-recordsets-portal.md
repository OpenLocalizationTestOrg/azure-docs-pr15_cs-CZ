<properties
   pageTitle="Správa sady záznamů DNS a záznamů pomocí portálu Azure | Microsoft Azure"
   description="Správa DNS záznamu nastaví záznamů a pokud je hostitelem vaší domény na Azure DNS."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a>Spravovat záznamy DNS a záznam nastaví pomocí portálu Azure


> [AZURE.SELECTOR]
- [Portál Azure](dns-operations-recordsets-portal.md)
- [Azure rozhraní příkazového řádku](dns-operations-recordsets-cli.md)
- [Prostředí PowerShell](dns-operations-recordsets.md)


Tento článek popisuje, jak spravovat sady záznamů a záznamy zóny DNS pomocí portálu Azure.

Je důležité pochopit rozdíl mezi sady záznamů DNS a jednotlivých záznamů DNS. Sada záznamů je sada záznamů v zóně, které mají stejný název a jsou stejného typu. Další informace najdete v tématu [Vytvoření DNS záznamu sady a záznamů pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Vytvoření nové sady záznamů a záznam

Vytvoření sady na portálu Azure záznamů najdete v tématu [Vytvoření záznamů DNS pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).


## <a name="view-a-record-set"></a>Zobrazení sada záznamů

1. Na portálu Azure přejděte na zásuvné **zóny DNS** .

2. Vyhledejte sady záznamů a vyberte ho. Otevře se vlastnosti sadě záznamů.

    ![Vyhledejte sada záznamů](./media/dns-operations-recordsets-portal/searchset500.png)


## <a name="add-a-new-record-to-a-record-set"></a>Přidání nového záznamu do sady záznamů

Přidání až 20 záznamů do libovolné sadě záznamů. Sada záznamů nesmí obsahovat dva identickými záznamy. Prázdný sady záznamů (s nulovou záznamů) se dají vytvářet, ale na názvové servery Azure DNS nezobrazují. Sady záznamů typu CNAME může obsahovat maximálně jeden záznam.


1. Na zásuvné **záznamu nastavit vlastnosti** zóny DNS klikněte na nastavit záznam, který chcete přidat záznam, který chcete.

    ![Vyberte sadu záznamů](./media/dns-operations-recordsets-portal/selectset500.png)

2. Určení záznam vlastností sady vyplněním pole.

    ![Přidání záznamu](./media/dns-operations-recordsets-portal/addrecord500.png)

2. Klepnutím na tlačítko **Uložit** v horní části zásuvné uložte nastavení. Zavřete zásuvné.

3. V rohu zobrazí se ukládá záznam.

    ![Uložení sada záznamů](./media/dns-operations-recordsets-portal/saving150.png)

Po uložení záznamu budou obsahovat hodnoty na zásuvné **zóny DNS** nový záznam.


## <a name="update-a-record"></a>Aktualizujte záznam

Když třeba aktualizujete záznam v existující sada záznamů, pole, která lze aktualizovat, závisí na typu záznamu pracujete s.

1. Na zásuvné **záznam nastavení vlastností** pro sada záznamů vyhledejte záznam.

2. Upravte záznam. Při úpravě záznamu, můžete změnit nastavení k dispozici pro záznam. V následujícím příkladu je vybráno pole **IP adresa** a IP adresu právě upravuje.

    ![Upravit záznam](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Klepnutím na tlačítko **Uložit** v horní části zásuvné uložte nastavení. V pravém horním rohu zobrazí se upozornění, které uložení záznamu.

    ![Uložení sada záznamů](./media/dns-operations-recordsets-portal/saved150.png)


Po uložení záznamu budou obsahovat hodnoty záznamu na zásuvné **zóny DNS** nastavit aktualizovaný záznam.


## <a name="remove-a-record-from-a-record-set"></a>Odebrání záznamu z sada záznamů

Portál Azure slouží k odebrání záznamů v sadě záznamů. Všimněte si, že odeberete posledním záznamu v sadě záznamů se neodstraní sady záznamů.

1. Na zásuvné **záznam nastavení vlastností** pro sada záznamů vyhledejte záznam.

2. Klikněte na záznam, který chcete odebrat. Klikněte na **Odebrat**.

    ![Odebrání záznamu](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Klepnutím na tlačítko **Uložit** v horní části zásuvné uložte nastavení.

3. Po odebrání záznam budou obsahovat hodnoty záznamu na zásuvné **zóny DNS** odstranění.


## <a name="delete"></a>Odstranění sada záznamů

1. Na **záznamu nastavit vlastnosti** zásuvné vašeho záznamu nastavit, klikněte na **Odstranit**.

    ![Odstranění sada záznamů](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Zobrazí se zpráva s dotazem, pokud chcete odstranit sady záznamů.

3. Ověřte, že název odpovídá sady záznamů, který chcete odstranit a potom klikněte na **Ano**.

4. Na zásuvné **zóny DNS** zkontrolujte, že sady záznamů už viditelné.


## <a name="work-with-ns-and-soa-records"></a>Práce se záznamy názvového serveru a SOA

Záznamy názvového serveru a SOA automaticky vytvořené je spravováno z jiných typů záznamů jinak.

### <a name="modify-soa-records"></a>Úprava SOA záznamů

Nelze přidat nebo odebrat záznamy z automaticky vytvořené záznamu SOA rozmístěné po vrcholu zóny (název = "@"). Však můžete změnit libovolné parametrů záznamu SOA (s výjimkou "host (hostitel)") a záznam nastavte hodnotu TTL.

### <a name="modify-ns-records-at-the-zone-apex"></a>Změna záznamů názvového serveru u vrcholu zóny

Nelze přidat, odebrat nebo změnit záznamy v automaticky vytvořené sady na vrcholu zóny záznamů názvového serveru (název = "@"). Pouze změny, které je povolen je upravit sady záznamů TTL.

### <a name="delete-soa-or-ns-record-sets"></a>Odstranění SOA nebo NS sady záznamů

Nelze odstranit nebo SOA a sady záznamů názvového serveru na vrcholu zóny (název = "@") , který se automaticky vytvoří při vytvoření zóny. Budou odstraněny automaticky po odstranění zóny.

## <a name="next-steps"></a>Další kroky

-   Další informace o službě DNS Azure najdete v článku [Přehled Azure DNS](dns-overview.md).
-   Další informace o automatizaci DNS najdete v článku [vytváření DNS zones a záznam nastaví pomocí .NET SDK](dns-sdk.md).
-   Další informace o zpětném DNS záznamy najdete v tématu [Správa obráceném záznamy DNS pro služby pomocí Powershellu](dns-reverse-dns-record-operations-ps.md).
