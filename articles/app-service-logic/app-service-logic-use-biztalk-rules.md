<properties
   pageTitle="Další informace o a vytvořte pravidla BizTalk rozhraní API aplikace v aplikaci logiky | Microsoft Azure"
   description="Toto téma popisuje funkce spojnice BizTalk pravidla a pokyny na jeho použití"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk pravidel

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Obchodní pravidla zapouzdřuje zásady a rozhodnutí, kterými se řídí obchodních procesů. Tyto zásady může být dříve podle postupu příručky, smluv nebo dohod nebo může existovat jako znalostí nebo odborných informací součástí zaměstnanců. Tyto zásady dynamické a vyměřené poplatky za jeho změn v průběhu času brzy změny ve firemní plány, předpisů nebo dalších důvodů.

Provádění tyto zásady v tradiční programovacím jazyce vyžaduje značné množství času a koordinaci a neumožňuje není programátory účastnit se vytváření a udržování podnikových zásad. BizTalk obchodní pravidla umožňuje rychle provádět tyto zásady a oddělit zbytek obchodnímu procesu. Díky týkající se uplatňování požadované změny u podnikových zásad bez vlivu na ostatní obchodní proces.

##<a name="why-rules"></a>Proč pravidel

Použití pravidel firmy BizTalk v obchodní proces 3 klíčové důvodů:

* Oddělit obchodní logiky z aplikace kódu
- Povolit obchodní analytici mít větší kontrolu nad obchodní logiky správy
+ Změny obchodní logiky přejděte na výrobní rychlejší

##<a name="rules-concepts"></a>Principy pravidel

##<a name="vocabulary"></a>Slovník

_Slovník_ je sada definice tvořené popisných názvů pro výpočetního objekty používané v pravidlo podmínek a akcí. Definice slovník zpřehledněte pravidla číst, pochopit a sdílet lidmi z určitého obchodního domény.

Vocabularies slouží k přemostění mezery mezi sémantiku business a implementaci. Vazby dat pro stav schválení může například přejděte na určité sloupce v řádku v některých databázi tvaru dotazu SQL. Místo vložení toto řazení znázornění složitých v pravidlu, můžete místo toho vytvořit definici slovník, přidružený k této vazba dat pomocí popisný název "Stav." Později můžete zahrnout "Stav" libovolný počet pravidla a modul pravidlo můžete načíst odpovídající data z tabulky.

##<a name="rule"></a>Pravidlo

Obchodní pravidla jsou deklarativní příkazy, které určují způsob vedení obchodních procesů. Pravidla se skládá z podmínky a akce. Podmínka Vyhodnocená každá její položka a pokud je vyhodnocen jako true (pravda), modul pravidlo zahájí jedné nebo víc akcí.
Pravidla v rámci pravidla firmy jsou definované pomocí v tomto formátu:

_IF_ _podmínky_ _Potom_ _Akce_

Příklad:

*Když velikost je menší nebo rovna hodnotě dostupnými finančními prostředky*  
*Vedení transakce a tisku potvrzení*

Toto pravidlo určuje, zda transakce zkouší použitím obchodní logiky ve formuláři můžete porovnat dvě peněžní hodnoty v podobě částka transakce a dostupnými finančními prostředky.
Obchodní pravidlo můžete použít k vytváření, úprava a nasazení obchodní pravidla. Můžete taky můžete provádět předchozí úloh programově.

###<a name="conditions"></a>Podmínky

Zadaná podmínka vyhodnotí jako true nebo false (logický) výraz, který se skládá z jedné nebo více predikáty.
V našem příkladu predikátu menší nebo rovna hodnotě platí pro výše a dostupnými finančními prostředky. Tato podmínka vyhodnotí vždy true nebo false.
Predikáty kombinací logické operátory AND, nebo a ne logický výraz, který je potenciálně velmi velké, ale bude vždy vyhodnocena jako PRAVDA nebo NEPRAVDA.

###<a name="actions"></a>Akce

Akce jsou funkční důsledky vyhodnocení podmínky. Pokud splnění podmínky pravidlo odpovídající akci nebo akce jsou, které iniciuje.
V našem příkladu jsou akce, které jsou při provádění a pouze pokud je podmínka (v tomto případě "Pokud velikost je menší nebo rovna hodnotě dostupnými finančními prostředky") pravdivá, "transakce vedení" a "Tisk potvrzení".
Akce jsou v rámci pravidla firmy tvořené provádění operací sady dokumentů XML.

##<a name="policy"></a>Zásady

Zásady je logické seskupení pravidel. Vytvořte zásady, uložíte ho, otestovat a, až budete spokojeni s výsledky, použijte v pracovním prostředí.

###<a name="policy-composition"></a>Složení zásad

