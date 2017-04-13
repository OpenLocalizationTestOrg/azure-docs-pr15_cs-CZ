<properties
    pageTitle="Nastavení názvu domény pro váš koncový bod úložiště objektů Blob | Microsoft Azure"
    description="Zjistěte, jak přiřadit koncový bod úložiště objektů Blob Azure úložiště účtu na portálu klasické Azure vlastní doménu."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurace vlastního názvu domény pro váš koncový bod úložiště objektů Blob

## <a name="overview"></a>Základní informace

Můžete nastavení vlastní domény pro přístup k datům ve vašem účtu Azure úložiště objektů blob. Je výchozí koncový bod pro úložiště objektů Blob `<storage-account-name>.blob.core.windows.net`. Pokud mapujete vlastní doménu a subdoménu třeba **www.contoso.com** objektů blob koncový bod pro váš účet úložiště a pak uživatelé můžou taky přístup k datům objektů blob ve vašem účtu úložiště pomocí této domény.

>[AZURE.IMPORTANT] Azure úložiště ještě podporuje HTTPS s vlastní domény. Jsme vědět, že zákazníci zajímají tuto funkci a bude k dispozici v budoucí verzi.

Existují dva způsoby tak, aby ukazovaly vaší vlastní domény objektů blob koncový bod pro váš účet úložiště. Nejjednodušší způsob je vytvořit záznam CNAME mapování vlastní doménu a subdoménu objektů blob koncový bod. Záznam CNAME je do DNS namapuje zdrojové domény na cílové doméně. V tomto případě je doména zdroj vlastní doménu a subdoménu – Všimněte si, že subdoména vždy povinný. Cílové doméně je koncový bod služby objektů Blob.

