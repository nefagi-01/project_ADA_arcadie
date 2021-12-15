# Social Graph - Quotebank


https://user-images.githubusercontent.com/73229139/146226763-bb3fea3d-0ed8-44c7-b5ac-00c635841ed8.mp4


# 1. Objective of our project

https://user-images.githubusercontent.com/73229139/146226837-b08db727-ce3b-4530-9f5f-85bcfe47777e.mp4

# 2. How we constructed our graph ?

https://user-images.githubusercontent.com/73229139/146237783-9757d343-5693-4972-b7e7-eb5d23348edd.mp4

Our Graph has YYY quotes, here is a snapchot of its columns : 

<img width="1199" alt="Capture d’écran 2021-12-15 à 19 09 00" src="https://user-images.githubusercontent.com/73229139/146241584-ee8c7972-c116-486f-bc79-808ece6d38c8.png">

# 3. A look at the most popular subjects

We define the popularity by the sum of the number of time a person was mentioned and the number of time he spoke! Let's have a look at the most popular subjects in our graph of the last couple of years :

![38b0abe1-1841-4cca-a201-1bb9d2668fc1](https://user-images.githubusercontent.com/73229139/146242198-38a3433e-3f8f-4872-9766-e81549299205.jpg)

A lot of political figures, and mainly american ones! To go further in our statistical analysis, we select the 500 most popular subjects of our graph. Thanks to the database Wikidata, we can link each subject with some features such as gender, age, political party, religion, academic degree and nationality! Lets check who we have in the 500 most popular subjects :


![example](https://user-images.githubusercontent.com/73229139/146243284-9500b08e-4cbc-48de-85ff-64cde57fb63d.png)


# 4. We now can plot our social graph based on the compound score!


![graph_image (3)-11](https://user-images.githubusercontent.com/73229139/146244008-17874d3c-6112-4edd-a172-9b183c22daae.png)

We use louvain method to find communities:

![graph_image (3)-1](https://user-images.githubusercontent.com/73229139/146244030-00aa6405-5ce4-4f1a-9ddd-b9eac3683b18.png)

The method found N communities, lets see how many subjects do we have per community :

![Artboard_wgjng](https://user-images.githubusercontent.com/73229139/146244111-4b5683d8-ee0e-472b-afde-f6f5fa74f4e3.png)



# 5. Lets identify the communities 

!!!show statistics about communities!!!

# 6. Changements over time 

Lets check for the different years 

# 7. Conclusion 

People are mean. 



****************************************************************************************************************************************

<iframe width="420" height="315" src="https://www.youtube.com/watch?v=dQw4w9WgXcQ" frameborder="0" allowfullscreen></iframe>



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

# 2. How did we construct our graph ? 




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
**Grab the nodes**:
{% include social_graph.html %}

# 4. Clusters and Communities

Here they are:
[Plot]

# 5. Changements over time

