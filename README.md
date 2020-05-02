# eSQuAD
extra SQuAD



# Finetune Workflow
```markdown
 - Tokenizer
    - BPE
    - Syntactic


 - Architecture
    - roBERTa (multi-lingual!)

    - alBERT (small model but slow)

    - Reformer (sparse attention, no pretrained model yet)

    - ELECTRA (different pretraining objective, 
              lower budget on pretraining)

    - BART (Generative, slow)

    - SLIDE (CPU Deep Learning: faster than GPU)

 - Data preparation:
    - List out all the tasks
    - Distribution of available dataset
    - Semi-balanced batches

 - Finetuning
    - Mixout (instead of dropout)
    - Early Stopping
 
    - Answerablitiy
       - Simple binary-classification
       - Retro-reader verification

 - Model Evaluations
    - Model Accuracy
    - Model Robustness
        - Adversarial attack

```



# Objective

 - extract information to objects
 
 ```
I want a book, priced under $100
``` 
```python
  "want {sth}": { 
    sth: [{ name:     "book",
            quantity:  1,
            price:    { value: 100, approx: True } }]
  }
```
 
 - extract multiple information 

```
Please a cheese burger and two apple pies.
``` 
```python
  "want {sth}": { 
    sth: [{ name: "cheese burger", quantity: 1 },
          { name: "apple pies",    quantity: 2 }],
  }
```
 
 - slot filling
 
 
```
can you please show me the way to toilet?
``` 
```python
  "where is {place}":{
    place: [{ name: "toilet" }]
  }
```
 
 
 - fake information resistant
 
 
```
I have a book. It is in my my bedroom!
``` 
```python
>>
  "want {sth}": { 
    sth: []
  },
  "where is {place}":{
    place: []
  }
```
 
 - intention matching
 
```
I am a bit confused about the instruction. Can you show me?
``` 
```python
  "I want to know how to use it.": True
```
``` 
How I use it is not relevant.
``` 
```python
  "How to use it?": False
```
 
 
 
 - dialog  compatible 

```
A: What do you want?
B: Do you have ice cream?
``` 
```python
# according to last line

  "want {sth}": { 
    sth: []
  },
  "Do you have {sth}":{
    sth: [{ name: "ice cream" }]
  }
  "What flavor do you have?": False,
```


```
A: What do you want?
B: Do you have ice cream?
A: Yes sure Do you want want some?
B: Yep! what flavor do you have?
``` 
```python
# according to last line

  "want {sth}": { 
    sth: [{"name": "ice cream"}]
  },
  "Do you have {sth}":{
    sth: []
  },
  "What flavor do you have?": True,
```



# Skill Coverage

We should consider the diversity in:

1. **Lingual properties**
2. **Situations**
3. **QA combinations**

---
### **Lingual properties**

```yaml
1. Vocabulary
   - Words   # similar words
   - Phrases # different meanings combined with other words
   - Tenses  # different meanings in different word form
   - Prepositions
   - Pronouns
   - (colloquial) discourse markers/stops # uh.. hm..

2. Sentence structure
   - Reposition without altering the meaning
   - Relative clause
   - If-clause

```
---
### **Situations**


```yaml
1. Format Texts
   - Wikipedia
   - Book
   - News
   - Documents
   - Manuals

2. Colloquial
   - Daily Conversations
   - Forums
   - Speech / Presentation
   - Oral report
   - (Artifect)

3. Datasheet
   - Form
   - Table
   - Lists
```


---
### **QA combinations**

```yaml
1. Question structure
    - Single question (Standard wh-)
        # When did Jena leave?
    - Single question (Statement + wh-)
        # Jena leave at what time?
    - Single question (Statement + wh-[hidden]) 
        # Jena leave at?     (can be ambiguous: time? place?)
    - Fill in the blank 
        # Jena leave at ___? (can be ambiguous: time? place?)
    - Based on previous questions
    - Form filling (dependent fields)
        ##################################
        ## What is broken : Two apples
        ##################################

        ##################################
        ## What is broken : apples
        ## How many are broken : Two
        ##################################

2. Mentioned with answer?
    - Yes
    - No
    - (dialog) do not want to answer
    - (dialog) forgot

3. Answer scope
    - Mentioned # Exact
    - Mentioned # Inexact
    - Mentioned # Don't know
    - Mentioned # Depends

4. Number of answers
    - Single    # Who made the mistake? Alfar 
    - Multiple  # Who corrected the mistake? Begal, Tena, Seca 

5. Information provided
    - What
      - Which subject
      - Definition
    - How many / How much # Integers/Decimals
      - Mentioned # two apple / $40.1 / half a dollar
      - Singular  # an apple / the apple / costs a dollar
      - Counting  # Mary and another 9 students
      - Relative  # Half of the population
    - When
      - Year/Month/Day, Hour/Minute/Second
      - from X to Y
      - Relative # the first sunday of the next month
    - How long
      - Year/Month/Day, Hour/Minute/Second
      - Relative # a year more than she does
    - Where
      - Relative # The red booth inside the party
      - Directional # North of Dehamber
    - Yes/No/mentioned to be unknown/Depends
    - Which # choice
    - Why
```