Můžete vytvořit zásady na portálu pravidel. Zásady může obsahovat libovolně velké sady pravidel, ale obvykle vytvořte zásady z pravidla, které se týkají specifickým obchodním domény v rámci aplikace, která budou používat zásadu.

###<a name="policy-testing"></a>Testování zásad
Test spustit zásad můžete provádět efektivně před použitím v pracovním prostředí. Na portálu pravidla umožňuje zadat vstupů do zásad, spusťte zásady a zobrazit jeho výstup. Výstup obsahuje protokoly, provádění pravidla, vyhodnocení podmínky a výsledná výstupů.

##<a name="sample-scenario---insurance-claims"></a>Ukázkový scénář – pojištění deklarací
Pojďme prohlédněte scénáři výběru a projděte si ho jako jsme vytvořit obchodní logiky pro stejný.

![Alternativní text][1]

Ve scénáři really simple pojištění deklarací odešle navrhující nárok pojištění (pomocí libovolného klienta jako webu, telefonní aplikace a další). Požádat o tomto deklaraci identity získá odesílány jednotku zpracování deklarace společnosti a na základě výsledku zpracování, deklaraci může být buď schváleno, odmítnuto nebo odesílané k dalšímu ruční zpracování.
Jednotka zpracování deklaraci identity v našem případě by být zahrnující obchodní logiky pro systém. Provedením této jednotky chcete prohlédnout podrobněji, vidíme, takto:

![Alternativní text][2]

Teď pomocí dejte nám firmy pravidel k provádění tohoto obchodní logiky.


##<a name="creation-of-rules-api-app"></a>Vytvoření pravidla rozhraní Api aplikace


1. Přihlášení k portálu Azure
2. Vyberte Nová -> Marketplace a vyhledejte *BizTalk pravidel*
3. Vyberte BizTalk pravidla ze seznamu výsledků. Otevře se zásuvné BizTalk pravidel
4. Klikněte na tlačítko *vytvořit* ![alternativní text][3]
1. V nové zásuvné, která se otevře zadejte následující informace:  
    1. Název – uveďte název pravidla rozhraní API aplikace
    1. Plán služeb aplikací – vyberte nebo vytvořte nový plán služeb aplikací
    1. Ceny osy – zvolit ceny osy má tuto aplikaci k uložena v
    1. Pole Skupina zdroje – vyberte nebo vytvořte místo, kam má aplikace nacházejí v pole Skupina zdroje
    2. Předplatné - vyberte předplatné, které chcete použít
    1. Umístění zvolte místo, kam se mají aplikací nasazena zeměpisné polohy.
4.  Vyberte možnost *vytvořit*. Objevit během pár minut by nevytvoří BizTalk pravidel rozhraní API aplikace.

##<a name="vocabulary-creation"></a>Vytvoření slovníku
Po vytvoření BizTalk pravidla rozhraní API aplikace, bude dalším krokem vytvořením vocabularies. Očekává se, že vývojář je nejčastěji používaných hlavní která by toto cvičení. Tady je postup, jak získat to udělat:


1. Spuštění svého BizTalk pravidla rozhraní API aplikace z portálu tak, že přejdete na Browse -> rozhraní API aplikace - ><Your Rules API App>. To se dostanete k řídicím panelu pravidla rozhraní API aplikace podobný pod:

   ![Alternativní text][4]

2. Vyberte "Slovník definice". Zobrazí se 3 slovník pro vytváření obrazovky vyberte možnost "Přidat" Chcete-li začít přidávat nové definice slovníku.
Existuje 2 typy definice slovník v současnosti podporované – literálů a XML.

##<a name="literal-definition"></a>Literál typu definice
1.  Po klepnutí na "Přidat", nové zásuvné "Přidat definice" se objeví. Zadejte následující hodnoty
  1.    Jméno – pouze alfanumerické znaky očekává se, že bez žádné speciální znaky. To musí být jedinečný do existující definici seznamu slovníku.
  2.    Popis – volitelné pole.
  3.    Definice zadejte – existuje 2 typy podporované. V tomto příkladu zvolte literálů
  4.    Datový typ – umožňuje uživatelům vyberte datový typ definice. Aktuálně podporované 4 datové typy: můžu.  Řetězec – tyto hodnoty musí být zadány do dvojitých uvozovek ("příklad řetězec")  
    II. Logická – to může být buď true nebo false  
    III.    Číslo – může být desetinné číslo  
    IV. Data a času – to znamená, že rozlišení typu typu datum. Data musí být zadány pomocí tento formát – hh: mm: mm/dd/rrrr AM\PM  
  5. Při zadávání – jedná místo, kam zadáte hodnotu definici. Hodnoty, který tady zadáte musí odpovídat vybraného datového typu. Buď můžete zadat jednu hodnotu, sadu hodnot oddělených čárkou nebo rozsahu hodnot pomocí části klíčových slov *do*. Například můžete zadat jedinečná hodnota 1. Sada 1, 2, 3; nebo oblast 1 až 5. Všimněte si, že rozsah je podporována pouze pro čísla.
  6. Klikněte na *OK*.

