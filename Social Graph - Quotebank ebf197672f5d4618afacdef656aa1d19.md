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

# 2. Some statistics about the data

## 2.1 How much data?

As a starting point looking at the proportion of quotes mentioning someone and those not mentioning someone can give us an idea of the size of data we dispose of.

[Plot]

## 2.2 Who speaks more? Who more mentioned?

Before creating the graph, we're curious to see who are the most mentioned people and who are the people quoting the most.

[Plot]

## 2.3 Sentiment statistics

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

## 3.1 How do we create it?

## 3.2 Final result

Here it is:

[Plot]

# 4. Statistics on the graph

## 4.1 General statistics

### 4.1.1 Statistics per year

### 4.1.2 Compound Score

### 4.1.3 Most popular people

## 4.2 Fun facts

### 4.2.1 Old vs Young

### 4.2.2 Women vs Man

### 4.2.3 Trump and Biden vs Gender

Study of julien

# 5. Communities with Louvain Algorithm

Here they are:
[Plot]

# 5. Changements over time

[https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)