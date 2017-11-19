---
layout: post
title: Improving Information Extraction by Acquiring External Evidence with Reinforcement Learning Summary
categories: jekyll update
---

<style>
{% include blogposts.css %}
</style>

Information extraction (IE) is the task of automatically extracting structured information from unstructured or semi-structured machine readable documents. IE systems require large amounts of annotated data to deliver high performance. This paper summary describes the task of acquiring and incorporating <b>external evidence</b> that improves extraction accuracy when <b>training data is scarce</b>. 

## Databsases used in the paper 
1. Shooting incidents
2. Food adulteration cases

## Sample IE system
### The problem

![fig1](/images/sample_news_article.png){: .center-image }
<center><b>Figure 1: Sample news article on a shooting case.</b></center>
<br>
The document does not explicitly name the shooter (<i>Scott Westerhuis</i>). The total number of fatalities is also not explicitly metioned (<i>A couple and four children</i>*=6*) <br>
This paper suggests that a large annotated training set may not cover all such cases and proposes a system that utilizes external sources to resolve such text interpretation ambiguities.
### The solution
![fig2](/images/2articles.png){: .center-image }
<center><b>Figure 2: Articles with explicit mentions of fatality count and shooter name.</b></center>
<br>
Such stereotypical phrasing is easier to process for most IE systems and the new system boosts extraction accuracy compared to traditional extractor systems by upto 7%.
### Challenges with the new system 
1. Performing event coreference -
	Retrieving suitable articles describing the same event.
2. Reconciling the entities extracted from different documents obtained -
	Sometimes tangential results are obtained as querying for similar documents returns documents about other similar incidents.
	Additionally, values extracted from different sources may be different (as some sources are inaccurate).
	
## Framework
Markov Decision Process(MDP) tackles the challenge of reconciling extracting entities. The system goes through alternating phases of querying to retrieve the articles and integrating the extracted values if the confidence is sufficiently high.<br>
A Reinforcement Learning(RL) approach is is used to combine query formulation(for event coreference), extraction from new sources and value reconciliation.
<br>
![fig3](/images/reinforcement_learning.png){: .floatright}

The model selects good actions for both article retrieval and value reconciliation(<b>action</b>) in order to optimize the <b>reward function</b> that reflects extraction accuracy and penalties for extra moves.<br>
<b>RL agent</b> is trained using a [Deep Q-Network(DQN)](https://deepmind.com/research/dqn/) that simultaneously predicts both querying and reconciliation choices.<br>
Maximum entropy model is used as the base extractor for <b>interpretation</b>.
<br>
## Information extraction task as a Markov Decision Process(MDP)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
		
## Acknowledgements
<cite> 
Narasimhan, Karthik, Adam Yala, and Regina Barzilay. "Improving Information Extraction by Acquiring External Evidence with Reinforcement Learning." Proceedings of the 2016 Conference on Empirical Methods in Natural Language Processing, 2016. doi:10.18653/v1/d16-1261.
</cite>
<br> <br>
https://github.com/karthikncode/DeepRL-InformationExtraction




