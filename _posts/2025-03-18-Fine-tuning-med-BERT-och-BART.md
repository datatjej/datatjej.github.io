---
layout: post
title: 59. Fine-tuning med BERT och BART
---

De senaste veckorna har jag experimenterat lite med fine-tuning av språkmodellerna BERT och BART. Funderade på om man kan beteckna dem som LLM:er (stora språkmodeller) trots att det begreppet snarare verkar ha börjat användas i samband med att ChatGPT lanserades under 2022 - och har landat i att man nog kan det. Båda modeller har tränats på stora mängder data och den ena, BART, har även generativa förmågor.

## BERT

BERT (eng. *Bidirectional Encoder Representations from Transformers*) beskrevs första gången i ett papper publicerat 2019 [[1]](https://arxiv.org/abs/1810.04805v2) av Google. Den lyftes fram som en förbättring gentemot de tidigar språkmodellerna ELMo och Open GPT eftersom den var tränad på ett dubbelrikat sätt - betingadad att se till meningskontexten både till höger och vänster - och inte bara från vänster till höger som de enkelriktade modellerna. Detta uppnås genom träning på två olika uppgifter: maskerad språkmodellering (där vissa löpord i indatan maskeras och modellen måste lista ut vilket ord det rör sig om) samt nästa mening-prediktion. 

Man definierar två olika sätt att applicera en förtränad modells representationer på mer specifika uppgifter: genom **features** eller **fine-tuning**. Den feature-baserade metoden, skriver man, använder en uppgiftsspecifik arkitektur där de förtränade representationera är en av flera features/egenskaper i modelleringen (t.ex. ELMo). Den fine-tuning-baserade metoden innebär istället att man slänger på ett sista klassificeringslager på den förtränade modellen och justerar alla förtränade parameterar. 

## BART

Inte långt efter att BERT-pappret publicerats kom Facebook med modellen BART (eng. *Bidirectional and Auto-Regressive Transformers*) [[2]](https://arxiv.org/abs/1910.13461). Medan BERT-artkitekturen enbart består av en så kallad kodare som fångar upp språkliga representationer och kan användas för att klassificera text så har BART även en avkodare-del som kan generera ny text. Den har tränats på återskapa text som på något sätt har korrumperats (t.ex. att meningarna har skyfflats om eller att delar av texten har maskerats).

BART kan användas för bl.a. maskinöversättning genom att BART-kodarens representationslager ersätts av en ny slumpmässigt initialiserad kodare. Den nya kodaren tränas på att mappa ett främmande källspråk till ett målspråk. Det främmande språket kan då ses som en korrumperad variant av målspråket. BART kan precis som BERT även användas för klassificeringsuppgifter på menings- och ordnivå om det sista gömda lagret i avkodaren matas in i ett linjärt klassificeringslager.

## huggingface/Transformers

Både BERT och BART har en så kallad Transformer-arkitektur och finns tillgängliga för nedladdning via webbsidan [huggingface](https://huggingface.co/). Huggingface har också ett kodramverk som heter just [`Transformers`](https://huggingface.co/docs/transformers/en/index) och som tillhandahålller API:er och andra verktyg för att träna och fine-tunea den här typen av modeller.

För mina fine-tuning-experiment började jag därför med att skapa upp en [Python-miljö](https://datatjej.github.io/Skapa-en-virtuell-milj%C3%B6-f%C3%B6r-Python-med-conda/) (dock med `venv` istället för `conda`) på en server med GPU-resurser. Jag installerade Jupyter Notebook och övriga mjukvarupaket som nämndes i [Quick start-guiden](https://huggingface.co/docs/transformers/en/quicktour) med `pip`.

### Ladda in modellerna

I mitt fall ville jag använda de BERT- och BART-modeller som redan fine-tuneats ytterligare av Kungliga biblioteket på deras svenska språkresurser: [`KB/bert-base-swedish-cased`](https://huggingface.co/KB/bert-base-swedish-cased) och [`KBLab/bart-base-swedish-cased`](KBLab/bart-base-swedish-cased):

``` python
tokenizer = AutoTokenizer.from_pretrained('KB/bert-base-swedish-cased')
model = AutoModel.from_pretrained('KB/bert-base-swedish-cased')
```

### Ladda in den egna datan

I mitt fall vill jag fine-tunea BERT/BART-modellerna på lemmatisering av fornsvenska. För att ladda in den egna datan använde jag `load_dataset`-funktionen från `datasets`-API:et. Den funktionen kan lista ut vad som är train/val/test utifrån filnamn, mappstruktur och annan metadata. Men om inget sånt finns förusätter den att allt är `train`-data. Man kan också definiera sina egna skräddarsydda uppdelningar av datan.

I mitt fall består datmängden av olika texter uppdelade i olika filer. Jag började därför med att ladda in varje text som sin egen split:

``` python
mathir_files = {
    "abota": "path/till/filen/abota.jsonl",
    "avgl": "path/till/filen/avgl.jsonl",
    "moses": "path/till/filen/moses-b.jsonl",
    "ogla": "path/till/filen/ogl-a.jsonl",
    "tungulus": "path/till/filen/tungulus.jsonl",
}

mathir_dataset = load_dataset("json", data_files=mathir_files)
```

### Tokenisering 

När det kommer till tokenisering måste man använda den tokeniseringsfunktion som modellen man vill fine-tunea använder. I fallet med BERT och BART måste man också vara medveten om att de använder orddelstokenisering (eng. *subword tokenization*). Du kan läsa mer om det [här](https://huggingface.co/learn/nlp-course/en/chapter2/4), men i korthet innebär det ovanliga eller okända ord delas upp i mindre beståndsdelar, t.ex: 


``` python
tokenizer = AutoTokenizer.from_pretrained('KB/bert-base-swedish-cased')
tokenized_input = tokenizer.tokenize("Olyckligtvis behöver han ambulanshjälp")

print(tokenized_input)
```

som ger `['Olyck', '##ligtvis', 'behöver', 'han', 'ambulans', '##hjälp']`. Exemplet ovan är genererat med BERT:s tokeniserare, som använder **WordPiece**-algoritmen (läs mer om det [här](https://huggingface.co/learn/nlp-course/en/chapter6/6)) för att skapa upp en intern vokabulär utifrån frekvenser i korpusen den tränats på. När ett ord tokeniseras försöker BERT-tokeniseraren matcha det mot det längsta matchande ordet i vokabulären, och sedan mot orddelar. Orddelarna markeras med `##`. 

BART använder istället något som kallas **byte-pair encoding** (BPE) men principen är liknade. BPE-algoritmen skapar merge-regler utfrån teckenfrekvenser i korpusen och applicerar sedan dessa vid tokenisering. Orddelar markeras med `Ġ` istället för `##`, t.ex:


``` python
tokenizer = AutoTokenizer.from_pretrained('KBLab/bart-base-swedish-cased')
tokenized_input = tokenizer.tokenize("Olyckligtvis behöver han ambulanshjälp")

print(tokenized_input)
```

som ger: `['ĠOlyck', 'ligtvis', 'Ġbehöver', 'Ġhan', 'Ġambulans', 'hjälp']`. 

En viktig skillnad mellan BERT- och BART-tokeniseringen är hur de hanterar okända ord, s.k. **OOV**-ord (*out out vocabulary*). Båda tokeniserare försöker i den mån det är möjligt att bryta ner ord i mindre beståndsdelar. Men medan BART alltid kan mappa okända ord och orddelar till deras byte-representation så markerar BERT dessa som okända: `[UNK]`. Hela OOV-ordet markeras som `[UNK]` även om BERT känner igen vissa av beståndsdelarna i ordet. 

## Specialord (TBA)

Både BERT och BART använder specialord för att ange var 

## Etiketter

Mitt fine-tuning-fall handlar alltså om lemmatisering, om att mappa löpord i text till deras respektive lemma/grundform. Lemmana i min annoterade datamängd blir allså etiketter (eng. *labels*) för träningen:

``` jsonl
{"id": "26061", 
"tokens": ["Her effter", "läsis", "aff", "en", "man", "och", "war", "en", "riddare", "och", "heth", "Tungulus"], 
"lemmata": ["här äptir", "läsa", "af", "en", "maþer", "ok", "vara", "en", "riddare", "ok", "heta", "Tungulus"], 
"pos_tags": ["Df", "V-", "R-", "Py", "Nb", "C-", "V-", "Py", "Nb", "C-", "V-", "Ne"]}
```

Men för **lemmma-klassificering** i antingen BERT eller BART behöver man omvandla etiketterna till numeriska representationer. Detta görs på tränings- och valideringsdelen av datamängden. I och med att jag förväntar mig att de flesta lemman även finns i Söderwalls medeltidssvenska ordbok matar jag även in dessa etiketter till min lemma2id-struktur:



För **lemma-generering** i BART däremot behövs ingen manuell numerisk etikett-omvandling. Man tokaniserar istället etikett-datan på samma sätt som text-indatan. 

## Etikett-mappning



## Etiketter



We use WordPiece embeddings (Wu et al.,
2016) with a 30,000 token vocabulary. 


## Referenser

[1] [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/abs/1810.04805v2). (2019).
[2] [BART: Denoising Sequence-to-Sequence Pre-training for Natural Language Generation, Translation, and Comprehension](https://arxiv.org/abs/1910.13461). (2019).

