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

Few interesting questions though:


```python
{
    "id":          "57344d20acc1501500babdd2",
    "question":    "What does the Indian word \"shikaris\" mean in English?",
    "answer_text": "big-game hunters",
}

```

# Errors

This is a crowdsourced work and contains errors in it:

```python
{
    "id":             "57363293012e2f140011a21a",
    "question":       "What percent of sales are contributed by hunters?",
    "answer_text":    "eighty-seven",
                      @ should be "eighty-seven percent"
}
```

```python
{
    "id":             "56cd96c162d2951400fa6758",
    "question":       "How long did it take to implement riding horses in a believable manner?",
    "answer_text":    "four",
                      @ should be "four months"
}
```



```python
{
    "id":             "56e79aa800c9c71400d77370",
    "question":       "1989?",
    "answer_text":    "Saturday",
                      @ should be "No answer"
}
```


```python
{
    "id":             "56f8a6c39b226e1400dd0d54",
    "question":       "2007?",
    "answer_text":    "3,570",
                      @ should be "No answer"
}
```

# Missing Information I want to recover

1. Some answers actually can contain richer numberic answer, but some also cant:

```python
{
    "id":             "5709ee056d058f1900182c37",
    "question":       "What was the monetary base value in 1994?",
    "answer_text":    "400 billion dollars",
                      @ better be "approximately 400 billion dollars"
}
```

```python
{
    "id":             "570a4f184103511400d59605",
    "question":       "How much did the Grimm claim his employers said he should raise in grant money per year?",
    "answer_text":    "£200,000",
                      @ better be "at least £200,000"
}
```

# Trying to translate to Chinese

Using Google Translate (transforming the text into HTML with answer spans enclosed with a <span id="xxx">), I get a copy of SQuAD in Chinese. For sure, it contains quite a lot of non-senses:

1. Answer span is shifted:

```python
{
    "id":               "56d1196917492d1400aab93f",
    "question":         "百老匯與哪個行業有關？",
    "answer_text":      "是劇院",
    "question_orig":    "What industry is Broadway associated with?",
    "answer_text_orig": "the theater",
                        @ should be "劇院"
}
    
```

```python
{
    "id":               "56cf3e29aab44d1400b88ed4",
    "question":         "昆騰是其他哪個組織的部門？",
    "answer_text":      "幽靈》還是",
    "question_orig":    "Quantum is a division of what other organization? ",
    "answer_text_orig": "Spectre",
                        @ should be "幽靈"
}
```

```python
{
    "id":               "56df322e96943c1400a5d2d5",
    "question":         "納薩拉（Nasara）一詞何時在現代得到更多使用？",
    "answer_text":      "2014",
    "question_orig":    "When did the term Nasara become used more in modern times?",
    "answer_text_orig": "July 2014",
                        @ should be "2014年7月"
}
```

Why here? :(

```python
{
    "id":               "56e42e3739bdeb1400347920",
    "question":         "舊式拼字法是從哪個其他國家的拼字法獲得的依據？",
    "answer_text":      "拼字法是",
    "question_orig":    "From what other country's orthography did the Older Orthography get its basis?",
    "answer_text_orig": "standard German orthography",
}
```


OK this should be my bad

2. Not enough information for Google translate to infer the correct translation

```python
{
    "id":          "573403a24776f419006616ef",
    "question":    "What are urs?",
    "question-zh": "什麼是我們的？",
    "context":     "The fairs held at the shrines of Sufi saints are called urs. ..." 
}
```

Google is trying to be smart though XD



3. Mentioned above the interesting question. Asking the word in English when there is only Chinese


4. Chinese just do not speak like English since we are not mapping word to word

```python
    "id":               "56d23d72b329da140004ec4c",
    "question":         "佛陀提醒他的聽眾，那是什麼精神？",
    "question_orig":    "the Buddha reminds his hearers that it is the spirit that what?",
    "answer_text_orig": "counts",
    "context":          "Regarding the monastic rules, the Buddha constantly reminds his hearers that it is the spirit that counts. ..."
}

```
