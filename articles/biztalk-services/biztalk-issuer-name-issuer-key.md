<properties 
    pageTitle="Název vydavatele a Vystavitel klíč služby BizTalk | Microsoft Azure" 
    description="Zjistěte, jak načíst název vydavatele a Vystavitel klíč pro Bus služby nebo aplikace Access (ACS Control) služby BizTalk. MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk služby: Název vydavatele a Vystavitel klíč

Azure přihlašovacích údajů BizTalk používá název služby Bus Vystavitel a Vystavitel klíč a Vystavitel název ovládacího prvku přístup a Vystavitel klíč. Konkrétně:

Úkol | Jaké Vystavitel jméno a Vystavitel klíč
--- | ---
Nasazení aplikace Visual Studio | Přístup k název ovládacího prvku Vystavitel a Vystavitel klíč
Konfigurace portálu služby Azure BizTalk | Přístup k název ovládacího prvku Vystavitel a Vystavitel klíč
Vytváření LOB relé se službami adaptér BizTalk ve Visual Studiu | Název Vystavitel Bus služby a Vystavitel klíč

Toto téma obsahuje kroky k načtení název vydavatele a Vystavitel klíče. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Přístup k název ovládacího prvku Vystavitel a Vystavitel klíč
Název ovládacího prvku Vystavitel přístup a Vystavitel klíč používá následující:

- Aplikace služby Azure BizTalk vytvořené v aplikaci Visual Studio: úspěšně nasadit aplikaci služby BizTalk ve Visual Studiu na Azure, zadáte název ovládacího prvku Vystavitel přístup a Vystavitel klíče. 
- Azure BizTalk služby portálu: Při vytvoření služba BizTalk a otevřete portál služeb BizTalk, váš název ovládacího prvku Vystavitel přístup a Vystavitel klíč jsou automaticky registrované nasazení se stejnými hodnotami řízení přístupu.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopírování a vkládání název ovládacího prvku Vystavitel přístup a Vystavitel klíč

1. Přihlaste se k [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BizTalk služby**.
3. Vyberte službu BizTalk. 
4. **Informace o připojení** vyberte na hlavním panelu. Ovládací prvek Namespace přístup, výchozí vystavitel (Vystavitel název) a výchozí klávesu Vystavitel jsou uvedeny a můžete zkopírování a vložení.  

Shrnutí:  
Název vydavatele = výchozí vydavatel  
Klíč Vystavitel = výchozí klíč


Je taky možné vybrat **Otevřít portálu Správa ACS** a získání hodnoty řízení přístupu:

1. Přihlaste se k [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BizTalk služby**.
3. Vyberte službu BizTalk.
4. Klikněte na tlačítko informace o připojení a vyberte **Otevřít portálu Správa ACS**.
5. Na portálu klikněte v části **Nastavení služby**vyberte **Identitami služby**. Zobrazí vaši identitu služby, což je název ovládacího prvku Vystavitel přístup hodnota. Vyberte odkaz služby identit zobrazíte heslo, které je vystavitel klíč hodnota. Můžete zkopírovat jejich hodnoty.<br/><br/>
Například ve **Služby identit**, uvidíte "vlastník". "Vlastník" je vaše jméno Vystavitel řízení přístupu. Když kliknete na odkaz "vlastník", uvidíte **heslo**. Když kliknete na odkaz "Heslo", uvidíte hodnotu. Tato hodnota heslo je vystavitel přístup CTRL.  

Shrnutí:  
Název vydavatele = název služby Identity  
Klíč Vystavitel = heslo hodnota

V levém navigačním podokně můžete také vybrat **Služby Active Directory** k načtení hodnot řízení přístupu. 

> [AZURE.IMPORTANT]Po vytvoření Namespace řízení přístupu pomocí **Služby Active Directory**, služby identit **není** vytvoří se automaticky. Když zřizujete služby BizTalk ovládací prvek aplikace Access Namespace služby identit s názvem "" (Vystavitel jméno vlastníka), heslo (Vystavitel klíč), a symetrickou klíč, vytvoří se automaticky.<br /> 
[Postup: pomocí služby správy ACS do konfigurace služby identit](http://go.microsoft.com/fwlink/p/?LinkID=303942) obsahuje další informace o přístupu ovládací prvek služby identit.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Název Vystavitel Bus služby a Vystavitel klíč
Název Vystavitel Bus služby a Vystavitel klíče slouží BizTalk adaptér služby. Služby BizTalk projektu ve Visual Studiu pomocí služby adaptér BizTalk pro připojení k systému místní řádku obchodní (LOB). Pokud chcete připojit, vytvořte LOB Relay a zadejte podrobnosti o vaší LOB systému. Přitom, můžete také zadat název Vystavitel Bus služby a Vystavitel klíč.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>K načtení Bus Vystavitel název a služby Vystavitel klíč

1. Přihlaste se k [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **Bus služby**.
3. Vyberte svůj obor názvů. V hlavním panelu vyberte **Informace o připojení**. Zobrazí se **Výchozí Vystavitel** (Vystavitel název) a **Výchozí Key** (klíč Vystavitel). Můžete zkopírovat jejich hodnoty.  

Shrnutí:  
Název vydavatele = výchozí vydavatel  
Klíč Vystavitel = výchozí klíč

## <a name="next"></a>Další
Další témata Azure BizTalk Services:

-  [Instalace služby Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Výukové programy pro: Služby Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Služby Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Viz taky
-  [Postup: pomocí služby správy ACS ke konfiguraci služby identit](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk služby: Vývojář Basic, Standard a Premium edice graf](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk služby: Zřizujete klasické portál pomocí Azure](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk služby: Diagram stavu zřizujete](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk služby: Řídicí panel, sledování a měřítko karty](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk služby: Zálohování a obnovení](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [Služby BizTalk: omezení](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
