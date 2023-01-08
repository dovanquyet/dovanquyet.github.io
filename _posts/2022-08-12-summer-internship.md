---
permalink: /posts/en/summer-internship-2022
date: 2022-08-12
title: 'What I Have Done During My Summer Internship'
tags:
  - work and research
---

This is my last day at Eureka Fintech. A journey has to finish, giving time for another journey to start - a new semester as a Research Postgraduate student. For the sake of revising what I have done during this internship, I write this blog.

## 0. Overview

### 0.1. Problem statement and current solution

Given an entity name (person or company), we need to find related important information about the entity. This info can be 1. identification (register ID, headquarter address, etc.) 2. business activity (general description about which business the entity involves in), and 3. any negative news (e.g sanction imposed on the entity) affecting the entity's business.

Currently, our solution is a pipeline consisting of the following steps:
1. search the entity's name on the search engine, then use the returned URLs to crawl relevant data
2. parse data to get information of both categories, using rule-based extraction or AI extraction
3. summarize business activity and negative news, then show the result to the user who queries the entity.

### 0.2. Summary of work done

The flow of my work is not as what I write below (of course), since the poor performance of one component may originate from other components, thus we have to iteratively go back and solve problems! However, in order to make it less repetitive and easier to get the whole picture, I group what I have done into several categories.

Four main done jobs:
1. familiarizing with the code, on-demand coding and re-organizing code base
2. data crawling
    - diffbot
    - tag-based html
    - trafilatura
3. data extraction
    - general keyword-based 
    - web-by-web rule-based
    - coreference resolution
    - machine learning approach
4. summarization
    - read academic papers
    - try online tools
    - try offline Gensim and DL models
    - deploy

Now, let's go through each specific sections.

## 1. Familiarizing with the code, on-demand coding and re-organizing code base

This part may not be so interesting, as it's a trivial step must taken by any newcomer.

### 1.1. Get familiar to code base

It took me two weeks in total. 

Firstly, I looked at the main file abc.py to understand the structure of the code base and then dived deeper into some specific functions so that later I knew where to add code to perform desired tasks. I studied some libraries (e.g google serpwow, spaCy, fuzzy, etc.) used in the project, played with these tools, and got a rough idea of what they would do in the program, later can employ them in some tasks. Also, due to the poor organization of abc.py (and other files), I thought about some ways to re-designed the code base.

### 1.2. Re-design code base

Along with developing new components for our program, I sometimes actualized the re-designing thought. These modifications could be splitting 'long' functions into smaller ones, grouping helper functions into files to make it clear about the functionality of each file, or making it more OOP-like to avoid having a lot of similar objects.

In the end, the code base is clearer, but still, needs improving =)

### 1.3. On-demand code work

Sometimes, other projects' work was more urgent to complete, I was assigned tasks to help complete them as soon as possible. For example, once I analyzed the HTML code of some websites and wrote code to scrape data of interest from these pages. These tasks were not difficult, and actually, I enjoyed doing them. However, they were just some task-switching moments to get a sense of completion but not my main job, and of course not the hard problem I have been encountering.

## 2. Data crawling

This part is more interesting. Thank to my teammates, I knew two powerful tools for crawling web articles.

### 2.1. Diffbot

Firstly, of course, let's take a look at this tool. "Like Google, Diffbot constantly crawls billions of Web pages, but instead of using that index to give people the best links to information, it provides businesses with data that they can then plug into their own analytics tools.", stated this [news](https://www.forbes.com/sites/jilliandonfro/2019/05/13/the-webs-all-seeing-eye-getting-in-on-googles-game-by-searching-a-trillion-facts/?sh=30b9ad5b3f93). CEO of Diffbot, Mike Tung, is a Master dropout from Stanford. "He envisioned a search engine that would only deliver concrete answers... he wanted every search to automatically surface either an exact piece of information or a massive dataset for analysis.", quite the same thing that Google has implemented. But okay, Diffbot gives us what we need - the content of articles given URLs.

After a short exploration, I backed to the main task. I sought a way to automatically crawl articles' content using Diffbot. "Using the API provided by Diffbot?" Hmm, not a good way, since we need to convert a normal URL into a JavaScript-like URL, e.g character '/' needs converting to '%2F'. As I didn't know what else to be changed, I preferred other methods that I just need to pass a normal URL. Fortunately, I found such a hosting API page, called RapidAPI. Great, the next step was to integrate this Diffbot code into the program to automatically crawl data for a 'long' list of companies. The task was not hard, just took time. Finally, I ran the code base to crawl data.

