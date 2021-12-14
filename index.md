# Social Graph - Quotebank

WE NEED MORE GRAPHS

# 1. Abstract and objectives

[first animation]

The starting point of this datastory is the ***[QuoteBank](https://dlab.epfl.ch/people/west/pub/Vaucher-Spitz-Catasta-West_WSDM-21.pdf)*** dataset,  "an open corpus of 178 million quotations attributed to the speakers who uttered them, extracted from 162
million English news articles published between 2008 and 2020".

The key idea of the projects revolves around the fact that in this enormous amount of quotation it may exist a good portion containing quotation referencing someone else.

For example, since many of these quotations come from news articles, it is likely that they will be from political exponents stating something about other political or well-known public figures.

From these considerations we draw our project goals:

1. Is it possible to create a *social graph* where each **node** is a **person** and the **edges** represent a **quotation** where "person A stated something about person B"?
2. What if these **edges** have a **weight** depending on the sentiment of the statement (i.e when a quotation is positive the weight is small, and when it is negative it is high)?
3. Would this particular choice of the weight lead to a graph where communities can be observed in clusters of the graph since people having good interaction between them (i.e having good statements about each other) would be near, whereas those having bad interactions would stay apart?
4. Which communities can we find in the clusters graph? (Political parties, Male/Female groups, etc...)
5. Can we see a changement in these clusters over time?

[second animation]

# 1. Some statistics about the data

## 1.1 How much data?

As a starting point looking at the proportion of quotes mentioning someone and those not mentioning someone can give us an idea of the size of data we dispose of.

[Plot]

## 1.2 Who speaks more? Who more mentioned?

Before creating the graph, we're curious to see who are the most mentioned people and who are the people quoting the most.

[Plot]

## 1.3 Sentiment statistics

What is the prevalent sentiment in the quotes from mostly news articles?

[Plot]

# 2. Project pipeline

For understanding how all the goals are achieved during the project, we first start by showing the pipeline we constructed.

The project pipeline consists of 3 main steps:

1. **Construct the graph with quotes from Quotebank**:

We will select only the quotes from **Quotebank** where a person is mentioned, and identify the person and the speaker in database **Wikidata**.

We then compute the positivity/negativity score for each quote and **construct the graph based on it**.
[second animation step1]
2. **Find communities with the Louvain method:**

Running the Louvain method will allow finding a number of communities based on the edges of the graph and the weights they have.  From all the communities found we will consider only the most populated ones.
[second animation step2]
3. **Identify shared characteristics within the communities:**

Thanks to the **ids of the nodes** and the database **Wikidata**, we can **access features** for every node )i.e person).  This will allow us to find common **characteristics within the communities** based on the distribution of the features.
[second animation step3]

The goal is to find **known** and **identifiable** communities! Remember that the graph is constructed **solely** on the positivity/negativity of the quotes!

No **prior knowledge** of history, personal relationship or identity of our subjects.

Go **watch the next video** to see if we succeeded!

# 3. Graph construction

## Cleaning

The first step to take before even starting processing the quotes is to clean them!

The main clean steps we took have been:

1. removing special characters
2. finishing all quotes with a period '.'

## Quote selection factory

Once the cleaning is done, the “Quote selection factory”, selects only the quotes that respect the **rules in the parchment.**

[slide with parchment]

Next, it uses WIKIDATA to match the speaker name of the quote with an id. And it searches for names in the quote, and match them with an id from WIKIDATA**.**

[animation with examples]

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled.png)

A big problem during the graph creation was the assignment of generic names as "Joe" present in a quote. Indeed in this case, it would be hard to decide to which is referred the quote since we only have the name of the mentioned person.

The solution we adopted was to consider only the person with the respective name which is more relevant, i.e which has spoken more times in the whole quotebank dataset.

[animation with biden]

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%201.png)

This solution seemed to be in its simplicity very effective, and this can be also by confirmed by the following example:

[sliden with quiz of who do you think it is joe vladimir]

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%202.png)

## Sentiment Analysis factory

This step leverages on the NTLK library, which allows us to assign a **compound score** to each quote. This score is value between -1 and 1 where positive value represent positive sentiment whereas negative values represent a negative sentiment in the quote.
In a second step the compound score will be updated considering the number of edges between two nodes.

It happens often indeed that 2 persons mention each other multiple times. We can think about this as a sign of the fact that they have a strong relationship, which can be both negative or positive, since they have a high interaction rate

[plot of the distribution of number of edges between 2 nodes]

In order to have a clean graph with at maximum 2  directed edge between 2 nodes ( one from A to B and one from B to A) we decided to aggregate all these edges into one. However it is important to decided what to do with the different compound scores in this step.