Proces mapování vaší vlastní domény koncový bod objektů blob můžete, ale za následek stručný období výpadek služeb pro doménu registrace domény na [Portálu klasické Azure](https://manage.windowsazure.com). Pokud vlastní domény momentálně podporuje aplikaci smlouvu o úrovni služeb (SLA), která vyžaduje, která tu žádné prostoje, můžete použít Azure **asverify** subdoménu k poskytování krok intermediate registrace tak, aby uživatelé budou mít přístup k vaší doméně při DNS mapování probíhá.

Následující tabulka zobrazuje ukázka adresy URL pro přístup k datům v s názvem **mystorageaccount**účtu úložiště objektů blob. Vlastní doménu zaregistrovanou účtu úložiště je **www.contoso.com**:

Pole Typ zdroje|Adresa URL formáty
---|---
Úložiště účtu|**Výchozí adresu URL:** http://mystorageaccount.blob.core.windows.net<p>**Vlastní adresu URL domény:** http://www.contoso.com</td>
Objektů BLOB|**Výchozí adresu URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Vlastní adresu URL domény:** http://www.contoso.com/mycontainer/myblob
Kořenový kontejner|**Výchozí adresu URL:** http://mystorageaccount.blob.core.windows.net/myblob nebo http://mystorageaccount.blob.core.windows.net/$ kořenové/myblob<p>**Vlastní adresu URL domény:** http://www.contoso.com/myblob nebo http://www.contoso.com/$ kořenové/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Registraci vlastní domény pro váš účet úložiště

Tento postup použijte k registraci vlastní domény, pokud máte pochybnosti o domény se stručně není k dispozici pro uživatele nebo pokud vlastní domény momentálně není hostitelem aplikace.

Pokud vlastní domény momentálně podporuje aplikace, která nemůže obsahovat žádné prostoje, pomocí pokynů uvedených v části <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">registraci vlastní domény pro váš účet úložiště pomocí poddomény zprostředkovatel asverify</a>.

Pro nastavení vaší vlastní doménou, vytvořte nový CNAME záznam u doménového registrátora. Určuje záznam CNAME aliasu pro název domény; v tomto případě mapuje adresu vaší vlastní domény koncový bod úložiště objektů Blob účtu úložiště.

Každý registrátora má podobné ale mírně odlišnou způsob, jak určit záznam CNAME, ale koncept je stejný. Poznámka: Mnoho balíčků registrace základní domény, budete muset upgradovat vaše domény registraci balíčku než vytvoříte záznam CNAME nenabízejí konfigurace DNS.

1.  [Azure klasické portál](https://manage.windowsazure.com)přejděte na kartu **úložiště** .

2.  Na kartě **úložiště** klikněte na název účtu úložiště pro které chcete mapovat vlastní doménu.

3.  Klikněte na kartu **Konfigurovat** .

4.  V dolní části obrazovky klikněte na **Spravovat doménu** zobrazíte dialogové okno **Spravovat vlastní doménu** . V textu v horní části dialogu zobrazí se informace o tom, jak vytvořit záznam CNAME. V tomto procesu ignorujte text, který odkazuje na subdoména **asverify** .

5.  Přihlaste se k webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Je možné v oddílu jako je **Název domény**, **DNS**nebo **Správa název serveru**.

6.  Najděte v části pro správu mají záznamy CNAME. Bude pravděpodobně nutné přejít na stránku upřesňující nastavení a hledejte slova **CNAME** **Alias**nebo **subdomény**.

7.  Vytvořte nový CNAME záznam a zadejte alias subdoménu, jako jsou **www** nebo **fotografie**. Zadejte název hostitele, což je objektů Blob služby koncový bod, ve formátu **mystorageaccount.blob.core.windows.net** (kde **mystorageaccount** je název účtu úložiště). Název hostitele používat navrhovaný text v dialogovém okně **Spravovat vlastní doménu** .

8.  Po vytvoření záznamu CNAME, vraťte do dialogového okna **Správa vlastní domény** a zadejte název vaší vlastní domény, včetně subdoménu, v poli **Vlastní název domény** . Pokud vaše subdoména je **www**domény je **contoso.com** , zadejte **www.contoso.com**; Pokud vaše subdoména je **fotografie**, zadejte **photos.contoso.com**. Všimněte si, že subdoména není potřeba.

9. Kliknutím na tlačítko **zaregistrovat** k registraci vlastní domény.

    Pokud zápis úspěšná, zobrazí se zpráva **vaši vlastní doménu je aktivní**. Uživatelé můžou nyní zobrazení objektů blob údajů o vaší vlastní domény, dokud mají příslušná oprávnění.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Registraci vlastní domény pro váš účet úložiště pomocí poddomény zprostředkovatel asverify

Pomocí tohoto postupu můžete zaregistrovat vaší vlastní domény v případě vaší vlastní domény momentálně podporuje aplikaci SLA, která vyžaduje, aby mohlo dojít bez výpadek služeb. Vytvořením CNAME odkazující na asverify. &lt;subdoménu&gt;. &lt;customdomain&gt; k asverify. &lt;storageaccount&gt;. blob.core.windows.net, můžete předem registraci svojí domény s Azure. Pak můžete vytvořit druhý záznam CNAME, která ukazuje z &lt;subdoménu&gt;. &lt;customdomain&gt; k &lt;storageaccount&gt;. blob.core.windows.net v tomto okamžiku umožnění datových přenosů do vaší vlastní domény přesměrovaní na váš koncový bod objektů blob.

Subdoména asverify je speciální subdoménu rozpozná Azure. Podle prepending **asverify** vlastní poddomény, povolit Azure rozpoznatelný vaší vlastní domény beze změny záznamu DNS pro doménu. Jakmile upravit záznam DNS pro doménu, bude připojen k objektů blob koncový bod se žádné výpadek služeb.

1.  [Azure klasické portál](https://manage.windowsazure.com)přejděte na kartu **úložiště** .

2.  Na kartě **úložiště** klikněte na název účtu úložiště pro které chcete mapovat vlastní doménu.

3.  Klikněte na kartu **Konfigurovat** .

4.  V dolní části obrazovky klikněte na **Spravovat doménu** zobrazíte dialogové okno **Spravovat vlastní doménu** . V textu v horní části dialogu zobrazí se informace o tom, jak vytvořit záznam CNAME použitím subdoména **asverify** .

5.  Přihlaste se k webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Je možné v oddílu jako je **Název domény**, **DNS**nebo **Správa název serveru**.

6.  Najděte v části pro správu mají záznamy CNAME. Bude pravděpodobně nutné přejít na stránku upřesňující nastavení a hledejte slova **CNAME** **Alias**nebo **subdomény**.

7.  Vytvořte nový CNAME záznam a zadejte alias subdomain (subdoména), který obsahuje subdoména asverify. Subdoména, kterou zadáte třeba budete mít ve formátu **asverify.www** nebo **asverify.photos**. Zadejte název hostitele, což je objektů Blob služby koncový bod, ve formátu **asverify.mystorageaccount.blob.core.windows.net** (kde **mystorageaccount** je název účtu úložiště). Název hostitele používat navrhovaný text v dialogovém okně **Spravovat vlastní doménu** .

8.  Po vytvoření záznamu CNAME, vraťte do dialogového okna **Správa vlastní domény** a zadejte název vaší vlastní domény v poli **Vlastní název domény** . Pokud vaše subdoména je **www**domény je **contoso.com** , zadejte **www.contoso.com**; Pokud vaše subdoména je **fotografie**, zadejte **photos.contoso.com**. Všimněte si, že subdoména není potřeba.

9.  Klepnutím na zaškrtávací políčko, že **Upřesnit: umožňuje preregister Moje vlastní domény "asverify" subdoména**.

10. Kliknutím na tlačítko **zaregistrovat** preregister vaši vlastní doménu.

    Pokud předběžné registraci je úspěšné, zobrazí se zpráva **vaši vlastní doménu je aktivní**.

11. V tomto okamžiku byly Azure ověřeny vaši vlastní doménu, ale umožnění datových přenosů do vaší domény není zatím směrovány ke svému účtu úložiště. Dokončete proces, vraťte se na webu registrátora vaší DNS a vytvořte další záznam CNAME, který odpovídá vaší subdoménu koncový bod služby objektů Blob. Subdoména například určete jako **www** nebo **fotografie**a hostname jako **mystorageaccount.blob.core.windows.net** (kde **mystorageaccount** je název účtu úložiště). Tento krok je dokončeno registraci vlastní domény.

12. Nakonec můžete odstranit záznam CNAME jste vytvořili pomocí **asverify**, bylo nutné pouze jako zprostředkovatel krok.

Uživatelé můžou nyní zobrazení objektů blob údajů o vaší vlastní domény, dokud mají příslušná oprávnění.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Ověření vlastní domény odkazuje koncový bod služby objektů Blob

Abyste ověřili, že vaši vlastní doménu je skutečně namapované koncový bod služby objektů Blob, vytvořte objektů blob v kontejneru veřejné v rámci vašeho účtu úložiště. Potom ve webovém prohlížeči, použijte identifikátor URI ve formátu následující pro přístup k objektů blob:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Například následující URI používáte pro přístup k webového formuláře prostřednictvím **photos.contoso.com** vlastní subdomény, která odpovídá objektů blob v kontejneru **myforms** :

-   http://Photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Registraci vlastní domény z vašeho účtu úložiště

Registraci vlastní domény, postupujte takto: 

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com). 

2. V navigačním podokně klikněte na **úložiště**. 

3. Na stránce **úložiště** klikněte na název účtu úložiště zobrazíte řídicího panelu. 

5. Na pásu karet klikněte na **Spravovat domény**. 

6. V dialogovém okně **Spravovat vlastní doménu** klikněte na **Unregister**. 


## <a name="additional-resources"></a>Další zdroje informací

-   [Jak mapovat vlastní doménu koncový bod obsahu doručení Network (CDN)](../cdn/cdn-map-content-to-custom-domain.md)
