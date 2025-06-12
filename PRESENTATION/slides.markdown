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

Tidig ASR och den statistiska eran (Automatic Speech Recognition)
==============
*  <span class="bla_hatt">**De första stegen**</span> **(1950-tal – tidigt 1970-tal): Banbrytande system**
    * **1952:** Bell Labs "Audrey" – Kände igen talade <span class="noice">**siffror**</span> från en enskild talare.
    * **1962:** IBM:s "Shoebox" – Kände igen <span class="noice">**16 engelska ord**</span> och utförde enkel aritmetik.


* <span class="bla_hatt">**Den statistiska eran**</span> **(1970-tal – 2000-tal): HMM och GMM dominerar**
    * **1970-talet:** DARPA:s SUR-program återupplivade forskningen, siktade på <span class="noice">**1000-ords**</span> vokabulärer.
    * <span class="gron_hatt">**Hidden Markov Models**</span>(HMM): Blev den dominerande metoden. En probabilistisk modell som ser tal som en sekvens av dolda fonetiska enheter.
    * <span class="gron_hatt">**Gaussian Mixture Models**</span>(GMM): Användes med HMM för att modellera distributionen av akustiska särdrag.
    * <span class="gron_hatt">**N-gram**</span> språkmodeller:** Kombinerades med HMM-GMM för att förutsäga sannolikheten för ordsekvenser.
    * **Mitten av 1980-talet:** IBM:s <span class="gron_hatt">**"Tangora"**</span> – Röststyrd skrivmaskin med <span class="noice">**20 000-ords**</span> vokabulär.
    * **1993:** <span class="gron_hatt">**CMU:s Sphinx-II**</span> – Första talaroberoende systemet för kontinuerligt tal med stort ordförråd (LVCSR).
    * **Kännetecken:** Datadrivet, statistiskt angreppssätt, bättre hantering av kontinuerligt tal och större vokabulärer.
*  <span class="bla_hatt">**Djupinlärningens intåg**</span> **(sent 1980-tal – tidigt 2000-tal): Hybridmodeller**
    * **Hybrid DNN-HMM-system:** <span class="gron_hatt">**Deep Neural Networks**</span> (DNN) ersatte GMM för akustisk modellering, vilket förbättrade differentieringen av fonem. HMM bibehölls för temporal modellering.

<!-- end_slide -->
<!-- incremental_lists: true -->
Revolutionen fortsätter: End-to-End-arkitekturer och språkmodellernas intåg
==============
* <span class="bla_hatt">**Mot End-to-End-paradigm**</span> **(tidigt 2000-tal – mitten av 2010-talet):**
    * <span class="gron_hatt">**Connectionist Temporal Classification**</span> (CTC) (2006): Möjliggjorde direkt träning av RNN:er på osegmenterad sekvensdata genom att använda en "blank" symbol och summera över alla möjliga anpassningar. Eliminerade behovet av förjusterad data.
    * <span class="gron_hatt">**Recurrent Neural Networks**</span> (RNN) & LSTM: Förenklade ASR genom att direkt mappa tal till text och hanterade temporala beroenden bättre än tidigare DNN:er. LSTM löste problem med försvinnande/exploderande gradienter.
* <span class="bla_hatt">**End-to-End**</span> **tar över (mitten av 2010-talet – nutid): Nya arkitekturer**
    * <span class="gron_hatt">**Listen, Attend, and Spell**</span> (LAS) (ca 2015-2016): Encoder (Listener) + Uppmärksamhetsbaserad Decoder (Speller). Uppmärksamhetsmekanismen lät dekodern fokusera på relevanta ljudsegment.
    * <span class="gron_hatt">**Transformer-arkitekturen**</span>(ca 2017-nutid): Förlitar sig helt på självuppmärksamhetsmekanismer för att modellera globala beroenden. Mycket parallelliserbar och utmärkt för lång kontext.
    * <span class="gron_hatt">**Recurrent Neural Network Transducer**</span> (RNN-T): Naturligt lämpad för strömmande ASR tack vare monoton, ramvis bearbetning.
    * **Kännetecken:** Förenklade ASR-pipelines (ett enda neuralt nätverk), betydande noggrannhetsförbättringar.
* <span class="bla_hatt">**Stora Språkmodeller**</span> **(LLM) gör entré (sent 2010-tal – nutid): Förbättrad språkförståelse**
    * **Motivering:** Övervinna språkmodelleringsflaskhalsar, utnyttja LLM:ers världskunskap och kontextuella förståelse.
    * **Tidiga integrationsstrategier:**
        * <span class="gron_hatt">**N-bästa omvärdering**</span> (Rescoring): Förtränade LLM:er (BERT, tidiga GPT:er) rangordnar hypoteser från ASR-system.
        * **Efterbearbetning & felkorrigering:** LLM:er "rensar" eller "översätter" ASR-utdata.
    * **Resultat:** Förbättrad hantering av sällsynta ord, kontext, tvetydigheter (t.ex. homofoner) och felkorrigering.