When running out of Diffbot's credit, we successfully got content of approximately 13k URLs, and stored them in a JSON 'database'. This database was for analysis, training and testing other components, and saving time and resources in later whole-program runs. Obviously, not most of them are fine-crawled. Some were just empty strings, some were mixed with HTML code. In order to understand the database, I conducted some analysis about the source of data (domain count), amount of content from each domain (word count), etc. That helped depict the way I treated data by source (describe in section 2.1)

### 2.2. Tag-based html crawl

Entailing the on-demand code work on data extraction from HTML format, I decided to take a look at several web domains that frequently appear in the search results, including US Treasury, Federal Register, Reuters, Bloomberg, etc. The action aimed to find an alternative (if exists) for Diffbot by just parsing the HTML file, which is easy to implement and FREE!

Basically, I observed the HTML tag whose content is exactly the article - the information that we want to take. As expected, they share a common property which helps indicate this part in HTML as the main article, but obviously, the property is not crystal-clear to transform to an algorithm. Specifically, the main article is usually stored in a `div` tag with the attribute `class` containing keywords like `content, body, text, detail` - all have the sense of the main article. However, since each web domain has a different way to manage its HTML, the algorithm constructed from the mentioned observation needed to be tested carefully.

Fortunately, before I started to implement it, my colleague introduced a more powerful tool to me.

### 2.3 Trafilatura

I was surprised by this [tool](https://trafilatura.readthedocs.io/en/latest/). It is a free tool for web content scraping and has been accepted to ACL in the System Demo track. It produces results, from my assessment, as good as Diffbot, but we don't have to pay anything. Great!

Integrating the tool into the system, I ran the main loop to get the result to develop the extraction component.

## 3. Data extraction

Trying several summarization models, I noticed that just giving the full content, i.e a list of articles related to the entity, to the models will not yield a good summary. The reason is that the full articles may not only focus on the interested entity, thus they confuse models on what needs summarizing. Therefore, I built a function that hopefully filters out all irrelevant or redundant information, so that we have a better input for the summarization step.

### 3.1. General keyword-based

This first approach uses a rule-based ranking mechanism to determine which sentence to take. It checks if a sentence hits any (blacklist) keyword or some identifiers representing the entity, then gives each sentence a score accordingly. Finally, it uses top negative sentences as input for negative news summarization, and top positive sentences as input for business activity info.

This approach definitely cannot totally solve the problem, since it's still general and contains little know-how to solve the extraction problem. One of its weaknesses is that it will catch sentences with keywords but irrelevant to the entity, while not yet being able to capture business activity since this information is highly customized in writing style.

### 3.2. Web-by-web rule-based

In fact, this approach likely deals with entities that are sanctioned. Looking at the statistics of Diffbot's crawled database, there were several websites that frequently show up in the query result, thus I thought it would benefit a lot if I 'manually' created rules to extract data from these pages. And that's what I did.

I dived into data of four pages: US Treasury, Federal Register, IFMAT, and Reuters. For each, I closely observed articles from the page and incrementally develop a rule to extract the desirable pieces of information. For example, on the US Treasury page, the first sentence of the article usually describes the main topic of the whole article and straightly mentions the sanction that relates to the entity. For that reason, I set the rule that takes this first sentence. Also, I employed a keywords matching method with a specific design for each webpage for extraction.

This method seemed to work better than the previous method, as it contains more know-how. However, a new problem emerged - the method still could not capture sentences that mentioned the entity but did not use its full name, e.g using pronouns or abbreviations as ways to paraphrase. Thus, I came up with the next method, as a new component added to this rule-based approach.

### 3.3. Coreference resolution

Thanks to the self-study period last year, I knew the exact terminology referring to the problem, as the name of this subsection. That helps me quickly search for relevant tools. As our solution significantly leverages spaCy, I preferred tools that are built upon the output of the spaCy Language object. Overall, I found three add-ons in the spaCy universe that deal with coreference resolution. One is Huggingface NeuralCoref, another is AllenNLP coref-spanbert, and the remaining is Spacy's Coreferee.

Again, I took a close look at each tool. AllenNLP Coref uses BERT as the model, then train that model to perform coreference resolution. One point that I didn't like about this tool is its bulkiness. Testing the model in our data, fortunately, the model performance was not as good as NeuralCoref, so I didn't have a headache selecting one from these. I cound remove AllenNLP Coref from the candidate list instead.

For the remaining two, it was promising that both tools would work well. At the beginning, I felt inclined to Coreferee, as it's a built-in tool from spaCy itself, and it does not require me to install another version of spaCy. But when I tested two tools on our actual data, Coreferee showed its deficit. In long documents, it only captured entities with a pretty similar name/pronoun, in other words, often could not cluster coreferences of an entity with pronoun substitution. On the other hand, although NeuralCoref did not work perfectly, it's significantly better than Coreferee. Therefore, I chose NeuralCoref as the coreference resolution model.

Integrating the model into the extraction pipeline yields a remarkably better extraction and summarization. Also, the coreference resolution model helps better extract core information from general webs.

### 3.4 Machine learning for alias detection

With the coreference resolution model, the extraction function works quite well. However, a new problem emerged. In articles resulting from a google search, there may be some misspelling errors regard to the entity's name. Also, some companies share the same name, while in fact, they are different entities as we can discriminate by the article describing these entities' business nature, address, etc.

The former case can be solved by `fuzzywuzzy` string matching, however, the latter case, so-called alias detection, likely requires a more sophisticated way to solve. Thus, I resort to my expertise =)) Machine Learning.

