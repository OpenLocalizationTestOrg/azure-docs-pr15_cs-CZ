<properties
    pageTitle="Zahrnout jednotky kurz míč"
    description="Postup vytvoření klasické jednotky vrátit míč hra, což je předpokladem pro všechny kurzy Engagement jednotky Mobile"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Vytvoření jednotky vrátit hry míč

Tento kurz provede hlavní kroky mírně změnila [jednotky zahrnout míč kurz](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Tato ukázka hra se skládá z kulovým "přehrávače" objekt, který řídí uživatelem aplikace a cílem herní je "shromažďovat" collectible objekty kolize přehrávače objekt s tyto collectible objekty. Předpokladem základní znalost jednotky editor prostředí. Pokud narazíte na problémy pak se seznamte s celou kurz. 

### <a name="setting-up-the-game"></a>Nastavení hry
Následující kroky lze z [kurz jednotky](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Otevřete **Editor jednotky** a klikněte na **Nový**. 
    
    ![][51] 
    
2. Zadejte **název projektu** & **umístění**, vyberte **3D** a klikněte na **vytvořit projekt**.
    
    ![][52]

3. Uložení výchozí scény vytvořený jako součást nový projekt s názvem **MiniGame** v rámci nového ** \_scény** složky pod složkou **prostředky** :
    
    ![][53]

4. Vytvoření **3D objektu -> rovině** jako pole přehrávání a přejmenujte rovině objekt jako **základu**

    ![][1]

5. Obnovte transformace komponenty pro tento objekt **základu** tak, aby byla na počátek. 

    ![][3]

6. Zrušte zaškrtnutí políčka **Zobrazit mřížku** v **nabídce si** objektu **základu** .

    ![][4]

7. Aktualizujte součást **Měřítko** pro objekt **základu** [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Přidejte nového **3D objektu -> koule** do projektu a přejmenujte koule objekt jako **Player**. 

    ![][6]

9. Vyberte objekt **přehrávače** a klepněte na tlačítko **Obnovit transformaci** podobně jako na objekt rovině. 

10. Aktualizace **Transformace -> pozici -> Souřadnice Y** komponenty y přehrávače jako 0,5.  

    ![][7]

11. Vytvořte novou složku s názvem **materiálů** v projektu, kde vytvoříme materiály pro změnu barvy přehrávač. 

12. Vytvoření nového **materiál** s názvem **pozadí** v této složce. 

    ![][8]

13. Aktualizujte barvy materiál aktualizací vlastnost **Albedo** od něj. Můžete vybrat hodnotu RGB [0,32,64]. 

    ![][9]

14. Přetáhněte tento materiál do zobrazení scény chcete použít barvu pro objekt **základu** . 

    ![][10]

17. Nakonec aktualizovat **Transformace -> otočení -> Y** 60 objektu směrová Light pro projekty. 

    ![][12]

### <a name="moving-the-player"></a>Přesunutí přehrávač
Následující kroky lze z [kurz jednotky](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Přidání součásti **RigidBody** k objektu **Player** . 

    ![][13]

2. Vytvoření nové složky s názvem **skriptů** v projektu. 

3. Klikněte na **Přidat součást -> nový skript -> C# skript**. Pojmenujte ho **PlayerController**a klikněte na **vytvořit a přidat**. Vytvoří a přiřadit skript k objektu Player.  

    ![][14]

5. Přesunutí tento skript ve složce **skriptů** v projektu. 

6. Otevřete skriptu pro úpravy v oblíbených script editor, aktualizujte kód skriptu následující kód a uložte jej. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Všimněte si, že nahoře skript používá vlastnost **rychlost** . V editoru jednotky aktualizované vlastnosti rychlosti na 10.  

    ![][15]

9. Přístupů **přehrávat** v editoru jednotky. Teď by měla budete moci určit míč pomocí klávesnice a měli otočit a pohyb. 

### <a name="moving-the-camera"></a>Přesunutí fotoaparátu
Kroků jsou z [kurz jednotky](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) a bude nevážou **Hlavní kameru, pokud** k objektu **Player** . 

1. Aktualizace **Transform.Position** být X = 0, Y = 10.5, Z =-10.  
2. Aktualizace **Transform.Rotation** být X = 45, Y = 0, Z = 0.  

    ![][16]

2. Přidejte nový skript s názvem **CameraController** **MainCamera** a přesunout na jiné místo ve složce skriptů. 

    ![][17]

3. Otevřete skriptu pro úpravy a přidat následující kód v něm:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. V prostředí jednotky – přetažením proměnnou přehrávače do úsek přehrávače objektu hlavní kamery dvou jsou přidružené k sobě navzájem. 

    ![][18]

6. Teď Pokud přístupů přehrávat v editoru jednotky a otočení objektu přehrávače míč pak uvidíte fotoaparátu v pohyb.  

### <a name="setting-up-the-play-area"></a>Nastavení oblasti pro přehrávání
Následující kroky lze z [kurz jednotky](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Vytvoříme okrajů kolem základu tak, aby objekt přehrávače míč nemá odkládací oblasti přehrávat v pohyb. 

1. Klikněte na **Vytvořit -> vytvořit prázdnou -> objekt hra** a nazvěte ji **zdi**

    ![][19]

2. V části tento objekt zdi – vytvoření nového **3D objektu -> datové krychle** a nazvěte ji "Západní zdi". 

    ![][20]

3. Aktualizace **Transformace -> umístění** a **Transformace -> měřítko** pro tento objekt západní zdi. 

    ![][21]

4. Duplikování zdi západní vytvořit **východ zdi** s aktualizovanou transformace polohu a velikost. 

    ![][22]

5. Duplikování zdi východ vytvořit **severní zdi** s aktualizovanou transformace pozice a měřítko. 

    ![][23]

6. Duplikování zdi Severní a vytvořte **jih zdi** s aktualizovanou transformace pozice a měřítko. 

    ![][24]

### <a name="creating-collectible-objects"></a>Vytváření Collectible objektů
Následující kroky lze z [kurz jednotky](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Vytvoříme některé atraktivní vypadající objekty, které tvoří skupiny collectible objektů, které přehrávače míč objekt je potřeba shromáždit tak, že kolize s nimi. 

1. Vytvoření nového **3D datové krychle objektu** a nazvěte ji odběru. 

2. Úprava **Transformace -> otočení** & **Transformace -> měřítko** výdeje objektu. 

    ![][25]

3. Vytvořte a připojte **Novou C# skript** s názvem **Rotator** výdeje objekt. Zkontrolujte, že umístění skript ve složce skriptů. 

    ![][26]

4. Otevřete tento skript pro úpravy a aktualizace mít takto: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Teď přístupů režimu přehrávání v editoru jednotky a prezentaci vyskladnění objekt otočení na ose.

6. Vytvoření nové složky s názvem **Prefabs** 

    ![][27]

7. **Odběru** objekt a přetáhněte ji do složky Prefabs.

    ![][28]

8. Vytvoření nového **prázdného hra objekt** s názvem **Pickups**. Obnovte polohy origin a potom přetáhněte vyskladnění objekt v rámci tohoto herní objektu.  

    ![][29]

9. Duplikování objekt **odběru** a šířit na požadovaném objektu **základu** kolem objektu **přehrávače** aktualizací hodnoty **X a Z jeho Transform.Position** řádně podporovat. 

    ![][30]

10. Vytvoření **nového materiálu** s názvem **odběru** a aktualizujte být červenou barvou aktualizací **Albedo vlastnost** podobný jsme akce pro aktualizaci objekt základu. 

    ![][31]

11. Použití materiálu 4 výdeje objekty.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Shromažďování vyskladnění objektů
Následující kroky lze z [kurz jednotky](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Přehrávač budeme aktualizovat tak, aby bylo možné na "shromažďovat" výdeje objekty kolize s nimi. 

1. Otevření skriptu **PlayerController** připojené k objektu přehrávače pro úpravy a aktualizace následujícím způsobem:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Vytvořit novou **značku** s názvem **Vyberte nahoru** (se musí shodovat co je v skript)  

    ![][33]
    
    ![][34]

3. Použijte tuto **značku** k objektu Prefab odběru. 

    ![][35]

4. Zaškrtávací políčko **IsTrigger** objektu Prefab povolte.

    ![][36]

5. Přidání pevné textu k odběru Prefab objektu. Pro optimalizaci výkonu aktualizujeme statické collider, který jsme použili k dynamických collider. 

    ![][37]
  
6. Nakonec zkontrolujte vlastnost **IsKinematic** prefab objektu. 

    ![][38]

7. Přístupů **přehrávat** v editoru jednotky a budou moct přehrát této **vrátit koule** hry přesunutím objekt přehrávače pomocí klávesové zkratky pro vstupní směr. 

### <a name="updating-the-game-for-mobile-play"></a>Aktualizace hry pro mobilní přehrávání
Výše uvedených v částech uzavřené základní kurz z jednotky. Teď můžeme změní hra snažíme usnadnit jeho mobilního zařízení popisný. Poznámka: jsme použili zadávání klávesnice hry zatím pro účely testování. Teď můžeme upravovat ji tak, aby jsme ovládání Přehrávač tedy pomocí pohybu telefonu použití zrychlení jako vstup. 

Otevřete **PlayerController** skriptu pro úpravy a upravte metodu **FixedUpdate** přesouvat se pomocí pohybu z zrychlení objekt Player. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Tento kurz uzavře základní herní vytvoření pomocí jednotky a to nástroje můžete nasazovat na zařízení podle svého výběru hry. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
