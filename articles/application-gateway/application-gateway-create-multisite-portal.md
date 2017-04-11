<properties
   pageTitle="Konfigurace brány existující aplikací pro publikování na více webů na portálu Azure | Microsoft Azure"
   description="Tato stránka obsahuje pokyny ke konfiguraci brány existující Azure aplikací pro publikování na více webových aplikací ve stejné bráně pomocí portálu Azure."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurace brány existující aplikací pro publikování na více webových aplikací

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-multisite-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Více hostování webů umožňuje nasazení více než jeden web aplikace ve stejné bráně aplikace. Závisí na přítomnosti záhlaví hostitele v pozvánce na schůzku příchozí HTTP, a zjistit, jaký posluchače dostal přenosy. Posluchače potom směrovat tak, aby umožnění datových přenosů do příslušné back-end fondu nakonfigurovaná v definici pravidla brány. Ve webových aplikacích SSL povolené aplikace brány závisí na koncovku označení název serveru (SNI) zvolit správný posluchače přenosů web. Společné použití pro více hostingu webu je načtení zůstatek žádosti o různých webových domén do různých serveru back-end fondů. Podobně víc subdomén stejné kořenovou doménu mohou být umístěny ve stejné bráně aplikace.

## <a name="scenario"></a>Scénář

V následujícím příkladu aplikace brány slouží návštěvníci contoso.com a fabrikam.com s dvěma fondů serveru back-end: contoso serveru fondu a fondu fabrikam serveru. Podobně jako nastavení může hostitele subdomény jako app.contoso.com a blog.contoso.com.

![více pracovišť scénářů][multisite]

## <a name="before-you-begin"></a>Než začnete

Tento scénář přidá do existující brány aplikace podporu více webu. Chcete-li dokončit tento scénář scénář existující brány aplikace musí být k dispozici pro nastavení. Navštěvujte blog o [Vytvoření aplikace brány pomocí portálu](./application-gateway-create-gateway-portal.md) se dozvíte, jak vytvořit základní aplikace brány na portálu.

## <a name="requirements"></a>Požadavky

- **Back-end serveru fondu:** Seznam IP adres servery back-end. IP adresy uvedené by měl buď patří do podsítě virtuální sítě nebo by měl být veřejnou IP/VIP. Plně kvalifikovaný název domény můžete taky použít.
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a zaevidují do všech serverů v rámci fondu.
- **Front-end portu:** Toto je veřejné port, který je otevřen v bráně aplikace. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má front-end port protokol (Http nebo Https tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít). Aplikace více webů s povolenou bran název hostitele a indikátory SNI taky přidají.
- **Pravidlo:** Pravidlo váže posluchače fondu serveru back-end a definuje které serveru back-end fondu přenos budou přesměrovány při narazí konkrétní posluchače.
- **Certifikátů:** Každý posluchače vyžaduje jedinečné certifikát, v tomto příkladu 2 posluchače vytvořené pro více webů. Dva .pfx certifikáty a hesla, je potřeba vytvořit.

## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Následují kroky potřebné k aktualizaci brány aplikace:

1. Vytvoření back-end fondů používat pro každý web.
2. Vytvoření nové posluchače pro každý web aplikace brány bude podporovat.
3. Vytváření pravidel pro mapování každý posluchače s příslušnou back-end.

## <a name="create-back-end-pools-for-each-site"></a>Vytvoření skupiny back-end pro každý web

Fond back-end pro každý web této aplikace brány bude potřeba podpory, v tomto případě 2 bude vytvořen, jeden pro contoso11.com a druhý pro fabrikam11.com.

### <a name="step-1"></a>Krok 1

Přejděte k existující bráně aplikace na portálu Azure (https://portal.azure.com). Vyberte **fondů back-end** a klikněte na **Přidat**

![Přidání fondů back-end][7]

### <a name="step-2"></a>Krok 2

Vyplňte informace o back-end fondu **pool1**, přidání ip adresy nebo plně kvalifikované názvy domén pro servery back-end a klikněte na tlačítko **OK**

![nastavení pool1 fondu back-end][8]

### <a name="step-3"></a>Krok 3

Na zásuvné back-end fondů kliknutím na **Přidat** přidejte další fond back-end **pool2**, přidání ip adresy nebo plně kvalifikované názvy DOMÉN pro servery back-end a klikněte na tlačítko **OK**

![nastavení pool2 fondu back-end][9]

### <a name="create-listeners-for-each-back-end"></a>Vytvoření posluchače pro každou back-end

Aplikace brány závisí na HTTP 1.1 hostitele záhlaví hostovat více než jeden web na stejné veřejnou IP adresu a portu. Základní posluchače vytvořené v portálu neobsahuje tuto vlastnost.

### <a name="step-1"></a>Krok 1

Klikněte na **posluchačů** na existující brány aplikace a klikněte na **více webů** přidání prvního posluchače.

![Přehled zásuvné posluchače][1]

### <a name="step-2"></a>Krok 2

Vyplňte informace pro posluchače, v tomto příkladu ukončení SSL nakonfigurovaný, vytvořte nový port frontend. Nahrajte .pfx certifikát, který chcete použít pro ukončení SSL. Rozdíl jenom na tento zásuvné ve srovnání s zásuvné standardní základní posluchače je název hostitele.

![Vlastnosti zásuvné posluchače][2]

### <a name="step-3"></a>Krok 3

Klikněte na **více webů** a vytvořte jiný posluchače podle popisu v předchozí krok pro druhý web. Zkontrolujte, že použít jiný certifikát pro druhý posluchače. Rozdíl jenom na tento zásuvné ve srovnání s zásuvné standardní základní posluchače je název hostitele. Vyplňte informace pro posluchače a klikněte na **OK**.

![Vlastnosti zásuvné posluchače][3]

> [AZURE.NOTE] Vytvoření posluchačů na portálu Azure aplikace brány dlouho pracovního úkolu je ji může chvíli trvat vytvořit dva posluchače v tomto scénáři. Po dokončení zobrazit posluchačů na portálu, jak je vidět na následujícím obrázku.

![Přehled posluchače][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Vytváření pravidel pro mapování posluchačů na fondů back-end

### <a name="step-1"></a>Krok 1

Přejděte k existující bráně aplikace na portálu Azure (https://portal.azure.com). Vyberte **pravidla** a zvolte existující pravidla výchozí **rule1 New** a klikněte na **Upravit**.

### <a name="step-2"></a>Krok 2

Vyplňte zásuvné pravidla, jak je vidět na následujícím obrázku. Výběr první posluchače a první fondu a klikněte na **Uložit** až budete hotovi.

![Úprava existujícího pravidla][6]

### <a name="step-3"></a>Krok 3

Klikněte na **základní pravidla** pro vytvoření 2 pravidla. Vyplňte formulář s druhý posluchače a druhý back-end fondu a uložte kliknutím na **OK** .

![Přidání základního pravidla zásuvné][10]

To dokončení konfigurace brány existující aplikační s podporou více webů portálu Azure.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak chránit vaše weby s [Aplikací brány - webové aplikace Firewall](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png