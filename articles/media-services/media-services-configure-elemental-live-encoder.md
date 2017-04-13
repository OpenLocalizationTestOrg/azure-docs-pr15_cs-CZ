<properties 
    pageTitle="Konfigurace encoder elementární Live odeslání jednoho bitrate živou toku | Microsoft Azure" 
    description="Toto téma ukazuje, jak nakonfigurovat encoder elementární Live jediný bitrate toku odešlete AMS kanály, kteří mají povolenou živou kódování." 
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
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Odeslání jednoho bitrate živou toku pomocí encoder elementární Live

> [AZURE.SELECTOR]
- [Elementární Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Toto téma ukazuje, jak nakonfigurovat encoder [Elementární Live](http://www.elementaltechnologies.com/products/elemental-live) jednoho bitrate toku odešlete AMS kanály, kteří mají povolenou živou kódování.  Další informace najdete v tématu [práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak provádět správu přes Azure Media Services Explorer (AMSE) nástroj Azure Media Services (AMS). Tento nástroj lze spustit pouze v počítači Windows. Pokud používáte Mac a Linux, použijte portál Azure k vytváření [kanálů](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

##<a name="prerequisites"></a>Zjistit předpoklady pro

- Musí mít praxi použití elementární Live webového rozhraní k vytvoření živou události.
- [Vytvořte účet Azure Media Services](media-services-portal-create-account.md)
- Zajistit, aby se Endpoint datových proudů s nejméně jednu streamování jednotku přidělit. Další informace najdete v tématu [Správa datových proudů koncové body v účtu služby média](media-services-portal-manage-streaming-endpoints.md).
- Nainstalujte nejnovější verzi nástroj [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Spuštění nástroje a připojte se ke svému účtu AMS.

##<a name="tips"></a>Tipy

- Kdykoli je to možné, použijte standardní kabelové připojení k Internetu.
- Existuje dobré pravidlo při určování požadavků na šířku pásma má poklikáním streamování bitrates. Není to povinné požadavku, vám pomůže zmírnit dopad přetížení sítě.
- Při používání softwaru na základě kodéry, uzavřete všechny nadbytečné aplikace.

## <a name="elemental-live-with-rtp-ingest"></a>Elementární Live s RTP jedí

Tato část popisuje konfigurace encoder elementární Live, která odesílá živou toku jednoho bitrate přes RTP.  Další informace najdete v tématu [MPEG-TS toku přes RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Vytvoření kanálu

1.  V nástroji AMSE přejděte na kartu **Live** a klikněte pravým tlačítkem myši v oblasti kanálu. Vyberte **vytvořit kanál...** v nabídce.

![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Zadejte název kanálu popis pole není povinné. V části nastavení kanálu vyberte **Standardní** možnost Live kódování s protokolem vstupní hodnotu **RTP (MPEG-TS)**. Můžete ponechat všechna ostatní nastavení je.


Ujistěte se, že je vybraná možnost **Spustit nový kanál** .

3. Klikněte na **vytvořit kanál**.
![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Kanál může trvat až 20 minut po počátečním.

Během spouštění kanál, můžete [nakonfigurovat encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Všimněte si, že fakturace spustí hned, jak kanálu přejde do připravena. Další informace najdete v tématu [státy kanálu](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Konfigurace encoder elementární Live 

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


####<a name="configuration-steps"></a>Kroky konfigurace

1. Přejděte na web rozhraní **Elementární Live** a nastavte encoder **UDP/TS** datového proudu. 

2. Po vytvoření nové události přejděte dolů do výstupu skupin a přidejte skupinu **UDP/TS** výstupů. 

3. Vytvořte nový výstup výběrem **nové toku** a potom kliknutím na **Přidat výstup**.  
    
    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Je vhodné, že má elementární událost časový kód nastavena na "Systémové hodiny" pomůže encoder znovu připojit v případě selhání proudu.

4. Teď výstup byl vytvořen, klikněte na **Přidat proudu**. Nyní je možné konfigurovat nastavení výstupu. 
5. Posuňte se dolů na "Proudu 1", který jste právě vytvořili, klikněte na kartu **videa** na levé straně a rozbalte oddíl **Upřesnit** nastavení. 

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Přestože elementární Live nemá širokou škálu dostupné přizpůsobení, doporučuje se při zahájení práce s datových proudů AMS následující nastavení. 
    
    - Řešení: 1280 × 720 
    - FrameRate: 30 
    - GOP velikost: 60 snímky 
    - Prokládání režim: postupné 
    - Bitrate: 5000000 bitů/s (to lze upravit podle omezeními sítě) 
    

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Pokud potřebujete vstupní adresa URL kanálu.
    
    Přejděte zpět k nástroji AMSE a informace o stavu dokončení kanálu. Jakmile se stav se změnil z **Počáteční** **spuštění**, můžete získat vstupní adresa URL.
      
    Při spuštění kanálu klikněte pravým tlačítkem myši na název kanálu, přejděte dolů k přechodu myší **Adresu URL vstupní kopírovat do schránky** a vyberte **Primární adresu URL vstupní**.  
    
    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Vložte tyto informace do pole **Primární cíle** elementární. Všechny ostatní nastavení můžete zůstane jako výchozí.
    
    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Navíc redundance opakujte tyto kroky s vedlejší URL vstupní vytvořením na samostatné kartě nezávislé "Výstup" UDP/TS datového proudu.
    
7. Klikněte na tlačítko **vytvořit** (Pokud byla vytvořena nová zvláštní událost) nebo **Aktualizovat** (Pokud upravujete stávajících událostí) a potom přejděte k zahájení encoder. 

>[AZURE.IMPORTANT]Než kliknete na tlačítko **Start** na webového rozhraní elementární Live, **musíte** zajistěte, aby byl kanálu připravení. 
>Navíc nezapomeňte není ponechat kanál připravena bez události po dobu delší než 15 minut >.

Po spuštění proudu pro 30 sekund, než přejdete zpět AMSE nástroj a otestujte přehrávání.  

###<a name="test-playback"></a>Test přehrávání
  
1. Přejděte na nástroj AMSE, a klikněte pravým tlačítkem na kanál otestuje. V nabídce najeďte myší na **přehrávání náhled** a vyberte **Azure Media Player**.  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Pokud proudu se zobrazí v přehrávači, potom encoder správně nakonfigurované pro připojení k AMS. 

Pokud dostali k chybě kanálu bude potřeba obnovit a kódovací nastavení upravit. Najdete v tématu [Poradce při potížích s](media-services-troubleshooting-live-streaming.md) pokyny.   

###<a name="create-a-program"></a>Vytvořit program

1. Jakmile je potvrzena přehrávání kanál, vytvořte program. Na kartě **Live** v nástroji AMSE klikněte pravým tlačítkem myši do oblasti program a vyberte **Vytvořit nový Program**.  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Zadejte název aplikace a v případě potřeby upravte **Délka okno archiv** , (které 4 hodiny ve výchozím nastavení). Můžete taky určit umístění pro uložení nebo její opuštění jako výchozí.  
3. Zaškrtněte políčko **Spustit Program nyní** .
4. Klikněte na **vytvořit Program**.  
  
    Poznámka: Vytvoření aplikace zkracuje než vytváření kanálů.    
 
5. Po spuštění programu potvrďte přehrávání kliknutí pravým tlačítkem myši program a přechod ke **přehrávání program (programy)** a následným výběrem **s Azure Media Player**.  
6. Jakmile pole potvrzeno, klikněte pravým tlačítkem aplikaci znova a vyberte **Kopírovat adresu URL výstup do schránky** (nebo načíst tyto informace z **informace o programu nastavení** možnosti v nabídce). 

Proudu je nyní připravena k vložené do přehrávač nebo úměrně cílové skupiny pro živá zobrazení.  

## <a name="troubleshooting"></a>Řešení potíží

Najdete v tématu [Poradce při potížích s](media-services-troubleshooting-live-streaming.md) pokyny. 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
