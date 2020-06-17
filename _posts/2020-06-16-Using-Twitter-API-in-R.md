---
layout: post
title: Using Twitter API in R
---

I used data from 100 @BrookingsInst tweets and created a graph to show the relationships between twitter users.

![](/images/iGraph%20Final.png)

While the size of the label implies number of mentions, the other prominent nodes did not show up until I changed the number of mentions to _more than 5_ (instead of Professor's program of more than 20). With a smaller data set of a 100 tweets, this makes sense.
Of course, labeling these other nodes would help identify potential major relationships with the Brookings Institute. As the Institue is a prominent think tank, potential influences in both directions could shed light on biases and more.

Thus, it's worth looking into, and even creating more graphs, for the accounts
* @blackcapitol
* @SandyDarity
* @DukeAAAS
* @Jenkinsbd
* @VFelbabaBrown

Here's my code, mainly using Dr. Ho's sample program:

    #100 Tweets from @BrookingsInst, last updated June 16
    brookings <- rtweet::search_tweets(q = "BrookingsInst", n=100, lang = "en")

    #Filtering Data
    filter(brookings,retweet_count>0)%>%
      select(screen_name, mentions_screen_name)%>%
      unnest(mentions_screen_name)%>%
      filter(!is.na(mentions_screen_name))%>%
      graph_from_data_frame() ->brookings10

    #LabelingNodes
    V(brookings10)$node_label <-unname(ifelse(degree(brookings10)[V(brookings10)]>5, names(V(brookings10)),""))
    V(brookings10)$node_size<-unname(ifelse(degree(brookings10)[V(brookings10)]>5,degree(brookings10),0))

    #Creating Graph using fr and labeling as x
    x<-ggraph(brookings10, layout = 'dh') +
      geom_edge_arc(edge_width=0.3,aes(alpha=..index..))+
      geom_node_label(aes(label=node_label,size=node_size),
                  label.size=0,color="blue",repel=T)+
      coord_fixed() +
      scale_size_area(trans="sqrt") +
      theme(legend.position="none") 

    #Centering Label
    fortitle <- theme(plot.title = element_text(hjust=0.5, face = "bold", size = (15)))

    #Finalizing Graph for Twitter User
    x + fortitle + labs(title="iGraph of BrookingsInst account and mentions",subtitle="Edge = Retweets \nScreename Size =     influence")
    
---
***
    
