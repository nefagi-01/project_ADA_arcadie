# Social Graph - Quotebank

# 1. Abstract and objectives

The starting point of this datastory is the ***[QuoteBank](https://dlab.epfl.ch/people/west/pub/Vaucher-Spitz-Catasta-West_WSDM-21.pdf)*** dataset,  "an open corpus of 178 million quotations attributed to the speakers who uttered them, extracted from 162
million English news articles published between 2008 and 2020".

The key idea of the projects revolves around the fact that in this enormous amount of quotation it may exist a good portion containing quotation referencing someone else.

For example, since many of this quotations come from news articles, it is likely that they will be from politic exponents stating something about other political or well-known public figures.

From this considerations we draw our project goals:

1. Is it possible to create a *social-graph* where each **node** is a **person** and the **edges** represent a **quotation** where "person A stated something about person B"?
2. What if this **edges** have a **weight** depending on the sentiment of the statement (i.e when a quotation is positive the weight is small, and when it is negative it is high)?
3. Would this particular choice of the weight lead to a graph where communities can be observed in clusters of the graph since people having good interaction between them (i.e having good statements about each other) would be near, whereas those having bad interactions would stay apart?
4. Which communities can we find in the clusters graph? (Political parties, Male/Female groups, etc...)
5. Can we see a changement of this clusters over time?

# 2. Some statistics about the data

## 2.1 How much data?

As a starting point looking at the proportion of quotes mentioning someone and those not mentioning someone can give us an idea of the size of data we dispose.

[Plot]

## 2.2 Who speaks more? Who more mentioned?

Before creating the graph, we're curious to see who are the most mentioned people and who are the people quoting the most.

[Plot]

## 2.3 Sentiment statistics

What is the prevalent sentiment in the quotes from mostly news articles?

[Plot]

# 3. The social-graph

Here it is:
{% include social_graph.html %}

# 4. Clusters and Communities

Here they are:
[Plot]

# 5. Changements over time

