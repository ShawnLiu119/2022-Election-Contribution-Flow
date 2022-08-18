# 2022-Election-Contribution-Flow
2022-Election-Contribution-Flow_Network Graph Analysis


### Abstract
Political contributions have a profound effect on elections. Campaign contributions could stem from a tiny, wealthy elite or a large corporation or organization that is purposely founded for election such as a PAC. Campaign donations are known to be socially contagious so mapping out and understanding how campaign donations diffuse through the network would be indicative and helpful on election prediction. In this study, a deep dive network analysis leveraging Python and Gephi is conducted to look into 2022 election donation data including organizational donors, receivers, donation amount and so on. The primary goal of this study is to dissect the network from different angle such as Party and organization type and identify top organizational entities measured by donation amount or connection based on network centralities.

### Introduction
Political contributions have a profound effect on elections. Campaign contributions could stem from a tiny, wealthy elite or a large corporation or organization that is purposely founded for election such as a PAC. Campaign donations are known to be socially contagious so mapping out and understanding how campaign donations diffuse through the network would be indicative and helpful on election prediction. In this study, a collection of data regarding election campaign funds are sourced from https://www.data-science-quarterly.io/ and used to generate network graph for further analysis. The graphs generated are visualized through Gephi with an attempt to identify underlying patterns. The network data is organized and stored into Neo4j which provides robust capability of storing and querying the data for further analysis.

###Literature Review
Please refer to the final report

### Method