We wanted to translate this aspect in our model, so we decided to update the compound score with the following formula:

$c=c *log(n*e)$
 ⇒ WE NEED TO CLIP THIS

This way if the compound score was negative (or positive), its negative value get intensified by a higher number of edges.

## Computational Factory

In this last step we define a function which takes the compound score and defines the weight of the respective oriented edge.

The function we decided to use was
$w=e^{ \alpha c}$

Where $\alpha$ is a constant which we tuned until we had good results.

In the end we found that $\alpha =5$ was a good value for the constant in order to obtain good results.

[final animation with  quote selection factory, sentiment and computetinal]

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%203.png)

## Grab the nodes

[interactive graph]

# 4. Statistics on the graph

Before diving in in the community detection, it can be interesting to start with some pre-analysis on the graph.

## 4.1 General statistics

The uppergraph gives us the following information :

- 2% of the quotes match our criterias :

Criteria 1 : speaker is not none

Criteria 2 : only one person is being mentionned in the quote (not 0, not more than 1)

Criteria 3 : the person is identified in our database id_alias_cleaned

- We have in average 26 000 quotes per year to construct our graph
- The years seem to have little effect on the features
- Our subjects (speaker + person being mentionned) are in average 80% men
- The speakers are in average 56 years old
- The persons being mentionned are in average 71 years old (15 years more than the speakers)

[COOL GRAPH OF THIS]

### 4.1.1 Compound score

Following we decided to observe the value of the compound score:

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%204.png)

The uppergraphs give us the following information :

- In average our quotes have a mean compound score of 0.24
- The maximal and minimal value of the compound score are respectevly 1 and -1
- The years seem to have little effect on the features
- 17% of our quotes are neutral (compound_score equal to 0)
- 60 % of our quotes are positive (100 000 quotes)
- 23 % of our quotes are negative (40 000 quotes)
- The histogram shows us that we do not have a gaussian distribution.

This means that we have 3 times more positive quotes than negative quotes.

### 4.1.2 Most popular people

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%205.png)

We can see that the most popular subjects are mainly man politicians. Donald Trump is leading the group almost every year, with a popularity score that can be 6 times bigger than the second most popular subjects in the same year.

## 4.2 Fun facts

### 4.2.1 Old vs Young

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%206.png)

### 4.2.2 Women vs Man

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%207.png)

### 4.2.3 Trump and Biden vs Gender

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%208.png)

In this step it would be nice to have a graph with the node which is bigger the count is high ( even drawn "by hand". Like in this image

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%209.png)

## 4.3 Too many null features

When we added the features from wikidata to each node in the graph, from a statistical analysis we observed mos of them are None.
Since our goal is to describe the communities we will detect in the next steps, we found important to have a good amount of features with no null values.

That's why we decided to reduce the number of the nodes in the graph to the top 500 and to label them manually.

This would bring us to have enough data to continue our studies.

[plot showing number of null features]

# 5. Communities with Louvain Algorithm

## 5.1 Community detection

In the last step we finally run the Louvain Algorithm for obtaining our communities based only on the weight of the edges which is given only by the sentimental analysis of the quotes.

The result we obtain is quite fascinating:

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%2010.png)

[plot graph]

From here we can observe how there are 3 main communities:

- green
- purple
- blue

We remark that also many other little communities have been found during the execution of the algorithm, but their population is quite small, then we won't consider them.

## 5.2 Community analysis

By studying the distribution of the features in the top 3 communities we observe:

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%2011.png)

Since the number of feature is relatevly high, it may be hard to understand which are the mos characteristich features of the communities.

In order to understand this we decided to use both a chi-squared and a mutual information measure between each feature and the community assignemnt value. This lead us to this results:

![Untitled](Social%20Graph%20-%20Quotebank%20ebf197672f5d4618afacdef656aa1d19/Untitled%2012.png)

We understand then that the most important features are:

1. party
2. religion
3. ethnic_group

Indeed by observing the graph previous distributions plotted we can find that the most popular party in the 2° community is the Democrat Party, whereas in the 3° community is the Republican.

We are now curious to see who is the most popular person in each of this communities.

We find out that:

- Trump is the main node in 3° community !!
- Biden is the main node in 2° community !!

This result is amazing: we managed to reobserve the 2 political parties in this social graph only based on the quotes of each speaker. 

[eventually there are other interesting community emerging of which we can talk about ]

**This leads us to say that by how we speak, and by who we speak about, it is possible to understand in which community we belong!**

[maybe some party animation]

[https://giphy.com/embed/IwAZ6dvvvaTtdI8SD5](https://giphy.com/embed/IwAZ6dvvvaTtdI8SD5)

# 5. Changements over time

In the end we decided to repeat the same procedure of before for observing 

[https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
