---
layout: default
---

![Famous Oscars 2014 Picture](https://img.theepochtimes.com/assets/uploads/2014/03/AP788938341654.jpg)
_The famous selfie shared by Ellen DeGeneres with actors, front row from left, Jared Leto, Jennifer Lawrence, Meryl Streep, Ellen DeGeneres, Bradley Cooper, Peter Nyongo Jr. and, second row, from left, Channing Tatum, Julia Roberts, Kevin Spacey, Brad Pitt, Lupita Nyongo and Angelina Jolie during the Oscars 2014 (AP Photo/Ellen DeGeneres) (AP)_

---

**"It's Not What You Know. It's Who You Know"**

Everyone has heard this quote or experienced it first hand. It is no secret that social network has a large impact on career and personal life.

Maybe the first industry that comes to mind is the movie industry. In this project we analyzed the social network of people who work in the movie industry. We used the [CMU Movie Summary Corpus](https://www.cs.cmu.edu/~ark/personas/) and [IMDb](https://www.imdb.com/interfaces/) datasets to do our analysis.

We analyzed the large social network by dividing into clusters and then we zoomed into the significant clusters to further investigate why these clusters exist and who are the key people that bridge different clusters. For each cluster, we looked at their common attributes and tied them into real movie industries.

**Our research questions,**

- Are there clusterings between the people in the film industry?
  - What are common attributes of people in the same cluster?
  - Are there people without a cluster? What makes them special?
- As representation and diversity have more space in conversations today, do we observe the same awareness in the film industry?
  - Do women are employed equally with men?
  - How diverse is each cluster?

# The Data We Worked With

We merged IMDb dataset into our given dataset to have more entries and features such as ethnicity which we will use in the later stages. Since we are investigating the connections between people, we wanted to minimize the matches between people who would not be able to meet in real life due to the year they were active. To choose the best time period, we looked at the number of movies per year.

![Initial dataframe](/images/initial_movie_count.png)

As seen from the plot, 1996-2011 (both inclusive) period has the highest number of movies. We have 14800 movies, 74973 people with 38 different professions.
One may ask why didn't we just count the number of rows and select a period? The answer will be clear after understanding what are we trying to visualize with the dataset.

## How to visualize?

First and foremost, we are interested in how people are connected to each other. Building a network graph is the best way to illustrate this. Thus, we need to define the nodes and edges of the graph. Nodes represent people and an undirected edge between two people exists if they worked in the same movie. To make our network less sparse, we made our initial period decision on movie count. Intiutively, when two people work with each other more, they build a stronger connection. To reflect that, our edges are weighted by common number of movies.

Now, lets build our graph to visualize what we talked about.

We turned our attention to the Fruchterman-Reingold force-directed algorithm, which produces beautiful networks. Those graphs emphasis the position of the nodes, assuring as few edge crossings and distance disparities as possible. The algorithm works similarly to interaction between attractive and repelling forces. The edges between nodes are the springs which pull closer, and the nodes themselves are the object exercing push.

🔎 Use the built-in magnifying glass to take a closer look.

<div class="img-magnifier-container">
  <img id="network" src="images/19960101-20120101_cropped.jpg" width="1000" height="900" alt="Initial network">
</div>

# Clustering

We see some natural clusterings occur, so we run a clustering algorithm to understand the reason. We used OPTICS clustering algorithm, which has foundations from DBSCAN but allows rejecting clusters under a certain size. We choose to reject clusters under 400 people to focus on more generalizable clusters.

To simplify the graph, we only show the names of people who were in more than 24 movies.

🔎 Use the built-in magnifying glass to take a closer look.

<div class="img-magnifier-container">
  <img id="optics" src="images/cluster_optics.jpg" width="1000" height="900" alt="OPTICS network">
</div>

Looking at the cluster graph, we observe 5 large clusters (differentiated by color) and some outliers. We are curious whether these clusters represent or resemble real clusters such as Hollywood, Bollywood and so on. The name of these movie industries come from where they are located. For example Hollywood is a neighbourhood in Los Angeles, California USA and Bollywood gets its name from its birthplace, Bombay (now Mumbai), India. Since movies are generally catered towards the local audience, the native languages are used. This also means that majority the actors, directors and writers are from that nationality or native speakers of the language.

From this analogy, we made an assumption that ethnicity data will reveal us which movie industry they resemble. The ethnicity information comes from CMU dataset, and available only for actors. But we can assume that the distribution of the people with ethnicities (although a small percentage) will be inline with the distribution if CMU dataset had the feature too.

## Diving into clusters

Let's plot the etnicity distribution of each cluster. We only considered non NaN values for the plotting. Note that the results of the clusters heavily depend on the algorithm and the parameters we used.

![Ethnicity in Clusters](/images/ethnicity_clusters.png)

**Cluster 1:** We can see that Americans with different descents are observed the most with ~34%. Thus we can say that this is the cluster that represents Hollywood.

**Cluster 2:** This cluster represents Bollywood with 60% Indian population. Punjabis who live in eastern Pakistan and northwestern India are also considered very active in Bollywood cinema. We will call this cluster Bollywood (North).

**Cluster 3:** This cluster is rather interesting. It has an equal percentage of Japanese and Chinese Americans. Then we have 2 more ethnicities with Asian descent. So we can call this Asian Cinema.

**Cluster 4:** This one is Korean Cinema with more than 80% Koreans in it.

**Cluster 5:** We also see Indians as a majority ethnicity similarly to Cluster 2. The difference is the second biggest ethnicity, Tamils are from India (Southern regions) and Sri Lanka. So this is also Bollywood, to distinguish we will call this Bollywood (South).

Let's plot the ethnicity plot again with new cluster names, we will use these names for each cluster from now on.

![Ethnicity in Clusters](/images/ethnicity_clusters_named.png)

Since we named the clusters, we can look further into the some interesting statistics.

## Popular Genres

Depending on many factors such as culture and history, each audience enjoy different genres of movies. We decided to plot the genre distribution of IMDb dataset because CMU genres are more specific but less consistent due to the crowdsourcing nature of data. And IMDb genres are less (19 of them) but more consistent.

![Genres in Clusters](/images/genre_clusters.png)

Drama is clearly a favorite of all clusters. It is by far the most popular genre in Bollywood, where in Asian cinema Action movies are as popular as drama movies. Comedy comes second in Hollywood. In Korean cinema we see a more equal distribution between genres.

> Globally, drama movies made around $42 billion in the 90s, and $75 billion in the 2000s. Adventure and action movies made around $26 billion and $29 billion, respectively, during the 90s, and by the end of 2010. (Source: [Box Office Mojo](https://www.boxofficemojo.com/))

## Profession

Our dataset has people with different professions such as actor, actresses, editors and so on. Let's take a look.

![Profession Distribution](images/clusters_professions_distribution.png)

Our professions data comes from IMDb, and is generated from the credits.
If we focus on the professions that have a higher percentage, they being actors, actresses, producers and directors, we see that all clusters have similar distribution with 5-10% difference.

Interestingly, we have much more actors than actresses with a large difference. According to New York Film Academy, for one actress there are 2.25 actors. Does this inequality happen also behind the scenes? Lets look at the general gender percentage.

## Gender

![Gender Percentage](images/gender_cluster_1.png)

Unfortunately, we see a deeper cut between male and female employment. Korean Cinema seems to be the most inclusive one with only a 10% difference. Whereas Hollwood has almost 30% difference, meaning there is one female per two male. And this is very recent times! 75.25% of people in our dataset have a gender attribute, so these results truly reflect the real distribution.

> Globally, the ratio of men working in movie industry to women is 5:1. (Source: [New York Film Academy](https://www.nyfa.edu/film-school-blog/gender-inequality-in-film/))

## Age

In movies we often see characters from different ages, this means an actor/actress can do their profession for a long time. But with any performative job, they have their "golden years". We are curious to see what these golden years mean for each cluster. We calculated the age of actors and actresses by taking the difference of the year the movie is released and their birthday. We only used actors and actresses, since Note that since some people played in multiple movies, they are counted as many times. We removed Bollywood (South) cluster in our analysis, since we only had 2 people who had a valid age value.

![Age](images/cluster_age_actor.png)

Not to our surprise, 21-40 years old actors and actresses are the majority in all clusters. It is followed by 41-60. We could say that ageism is very clearly seen in Hollywood and Bollywood. Korean Cinema has more representation for young(20-) and old(60+) people.

> A recent calculation cited by the Guardian, claimed that in 2014’s 20 highest-grossing films, on average 3 out of 10 actors were women, and only 8% of those actors were between 40 and 59, with a shockingly low 2% over 60. (Source: [Reader's Digest UK](https://www.readersdigest.co.uk/culture/film-tv/ageism-in-film-is-hollywood-finally-acting-its-age))

# Outside the Clusters

## Outliers

Before diving into the people, let's look at more data.
As we saw in the clustering part, we have discovered five clusters in our network and, in addition, more than half (53.4%) of our nodes are outliers. We define outliers as people who are not placed in a cluster. The reason they are outliers mainly caused by working in movies from different clusters.

Despite us classifying them as "outliers", these people are of course still connected to the graph. Let us compare the connectivity between each clusters and outliers.

![ECDF Connections](images/outliers_ecdf_connections.png)

We see that connectivity is heavy tailed. Additionally, outliers are less well connected than any of the clusters. Interestingly, the distributions are very similar.

As the connectivity distribution is heavy-tailed, we consider the top 1% most connected outliers for our subsequent analysis. Indeed, it makes sense that outliers are less connected on average than cluster members, otherwise they would have formed a cluster with other outliers, or joined another cluster. For this reason, the most connected outliers are what is interesting. They are the ones that might be, for example, bridges between clusters. Equally well connected to both, which lead them to not be assigned to either of them.

But how well these 1% is connected to the others?

![Outlier Connectivity](images/outliers_top1__connnectivity.png)

We see that even the top 1% of most connected outliers are not that well connected, with 74.7% of them having less than 100 connections. We see that some of them are truly isolated, but most of them are close to clusters.

Let's look at who are these 1% outliers.

![Outlier Professions](images/profession_outlier.png)

The plot shows that actors, music department directors and actresses are more likely to work globally. We can explain the lack of people with behind-the-scenes professions as outliers by saying that people with these professions are hired by film studios and they typically operate in their own countries.

Now let's look at the ethnicity distribution of outliers. To compare, we also plotted the ethnicities of people with clusters.

![Ethnicity Distribution](images/outliers_ethnicities_distribution.png)

We observe that native English speakers are well represented in clusters, and they are also represented as outliers are English. It makes sense since there are many movie industries which are located in English speaking countries.

We see some ethnicities located in India and neighbouring countries such as Punjabis, Marathis, Bengali adn Sindhis who are outliers. This could indicate that their native movie industries are not big enough to be standalone. Hence their members often collaborate with people from other movie industry hotspots such as Bollwood.

## Key People

We define the key people as outliers who have many connections to clusters or other outliers. Let's look at two notable people.

- A.R. Rahman, an Indian music composer. He is mainly known in the West for his work on Slumdog Milionaire, which he won 2 Oscars for. He worked on 52 movies, almost exclusively from India. Due to his popularity there, he is very well connected to the two India related clusters (112 and 90 connections respectively), as well as to other outliers (225) connections, due to his involvement in music videos and tv series.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 30%;"
    src="images/arrahman.jpg">

- Takashi Miike, a Japanese director most known for 13 Assassins. He directed 33 movies, and many more low budget TV series. He has very little connected to clusters (5 connections to cluster 0, 43 connections to cluster 2) compared to his 250 connections to other outliers.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 30%;"
    src="images/takashimiike.jpg">

# Conclusion

We found many clusters with various sizes by running OPTICS clustering algorithm, then we only kept the clusters with size larger than 400. When we looked at the common attributes of people in the same cluster, we saw that ethnicity value plays an important role. And we were able to map these clusters to real life clusters. Some clusters had more people and some had less due to the data we had. Mergin IMDb dataset with CMU dataset allowed us to have more rich data. Since CMU dataset was built by volunteers, it had some biases towards the volunteers' preferences, nationality and so on.

When we looked into the clusters, we saw that some clusters had more diverse ethnicities with non-negligable percentages. For example Korean Cinema cluster was predominantly Korean (80%), where as Hollwood had people from various ethnic backgrounds such as Jewish people and Italian Americans. Bollywood clusters also included many ethnic groups, which is not a surprise since India is very rich in terms of ethnicities (74 listed in Wikipedia!).

In terms of genre preferences, drama leads everywhere. In the second place, Asian Cinema and Korean Cinema has action; Hollwood and Bollywood (North and South) has comedy.

When we were analyzing professions, we saw that there are less actresses than actors. Looking closer, we saw the gender gap which was very obvious in all clusters. While Korean Cinema was the "most" inclusive, Hollywood and Bollywood had an overwhelming majority of males. Ageism was also very present in Hollywood, and Korean Cinema has more representation of ages compared to others.

More than half of the people were outliers, so it was worth analyzing them. Some outliers had many connections to the people in clusters, some had more connections to other outliers or not many connections at all (loners in the industry).

# Further Details

Our results could vary depending on the clustering algorithm used, the parameters we set such as cluster size cutoff. For example, Jackie Chan, a famous Chinese actor mostly known by his action movies globally, was placed in the Asian cluster. But he is at the very edge of the cluster (ie. very close to being an outlier).

Our main dataset which was sourced from Freebase had many attribute values missing which led us to make further assumptions, however by using IMDb dataset which is more reliable, our assumptions hold.

<img 
    style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 60%;"
    src="images/goodbye.gif">

<style>  
     * {box-sizing: border-box;}

.img-magnifier-container {
  position: relative;
}

.img-magnifier-glass {
  position: absolute;
  border: 3px solid #000;
  border-radius: 50%;
  cursor: none;
  /*Set the size of the magnifier glass:*/
  width: 200px;
  height: 200px;
}
</style>

<script>
function magnify(imgID, zoom) {
  var img, glass, w, h, bw;
  img = document.getElementById(imgID);

  /* Create magnifier glass: */
  glass = document.createElement("DIV");
  glass.setAttribute("class", "img-magnifier-glass");

  /* Insert magnifier glass: */
  img.parentElement.insertBefore(glass, img);

  /* Set background properties for the magnifier glass: */
  glass.style.backgroundImage = "url('" + img.src + "')";
  glass.style.backgroundRepeat = "no-repeat";
  glass.style.backgroundSize = (img.width * zoom) + "px " + (img.height * zoom) + "px";
  bw = 3;
  w = glass.offsetWidth / 2;
  h = glass.offsetHeight / 2;

  /* Execute a function when someone moves the magnifier glass over the image: */
  glass.addEventListener("mousemove", moveMagnifier);
  img.addEventListener("mousemove", moveMagnifier);

  /*and also for touch screens:*/
  glass.addEventListener("touchmove", moveMagnifier);
  img.addEventListener("touchmove", moveMagnifier);
  function moveMagnifier(e) {
    var pos, x, y;
    /* Prevent any other actions that may occur when moving over the image */
    e.preventDefault();
    /* Get the cursor's x and y positions: */
    pos = getCursorPos(e);
    x = pos.x;
    y = pos.y;
    /* Prevent the magnifier glass from being positioned outside the image: */
    if (x > img.width - (w / zoom)) {x = img.width - (w / zoom);}
    if (x < w / zoom) {x = w / zoom;}
    if (y > img.height - (h / zoom)) {y = img.height - (h / zoom);}
    if (y < h / zoom) {y = h / zoom;}
    /* Set the position of the magnifier glass: */
    glass.style.left = (x - w) + "px";
    glass.style.top = (y - h) + "px";
    /* Display what the magnifier glass "sees": */
    glass.style.backgroundPosition = "-" + ((x * zoom) - w + bw) + "px -" + ((y * zoom) - h + bw) + "px";
  }

  function getCursorPos(e) {
    var a, x = 0, y = 0;
    e = e || window.event;
    /* Get the x and y positions of the image: */
    a = img.getBoundingClientRect();
    /* Calculate the cursor's x and y coordinates, relative to the image: */
    x = e.pageX - a.left;
    y = e.pageY - a.top;
    /* Consider any page scrolling: */
    x = x - window.pageXOffset;
    y = y - window.pageYOffset;
    return {x : x, y : y};
  }
}
/* Execute the magnify function: */
magnify("network", 5);
magnify("optics", 3);
/* Specify the id of the image, and the strength of the magnifier glass: */
</script>
