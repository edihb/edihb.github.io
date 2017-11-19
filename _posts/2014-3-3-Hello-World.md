---
layout: post
title: "Improving Information Extraction by Acquiring External Evidence with Reinforcement Learning" Summary
category: jekyll update
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
This paper suggests that a large annotated training set may not cover all such cases and proposes a system that utilizes <b>external sources</b> to resolve such text interpretation ambiguities.
### The solution
![fig2](/images/2articles.png){: .center-image }
<center><b>Figure 2: Articles with explicit mentions of fatality count and shooter name.</b></center>
<br>
The new IE system involves querying for such alternative articles and extracting information from them instead of the original source. <br>
Such stereotypical phrasing is easier to process for most IE systems and the new system boosts extraction accuracy compared to traditional extractor systems by upto 7%.<br>
### Challenges with the new system <br>
1. <b>Performing event coreference -</b><br>
Retrieving suitable articles describing the same event.
2. <b>Reconciling the entities extracted from different documents obtained -</b><br>
Sometimes tangential results are obtained as querying for similar documents returns documents about other similar incidents.<br>
Additionally, values extracted from different sources may be different (as some sources are inaccurate).<br>
	
## Framework
<b>Markov Decision Process(MDP)</b> is introduced to tackle the challenge of reconciling extracted entities. The system goes through alternating phases of querying to retrieve the articles and integrating the extracted values if the confidence is sufficiently high. It satisfies the <i>Markov Property</i> as the future state of the model depends only on the present state and not on the sequence of all preceeding states.<br>
<b>A Reinforcement Learning(RL)</b> approach is is used to combine query formulation(for event coreference), extraction from new sources and value reconciliation.
<br>
![rl](/images/reinforcement_learning.png){: .floatright}

The model selects good actions for both article retrieval and value reconciliation(<b>action</b>) in order to optimize the <b>reward function</b> that reflects extraction accuracy and penalties for extra moves.<br>
<b>RL agent</b> is trained using a [Deep Q-Network(DQN)](https://deepmind.com/research/dqn/) that simultaneously predicts both querying and reconciliation choices.<br>
Maximum entropy model is used as the base extractor for <b>interpretation</b>.
<br>

## Information extraction task as a Markov Decision Process (MDP)
Introducing the MDP framework is important as it allows dynamic integration of entity predictions while simultaneously providing flexibility to choose the type of articles to extract form(query template).<br>

![mdp](/images/mdp_transition.png){: .center-image }
<center><b>Figure 3: Illustration of transition in MDP.</b></center>
<br>
The top box of each state depics the current entities and the bottom box shows new entities extracted from a downloaded article on the same event after querying.<br>
At each step the MDP decides whether to accept an entities value and reconcile it or continue inspecting further articles by generating queries from a template created using the title of the article along with words most likely to co-occur with each entity type.<br>
Since the IR System is looking at the shooter database, the entities extracted are as follows -<br>
![entities](/images/entity_values.png)
<br>
<b>MDP as a tuple: (S, A, T, R)</b><br>
Where, <br>
<i>S = {s}</i> is the state space<br>
![states](/images/state_rep_mdp.png)
The state of the MDP is represented based on the values of the entities.
The state representation depends on the following -<br>
1. Current confidence and new confidence of entities.
2. One hot encoding matches between current and new values.
3. Unigram/tf-idf counts of context words.
4. tf-idf similarity between original and new article.
<br>
<i>A = {a=(d,q)}</i> is the set of all actions depending on decision d and query choice q<br>
At each step, the agent makes a reconciliation decision <i>d</i> and query choice <i>q</i> with the episode ending when all entity values are accepted and the stop action is chosen.<br>
<i>R(s,a)</i> is the reward function<br>
![reward](/images/reward_function.png)
The reward function maximizes the final extraction accuracy while minimizing the number of queries by setting a negative reward to penalize the agent for longer episodes.<br>
<i>T(s'|s,a)</i> is the transition function<br>
The transition funciton maps the old state to the new state based on the action chosen by the agent.<br>

## Reinforcement Learning for Information Extraction

































	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
		
## Acknowledgements
<cite> 
Narasimhan, Karthik, Adam Yala, and Regina Barzilay. "Improving Information Extraction by Acquiring External Evidence with Reinforcement Learning." Proceedings of the 2016 Conference on Empirical Methods in Natural Language Processing, 2016. doi:10.18653/v1/d16-1261.
</cite>
<br> <br>
https://github.com/karthikncode/DeepRL-InformationExtraction









