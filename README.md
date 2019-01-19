# aaai_2019

Q - 
数据挖掘： 基于AAAI 2019会议录取的论文数据，从中提取出所有的：1.作者名；2.机构名；3.论文标题的主题分布

## Project Intro/Objective
The purpose of this project was to mine text from the [AAAI 2019 accepted papers PDF](https://aaai.org/Conferences/AAAI-19/wp-content/uploads/2018/11/AAAI-19_Accepted_Papers.pdf) document provided by Mos from Synced 机器之心. I attempted to use NLP methods to segment all text found in the pdf, and from segmented text find and tag author names 作者名, organization names 机构名 and paper topics 论文标题主题.

### Methods Used
* PDF convert to python read-able string
* Text pattern search
* NLP software setup
* Data munging (various methods)

### Technologies
* Python
* Apache Tika for parsing pdf document
* Python Regex for cleaning up formatting/escape characters
* Pandas, Jupyter
* Stanford CoreNLP

## Project Description

1. The first challenge was to turn .pdf text into something readable by python. I found a *parser* module in Apache Tika that allowed me to read in a .pdf as metadata and content. The content is all text from the accepted papers document in a single string with a lot of formatting annotations.

2. I looked for an existing NLP software that can segment and tag English text. This was the most time-consuming part as I struggled to find a useful model that I am able to set up, using resources on the Web. After some considerations, I gave the Stanford CoreNLP a try. 

3. The output from the Stanford CoreNLP NER (Named Entity Recognition) module still needed a bit of manual work to make presentable. I had assumed that someone would've built a script to format its output and made it public, but I had difficulty finding one so I wrote loops in Python to take out word segments labelled 'O', 'ORGANIZATION' and 'PERSON'.

4. With some manual formatting, some person names, organization names and 'O' which I assume to be other part-of-speech labels were uploaded to tables. I thought that 'O' tags contained the most contextual information for the titles of papers so I combined the text strings following the indices of entries found in the document.

## Challenges and Limitations

* I wanted to process each entry (defined by numerical indices) in the document however wasn't able to use the regex module to split it at digit-and-colon. In addition, there were 1.1k entries in total, using a for loop to append Stanford CoreNLP output seemed like it would take forever or crash my machine, but processing the whole document as one string was instantanous. This way I lost a lot of valuable information as keeping the segmented text from each entry would have made it much easier to determine accuracy, especially for author names.

* I did not have time to explore other NLP softwares in-depth, and therefoer did not compare the accuracy of different models against each other and figure out the best for this particular document. For example first and last names were split up by the Stanford CoreNLP Ner module, and proved difficult to put back together manually.

* I noticed a lot of names, especially Chinese pinyin ones were escaping the detection of the Stanford CoreNLP software. Instead they were often recognized as parts of organization names. If I had more time I may try to iteratively process them again, by concatenating all segments with 'ORGANIZATION' labels into one string, run it through Stanford CoreNLP and try to see if they can be found more accurately.

* In hindsight, the entries found in the document were highly structured. It may be more optimal to use the regex library to search for text structure that define the components of each entry (such as title followed by index number, colon and two spaces; author name before brackets and organization name within brackets in a new line after title). In the future I would like to explore the regex library more as I think it is an important tool to use in text processing.

## Getting Started

1. Clone this [repo](https://github.com/jodiqiao/aaai_2019.git) 
2. Pip install tika, nltk and other dependencies found in aaai_2019_nlp.ipynb
3. Click [here](https://blog.manash.me/configuring-stanford-parser-and-stanford-ner-tagger-with-nltk-in-python-on-windows-f685483c374a) and follow the instructions for [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/download.html) set up
4. Run aaai_2019_nlp.ipynb and obtain .csv outputs of final dataframes

