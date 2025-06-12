---
title: "**Transcriptomatic**" 
sub_title: Evaluering av speech to text modeller
author: Anton Lundström och Robban
theme:
  override:
    footer:
      style: template
      left:
        image: eghed-logo.png
      center: Transcriptomatic
      right: "{current_slide} / {total_slides}"
      height: 3
    palette:
      classes:
        noice: # For your new "Röd Hatt" (Comic relief) or general emphasis
          foreground: red
        svart_hatt: # Original Black Hat (Critical Analysis & Risks)
          foreground: black
        gul_hatt: # Original Yellow Hat (Optimism & Benefits)
          foreground: yellow
        gron_hatt: # Original Green Hat (Creativity & New Ideas)
          foreground: green
        bla_hatt: # For your new "Blå Hatt" (Dirigenten)
          foreground: blue
        lila_hatt: # For your new "Lila Hatt" (Frågegeneratorn)
          foreground: magenta # Standard terminal purple
---


<!-- incremental_lists: true -->
Varför gör vi detta?
==============
* **Utmaningen**: Hitta bästa Tal-till-Text (STT) på Svenska.
* **Varför detta projekt?**
    * Engelska: Gott om högkvalitativa STT-modeller.
    * Svenska: Utbudet är mer begränsat – vilka fungerar bäst *här*?
    * **Vårt fokus:** Hur klarar modellerna av **längre format**? (T.ex. från möten, intervjuer, poddar).
* **Mål**: Ge en tydlig bild av hur bra olika modeller är på att transkribera vid praktisk tillämpning.

<!-- end_slide -->
<!-- incremental_lists: true -->    
Sammanfattning: KBLab KB-Whisper
==============
- KBLab har utvecklat KB-Whisper, en ny tal-till-text-modell speciellt framtagen för svenska.

    * Den bygger på OpenAIs populära Whisper-modell men har finjusterats på en massiv datamängd om 50 000 timmar svenskt tal (från bland annat TV-textning och riksdagsprotokoll).
    * Resultatet är en kraftig förbättring av prestandan för svensk taligenkänning.
    * Tester visar en genomsnittlig minskning av ordfelet (WER) med hela 47 % jämfört med OpenAIs bästa modell (Whisper-large-v3).
    * En stor fördel är att även mindre KB-Whisper-modeller presterar bättre än OpenAIs större modeller. Detta gör högkvalitativ svensk taligenkänning mer tillgänglig och kostnadseffektiv.
    * Modellerna är fritt tillgängliga via KBLab på HuggingFace.    
- TLDR: KB-Whisper är KBLabs version av Whisper, optimerad för svenska med stora mängder data, vilket ger mycket bättre och effektivare taligenkänning än OpenAIs original för svenskt ljud.

<!-- end_slide -->
<!-- incremental_lists: true -->
Projektets Kärna - Vad undersöker vi?
==============
* **Syfte:**
    * Utvärdera och jämföra olika STT-modeller för svenska, särskilt för längre tal.
    * Bidra med kunskap: Vilka modeller passar bäst för svenskt tal.
* **Nyckelfrågor vi vill besvara:**
    1.  Hur presterar olika modeller (särskilt KBLabs KB-Whisper) på längre ljudformat på svenska?
    2.  Skillnader mellan open-source och kommersiella alternativ?
    3.  Vilka är deras styrkor och svagheter för just svenskt tal?



<!-- end_slide -->
<!-- incremental_lists: true -->
Så Gjorde Vi: Metod & Avgränsningar
==============
* **Data:**
    * Poddar vi har fått tillåtense att använda.
    * Material från Riksdagen (fritt att använda).
    * All data har **manuellt transkriberats** för att ha en "facit".
    * ~ 5 timmar data.
    * Moseboken som verifieringsdata. 
* **Avgränsningar:**
    * Fokus på specifika, tillgängliga open-source och kommersiella modeller.
    * Prioriterar **långformat ljud**.
    * *Ej primärt fokus:*
        * Korta, isolerade ljudklipp.
        * Extremt breda dialektstudier.
* **Övergripande Strategi:**
    * **Kvantitativ metod:** Objektiv mätning och jämförelse.
    * Utvecklat ett **eget utvärderingsskript** för systematisk testning.