![Alternativní text][5]
##<a name="xml-definition"></a>Definicí XML
Pokud ve formátu XML je nastavený typ slovník, je potřeba zadat následující vstupní hodnoty  
  na.    Schéma – klepnutí na k tomu otevře se nové zásuvné umožňuje uživatelům vybrat ze seznamu už odeslaného schémat nebo povolení nahrajte novou.
b.    Výraz XPATH – vstupní získá odemknout až po výběru schéma v předchozím kroku. Po kliknutí na to se prezentace zobrazí schéma, které jste vybrali a umožňuje uživatelům vybírat na uzel pro které je potřeba definici Slovníček vytvořit.  
  c.    Faktoriál – tyto vstupní identifikuje objektu data by krmena modulu pravidla ke zpracování. To je Upřesnit vlastnosti a ve výchozím nastavení je nastavena na nadřazené vybrané XPATH. Faktoriál bude velmi důležitým shromažďování a zřetězení scénářích.

![Alternativní text][6]

### <a name="add-bulk"></a>Hromadné přidání
Výše uvedené kroky zaznamenali prostředí pro vytváření definice slovníku. Po vytvoření definice vytvořené slovník se zobrazí ve formuláři seznam. Existuje požadavky, které budou moct vytvářet několika definice ze stejné schéma místo opakování výše uvedené kroky pokaždé jeden. Je to, kde bude velmi užitečné funkce hromadné přidání.
Výběr "Hromadné přidání" přejdete do nového zásuvné. Cílem prvního kroku je vyberte schéma, pro které mají vytvořit více definice. Pokud vyberete tuto otevře nový zásuvné povolení zvoleného buď ze seznamu už odeslaného schémata a povolení nahrajte novou.
Vlastnost PŘEPNĚTE získá teď odemknout. Pokud vyberete tuto se otevře v prohlížeči schématu kde lze vybrat více uzlů.
Názvy pro více definice vytvořili bude výchozí název vybraný uzel. Tyto můžete změnit vždy po vytvoření.

![Alternativní text][7]

##<a name="policy-creation"></a>Vytváření zásad
Po vytvoření požadovaných vocabularies vývojář očekává se, že obchodní analytik bude jeden vytváření podnikových zásad prostřednictvím portálu Azure.  
    1. na pravidla aplikaci vytvořili je zásad lens kliknutím na kterém uživatel přejde na stránku vytvoření zásad.  
    2. na této stránce se zobrazí v seznamu zásad, které obsahuje tato aplikace určitého pravidla. Analytik můžete přidat nové zásady jednoduše zadání názvu zásady a dvakrát zasažení kartu. Více zásad mohou být uloženy v jedné aplikaci rozhraní API pravidel.  
    3. výběrem vytvořené zásady bude trvat uživatele na stránce podrobnosti zásad kde jednu vidí pravidla, které jsou v zásadu.  
    ![Alternativní text][8]
 4.  Vyberte "Přidat" přidejte nové pravidlo. Tím přejdete do nového zásuvné.

##<a name="rule-creation"></a>Vytvoření pravidla
Pravidlo je kolekce podmínku a akci příkazy. Akce jsou provedeny, pokud podmínka vyhodnotí jako PRAVDA. Zásuvné vytvořit pravidlo pojmenujte zadejte do pravidla jedinečný název (pro tuto zásadu) a popis (volitelné).
Pole Stav (je-li) můžete použít k vytvoření složité podmíněných příkazů. Tady jsou podporované klíčová slova:  
1.  A – podmíněné operátor  
2.  Nebo – podmíněné operátor  
3.  znamená\_není\_existují  
4.  existuje  
5.  NEPRAVDA  
6.  je\_rovností\_k  
7.  je\_větší\_než  
8.  je\_větší\_než\_rovností\_k  
9.  je\_v  
10. je\_méně\_než  
11. je\_méně\_než\_rovností\_k  
12. je\_není\_v  
13. je\_není\_rovností\_k  
14. MOD  
15. PRAVDA  

Pole Action(THEN) může obsahovat více příkazů na jeden řádek, vytvoření akce, které se mají provést. Tady jsou podporované klíčová slova:  
1.  rovná se  
2.  NEPRAVDA  
3.  PRAVDA  
4.  zastavení  
5.  MOD  
6.  Null  
7.  aktualizace  

Pole podmínku a akci poskytují technologie Intellisense můžete rychle vytvářet pravidla. To je možné spouštět zasažení kombinaci kláves ctrl + mezera nebo jenom začnete psát. Klíčová slova odpovídající znakům bude automaticky filtrováno dolů a zobrazí. Okno technologie intellisense zobrazí všechny klíčová slova a definice slovníku.
![Alternativní text][9]

