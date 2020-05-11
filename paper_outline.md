# Make GraphRNN scalable #

## 1.Introduction ##
Graphs are omnipresent and they are the natural presentations of many phenomena in our lives, e.g. Social networks, Road networks, Gene Regulatory networks and Bitcoin Transaction Graphs. Graph analysis to unravel the underlying patterns and dynamism, as a fundamental research direction, has been attracting a lot of interests. Graph generative models is based on graph analysis and aims to generate synthetic graphs that matches the properties of target graphs. 

There are many applications of graph generative model. For example, a coroporation would like to share its user network with its partner for development, but due to the information security, the real graph can not be shared, so such a similar but synthetic graph is a good replacement. Also (another example)

The generative models have a long history. Dating back to 1959, Paul Erdős and Alfréd Rényi have proposed the first graph generative model Erdős–Rényi model to generate random graphs. Instead of generating random graphs, later studies tried to model some properties of target graphs. Barabási–Albert model aims to generate scale-free networks whose degree distribution follows a power law which is very common in human-made networks such as: the internet, world wide web, social network and so on. (Other traditional graph generatvie models) 

All above models are considered as traditional graph generative models since they all have a priori assumption about the structure of target graphs. However, for complicated real-world graphs, these assumptions may not accurately describe the underlyling structural information or only consider part of properties. Therefore, traditional graph generative models do not perform well on real-world graphs. To overcome this problem, DNN-based models have been proposed. Instead of setting assumptions about the properties of target graphs, DNN-based models learn the properties directly from a set of target graphs. DNN-based models hence are much more flexible than traditional ones and are able to learn and generate various types of graphs (studies have shown DNN-based models outperform traditional ones and briefly introduce some DNN-based models)

However, DNN-based models have been suffering from scalability problems. All experiments done in those works only use small graphs (list the largest graph size in some DNN-based models). GraphRNN, as one of the most influencial DNN-based graph generative models, also faces the scalability problem. In our experiments, the largest graphs that can be trained in GraphRNN under its default settings only contain about 1500 vertices (on a GPU with 12GB memory), which is far below the scale of most real-world graphs. This problem has severely hindered the usage of GraphRNN in a broader range of real-world applications.

The reason for the scalability problem of GraphRNN is based on the __constraint__: the training requires the entire graph info for both forward propogation and backpropogation to function well because of the __common perception__:

    There are strong interactions among graph vertices and edges: the newly generated vertices are based on previously generated vertices and, on the side, newly generated vertices affect the degrees and connectivities of previously generated nodes.

In GraphRNN, graphs are represented as sequences. Therefore, long sequences of large entire graphs as well as corresponding large underlying computaional graphs are all maintained in the GPU memory, which are memory-costly.

In this work, we give a systematic study on the scalability issue. Specifically, we aim at answering the following questions:

 * Is the common perception correct? Although the perception is
 intuitive at the first glance, its truthfulness has never been
 systematically examined.

 * Is it possible to relax the assumed constraint to both address the
   scalability issue and maintain enough quality of the generated
   graphs?

 * What are the possible ways to do that? How well do they each work
   empirically, in terms of result quality, execution speed, and scalability?

In the following sections, we will introduce our explorations to achieve the scalability of GraphRNN by relaxing the constraint. By comparing the performance of GraphRNN and our scalable methods, we give our answers to all three questions. 

The scalable methods we have investigated can be divided into 3 categories: 

   * Split-Merge methods (give the general idea of Split-Merge methods). To be more specific we have explored two kinds of split-merge methods: (1) Plain split-merge; (2) Snapbutton.

   * Merge-split method (give the general idea of Merge-split methods). Herein, we investigate maximal-matching based Merge-split method

   * Continuous training method (give the general idea of continuous training methods). We have designed a truncated-backpropogation method (* checkpointing: optional,based on progress)

## 2. Background on GraphRNN ##

(Introductions to the mechanism of GraphRNN)

## 3. Scalable methods ##

(Briefly introduce the general idea behind scalable methods: reducing the size of the graph as well as its corresponding computational graph that are maintained in the GPU memory.) 

### 3.1 Split-Merge methods ###
#### 3.1.1 Plain split-merge ####
(introduce the Plain split-merge method in which we split graphs using Fennel and merge using random connection)
#### 3.1.2 Snapbotton method ####
(introduce the Snapbutton method, the motivation is that it avoids the random connection)

### 3.2 Merge-Split methods ###
#### 3.2.1 maximal-matching based Merge-split method ####
(introduce the maximal-matching based Merge-split method, the general idea is opposite to Split-Merge methods )

### 3.3 Continuous training method ###
#### 3.3.1 truncated-backpropogation method ####
(introduce the truncated-backpropogation method, which is from a different aspect than previous methods. Previous methods are to split graphs, truncated-backpropogation method is to split sequence)

## 4. Experiments ##

### 4.1 Performance on MMD score ###
(This experiment should be done for all scalable methods as well as GraphRNN. Since we need to test on GraphRNN, the graph size should not be very large. I plan to test on 4 to 5 types of graphs. For each graph, I test on two sizes (small and large), large is the one that achieves the upper limit of GraphRNN)

### 4.2 Performance on Scalability ###
(This experiment probe the performance of our scalable methods on large graphs which could not be handled by GraphRNN. For this part I hope to test graphs up to 20000 vertices with 2 to 3 types of graphs. I guess that some scalable methods (e.g merge-split) may perform bad, if so, I will exclude it)

### 4.3 Further scalablitily ###
(This part, I will test on even larger graphs, i.e. 200,000 vertices, but due to the time costs, I only report the training time for one epoch.)

## 5. Conclusion ##
To the best of our knowledge, this work is the first systematic exploration of the scalaiblity of GraphRNN. It shows that the common perception of the needs for having the entire graph info in memory during the whole training process is not always necessary. We have proposed multiple scalable methods, which makes the training on large graphs possible. Some of them have shown close performance to GraphRNN while takes a small portion of memory consumption compared with that in GraphRNN. By making graphRNN scalable, a large number of real-world applications could benefit from it. 