<!-- end_slide -->
<!-- incremental_lists: true -->
Evalueringsmetriker – Hur mäter vi?
==============
Här introducerar vi de mått vi använt för att bedöma modellernas prestanda.
Vi kommer gå igenom:
* Word Error Rate (WER)
* Character Error Rate (CER)
* BERTScore
* METEOR
* LLM utvärdering

<!-- end_slide -->
<!-- incremental_lists: true -->
Word Error Rate (WER) - Felprocent på Ord
==============
* **Vad det är**: Tänk dig att du jämför vad datorn hörde med vad som faktiskt sades. WER räknar hur många ord som är fel, saknas eller lagts till för mycket. Man delar detta antal fel med det totala antalet ord som skulle ha sagts.
* **Exempel:**
    * **Rätt**:
      *  "köp mjölk och bröd" (4 ord)
      * Datorn hörde: "köp filmjölk och"
    * **Fel:** 
      * "mjölk" byttes mot "filmjölk" (1 fel - byte)
      * "bröd" (saknas) (1 saknat ord)
        * 
    * WER = (Antal byten + Antal saknade + Antal extra) / Totalt antal rätta ord
* **Mål**: <span class="noice">**Lägre är bättre**</span> (0% är perfekt).



<!-- end_slide -->
<!-- incremental_lists: true -->
Character Error Rate (CER) - Felprocent på Bokstäver
==============
* **Vad det är**: Fungerar precis som WER, men räknar fel på bokstavsnivå istället för ordnivå. Hur många bokstäver är fel, saknas eller är extra? Dela med totala antalet bokstäver som borde finnas. Används ofta för språk utan tydliga ordmellanrum eller för att fånga små stavfel.
* **Exempel:**
    * **Rätt**:
      * "hej" (3 bokstäver)
      * Datorn hörde: "nej"
    * **Fel:** 
      * 'h' byttes mot 'n' (1 bokstavsfel - byte).

    * CER = (Antal bokstavsbyten + Antal saknade bokstäver + Antal extra bokstäver) / Totalt antal rätta bokstäver
    * CER = (1 + 0 + 0) / 3 ≈ 0.33 (eller 33%)
* **Mål**: <span class="noice">**Lägre är bättre**</span> (0% är perfekt).




<!-- end_slide -->
<!-- incremental_lists: true -->
BERTScore - Mäter Likhet i Betydelse
==============
* **Vad det är**: Detta mått kollar hur lika i betydelse datorns text är jämfört med den korrekta texten. Den använder smart AI (BERT) för att förstå om orden betyder ungefär samma sak, även om de inte är exakt desamma.
* **Exempel:**
    * **Rätt**:
      * "Bilen är röd"
      * Datorn hörde: "Fordonet är rött"
    * Analys: WER skulle se detta som fel eftersom "Bilen" != "Fordonet" och "röd" != "rött". Men BERTScore förstår att meningen betyder nästan exakt samma sak och ger därför ett högt betyg (nära 1).

* **Mål**: <span class="noice">**Högre är bättre**</span> (närmare 1).
* Vi har använt **bert-base-multilingual-cased** i våra test.

<!-- end_slide -->
<!-- incremental_lists: true -->
METEOR - Smartare Ordmatchning
==============
* **Vad det är**: Det här måttet är lite smartare än WER. Det försöker matcha ord som betyder samma sak (t.ex. 'bil' och 'fordon') eller är olika former av samma ord (t.ex. 'springer' och 'sprang'). Målet är att bättre matcha hur en människa skulle bedöma om texten är bra, även om exakt samma ord inte används.
* **Exempel:**
    * **Rätt**:
      * "Hunden sprang snabbt"
      * Datorn hörde: "Valpen springer fort"
    * Analys: WER ser många fel. METEOR kan matcha "Hunden" med "Valpen" (liknande betydelse), "sprang" med "springer" (samma grundord) och "snabbt" med "fort" (synonymer). Därför ger METEOR ett högre betyg än WER.
* **Mål**: <span class="noice">**Högre är bättre**</span> (närmare 1).


<!-- end_slide -->

Resultat alla modeller
==============
![image:width:100%](_interesting_models_all.png)



<!-- end_slide -->
<!-- incremental_lists: true -->
Resultat lokala modeller
==============
![image:width:100%](_local_model.png)




<!-- incremental_lists: true -->
<!-- end_slide -->

Jämnförelse mellan städad och rå data.
==============
![image:width:100%](is_it_clean.png)



<!-- end_slide -->
<!-- incremental_lists: true -->