##<a name="explicit-forward-chaining"></a>Explicitní přeposlat zřetězení
Podporuje BizTalk pravidla explicitní předat dál, pokud uživatelé chtějí znovu vyhodnotí pravidla odpovědi na určité akce řetězení, je to aktivace pomocí určitá klíčová slova. Klíčová slova podporované jsou následující:  
   1.   aktualizace <vocabulary definition> – tohoto klíčového slova znovu vyhodnotí všechna pravidla, které používají definici zadaný slovník v jeho stavu.  
   2.   Zastavení – tohoto klíčového slova ukončí všechna spuštění pravidel

##<a name="enabledisable-rules"></a>Enable\Disable pravidel
Každé pravidlo zásady můžete povolit nebo zakázat. Ve výchozím nastavení jsou povoleny všechny pravidla. Vypnutí pravidla nelze provést během provádění zásad. Enable\Disable pravidel lze provést některou z zásuvné pravidlo přímo – příkazy jsou k dispozici v řádku nabídek v horní části zásuvné nebo ze zásad, má místní nabídky (klikněte pravým tlačítkem myši na pravidlo) možnost enable\disable.

##<a name="rule-priority"></a>Pravidlo priority (priorita)
Všechna pravidla zásady zpracují uvedené v pořadí. Priority (priorita) spuštění je určený podle pořadí, ve kterém se objevují v zásadu. Tato priorita lze změnit přetažením jednoduše pravidlo.

##<a name="test-policy"></a>Testování zásad
Otestujte vaše zásady pomocí příkazu "Testování zásad" v zásuvné testování zásad. V tomto zásuvné uvidíte seznam definice slovník, které se používají v zásad, které vyžadují vstup uživatele. Uživatele můžete ručně zadat hodnoty pro tyto vstupů pro jejich scénář testování. Můžete také uživatelé mohou také zvolit import testování XMLs pro zadávání. Jakmile dostanete všechny vstupy, zkoušku jdou spustit a zobrazí se výstupy každá definice Slovníček ve sloupci výstup pro snadné porovnání. Zobrazit obchodní analytik popisný protokoly, klikněte na "Zobrazení protokolů" Chcete-li zobrazit protokoly spuštění. Uložení protokolů, možnost "Uložit výstupní" je k dispozici pro ukládání všechny zkušební související data nezávislé analýzy.

## <a name="using-rules-in-logic-apps"></a>Pomocí pravidel v aplikacích pro použití logických operátorů
Jakmile zásady byl vytvořen a testováno, je nyní připravena pro spotřebu. Vytvoření nové aplikace logiku tak, že vyberete logiku aplikace na levé straně domovské stránky na portálu. Po vytvoření aplikace pro použití logických operátorů ji spustit a pak vyberte *aktivačními událostmi a akce*. Vyberte šablonu *vytvořit od začátku* . Podle pokynů přidejte BizTalk pravidla rozhraní API aplikace do aplikace logiky. Po dokončení, bude možnost rozhodnout, jaký pravidel rozhraní API aplikace (akce) do cíle. Akce seznamu zásad, které se mají provést zahrnutí. Zvolte zvláštní zásady po jejímž uplynutí vstupů povinné zásady musí mít. Uživatele můžete použít výstup z aplikace rozhraní API pravidla za pro rozhodování Další.

## <a name="using-rules-via-apis"></a>Použití pravidel prostřednictvím rozhraní API
Rozhraní API aplikace pravidel lze také vyvolat pomocí celá řada rozhraní API. Tento způsob, jak uživatelé nejsou omezena pouze pomocí aplikace použití logických operátorů a pomocí pravidel v libovolné aplikaci provedením ZBÝVAJÍCÍCH volání. K dispozici přesné REST API lze zobrazit kliknutím na "Rozhraní API linkové" lens v řídicím panelu pravidla.

![Alternativní text][10]

Následuje příklad možnosti, jak jeden mohly využít tento rozhraní API v C#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Všimněte si, že nahoře rozhraní API aplikace pravidla má jeho zabezpečení nastavení "Veřejné parametr Anon". Tím se dá nastavit z prostředí aplikace rozhraní API pomocí - všechna nastavení -> Nastavení aplikace -> úroveň přístupu.

![Alternativní text][11]

## <a name="editing-vocabulary-and-policy"></a>Úpravy slovník a zásad
Jednou z hlavních výhod pomocí pravidel pro firmy je, že změny obchodní logiky můžete použiti na výrobní mnohem rychlejší. Všechny změny provedené v slovník a zásady se okamžitě použije ve výrobním. Uživatel musí jednoduše přejděte do příslušné slovník definice nebo zásady a provádění změn na něm uplatnit.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
