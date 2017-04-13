<properties 
    pageTitle="Konfigurace encoder Telestream Wirecast odeslání jednoho bitrate živou toku | Microsoft Azure" 
    description="Toto téma ukazuje, jak nakonfigurovat živou encoder Wirecast toku jednoho bitrate odešlete AMS kanály, kteří mají povolenou živou kódování. " 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Odeslání jednoho bitrate živou toku pomocí Wirecast encoder

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Elementární Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Toto téma ukazuje, jak nakonfigurovat živé encoder [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) toku jednoho bitrate odešlete AMS kanály, kteří mají povolenou živé kódování.  Další informace najdete v tématu [práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak provádět správu přes Azure Media Services Explorer (AMSE) nástroj Azure Media Services (AMS). Tento nástroj lze spustit pouze v počítači Windows. Pokud používáte Mac a Linux, použijte portál Azure k vytváření [kanálů](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).


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

1.  V nástroji AMSE přejděte na kartu **Live** a klikněte pravým tlačítkem myši v oblasti kanálu. Vyberte **vytvořit kanál …** v nabídce.

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Zadejte název kanálu popis pole není povinné. V části nastavení kanálu vyberte **Standardní** možnost Live kódování s protokolem vstupní hodnotu **RTMP**. Můžete ponechat všechna ostatní nastavení je.


Ujistěte se, že je vybraná možnost **Spustit nový kanál** .

3. Klikněte na **vytvořit kanál**.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] Kanál může trvat až 20 minut po počátečním.

Během spouštění kanál, můžete [nakonfigurovat encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Všimněte si, že fakturace spustí hned, jak kanálu přejde do připravena. Další informace najdete v tématu [státy kanálu](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_wirecast_rtmp></a>Konfigurace Telestream Wirecast encoder

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

1. Otevřete aplikaci Telestream Wirecast ve počítače probíhá použít a nastavit pro přenos RTMP.
2. Výstup nakonfigurujte tak, že přejdete na kartu **výstup** výběr **... Nastavení výstupu**.
    
    Ujistěte se, že **Cílový výstup** je nastavený na **Serveru RTMP**.
3. Klikněte na **OK**.
4. Na stránce nastavení nastavte pole **cíl** být **Azure Media Services**.
 
    Profil kódování je předem vybraných **Azure H.264 720 p 16:9 (1280 × 720)**. Přizpůsobení nastavení, vyberte ikonu ozubeného kola vpravo od rozevíracího seznamu a klikněte na **Nové přednastavené**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Konfigurace encoder předvolby.

    Pojmenujte položku předvolba a klikněte pro následující doporučené nastavení:

    **Video**
    
    - Encoder: MainConcept H.264
    - Snímky za druhé: 30
    - Přenosová průměrná rychlost: 5000 kbits/sec (lze upravit podle omezeními sítě)
    - Profil: hlavní
    - Klíčový snímek každé: 60 snímků

    **Zvuk**

    - Cílová přenosová rychlost: 192 kbits/sec
    - Ukázka sazba: 44 100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Stisknutím klávesy **Uložit**.

    Pole kódování teď bude k dispozici pro výběr nově vytvořený profilu. 

    Ujistěte se, že je vybraná možnost Nový profil.

7. Potřebujete vstupní adresa URL kanálu Pokud chcete ji přiřadit Wirecast **RTMP koncového bodu**.
    
    Přejděte zpět k nástroji AMSE a informace o stavu dokončení kanálu. Jakmile se stav se změnil z **Počáteční** **spuštění**, můžete získat vstupní adresa URL.
      
    Při spuštění kanálu klikněte pravým tlačítkem myši na název kanálu, přejděte dolů k přechodu myší **Adresu URL vstupní kopírovat do schránky** a vyberte **Primární adresu URL vstupní**.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. V okně Wirecast **Nastavení výstupní** vložte tyto informace do pole **adresa** oddílu výstup a přiřadit název datového proudu. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Klikněte na **OK**.

10. Na hlavní obrazovce **Wirecast** potvrďte vstupní zdroje pro video a zvuk připraveni a poté stiskněte **toku** v levém horním rohu.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]Než kliknete na možnost **toku** **musí** zajistěte, aby byl kanálu připravení. 
>Navíc nezapomeňte není ponechat kanál připravena bez vstupní příspěvek informačního kanálu po dobu delší než 15 minut >.

##<a name="test-playback"></a>Test přehrávání
  
1. Přejděte na nástroj AMSE, a klikněte pravým tlačítkem na kanál otestuje. V nabídce najeďte myší na **přehrávání náhled** a vyberte **Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Pokud proudu se zobrazí v přehrávači, potom encoder správně nakonfigurované pro připojení k AMS. 

Pokud dostali k chybě kanálu bude potřeba obnovit a kódovací nastavení upravit. Najdete v tématu [Poradce při potížích s](media-services-troubleshooting-live-streaming.md) pokyny.  

##<a name="create-a-program"></a>Vytvořit program

1. Jakmile je potvrzena přehrávání kanál, vytvořte program. Na kartě **Live** v nástroji AMSE klikněte pravým tlačítkem myši do oblasti program a vyberte **Vytvořit nový Program**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

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