<!-- end_slide -->
<!-- incremental_lists: true -->
Den moderna ASR-eran: LLM-synergi, utmaningar och visioner
=================================================================
* <span class="bla_hatt">**Djupintegration av LLM**</span> **(tidigt 2020-tal – nutid):**
    * <span class="gron_hatt">**Textbaserad:**</span> LLM bearbetar text från ASR (t.ex. generative error correction - GER).
    * <span class="gron_hatt">**Latent representationsbaserad:**</span> Talrepresentationer från en encoder anpassas och matas till en LLM.
    * <span class="gron_hatt">**Ljud-token-baserad:**</span> Tal konverteras till diskreta semantiska eller akustiska "tokens" som LLM:er bearbetar.
    * **Påverkan:** Ytterligare noggrannhetsförbättringar, särskilt i komplexa lingvistiska sammanhang. Förbättrad igenkänning av sällsynta ord och motståndskraft mot brus.
* <span class="bla_hatt">**Aktuella Utmaningar**</span>:
    * **Beräkningseffektivitet:** Särskilt för realtidsanvändning på enheter med begränsade resurser.
    * **Bias och rättvisa:** Hantera och mildra bias i träningsdata och modeller.
    * **Lågresursspråk:** Utveckla högpresterande modeller för språk med begränsad träningsdata.
    * **Robusthet:** Förbättra prestanda i bullriga miljöer och med varierande talstilar.
* <span class="bla_hatt">**Framtida Riktningar och Visioner**</span>:
    * **Optimala integrationsstrategier:** Fortsatt forskning kring hur tal- och språkkomponenter bäst kombineras.
    * **Parameter-Efficient Fine-Tuning (PEFT):** Effektiva metoder (t.ex. LoRA) för att anpassa stora LLM:er.
    * **Tillförlitlighet:** Minska hallucinationer och öka modellernas pålitlighet.
    * **Multimodal förståelse:** System som kan bearbeta och förstå information från flera källor (t.ex. ljud och video).
    * **Konversationell AI:** Mer naturliga och kontextmedvetna dialogsystem.


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


Del 2 Mötesassistent
==============
![image:width:100%](agent3.png)



<!-- end_slide -->
<!-- incremental_lists: true -->
Introduktion Mötesassistent - Utmaningen
==============
* **Problemet:**
    * Fånga och förstå talad information i realtid är svårt (möten, föreläsningar).
    * Manuell transkribering och analys är tidskrävande.
    * Extrahera olika insikter (fakta, risker, idéer, känslor) kräver olika perspektiv.
* **Vår Lösning: Mötesassistenten**
    * System som fångar ljud, transkriberar live.
    * Använder intelligenta **agenter** för mångfacetterad analys.
    * Ger återkoppling direkt till användaren (t.ex. relevanta frågor, input i realtid).





<!-- end_slide -->
<!-- incremental_lists: true -->
Hur Det Fungerar - Flödet
==============
![image:width:100%](flow.png)



<!-- end_slide -->
<!-- incremental_lists: true -->
Agentsystemet - Kärnan i Mötesassistenten
==============
* **Koncept:** Multi-agentsystem där varje agent har distinkt "roll" och analytiskt fokus.
    * Inspirerat av "Six thinking hats".
* **Orkestrering (`agent_runner.py`):**
    * `set_active_meeting(meeting_id)`: Anger aktivt möte.
    * `agent_scheduler()`: Bakgrundstråd som periodiskt triggar agentkörning.
    * `run_agent_suite(meeting_id, fetch_mode)`:
        * Hämtar LLM-konfiguration.
        * Hämtar nya/fullständiga transkriptioner.
        * Initierar och kör varje agent.
* **Verktyg (`agent_utils.py`):**
    * `create_azure_model()`: Konfigurerar LLM-anslutning (**Azure OpenAI / AI Foundry**).
    * `get_transcription_context()`: Ger transkriptionsdata till agenter.
    * `save_agent_output_to_db()`: Sparar agentresultat.





<!-- end_slide -->
<!-- incremental_lists: true -->
Six Thinking Hats - Anpassad Metodik
==============
### Ursprunglig Metod (Edward de Bono)
Förbättrar tänkande i möten genom sex perspektiv:
* **Vit hatt**: Fakta & information.
* <span class="noice">**Röd hatt**</span>: Känslor & intuition.
* <span class="svart_hatt">**Svart hatt**</span>: Kritisk analys & risker.
* <span class="gul_hatt">**Gul hatt**</span>: Optimism & fördelar.
* <span class="gron_hatt">**Grön hatt**</span>: Kreativitet & nya idéer.
* <span class="bla_hatt">**Blå hatt**</span>: Översikt & processhantering. 
### Vår Anpassning för LLM-agenter
Initiala tester visade att Röd och Blå hatt var svåra för LLMs. Vi designade om och la till:
* <span class="noice">**Röd hatt (Ny):** Comic relief</span> - Skapa (försök till) skämt.
* <span class="bla_hatt">**Blå hatt (Ny):** Dirigenten</span> - Bedöm relevans/kvalitet på andra agenters output.
* <span class="lila_hatt">**Lila hatt (Ny):** Frågegeneratorn</span> - Ställ relevanta uppföljningsfrågor.
* **Miro (Under utveckling):** Skapa Miro-objekt.

