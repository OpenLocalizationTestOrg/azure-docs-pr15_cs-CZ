<properties
   pageTitle="Jak provést revizi přístup | Microsoft Azure"
   description="Po spuštění aplikace access revize v Azure AD privilegovaných Správa identit, přečtěte si, jak dokončit, protože se a zobrazit výsledky"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Jak provést revizi přístup správy identit Azure AD privilegovaných


Privilegovaných role správce můžete zkontrolovat přístup privilegovaných jednou [spustila zabezpečení revize](active-directory-privileged-identity-management-how-to-start-security-review.md). Správa identit Azure AD oprávněními (osobních) automaticky odesílat e-mailu, která vás vyzve, uživatelé zobrazíte jejich přístup. Pokud uživatel není obdržel oznámení o e-mailu, můžete jim poslat pokynů v [článku Jak provést revizi zabezpečení](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Po období revize zabezpečení se nebo všem uživatelům zatím nebylo dokončeno jejich vlastním zkontrolovat, postupujte podle kroků v tomto článku spravovat revize a uvidíte výsledky.

## <a name="manage-security-reviews"></a>Správa zabezpečení recenze

1. Přejděte na [portál Azure](https://portal.azure.com/) a vyberte aplikaci **Azure AD privilegovaných Správa identit** na řídicím panelu.
2. Vyberte požadovaný oddíl **aplikace Access zkontroluje** řídicího panelu.
3. Vyberte položku revize přístup, který chcete spravovat.

Na zásuvné podrobností revize access jsou možnosti čísel pro správu, které revize.

![Správce osobních informací aplikace access revize tlačítka - snímek][1]

### <a name="remind"></a>Připomenutí

Pokud access revize nastavenou tak, aby uživatelé zkontrolujte sami, tlačítko **připomenutí** odešle oznámení. 

### <a name="stop"></a>Stop

Všechny recenze přístup mají koncové datum, ale můžete pomocí tlačítka **Zastavit** nejdříve možné dokončení. Pokud všichni uživatelé ještě revizi tak, že tentokrát, nebudou moct po ukončení revize. Seznámit nelze restartujte po je zastaveno.

### <a name="apply"></a>Použití

Po dokončení revize přístup buď protože dosáhne koncového data nebo zastavení ručně, na tlačítko **použít** používá výsledek revize. Pokud byl odepřen přístup uživatele na stránce Kontrola, jedná se o krok, který odebere jejich přiřazování rolí.  

### <a name="export"></a>Export

Pokud chcete použít výsledky kontroly zabezpečení ručně, můžete exportovat revize. Tlačítko **Exportovat** bude spusťte stahování souboru CSV. Můžete spravovat výsledků v aplikaci Excel nebo jiných programů, které otevřít soubory CSV.

### <a name="delete"></a>Odstranění

Pokud nejste zajímaly i další revize, odstraňte ji. Tlačítko **Odstranit** odebere revize ze správce osobních informací aplikace.

> [AZURE.IMPORTANT] Se zobrazí upozornění než dojde k odstranění, proto, že chcete odstranit tento revize.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
