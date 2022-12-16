---
layout: default
---

![Famous Oscars 2014 Picture](https://img.theepochtimes.com/assets/uploads/2014/03/AP788938341654.jpg)

###### The famous selfie shared by Ellen DeGeneres with actors, front row from left, Jared Leto, Jennifer Lawrence, Meryl Streep, Ellen DeGeneres, Bradley Cooper, Peter Nyongo Jr. and, second row, from left, Channing Tatum, Julia Roberts, Kevin Spacey, Brad Pitt, Lupita Nyongo and Angelina Jolie during the Oscars 2014 (AP Photo/Ellen DeGeneres) (AP)

_"It's Not What You Know. It's Who You Know"_

Everyone has heard this quote or experienced it first hand. It is no secret that social network has a large impact on career and personal life.

Even going further, we could argue that this quote is created by someone from the movie industry. In this project we analyzed the
social network of actors, writers and directors. We used the [CMU Movie Summary Corpus](https://www.cs.cmu.edu/~ark/personas/) and [IMDb](https://www.imdb.com/interfaces/) datasets to do our analysis.

We analyzed the large social network by dividing into clusters and then we zoomed into the significant clusters to further investigate the people who know each other, why these clusters exist and who are the key people that bridge different clusters. For each cluster, we looked at their common attributes and tied them into real-life clusters such as Bollywood, Hollywood etc.

# The Data We Worked With

We merged IMDb dataset into our given dataset to have more entries, especially for writers and directors. Since we are investigating the connections between people, it would not make sense to have Charlie Chaplin and Brad Pitt in the same dataset. Thus, we decided to run our analysis on 1996-2012 which has the highest number of movies.

![Initial dataframe](/images/initial_movie_count.png)

One may ask why didn't we just count the number of rows and select a period? The answer will be clear after understaind what are we trying to visualize with the dataset.

## How to visualize?

First and foremost, we are interested in how people are connected to each other. Building a network graph is the best way to illustrate this. Thus, we need to define the nodes and edges of the graph. Nodes represent people and an undirected edge between two people exists if they worked in the same movie. To make our network less sparse, we made our initial period decision on movie count. Intiutively, when two people work with each other more, they build a stronger connection. To reflect that, our edges are weighted by common number of movies.

Now, lets build our graph to visualize what we talked about.

We turned our attention to the Fruchterman-Reingold force-directed algorithm, which produces beautiful networks. Those graphs emphasis the position of the nodes, assuring as few edge crossings and distance disparities as possible. The algorithm works similarly to interaction between attractive and repelling forces. The edges between nodes are the springs which pull closer, and the nodes themselves are the object exercing push.

![Initial network](/images/initial_network.jpg)

We see some natural clusterings occur, so we run a clustering algorithm to understand the reason. We used OPTICS clustering algorithm, which has foundations from DBSCAN but allows rejecting clusters under a certain size. We choose to reject clusters under 400 people to focus on more generalizable clusters.

![Cluster Optics](/images/cluster_optics.jpg)

We have 5 large clusters (differentiated by color) and some outliers. Now we can look into each cluster, understand the patterns inside and who these outliers are.

# Cluster 1

- Cluster size, the common attributes, statistics of gender, ethnicity, age etc.
- Try to name the cluster to the real-life cluster (is it hollywood etc.)

# Cluster 2

# Cluster 3

# Cluster 4

# Cluster 5

# Outliers / Key people

# Conclusion