![image](https://user-images.githubusercontent.com/43327902/185513353-9bf1a475-42bc-4252-afe8-e4823700ebee.png)

The dataset used for this study was downloaded from https://www.data-science-quarterly.io/, which contains contribution and expenditures of 2022 Election Cycle to Date obtained from the Federal Elections Commission. The dataset is structured into two files, one is “Nodes with Attributes”, which contains individual committee names with associated attributes such as organization type, Party name and so on. The other file is “Nodes, Edges, with Corresponding Weights”, which tracks the money flow as edges with amount as weights. The data was then loaded into python notebook and constructed into directed network graph object using networkX package. Then the network graph object was converted into .gexf file and loaded into Gephi for visualization. Meanwhile, the network dataset is saved into graph database by Neo4j via naxneo4j package.

### Result
There are a total of 4,064 nodes (entities) and 4,558 edges in the network. Some exploratory analysis is conducted through embedded feature called Statistics within Gephi. As shown in the Figure 2, the weighted degree is right skewed with average level at $80,398.44 as donation amount. There are 566 connected components detected overall with average degree 1.122.

![image](https://user-images.githubusercontent.com/43327902/185513383-ceed2782-3d67-4e20-9a2e-64fd348e793a.png)
![image](https://user-images.githubusercontent.com/43327902/185513390-9be4f760-14e3-4fee-942a-9b8d223c5b3c.png)

Figure 2. Data Exploratory Analysis

The visualization is primarily implemented through Gephi. A snapshot (Figure 3) of overall network is generated under OpenOrd layout, which seems to be outstanding layout converge faster with capability to separate congested nodes into communities in a cleaner format.

![image](https://user-images.githubusercontent.com/43327902/185513423-b7aed20b-00c5-4ccf-a423-8c194a37b173.png)

Figure 3. Snapshot of network graph

To demonstrate the pattern and extract key insights out of the pattern, a few experiments have been done to exaggerate some aspects of the graph. Nodes are resized (ranking) by degrees and color coded based on modularity class, while edges are partitioned by weight intuitively as shown in Figure 4.

![image](https://user-images.githubusercontent.com/43327902/185513456-792b3993-2779-4ada-8904-aac4db3923f4.png)

Figure 4. Network Graph with Node & Edge Partition

By apply filtering feature based on degree range (degree > 70), the bigger nodes in the Figure remain including Actblue (153), Fair Fight(115), Winred(169), American Veteran Society PAC(149), Morongo Band of Mission Indians(81), Poarch Band of Creek Indians(81), The Chickasaw Nation(86). It makes good sense that Actblue has highest degree centrality as the organization dedicates to grassroot fundraising programs so that more individual donors that regular organizations who focus on enterprises. Interestingly, all these nodes except American Veteran Society PAC are categorized under same Modularity Class, suggesting their closer relationship.

![image](https://user-images.githubusercontent.com/43327902/185513485-16753887-4b75-4b82-b41f-e94d1ac0a20e.png)

Apart from nodes with high degree centrality, several weighted edges are also highlighted in the graph. Below is the list of edges (donation) with fund more than $ 10 million.

![image](https://user-images.githubusercontent.com/43327902/185513516-aff19750-97d5-4617-aab2-ecfcaa5a1d98.png)

Community detection analysis is conducted via Modularity feature embedded in Gephi (randomized, resolution 10). 569 communities have been detected suggesting the sparse distribution of nodes across different communities. The top three communities are color-coded accounting for almost 50% of nodes with largest community accounting for 27.26% nodes as shown in Figure 3. Five out of seven nodes with 70+ degrees are within this community including Winred, Actblue, The Chickasaw Nation and Morongo Band of Mission Indians, and Poarch Band of Creek Indians, which probably makes sense that the latter three nodes are all Indian tribe related as details shown in Figure 4.

![image](https://user-images.githubusercontent.com/43327902/185513539-6711dbb2-5c9c-4b5c-9c88-387b361e5d4b.png)
![image](https://user-images.githubusercontent.com/43327902/185513552-4b7a02ad-16ef-4148-b39e-325b22f3a0b7.png)

Figure 5. Modularity Community Detection – Gephi

![image](https://user-images.githubusercontent.com/43327902/185513568-c54e224b-02c9-45af-9576-a26696feec53.png)

Figure 4. Nodes with 70+ degrees in community 254

Another look was taken on betweenness centrality of nodes. The betweenness centrality above 200 have been highlighted. Winred and Actblue are ranked top 2 with highest betweenness centrality, which is also reflected by their in-degree and out-degree. Winred is with 129 in-degree and 40 out-degree while Actblue has 82 in-degree and 71 out-degree. This could be partially explained by the fact that WinRed is the official secure payment technology via which a lot of donations are wired to candidates (https://winred.com/about/).

![image](https://user-images.githubusercontent.com/43327902/185513587-7cca59ec-2db0-4a57-a4a3-47d9a72ec35d.png)

Figure 6. Network Graph – Nodes Partitioned by Between Centrality

Views of nodes portioned by attributes have been mapped out. One experiment carried out is to compare the network of nodes with partition by Ctype and ranking by degree versus weighted degree.

![image](https://user-images.githubusercontent.com/43327902/185513608-d7cf5a6c-3c69-4ba8-b952-23a0ed6f822d.png)
![image](https://user-images.githubusercontent.com/43327902/185513620-10a9b747-3d59-4ba5-81cf-5a367c20c94b.png)

Figure 6. Network Graph – Nodes with Size Ranking by Degree vs Weighted Degree

In the graph in which nodes are sized by degree, WinRed and Actblue are the top 2, both are PAC with non-contribution account – nonqualified. While in the graph in which nodes are sized by weighted degree, two independent Super PACs pop out, Senate Majority PAC and Senate Leadership Fund, as top 2 with weighted average degree above $25 million. The former serves solely Democratic while the latter offers support to Republicans, both are legacy PAC running for senate race with donations mainly from large enterprises or group.

A deeper dive is taken to look at the e go network of top 2 super PACs. Figure 7 and 8 shows the ego network of SMP (Senate Majority PAC) and Senate Leadership Fund. For SMP, the biggest donation comes from Majority Forward with $14.3 Million, Majority Forward is a 501(c)(4) organization working with Senate Majority PAC as clarified on their website (https://www.majorityforward.com/). While the biggest donation source for Senate Leadership Fund is One Nation.

![image](https://user-images.githubusercontent.com/43327902/185513676-7464c1f3-9faa-442d-b1fb-2de444113a81.png)

Figure 7. Ego Network Subgraph – SMP

![image](https://user-images.githubusercontent.com/43327902/185513688-a3c5331a-7e55-4ec6-a71d-feb19249181c.png)

Figure 8. Ego Network Subgraph – Senate Leadership Fund

Figure 9 demonstrates the ego network of WinRed including 166 nodes and 203 edges, there are more neighboring nodes and associated edges out of which the highest two transaction are wired to NRSC (National Republican Senatorial Committee) with $504.8 thousands and to NRCC (National Republican Congressional Committee) with $477.7 thousands. Figure 10 illustrates the ego network of Actblue, the subgraph involves 154 nodes and 192 edges with highest weight of $20k flowing to Sunflower Seeds PAC.

![image](https://user-images.githubusercontent.com/43327902/185513731-dcaf164e-a853-4784-9652-69bcfeacaad6.png)

Figure 9. Ego Network Subgraph – WinRed

![image](https://user-images.githubusercontent.com/43327902/185513749-1236b105-4dd7-472e-afe0-f2193065ce6d.png)

Figure 10. Ego Network Subgraph – Actblue

To get better understanding on the nodes where the money ends up with high in-flow, another graph view is generated so that nodes are resized based on weighted in-degree and portioned by Ctype as shown in Figure 10. Among the top 10 entities, 8 of them are super PACs including SMP, Senate Leadership Fund, 1 of them is House Majority PAC, which is PAC with non-contribution account as one type of hybrid PAC according to Federal Election Commission. The remaining one is NRCC, as party-qualified for Republican

![image](https://user-images.githubusercontent.com/43327902/185513788-47f3f988-66ea-4fe8-a33a-3d1a05fe78a5.png)

Figure 11. Ego Network Subgraph – Actblue

![image](https://user-images.githubusercontent.com/43327902/185513803-5e9a0308-7c6d-447e-a6fa-6edf2ac23748.png)

While the data has been pumped into Gephi for visualization, it has been populated into Neo4j database for storage. The Figures below provides some snapshots of the “follow the money” data stored in Neo4j and some queries that have been run for testification purpose.

![image](https://user-images.githubusercontent.com/43327902/185513813-60fcda8e-0c56-4fbf-816a-b30f03b37987.png)

![image](https://user-images.githubusercontent.com/43327902/185513821-b1fcd87b-becf-4152-abf9-45a5aee3dd33.png)

![image](https://user-images.githubusercontent.com/43327902/185513833-34a9c2c1-e46d-4b33-96c0-c8fcf55f3530.png)

![image](https://user-images.githubusercontent.com/43327902/185513867-0eabbf4f-3495-4fea-b0de-16cee727c1e8.png)


### Conclusion
Super PACs such as SMP (Senate Majority PAC) and Senate Leadership Fund are still the final destination for large lump sum of donation fund. For more fragmented donations from individual donors or smaller organizations, the donation are likely to be wired through middle man such as WinRed, those with higher between centralities, and converges at the Super PACs or some other larger organizations.






