# Datasets
## SQuAD 2.0

 - Stanford Question Answering Dataset (SQuAD)
 - from **Wikipedia articles**
 - ~100,000 + ~50,000 (no answer) QAs
 - annotated with:
     1. span answer
     2. not given by passage


(leaderboard: https://rajpurkar.github.io/SQuAD-explorer/)



## CoQA

 - A Conversational Question Answering Challenge
 - conversational QA (i.e. asking questions depending on previous asked questions)
 - ~127,000 QAs
 - annotated with free-form
 

An important feature of the CoQA dataset is that the answer to some of its questions could be free-form. Therefore, around 33.2% of the answers do not have an exact match subsequence in the given passage. 

Among these answers, the answers Yes and No constitute 78.8%. The next majority is 14.3% of answers which are edits to text span to improve fluency. The rest includes 5.1% for counting and 1.8% for selecting a choice from the question.

(paper: https://arxiv.org/abs/1909.10772)

(leaderboard: https://stanfordnlp.github.io/coqa/)


## boolQ
 - Boolean Q
 - 15942 QAs
 - annotated with yes/no answer

(paper: https://arxiv.org/abs/1905.10044)

(reference: https://github.com/google-research-datasets/boolean-questions)


## mutlirc-v2
 - Reading Comprehension over Multiple Sentences
 - Multiple choice questions
 - More than 1 valid choice in each question

(leaderboard: https://cogcomp.seas.upenn.edu/multirc/)



## HotpotQA

 - multi-hop questions, with strong supervision for supporting facts
 - free-form

(leaderboard: https://hotpotqa.github.io/)


## RACE

 - **R**e**A**ding **C**omprehension dataset collected from **E**nglish Examinations
 - Multiple choice questions


(leaderboard: http://www.qizhexie.com/data/RACE_leaderboard.html)

## ReCoRD

 - Reading Comprehension with Commonsense Reasoning Dataset

(leaderboard: http://www.qizhexie.com/data/RACE_leaderboard.html)




## cmrc2018

 - span extraction

(leaderboard: https://hfl-rc.github.io/cmrc2019/leaderboard)

Best EM: 74.178
Best F1: 88.145



## cmrc2019

 - Fill in the blanks with sentences from choices
 
(leaderboard: https://hfl-rc.github.io/cmrc2019/leaderboard)

Best dev PAC: 60.0
Best dev QAC: 90.93


# Types of QAs

Let's dive into the dataset, and we can easily find that we have different types of questions:

|    | Format        | Information              | Example                                                                                                                                                                                                                                       |
|----|---------------|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1  | What is       | ℹ️ Definition             | - What is botany?<br>- What is transpiration?<br><br>- What is the meaning of dichotomous keys?                                                                                                                                               |
| 2  | What/Who do   | 🚹 Specific Entity        | - What could a Bengal tiger be hunted from the back of?<br>- Buddhism is based on the teaching of who?                                                                                                                                        |
| 3  | Where is/do   | 🌎 Place/Location         | - What city did Frédéric visit in June 1837?<br>- Where did Nintendo preview the horseback riding feature?                                                                                                                                    |
| 4  | When is/do    | 📆 Date/Time              | - When did Chopin and Sand become lovers?<br>- How early was this term used?                                                                                                                                                                  |
| 5  | How do        | 🧠 Method/Attribute       | - how does life express impermanence?<br>- 808s & Heartbreak was released by what company?<br>- How is childcare viewed in a hunter-gatherer society?<br>- how were altarpieces often decorated in this period?                               |
| 6  | How many/much | 💰 Number/Amount          | - How many years older was George Sand compared to Frédéric?<br>- How much older was George Sands than Chopin?<br>- How much was allocated?<br>- How tall does Schwarzenegger claim to be?<br>- what anniversary was house music celebrating? |
| 7  | What percent  | 💯 Percentage             | - What percentage of sea immigrants asked for asylum in Greece?<br>- in 1720 what % of  import goods were from India?                                                                                                                         |
| 8  | Why           | ❓ Reason behind sth      | - Immigrants arriving by sea are coming mainly for what reason?<br>- What was the reason the Allen brothers sold land in 1836?                                                                                                                |
| 9  | X or Y        | 💕 Decide based on choice | - Are written records of Old Dutch rare or common?<br>- Is Afrikaans more or less complex compared to Dutch?                                                                                                                                  |
| 10 | Is it         | 👌 Yes/No                 | - Does chicken contain fat?<br>- did the word "computer" take its well known meaning of today?                                                                                                                                                |




## Few interesting questions though:


```python
{
    "id":          "57344d20acc1501500babdd2",
    "question":    "What does the Indian word \"shikaris\" mean in English?",
    "answer_text": "big-game hunters",
}

```








# Dataset mismatch

## Richer information
Some answers actually can contain richer numeric answer, but some also cant:

```python
{
    "id":             "5709ee056d058f1900182c37",
    "question":       "What was the monetary base value in 1994?",
    "answer_text":    "400 billion dollars",
                      # better be "approximately 400 billion dollars"
}
```

```python
{
    "id":             "570a4f184103511400d59605",
    "question":       "How much did the Grimm claim his employers said he should raise in grant money per year?",
    "answer_text":    "£200,000",
                      # better be "at least £200,000"
}
```

Fail case:

```python
{
    "id":             "570a4f184103511400d59605",
    "question":       "How much did the Grimm claim his employers said he should raise in grant money per year?",
    "answer_text":    "£200,000. The amount is just roughly",
                      # better be "£200,000 roughly"
}
```

## Yes/No Questions

Yes/No Questions should output yes/no to me, but SQuAD gives relevant evidence.

```python
{
    'id':             '572ea6d5cb0c0d14000f13f1',
    'question':       'Is Canada bilingual?',
    'answer_text':    'United States never developed bilingualism as Canada did.',
                      # just evidance
}

```

## Choice Question
Choice questions should output the choice to me, but SQuAD gives relevant evidence
```python
{
    'id':             '572782e0708984140094dfa6',
    'question':       'What is more important for a textual critic: quality or quantity?',
    'answer_text':    'Readings are approved or rejected by reason of the quality, and not the number, of their supporting witnesses',
                      # just evidance
}
```

```python
{
    'id':             'fake',
    'question':       'Is the top quartile of HDI considered "high" or "very high" human development?'
    'answer_text':    'extremely high',
                      # this is a span answer though
}
```

## Conversational Question 

Only in CoQA, Questions will depend on previous questions.

```python
{
    'id':             '573426864776f41900661980',
    'question':       'What year was Montana's statehood approved?'
    'answer_text':    '1889',
},
{
    'id':             '573426864776f41900661980',
    'question':       'What year was Montana's statehood approved? What other three states were approved in the same year?'
    'answer_text':    'North Dakota, South Dakota and Washington',
},
```


           
## Multuple answer Extraction

QAs with multiple answers naturally occur in the datasets, but they are span-answer.

```python
{
    'id':             'fake',
    "context":        "... Peter is good at basketball, and he excels at volleyball too ..."
    'question':       'Peter is good at playing what sport?'
    'answer_text':    'basketball, and he excels at volleyball',
                      # should be "basketball" and "volleyball"
}
```







# Unexpected QA

## Typos

All datasets are crowdsourced and contains typos.

e.g. think --> tink
e.g. Were they hungry --> where they hungry




## Arabic numbers chosen but not the word

```python
{
    "id":             "56e0914c7aa994140058e5f8",
    "context":        "... there are over 100 binary borane hydrides known, but only one binary aluminium hydride ..."
    "question":       "How many binary aluminum hydrides are there?",
    "answer_text":    "1",
                      # should be "only one"
}
```

## Not wrong... but just not exactly

```python
{
    "id":             "56e092177aa994140058e5fe",
    "question":       "What do hydrides that are bridging ligands link up?",
    "answer_text":    "link two metal centers",
                      # should be "two metal centers"
}
```

```python
{
    "id":             "57268af3dd62a815002e88eb",
    "question":       "When did the term for the people of Burma become a common place word in English?",
    "answer_text":    "The name Burma has been in use in English since the 18th century",
                      # should be "since the 18th century"
}
```
## Errors

This is a crowdsourced work and contains errors in it:

```python
{
    "id":             "57363293012e2f140011a21a",
    "question":       "What percent of sales are contributed by hunters?",
    "answer_text":    "eighty-seven",
                      # should be "eighty-seven percent"
}
```

```python
{
    "id":             "56cd96c162d2951400fa6758",
    "question":       "How long did it take to implement riding horses in a believable manner?",
    "answer_text":    "four",
                      # should be "four months"
}
```


```python
{
    "question":       "For hydrogen what is the enthalpy of combustion?",
    "id":             "56e07476231d4119001ac17f",
    "answer_text":    "286 kJ/mol",
                      # should be "-286 kJ/mol"
}
```




# Trying to translate to Chinese

Using Google Translate (transforming the text into HTML with answer spans enclosed with a <span id="xxx">), I get a copy of SQuAD in Chinese. For sure, it contains quite a lot of non-senses:

## Answer span shifted

```python
{
    "id":               "56d1196917492d1400aab93f",
    "question_orig":    "What industry is Broadway associated with?",
    "answer_text_orig": "the theater",
    "question":         "百老匯與哪個行業有關？",
    "answer_text":      "是劇院",
                        # should be "劇院"
}
    
```

```python
{
    "id":               "56cf3e29aab44d1400b88ed4",
    "question_orig":    "Quantum is a division of what other organization? ",
    "answer_text_orig": "Spectre",
    "question":         "昆騰是其他哪個組織的部門？",
    "answer_text":      "幽靈》還是",
                        # should be "幽靈"
}
```

```python
{
    "id":               "56df322e96943c1400a5d2d5",
    "question_orig":    "When did the term Nasara become used more in modern times?",
    "answer_text_orig": "July 2014",
    "question":         "納薩拉（Nasara）一詞何時在現代得到更多使用？",
    "answer_text":      "2014",
                        # should be "2014年7月"
}
```

```python
{
    "id":               "56e42e3739bdeb1400347920",
    "question_orig":    "From what other country's orthography did the Older Orthography get its basis?",
    "answer_text_orig": "standard German orthography",
    "question":         "舊式拼字法是從哪個其他國家的拼字法獲得的依據？",
    "answer_text":      "拼字法是",
                        # should be "標準的德國拼字法"
}
```



## Incorrect Translation
Not enough information for Google translate to infer the correct translation

```python
{
    "id":          "573403a24776f419006616ef",
    "context":     "The fairs held at the shrines of Sufi saints are called urs. ...",
    "question":    "What are urs?",
    "question-zh": "什麼是我們的？",
}
```

Google is trying to be smart though XD


More:

 - fried sole --> 油炸鞋底  (油炸比目魚)


## Tailored Questions in English


```python
{
    "id":          "57344d20acc1501500babdd2",
    "question":    "What does the Indian word \"shikaris\" mean in English?",
    "answer_text": "big-game hunters",
}
```

## Chinese just do not speak like English since we are not mapping word to word

```python
{
    "id":               "56d23d72b329da140004ec4c",
    "context":          "Regarding the monastic rules, the Buddha constantly reminds his hearers that it is the spirit that counts. ...",
    "question":         "佛陀提醒他的聽眾，那是什麼精神？",
    "question_orig":    "the Buddha reminds his hearers that it is the spirit that what?",
    "answer_text_orig": "counts",
}

```



# Data Augmentation

## Unanswerable yes/no QAs

Since unanswerable QA pairs come mainly from SQuAD 2.0, there are very few unanswerable yes/no questions. We augment data from boolQ.



## Unanswerable MC QAs

Since unanswerable QA pairs come mainly from SQuAD 2.0, there are very few unanswerable MC questions. We augment data from RACE and multirc.



## Unanswerable span QAs

To further balance the number of unanswerable QAs, we trim paragraphs from HotPotQA such that there are not enough information to infer the answer.



# Model Approach

## Reference

 - span-extraction from LM
   - alBERT
   - roBERTa
 
 - generative LM
   - BART 


## Proposed Solutions


### span+head

Use roBERTa / XLMR / alBERT as LM

**Input Representation**

| [CLS] | **Context** | [SEP] |
|-------|:-----------:|------:|


| **Question**<br>**(history 1)** | [SEP] | ... | **Question**<br>**(history N)** | [SEP] | **Target** <br>**Question** | [SEP] | **Choice 1** | [SEP] | ... | **Choice N** | [SEP] |
|---------------------------------|-------|-----|---------------------------------|-------|-----------------------------|-------|--------------|-------|-----|--------------|-------|



**Output**
 1. Span-head (start + end)
 2. Answerable-head (CLS head)


**Loss**
 1. softmax over span start and end (choice or yes/no = span answer)
 2. answerable 1 or 0




### Possible System outputs

 - [x] Information Extraction from span (single answer)
 - [x] Yes/no (can be unanswerable) 
 - [x] Best answer from given choice (single answer + can be unanswerable)
 - [ ] Information Extraction from span (Multiple answers)
 - [ ] Best answer from given choice (Multiple answers)
 - [ ] Arithmetic problem (e.g. counting, datetime offset)


