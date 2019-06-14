---
layout: post
title: "Finnish Lemmatization With Python"
categories: Lemmatization Finnish
---
Natural language processing in Finish language is very limited. There are very few libraries and documentation is almost nonexistence. This problem arises, especially with lemmatization. there are only a few good options to lemmatize Finnish text. This blog post covers how to lemmatize Finnish text with Python.

Lemmatization is a process where a word is converted to its baseform which is a meaningful word.   Another option is [stemming](https://en.wikipedia.org/wiki/Stemming). In stemming algorithm removes the last few characters using some algorithm. Both methods are very powerful NLP techniques and it often depends on the case in which one is better.  [NLTK](https://www.nltk.org/api/nltk.stem.html) library already has Finnish stemmer and it is very well documented so I don’t cover it in this blog post.
<h2>Lemmatization with Voikko library</h2>

Voikko is a very powerful library and it is used in most free linguistic tools in open source. But there is a big catch if you are planning using it your property software project. Voikko is licensed with GPL v3 so there might be some licensing problems. But if you are using it in open source or your hobby project Voikko is the best option.

Installing Voikko is pretty straightforward in Ubuntu.
{% highlight shell %}
sudo apt  install  voikko-fi python-libvoikko
{% endhighlight %}

After install, you can try Voikko in python, it is pretty easy and straightforward.
{% highlight python %}
import libvoikko

lemmatizer = libvoikko.Voikko(u"fi") 

lemmatized = lemmatizer.analyze("junassa")[0]

print(lemmatized['BASEFORM'])
{% endhighlight %}

<h2>Lemmatization with FINNPOS</h2>

FINNPOS is an open source toolkit. Which is used tagging and lemmatizing morphologically rich languages like Finnish. It has Apache 2 license so it much more forgiving than Voikko, and it can also be used in closed source projects.  FINNPOSS is a new project, so installing and using it is not as straightforward as with Voikko. First clone FINNPOS from github.
{% highlight shell %}

git clone https://github.com/mpsilfve/FinnPos
{% endhighlight %}

Then you have to get a Morphological analyzer. You can find it [here](https://github.com/mpsilfve/FinnPos/wiki/hfst.sf.net) and put it to FinnPos/share/finnpos/omorfi/ directory. After that, you can install FINPOSS

{% highlight shell %}
Make & make ftb-omorfi-tagger

Sudo make install & Sudo make install-models

{% endhighlight %}
You can try it on command line  {% highlight shell %}
echo "junassa" | ftb-label {% endhighlight %}. 
I used subprocess in my python implementation.

{% highlight python %}
from subprocess import run, PIPE   

#ftl-label uses new lines to separate words. So if you want pipe many words the same time to ftl-label you have add \n to between every word.
text='junassa\nolivat\noravat'

#We are using subprocess where we pipe text
ftb_label = run(['ftb-label', ], stdout=PIPE, input=text, encoding='utf-8')

#get results from stdout                                          
splitted_result = ftb_label.stdout

#split results own lines                                       
splitted_result = splitted_result.split('\n')

#extract the base form from results                              
lemmatized_text = [word.split('\t')[2] 
			for word in splitted_result 
			if len(word.split('\t')) >= 3] 

print(lemmatized_text)

{% endhighlight %}

<h2>Conclusion</h2>

So those are two very simple but not so well documented ways to lemmatize Finnish text. I am happy to know that if there are any bugs or missed something crucial. you can always contact me with email or Twitter. Happy hacking.