<!-- end_slide -->
<!-- incremental_lists: true -->
Exempel på en agent
==============
**DESCRIPTION** = """Agent Green: Expert på kreativ katalysering, specialiserad på att generera innovativa idéer och alternativa lösningar baserade på mötesprotokoll. Rollen är att utforska nya perspektiv, brainstorma kreativa tillvägagångssätt, föreslå okonventionella lösningar och stimulera nytänkande kring de diskuterade ämnena."""

**INSTRUCTIONS** = """
Läs det tillhandahållna mötesprotokollet noggrant.

Granska mötesprotokollet noggrant. 

Identifiera nyckelproblem, mål och idéer från mötet.

Brainstorma nya idéer:

Alternativa lösningar som inte nämndes.

Kreativa sätt att tackla utmaningar, inspirerade 
av risker/svagheter.

Okonventionella vägar till målen.

Skapa "Tänk om...?"-scenarier för bredare perspektiv.

Formulera två öppna frågor för att utforska idéerna vidare.
"""



<!-- end_slide -->
<!-- incremental_lists: true -->
Exempel på kod till agent
==============
```python
class GreenHatAgent:
    def __init__(self, llm_config: dict, meeting_id: int = None): 
        model = create_azure_model(llm_config)
        context = get_transcription_context(meeting_id) if meeting_id else
        "No meeting context provided."
        self._agent = Agent(
            model=model,
            description=DESCRIPTION,
            instructions=INSTRUCTIONS,
            additional_context=context,
            markdown=True  # Ensure markdown formatting is enabled
        )
    def run(self, user_input: str, meeting_id: int = None):
        response = self._agent.run(user_input=user_input)
        if hasattr(response, 'content'):
            output_text = response.content
        else:
            output_text = response        
        if meeting_id is not None:
            from .agent_utils import save_agent_output
            save_agent_output(meeting_id=meeting_id, agent_id=4, agent_name="Green Hat", output_text=output_text)            
        return output_text

```

<!-- end_slide -->
<!-- incremental_lists: true -->
Insikt med agentsystem
==============
![image:width:100%](prompt3.png)


<!-- end_slide -->
<!-- incremental_lists: true -->
Teknisk Stack 
==============

* **Frontend:** **React** (JavaScript/JSX, HTML, CSS), **Web Audio API** för ljudupptagning, **WebSockets** för realtidskommunikation.  
* **Backend:** **Python** med **FastAPI** som webbramverk.  
* **Transkriberingsmotor:** **KBLab Whisper-modeller** (t.ex. KBLab/kb-whisper-small, KBLab/kb-whisper-base) via Hugging Face transformers och **PyTorch**.  
* **Språkmodeller (LLM):** **Azure OpenAI** (t.ex. GPT-4o-mini) och **Azure AI Foundry** (t.ex. DeepSeek-modeller) via agno-biblioteket (konfigurerat i constants.py).  
* **Databas:** **SQLite** via **SQLAlchemy** .  


<!-- end_slide -->
<!-- incremental_lists: true -->
Lärdomar
==============
* Databaser är svårt.  

* Multi-agentsystem är komplicerat.  

* Manuellt transkribera är tråkigt.  

* KBLab visar att man kan komma långt men en specialiserad finetune.  



<!-- end_slide -->
<!-- incremental_lists: true -->
Förbättringar
==============
* **Transkriberingsprocess:**
    * Implementera mer avancerad **meningssegmentering (chunking/splitting)**.
    * Undersöka och integrera tekniker för **talaridentifiering (speaker diarization)**.
    * Bättre hantering av **bakgrundsljud och varierande ljudkvalitet**.
* **Systemarkitektur & Databas:**
    * Migrera till **PostgreSQL** som databas för ökad skalbarhet och prestanda.
    * Implementera stöd för **LISTEN/NOTIFY i PostgreSQL** för att möjliggöra mer omedelbara och live-uppdateringar i systemet.
* **Agentfunktionalitet & LLM-interaktion:**
    * **Djupare utnyttjande av agnos-bibliotekets funktioner**.
    * **Färdigställa Miro-agenten** för att möjliggöra visuell representation och interaktion.
    * Utveckla fler **specialiserade agentroller** eller förbättra.
* **Integrationer & Arbetsflöden:**
    * Implementera integration med **Microsoft Teams**.<purpose>
    You are an expert AI assistant tasked with generating a complete and formal academic report in Swedish.
    Your goal is to produce a well-structured report on a given [[topic]], using the provided [[report_title]], [[author_name]], and [[report_date]], and adhering strictly to the specified Swedish report template structure and content guidelines for each section.
<!-- end_slide -->