As suggested by my IT team and several academic papers on alias detection (in the ground "same name does not imply same entity"), I better took the article's content into consideration. Thus, I ended up with a quite sophisticated algorithm hence the annotation design (hope that my colleagues can understand it @@), leveraging the article's content to 1. predict if an article is relevant (in a vague sense) to the entity and 2. predict if two articles refer to the same entity (that specifically helps solve case 2). Each kind of prediction will be produced from a classification Language Model.

It costed me one week to annotate data, and three more days to train, test, and deploy both models. However, due to the limited amount of time, model 1 still fails in adversarial cases, while model 2 performs badly. Negative sampling may help model 1, while a lot more data is needed to train model 2.

## 4. Summarization.

### 4.1. Reading academic papers

At the beginning of this internship, I thought that I will spend a lot of time working on Deep Learning methods for summarization, thus I spent several days finding and reading papers on summarization, especially those focusing on entity-centric and multi-news summarization.

I learned two things. The first one is that multi-news summarization is inherently more complicated than single-news summarization, as it has to deal with a lot of repetition which single-news one does not have to deal with. The second is that there are several methods for multi-news summarization, they are all not yet ready for industrial use, as their code base only serves the standard training and testing pipeline with standard datasets. Thus, I had better start finding some available industrial-used summarization tools and test them on our data.

### 4.2. Try some online tools

I took a tour of available online summarization tools, to assess their current state. I tried 5 tools (again, with our own data), and all of them are extractive rather than abstractive. Some of them also provide API for commercial use. Overall, as developed for the general use cases, these tools do not surpass Gensim Summarizer, another prevalent extractive summarization tool, while they cost time (and money) to be integrated into our code. Thus, I still kept Gensim Summarizer.

### 4.3. Try offline models

Wandering in the huggingface models hub, I found some Deep Learning candidates to compare with Gensim Summarizer, namely Pegasus Xsum, Pegasus MultiNews, Longformer-Encoder-Decoder (LED) for Legal Judgement, and BART MultiNews. They are DL models pre-trained in single- or multiple-news datasets that from my point of view will suit our problem.

Quickly took a look at these models and tested them on our data, then considered both performance, computation cost, and installation difficulty, I grounded the candidate lists to only three options: Gensim Summarizer, Pegasus Xsum, and Pegasus MultiNews. Great! Later, I ran the whole pipeline on data from a medium set of query (as real deployment) to have a comprehensive judgment.

The result, to some extent, surprised me. Based on the preliminary result, I thought that Pegasus MultiNews would be the best fitted, as it's powerful and pretrained in the same setting. However, in the chosen set of data, it performed quite poorly! It usually added some fictional content to the summarization, such as citing the wrong sources (the phrase "Wall Street Journal reported" appeared a lot of times), or telling a totally strange story. About the main content, its abstractive summarization mechanism distorted the real description by over-paraphrasing. What an upset! Thus, I had no choice but to exclude it from the candidate list.

There were two remainings. Although Pegasus Xsum yielded more concise and straightforward results than MultiNews, it still sometimes generated fictional content, sharing this property with its brother Xsum. Meanwhile, Gensim Summarizer surprisingly worked decently. It could pick the most important sentences from the news and put them into the summary, based on its sentence ranking algorithm. Of course, there is still a gap between its and human performance, but it's significantly better than other candidates, while far quicker and computationally cheaper. For all of that reasons, I keep the summarization model as Gensim.

### 4.4. Deploy

Actually, I needed to write code for model deployment before I tested models in the above section. I borrowed code from huggingface tutorials, then wrapped it into classes with an identical API, so that we can switch between these models easily. Due to the time constraint, I have not yet trained any summarization model.


## 5. Conclusion

This internship brings me a lot of experience in my specialization field. It's a reminder to me about the difference and relationship between research and real application. Research originates from the need for real application, while real application needs to be fueled by research to succeed. To be a good researcher, I should embrace both aspects.