# eSQuAD
extra SQuAD


# Background

Stanford Question Answering Dataset (SQuAD) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of **Wikipedia articles**, where the answer to every question is a segment of text, or span, from the corresponding reading passage, or the question might be unanswerable.


From the official text, we know there are **mainly 2 types of answers**:

1. span answer
2. Not given by passage


# What's next

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

# Unexpected QA

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
    "id":             "56cd96c162d2951400fa6758",
    "question":       "How long did it take to implement riding horses in a believable manner?",
    "answer_text":    "four",
                      # should be "four months"
```


```python
{
    "question":       "For hydrogen what is the enthalpy of combustion?",
    "id":             "56e07476231d4119001ac17f",
    "answer_text":    "286 kJ/mol",
                      # should be "-286 kJ/mol"
}
```


# Missing Information I want to recover

## Richer information
Some answers actually can contain richer numberic answer, but some also cant:

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

## Yes/No Questions
I want Yes/No Questions output yes/no to me instead of relevant evidence
```python
{
    'id':             '572ea6d5cb0c0d14000f13f1',
    'question':       'Is Canada bilingual?',
    'answer_text':    'United States never developed bilingualism as Canada did.',
}

```

## Choices Question
I want Choices Questions output the choice to me instead of relevant evidence
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
    'id':             '56de30cdcffd8e1900b4b650',
    'question':       'Is the top quartile of HDI considered "high" or "very high" human development?'
    'answer_text':    'very high',
                      # this is a span answer though
}
```

## Conversational Question 

Actually there is another dataset CoQA, check it out!

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
Since sometimes we have more than 1 correct answer, and they may not be placed together.

```python
{
    'id':             'fake',
    "context":        "... Peter is good at basketball, and he excels at volleyball too ..."
    'question':       'Peter is good at playing what sport?'
    'answer_text':    'basketball, and he excels at volleyball',
                      # should be "basketball" and "volleyball"
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

Why here? :(

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
    "id":               "56d23d72b329da140004ec4c",
    "context":          "Regarding the monastic rules, the Buddha constantly reminds his hearers that it is the spirit that counts. ...",
    "question":         "佛陀提醒他的聽眾，那是什麼精神？",
    "question_orig":    "the Buddha reminds his hearers that it is the spirit that what?",
    "answer_text_orig": "counts",
}

```
