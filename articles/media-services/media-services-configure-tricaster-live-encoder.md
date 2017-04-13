<properties 
    pageTitle="Konfigurace encoder NewTek TriCaster odeslání jednoho bitrate živou toku | Microsoft Azure" 
    description="Toto téma ukazuje, jak nakonfigurovat živou encoder Tricaster toku jediný bitrate odešlete AMS kanály, kteří mají povolenou živou kódování." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako;cenkd;anilmur"/>

#<a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Odeslání jednoho bitrate živou toku pomocí NewTek TriCaster encoder

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Elementární Live](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Toto téma ukazuje, jak nakonfigurovat živou encoder [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) toku jednoho bitrate odešlete AMS kanály, kteří mají povolenou živou kódování. Další informace najdete v tématu [práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak provádět správu přes Azure Media Services Explorer (AMSE) nástroj Azure Media Services (AMS). Tento nástroj lze spustit pouze v počítači Windows. Pokud používáte Mac a Linux, použijte portál Azure k vytváření [kanálů](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

>[AZURE.NOTE]Při použití Tricaster pro odeslání do příspěvku informačního kanálu AMS kanály, kteří mají povolenou živou kódování, může být poruch video nebo zvuk v živé události Pokud používáte některé funkce Tricaster, jako je rychlé vyjmutí mezi kanálů nebo přechod z potřeby. Tým AMS pracuje na řešení těchto problémů do té doby, ho není doporučit použití těchto funkcí.


##<a name="prerequisites"></a>Zjistit předpoklady pro

- [Vytvořte účet Azure Media Services](media-services-portal-create-account.md)
- Zajistit, aby se Endpoint datových proudů s nejméně jednu streamování jednotku přidělit. Další informace najdete v tématu [Správa datových proudů koncové body v účtu služby médií](media-services-portal-manage-streaming-endpoints.md)
- Nainstalujte nejnovější verzi nástroj [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Spuštění nástroje a připojte se ke svému účtu AMS.

##<a name="tips"></a>Tipy

- Kdykoli je to možné, použijte standardní kabelové připojení k Internetu.
- Existuje dobré pravidlo při určování požadavků na šířku pásma má poklikáním streamování bitrates. Není to povinné požadavku, vám pomůže zmírnit dopad přetížení sítě.
- Při používání softwaru na základě kodéry, uzavřete všechny nadbytečné aplikace.

## <a name="create-a-channel"></a>Vytvoření kanálu

1.  V nástroji AMSE přejděte na kartu **Live** a klikněte pravým tlačítkem myši v oblasti kanálu. Vyberte **vytvořit kanál...** v nabídce.

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Zadejte název kanálu popis pole není povinné. V části nastavení kanálu vyberte **Standardní** možnost Live kódování s protokolem vstupní hodnotu **RTMP**. Můžete ponechat všechna ostatní nastavení je.


Ujistěte se, že je vybraná možnost **Spustit nový kanál** .

3. Klikněte na **vytvořit kanál**.
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] Kanál může trvat až 20 minut po počátečním.


Během spouštění kanál, můžete [nakonfigurovat encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

>[AZURE.IMPORTANT] Všimněte si, že fakturace spustí hned, jak kanálu přejde do připravena. Další informace najdete v tématu [státy kanálu](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_tricaster_rtmp></a>Konfigurace NewTek TriCaster encoder

V tomto kurzu se používají následující nastavení výstupu. Zbytek Tato část popisuje konfigurační kroky podrobněji. 

**Video**:
 
- Kodek: H.264 
- Profil: Vysoký (úroveň 4.0) 
- Bitrate: 5000 kB / 
- Klíčový: 2 sekund (60 sekund) 
- Snímek sazba: 30
 
**Zvuku**:

- Kodek: AAC (LC) 
- Bitrate: 192 kb / 
- Ukázkové sazba: 44,1 kHz


###<a name="configuration-steps"></a>Kroky konfigurace

1. Vytvoření nového projektu **NewTek TriCaster** podle toho, jaké videa vstupní zdroj používá. 
2. Jakmile v rámci projektu, najít tlačítko **toku** a klikněte na ikonu ozubeného kola vedle ní přístup k nabídce konfigurace toku.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Jakmile v nabídce otevře, klikněte na **Nový** pod záhlavím připojení. Po zobrazení výzvy pro tento typ připojení, vyberte **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Klikněte na **OK**.

5. Profil aplikace FMLE nyní lze tak, že kliknete na šipku rozevíracího seznamu pod položkou **Profil datových proudů** a přechod na **Procházet**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. Přejděte na uložení nakonfigurovaný FMLE profil.
7. Vyberte ho a klikněte na tlačítko **OK**.

    Po nahrání profilu, pokračujte dalším krokem.

6. Potřebujete vstupní adresa URL kanálu Pokud chcete ji přiřadit Tricaster **RTMP koncového bodu**.
    
    Přejděte zpět k nástroji AMSE a informace o stavu dokončení kanálu. Jakmile se stav se změnil z **Počáteční** **spuštění**, můžete získat vstupní adresa URL.
      
    Při spuštění kanálu klikněte pravým tlačítkem myši na název kanálu, přejděte dolů k přechodu myší **Adresu URL vstupní kopírovat do schránky** a vyberte **Primární adresu URL vstupní**.  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. Vložte tyto informace do pole **umístění** v části **Flash Server** v rámci projektu Tricaster. Přiřadíte také toku název do pole **ID proudu** . 

    Pokud toku informací bylo přidáno do profilu FMLE, ji můžete taky importovat do této části tak, že kliknete na **Nastavení importu**, přejdete do uložené FMLE profilu kliknutím na tlačítko **OK**. Do příslušných polí serveru Flash by měl naplnění s informacemi z FMLE.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. Až budete hotovi, klikněte na **OK** v dolní části obrazovky. Jakmile video a zvuk vstupů do Tricaster připraveni, začněte datových proudů AMS kliknutím na tlačítko **proudu** .

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT]Než kliknete na možnost **toku** **musí** zajistěte, aby byl kanálu připravení. 
>Navíc nezapomeňte není ponechat kanál připravena bez vstupní příspěvek informačního kanálu po dobu delší než 15 minut >. 

##<a name="test-playback"></a>Test přehrávání
  
1. Přejděte na nástroj AMSE, a klikněte pravým tlačítkem na kanál otestuje. V nabídce najeďte myší na **přehrávání náhled** a vyberte **Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Pokud proudu se zobrazí v přehrávači, potom encoder správně nakonfigurované pro připojení k AMS. 

Pokud dostali k chybě kanálu bude potřeba obnovit a kódovací nastavení upravit. Najdete v tématu [Poradce při potížích s](media-services-troubleshooting-live-streaming.md) pokyny.  

##<a name="create-a-program"></a>Vytvořit program

1. Jakmile je potvrzena přehrávání kanál, vytvořte program. Na kartě **Live** v nástroji AMSE klikněte pravým tlačítkem myši do oblasti program a vyberte **Vytvořit nový Program**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. Zadejte název aplikace a v případě potřeby upravte **Délka okno archiv** , (které 4 hodiny ve výchozím nastavení). Můžete taky určit umístění pro uložení nebo její opuštění jako výchozí.  
3. Zaškrtněte políčko **Spustit Program nyní** .
4. Klikněte na **vytvořit Program**.  
  
    Poznámka: Vytvoření aplikace zkracuje než vytváření kanálů.    
 
5. Po spuštění programu potvrďte přehrávání kliknutí pravým tlačítkem myši program a přechod ke **přehrávání program (programy)** a následným výběrem **s Azure Media Player**.  
6. Jakmile pole potvrzeno, klikněte pravým tlačítkem aplikaci znova a vyberte **Kopírovat adresu URL výstup do schránky** (nebo načíst tyto informace z **informace o programu nastavení** možnosti v nabídce). 

Proudu je nyní připravena k vložené do přehrávač nebo úměrně cílové skupiny pro živá zobrazení.  


## <a name="troubleshooting"></a>Řešení potíží

Najdete v tématu [Poradce při potížích s](media-services-troubleshooting-live-streaming.md) pokyny. 


##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
