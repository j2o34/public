AI Open 2 (2021) 100–126
Available online 24 July 2021
2666-6510/©2021 The Authors. Publishing services by Elsevier B.V. on behalf of KeAi Communications Co. Ltd. This is an open access article under the CC BY
license (http://creativecommons.org/licenses/by/4.0/).Advances and challenges in conversational recommender systems: A survey
Chongming Gao a, Wenqiang Lei b,*, Xiangnan He a, Maarten de Rijke c,d, Tat-Seng Chua b
a University of Science and Technology of China, China
b National University of Singapore, Singapore
c University of Amsterdam, Amsterdam, the Netherlands
d Ahold Delhaize, Zaandam, the Netherlands
A R T I C L E I N F O
Keywords:
Conversational recommendation system
Interactive recommendation
Preference elicitation
Multi-turn conversation strategy
Exploration-exploitation
1. Introduction
Recommender systems have become an indispensable tool for in-
formation seeking. Companies such as Amazon and Alibaba, in e-com-
merce, Facebook and Wechat, in social networking, Instagram and
Pinterest, in content sharing, and YouTube and Netflix, in multimedia
services, all have the need to properly link items (e.g., products, posts,
and movies) to users. An effective recommender system that is both
accurate and timely can help users find the desired information and
bring significant value to the business. Therefore, the development of
recommendation techniques continues to attract academic and indus-
trial attention.
Traditional recommender systems, which we call static recommen-
dation models in this survey, primarily predict a user’s preference
towards an item by analyzing past behaviors offline, e.g., click history,
visit log, ratings on items. Early methods, such as collaborative filtering
(CF) (SarwarGeorge et al., 2001; Schafer et al., 2007), logistic regression
(LR) (Nelder and Wedderburn, 1972), factorization machine (FM)
(Rendle, 2010), and gradient boosting decision tree (GBDT) (Ke et al.,
2017), have been intensively used in practical applications due to the
efficiency and interpretability. Recently, more complicated but powerful
neural networks have been developed, including Wide & Deep (Cheng
et al., 2016), neural collaborative filtering (NCF) (He et al., 2017), deep
interest network (DIN) (Zhou et al., 2018a), tree-based deep model
(TDM) (Zhu et al., 2018), and graph convolutional networks (GCNs)
(Ying et al., 2018; Wu et al., 2019b; He et al., 2020).
Inherent Disadvantages of Static Recommendations. Static
recommendation models are typically trained offline on historical
* Corresponding author.
E-mail addresses: chongminggao@mail.ustc.edu.cn (C. Gao), wenqianglei@gmail.com (W. Lei), hexn@ustc.edu.cn (X. He), m.derijke@uva.nl (M. de Rijke),
chuats@comp.nus.edu.sg (T.-S. Chua).
URL: http://chongminggao.me (C. Gao).
Contents lists available at ScienceDirect
AI Open
journal homepage: www.keaipublishing.com/en/journals/ai-open
https://doi.org/10.1016/j.aiopen.2021.06.002
Received 31 January 2021; Received in revised form 27 May 2021; Accepted 17 June 2021
AI Open 2 (2021) 100–126
101behavior data, which are then used to serve users online (Covington
et al., 2016). Despite their wide usage, they fail to answer two important
questions:
1. What exactly does a user like? The learning process of static models is
usually conducted on historical data, which may be sparse and noisy.
Moreover, a basic assumption of static models is that all historical
interactions represent user preference. Such a paradigm raises crit-
ical issues. First, users might not like the items they chose, as they
may make wrong decisions (Wang et al., 2020a, 2020b). Second, the
preference of a user may drift over time, which means that a user’s
attitudes towards items may change, and capturing the drifted
preference from past data is even harder (Jagerman et al., 2019). In
addition, for cold users who have few historical interactions,
modeling their preferences from data is difficult (LeeJinbae et al.,
2019). Sometimes, even the users themselves are not sure of what
they want before being informed of the available options (Wang and
Benbasat, 2013). In short, a static model can hardly capture the
precise preference of a user.
2. Why does a user like an item? Figuring out why a user likes an item is
essential to improve recommender model mechanisms and thus in-
crease their ability to capture user preference. There are many fac-
tors affecting a user’s decisions in real life (MaChang et al., 2019; Cen
et al., 2020; Gao et al., 2019c). For example, a user might purchase a
product because of curiosity or being influenced by others (Yu et al.,
2019a). Or it may be the outcome of deliberate consideration. It is
common that different users purchase the same product but their
motivations are different. Thus, treating different users equally or
treating different interactions by the same user equally, is not
appropriate for a recommendation model. In reality, it is hard for a
static model to disentangle different reasons behind a user’s con-
sumption behavior.
Even though much effort has been done to eliminate these problems,
they make limited assumptions. For example, a common setting is to
exploit a large amount of auxiliary data (e.g., social networks, knowl-
edge graphs) to better interpret user intention (Shi et al., 2014). How-
ever, these additional data may also be incomplete and noisy in real
applications. We believe the key difficulty stems from the inherent
mechanism: the static mode of interaction modeling fundamentally
limits the way in which user intention can be expressed, causing an
asymmetric information barrier between users and machines.
Introduction of CRSs. The emergence of conversational recom-
mender systems (CRSs) changes this situation in profound ways. There is
no widely accepted definition of CRS. In this paper, we define a CRS to
be:
A recommendation system that can elicit the dynamic preferences of
users and take actions based on their current needs through real-time
multi-turn interactions.
Our definition highlights a property of CRSs: multi-turn interactions.
By a narrow definition, conversation means multi-turn dialogues in the
form of written or spoken natural language; from a broader perspective,
conversation means any form of interactions between users and systems,
including written or spoken natural language, form fields, buttons, and
even gestures (Jannach et al., 2020). Conversational interaction is a
natural solution to the long-standing asymmetry problem in information
seeking. Through interactions, CRSs can easily elicit the current pref-
erence of a user and understand the motivations behind a consumption
behavior. Fig. 1 shows an example of a CRS where a user resorts to the
agent for music suggestions. Combining the user’s previous preference
(loving Jay Chou’s songs) and the intention elicited through conversa-
tional interactions, the system can offer desired recommendations
easily. Even if the produced recommendations do not satisfy the user,
the system has chances to change recommendations based on user
feedback.
Recently, attracted by the power of CRSs, many researchers have
been on focusing on exploring this topic. These efforts are spread across
a broad range of task formulation, in diverse settings and application
scenarios. We collect the papers related to CRSs by searching for
“Conversation* Recommend*” on DBLP 1 and visualize the statistics of
them with regard to the published year and venue in Fig. 2. There are
148 unique publications up to 2020, and we only visualize the top 10
venues, which contain 53 papers out of all 148 papers at all 89 venues. It
is necessary to summarize these studies which put efforts into different
aspects of CRSs.
Connections with Interactive Recommendations. Since the born
of recommender systems, researchers have realized the importance of
the human-machine interaction. Some studies propose interactive
recommender systems (He et al., 2016; Wang et al., 2017; Chen et al.,
2019ba; Zhou et al., 2020d) and critiquing-based recommender systems
(Tou et al., 1982; Tversky and Simonson, 1993; Burke et al., 1997;
Smyth and McGinty, 2003; Pu and Faltings, 2004; Chen and Pu, 2012;
Luo et al., 2020b; LuoScott et al., 2020), which can be viewed as early
forms of CRSs since they focus on improving the recommendation
strategy online by leveraging real-time user feedback on previously
recommended items.
In the setting of interactive recommendations, each recommendation
is followed by a feedback signal indicating whether and how much the
user likes this recommendation. However, interactive recommendations
suffer from low efficiency, as there are too many items. An intuitive
solution is to leverage attribute information of items, which is self-
explanatory for understanding users’ intention and can quickly narrow
down candidate items. The critiquing-based recommender system is
such a solution that is designed to elicit users’ feedback on certain at-
tributes, rather than items. Critiquing is like a salesperson who collects
Fig. 1. A toy example of a conversational recommender system in music
recommendation.
1 https://dblp.org/search?q=conversation*%20recommend*.
C. Gao et al.
AI Open 2 (2021) 100–126
102user preference by asking questions proactively on item attributes. For
example, when seeking mobile phones, a user may follow the hint of the
system and provides feedback such as “cheaper” or “longer battery life.”
Based on such feedback, the system will recommend more appropriate
items; this procedure repeats several times until the user finds satisfac-
tory items or gives up. The mechanism gives the system an improved
ability to infer user preference and helps quickly narrow down recom-
mendation candidates.
Though effective, existing interactive and critiquing methods have a
limitation: the model makes a recommendation each time after receiving
user feedback, which should be avoided as the recommendation should
only be made when the confidence is high. This problem is solved in
some CRSs by developing a conversation strategy determining when to
ask and recommend (Lei et al., 2020a, 2020b). Besides, the interactive
and critiquing methods are constrained by their representation ability
since users can only interact with the system through a few predefined
options. The integration of a conversational module in CRSs allows for
more flexible forms of interaction, e.g., in the form of tags (Christako-
poulou et al., 2018), template utterances (Sun and Zhang, 2018), or free
natural language (Li et al., 2018). Undoubtedly, user intention can be
more naturally expressed and comprehended through a conversational
module.
Connections with Other Conversational AI Systems. Besides
CRSs, there are other conversational AI systems, e.g., task-oriented
dialogue systems (Chen et al., 2017; Zhang et al., 2020b; Pei et al.,
2021), social chatbots (Ma et al., 2021; Li et al., 2021a; Wu and Yan,
2018), conversational searching (Voskarides et al., 2020; Rosset et al.,
2020; Ren et al., 2021), and conversational question answering (QA)
(Zhu et al., 2021). The common point of them is to utilize natural lan-
guage as a powerful tool to convey information and thus to provide a
natural user interface. Though these research topics all possess the
keyword “conversation”, the central tasks are different. For example,
while task-oriented dialogue systems aim to fulfill a certain task in
human-machine dialogue, the concentration of effort is mainly on
handling information in the textural language-based dialogue, e.g.,
natural language understanding (NLU), dialogue state tracking (DST),
dialogue policy learning (DPL), and natural language generation (NLG)
(Chen et al., 2017; Zhang et al., 2020b; Gao et al., 2019a). In CRSs,
however, the multi-turn conversation can be built on any form of
interaction (e.g., form fields, buttons, and even gestures (Jannach et al.,
2020)) instead of merely textual form. Because CRSs concentrate on
recommendation logic, the textual dialogue is just one possible means to
convey information, i.e., it is auxiliary, not necessary. Although there
are some CRSs implemented as end-to-end dialogue systems (Li et al.,
2018; Chen et al., 2019bb), the human evaluation conducted by Jannach
and Manzoor (2020) suggests the performance is not ideal and more
efforts should be put on improving both recommendation and language
generation.
Other conversational AI systems can also be distinguished from CRSs
by their specific scenarios. For instance, conversational searching fo-
cuses on analyzing the input query (in contrast to eliciting user prefer-
ence in CRSs); conversational QA focuses on the single-turn question
answering (in contrast to multi-turn interaction in CRSs). Therefore, it is
essential to identify the central tasks and primary challenges in CRSs to
help the beginner and future researchers set foot in this field and keep up
with state-of-the-art technologies.
Focuses of This Survey. Although many studies have been done on
CRSs, there is no uniform task formulation. In this survey, we present all
CRSs as the general framework that consists of three decoupled com-
ponents illustrated in Fig. 3. Specifically, a CRS is made of a user
interface, a conversation strategy module, and a recommendation en-
gine. The user interface serves as a translator between the user and
machine; generally, it extracts information from raw utterances of the
user and transforms the information into machine-understandable rep-
resentation, and it generates meaningful responses to the user based on
the conversation strategy. The conversation strategy module is the brain
of the CRS and coordinates the other two components; it decides the core
logic of the CRS such as eliciting user preference, maintaining multi-turn
Fig. 2. Statistics of the publications related to CRSs, grouped by the publication
year and venue. Only the top 10 venues are used in the visualization.
Fig. 3. Illustration of the general framework of CRSs and our identified five primary challenges on the three main components.
C. Gao et al.
AI Open 2 (2021) 100–126
103conversations, and leading new topics. The recommendation engine is
responsible for modeling relationships among entities (e.g., the user-
item interaction or item-item linkage), learning and recording user
preference on items and attributes of items, retrieving the required
information.
There are many challenges in the three components, we summarize
five main challenges as following.
● Question-based User Preference Elicitation. CRSs provide the opportu-
nity to explicitly elicit user preference by asking questions. Two
important questions are needed to be answered: (1) What to ask? (2)
How to adjust the recommendations based on user response? The
former focuses on constructing questions to elicit as much informa-
tion as possible; the latter leverages the information in user response
to make more appropriate recommendations.
● Multi-turn Conversational Recommendation Strategies. The system
needs to repeatedly interact with a user and adapts to the user’s
response dynamically in multiple turns. An effective strategy con-
cerns when to ask questions and when to make recommendations, i.
e., let the model choose between (1) continuing to ask questions so as
to further reduce preference uncertainty, and (2) generating a
recommendation based on estimation of current user preference.
Generally, the system should aim at a successful recommendation
using the least number of turns, as users will lose their patience after
too many turns (Lei et al., 2020a). Furthermore, some sophisticated
conversational strategies try to proactively lead dialogues (Wu et al.,
2019; Balaraman and Magnini, 2020), which can introduce diverse
topics and tasks in CRSs (Liu et al., 2020ab; Zhou et al., 2020c; Lewis
et al., 2017; Wang et al., 2019).
● Natural Language Understanding and Generation. Communicating like
a human being continues to be one of the hardest challenges in CRSs.
For understanding user interests and intentions, some CRS methods
define the model input as pre-defined tags that capture semantic
information and user preferences (Christakopoulou et al., 2018; Lei
et al., 2020a, 2020b; Zou et al., 2020). Some methods extract the
semantic information from users’ raw utterances via slot filling
techniques and represent user intents in slot-value pairs (Zhang et al.,
2018; Sun and Zhang, 2018; Ren et al., 2020). And for generating
human-understandable responses, CRSs use many strategies such as
directly providing a recommendation list (Zou et al., 2020; Zhang
et al., 2018), incorporating recommended items in a rule-based
natural language template (Sun and Zhang, 2018; Lei et al.,
2020a, 2020b). Moreover, some researchers propose the end-to-end
framework to enable CRSs to precisely understand users’ sentiment
and intentions from the raw natural language and to generate
readable, fluent, consistent, and meaningful natural language re-
sponses (Li et al., 2018; Liu et al., 2020ab; Ren et al., 2020; Chen
et al., 2019bb; Zhou et al., 2020a).
● Trade-offs between Exploration and Exploitation (E&E). One problem
of recommender systems is that each user can only interact with a
few items out of the entire dataset. A large number of items that a
user may be interested in will remain unseen by the user. For cold-
start users (who have just joined the system and have zero or very
few interactions), the problem is especially severe. Thanks to the
interactive nature, CRSs can actively explore the unseen items to
better capture the user preference. In this way, users can benefit from
having chances to express their intentions and obtain better-
personalized recommendations. However, the process of explora-
tion comes at a price. As users only have limited time and energy to
interact with the system, a failed exploration will waste time and lose
the opportunity to make accurate recommendations. Moreover,
exposing unrelated items hurts user preference, compared to
exploiting the already captured preference by recommending the
items of high confidence (Schnabel et al., 2018; Li et al., 2015;
Gilotte et al., 2018). Therefore, pursuing E&E trade-offs is a critical
issue in CRSs.
● Evaluation and User Simulation. Evaluation is an important topic.
Unlike static recommender models that are optimized on offline
data, CRSs emphasize the user experience during dynamic in-
teractions. Hence, we should not only consider the turn-level eval-
uation for both recommendation and response generation but also
pay attention to the conversation-level evaluation. Besides, evalu-
ating CRSs requires a large number of online user interactions, which
are expensive to obtain (Li et al., 2015; Jagerman et al., 2019; Huang
et al., 2020). Practical solutions include: (1) leveraging the off-policy
evaluation which assesses the target policy using the logged data
under the behavior policy (Gilotte et al., 2018; Jagerman et al.,
2019), and (2) directly introducing user simulators to replace the
true users in evaluation (Zhang and Balog, 2020; Sun et al., 2021).
The five challenges are allocated to the corresponding component as
illustrated in Fig. 3, where trading off the E&E balance is exclusive to the
recommender engine; handling natural language understanding and
generation is exclusive to the conversation module. The rest three
challenges are related to both the components. We illustrate in Table 1
the solutions of some classic CRSs that focus on these directions. Limited
by space, we only give part of the classic studies here. We will further
discuss existing solutions in the following sections.
Differences with Existing Related Surveys. Recently, A number of
related survey papers have been published. There are survey papers
focusing on certain cutting-edge aspects in recommender systems, such
as the bias issues and debiasing methods (Chen et al., 2020a), explain-
ability/interpretability (Zhang and Chen, 2020), evaluation issues (Sil-
veira et al., 2019), and novel methods that leverage deep neural
networks (Wu et al., 2020, 2021; Zhang et al., 2019a), knowledge graphs
(Guo et al., 2020), or reinforcement learning (Afsar et al., 2021) to
improve the ability of recommendation systems. Also, there are survey
papers that summarize new frontiers in conversational AI systems, such
as the advanced methods (Chen et al., 2017; Gao et al., 2019a; Zhang
et al., 2020b) and the evaluation issues (Celikyilmaz et al., 2020; Deriu
et al., 2021) in dialogue systems. However, there is only one survey
paper published in 2020 that focuses on CRSs (Jannach et al., 2020).
Jannach et al. (2020), for the first time, delved into different aspects
of CRSs and made a comprehensive survey of CRSs. Specifically, they
categorize existing CRSs in various dimensions, for instance, in terms of
interaction modalities (e.g., buttons or written language), supported
tasks (e.g., recommend or explain), or the knowledge CRSs use in the
background (e.g., item-related information or dialogue corpora). Their
survey provides a structured description of the CRS. Therefore, the
audience, after reading this survey, can answer what a CRS is, for
example, what the input/output or the functions of a CRS are. However,
they may be still unsure about what the key challenges are, or what to do
next. In our survey, we not only give the review of the current progress
on CRSs including the existing assumptions and exploration but also
refine the problems in state-of-the-art methods and summarize five
challenges. We are trying to answer the three questions above, and we
hope to provoke deeper thought and spark new ideas for the audience.
Survey Organization. The remainder of this paper is organized as
follows. In next several sections, we discuss the main challenges in CRSs.
Specifically, in Section 2, we illustrate how CRSs can elicit user prefer-
ences by asking informative questions. In Section 3, we describe the
strategies in CRSs to interact with users in a multi-turn conversation. In
Section 4, we point out the problems and provide solutions in dialogue
understanding and generation for CRSs. In Section 5, we discuss how
CRSs can balance the exploration-exploitation trade-off. In Section 6, we
explore metrics and present techniques for evaluating CRSs. In Section 7,
we envision some promising future research directions. And in Section 8,
we conclude this survey.
C. Gao et al.
AI Open 2 (2021) 100–126
1042. Question-based user preference elicitation
A user looking for items with specific attributes may get assess to
them by actively searching. For instance, a user may search “iphone12
red 256gb”, where the key phrases “red” and “256gb” are the attributes
of the item iPhone12. In this scenario, users construct a query them-
selves, and the performance relies on both the search engine and the
user’s expertise in constructing queries. Even though there are efforts on
helping users complete queries by suggesting possible options based on
what they entered (Ma et al., 2008; Bar-Yossef and Kraus, 2011; Deh-
ghani et al., 2017; Cai and de Rijke, 2016), users still need to figure out
appropriate query candidates. Besides, searching in this way requires
users to be familiar with each item they want, which is not true in
practice. Recommender systems introduce users to the potential items
that they may like. However, traditional recommender systems can only
utilize the static historical records as the input, which results in the two
main limitations mentioned in mysecintro.
Fortunately, CRSs can bridge the gap between the search engine and
recommender system. Empowered by real-time interactions, CRSs can
proactively consult users by asking questions. And with the feedback
returned by users, CRSs can directly comprehend users’ needs and at-
titudes towards certain attributes, hence making proper recommenda-
tions. Even if users are not satisfied with the recommended items, a CRS
has the opportunity to adjust its recommendations in the interaction
process.
Question-driven methods focus on the problem of what to ask in
conversations. Generally, there are two kinds of methods: (1) asking
about items (Zhao et al., 2013; Christakopoulou et al., 2016; Sepliar-
skaia et al., 2018), or (2) asking about attributes/topics/categories of
items (Lei et al., 2020a, 2020b).
2.1. Asking about items
Early studies directly ask users for opinions about an item itself (Zhao
et al., 2013; Wang et al., 2018; Christakopoulou et al., 2016; Zou et al.,
2020b; Vendrov et al., 2020). Unlike traditional recommender systems
which need to estimate user preferences in advance, CRSs can construct
and modify the user profile during the interaction process.
In traditional recommender system models, the recommended items
are produced in a relatively stable way from all candidates. In the CRS
scenario, the recommended items should be updated after the system
receives feedback from a user and it could be a complete change in order
to adapt to the user’s real-time preferences. Hence, instead of merely
updating parameters of models online, some explicit rules or mecha-
nisms are required. We introduce three methods that can elicit users’
attitudes towards items and can quickly adjust recommendations. Most
of these methods did not use natural language in their user interface, but
it can easily integrate an natural language-based interface to make a
CRS.
Choice-based Methods. The main idea of choice-based preference
elicitation is to recurrently let users choose their preferred items or item
sets from the current given options. The common strategies include (1)
choosing an item from two given options (Sepliarskaia et al., 2018), (2)
selecting an item from a list of given items (Jiang and QiHe, 2014; Graus
and Willemsen, 2015; Saavedra et al., 2016), and (3) choosing a set of
items from two given lists (LoeppTim Hussein and Ziegler, 2014).
After the user chooses preferred items, the methods change the
recommendations according to the user’s choice. For example, Loepp
et al. (LoeppTim Hussein and Ziegler, 2014) use the matrix factorization
(MF) model (Bell et al., 2007) to initialize the embedding vectors of
users and items, then select two sets of items from the item embedding
space as candidate sets and let a user choose one of the two sets. It is
important to ensure that the two candidate sets are as different or
distinguishable as possible. To achieve this, the authors adopt a
factor-wise MF algorithm (Bell et al., 2007), which factorizes the
user-item interaction matrix and obtains the embedding vectors one by
one in decreasing order of explained variance. Hence, the factors, i.e.,
different dimensions of embedding vectors, are ordered by distinctive-
ness. Then, the authors iteratively select two item sets with only a single
factor value varying. For example, if two factors represent the degree of
Humor and Action of movies, respectively, then the two candidate sets
are one set of movies with a high degree of Humor and another with a
low degree of Humor, while the degree of Action of the two sets is fixed to
the average level. When a user chooses one item set, the user’s prefer-
ence embedding vector is set to the average of the embedding vectors of
the chosen items. The choice becomes harder as the interaction process
continues. Users can choose to ignore the question, which means the
users cannot tell the difference between the two item sets or they do not
care about it. Carenini et al. (2003) further explore other strategies to
select query items, e.g., selecting the most popular or the most diverse
items in terms of users’ history.
Bayesian Preference Elicitation. In addition, there are studies
based on a probabilistic view of preference elicitation, which has been
researched for a long time (Chajewska et al., 1998; Boutilier, 2002;
Vendrov et al., 2020). Basically, there is a utility function or a score
function u xj, ui
) representing user i’s preference for item j. Usually, it
can be written as a linear function as
u xj, ui
) = xT
j ui. (1)
In a Bayesian setting, user i’s preference is modeled by a probabilistic
distribution instead of a deterministic vector, which means that the
vector ui is sampled from a prior user belief P 𝒰(i) ). Therefore, the utility
Table 1
Five primary challenges in CRSs and part of the classic methods that contribute to these challenges.
Primary Challenges in CRSs Contributions of Existing
Studies
Classic Publications
Question-based User Preference
Elicitation
Asking about items (Zhao et al., 2013; Christakopoulou et al., 2016; Yu et al., 2019b; Zou et al., 2020b; Mangili et al., 2020; Vendrov
et al., 2020; LoeppTim Hussein and Ziegler, 2014)
Asking about attributes (Mangili et al., 2020; YangScott et al., 2021; Wu et al., 2019a; Christakopoulou et al., 2018; Zhang et al., 2018; Sun
and Zhang, 2018; Lei et al., 2020a, 2020b; Zhou et al., 2020a)
Multi-turn Conversational
Strategies
Explicit strategies (Sun and Zhang, 2018; Zhang et al., 2018; Lei et al., 2020a; Xu et al., 2021)
Leading diverse topics (Liu et al., 2020ab; Zhou et al., 2020c)
Language Understanding and
Generation
End-to-end dialogue
systems
(Li et al., 2018; Chen et al., 2019bb; Zhou et al., 2020a; Xu et al., 2020; Moon et al., 2019)
Exploration and Exploitation
Trade-offs
Leveraging multi-armed
bandits
(Christakopoulou et al., 2016; Zhang et al., 2020c; Li et al., 2021b; Yu et al., 2019b)
Evaluation and User Simulation Evaluation (Gilotte et al., 2018; Huang et al., 2020)
User simulation (Zhang and Balog, 2020; Sun et al., 2021)
C. Gao et al.
AI Open 2 (2021) 100–126
105of an item j for a user i is computed as the expectation:
E[u xj, ui
) ] =
∫
ui ∼𝒰(i)
P(ui)u xj, ui
)dui. (2)
The item with the maximum expected utility for user i is considered
as the recommendation items:
arg maxj E[u xj, ui
) ]. (3)
Based on the utility function, the system can select some items to
query. And the user belief distribution can be updated based on users’
feedback. Specifically, given a user response ri to the question q, the
posterior user belief P(ui|q, ri) can be written as:
P(ui|q, ri) = P(ri |q, ui )P(ui)
∫
𝒰(i) P(ri |q, ui)P(ui)dui
. (4)
As for the query strategy, i.e., selecting which items to ask, there are
different criteria. For example, Boutilier (2002) propose a partially
observed Markov decision process (POMDP) framework as the sequen-
tial query strategy. And Vendrov et al. (2020) and Guo and Sanner
(GuoScott, 2010) use the expected value of information (EVOI) para-
digm as a relatively myopic strategy to select items to query. Further-
more, the query type can be classified into two different types:
(1) a pairwise comparison query, in which the users are required to
choose what they prefer more between two items or two item sets
(Christakopoulou et al., 2016; GuoScott, 2010; Sepliarskaia et al., 2018);
or (2) a slate query, where users need to choose from multiple given
options (Vendrov et al., 2020).
Interactive Recommendation. Interactive recommendation
models are mainly based on reinforcement learning. Some researchers
adopt a multi-armed bandit (MAB) algorithm (Zhao et al., 2013;
Christakopoulou et al., 2016; Wang et al., 2018). The advantage is
two-fold. First, MAB algorithms are efficient and naturally support
conversational scenarios. Second, MAB algorithms can exploit the items
that users liked before and explore items that users may like but never
tried before. There are also researchers formulate the interactive
recommendation as a meta learning problem which can quickly adapt to
new tasks (Zou et al., 2020b; LeeJinbae et al., 2019). A task here is to
make recommendations based on several conversation histories. Meta
learning methods and MAB-based methods have the capability of
balancing exploration and exploitation. We will describe it later in
Section 5.
Recently, researchers incorporate deep reinforcement learning (DRL)
models into interactive recommender systems (Zhao et al., 2018; Chen
et al., 2019ba; Xian et al., 2019; Zheng et al., 2018; HuQing et al., 2018;
Zou et al., 2019; Chen et al., 2019aa; Ie et al., 2019; Liao et al., 2018;
Pecune et al., 2019; Zhou et al., 2020b; Zou et al., 2020a; Wang et al.,
2020c). Unlike MAB-based methods which usually assume the user
preference is unchanged during the interaction, DRL-based methods can
model a dynamic preference and long-term utility. For example, Mah-
mood and Ricci (2007) introduce a model-based techniques and use the
policy iteration algorithm (Sutton and Barto, 2018) to acquire an
adaptive strategy. Model-free frameworks such as deep Q-network
(DQN) (Zhao et al., 2018; Zheng et al., 2018; Zou et al., 2019; Zhou
et al., 2020b) and deep deterministic policy gradient (DDPG) (HuQing
et al., 2018) are used in interactive recommendation scenarios. Most
reinforcement learning (RL)-based methods often suffer from low effi-
ciency issues and cannot handle cold-start users. Zhou et al. (2020b)
propose to integrate a knowledge graph into the interactive recom-
mendation to solve these problems.
For more works that leverage RL in interactive recommender sys-
tems, we refer the interested readers to the comprehensive survey con-
ducted by Afsar et al. (2021).
However, directly requiring items is inefficient for building the user
profile because the candidate item set is large. In real-world CRS ap-
plications, users will get bored as the number of conversation turns
increases. It is more practical to ask attribute-centric questions, i.e., to
ask users whether they like an attribute (or topic/category in some
works), and then make recommendations based on these attributes
(Zhang et al., 2018; Lei et al., 2020a). Therefore, the estimation and
utilization of a user’s preferences towards attributes become a key
research issue.
2.2. Asking about attributes
Asking about attributes is more efficient because whether users like
or dislike an attribute can significantly reduce the recommendation
candidates. The challenge is to determine a sequence of attributes to ask
so as to minimize the uncertainty of current user needs (Mirzadeh et al.,
2005; Thompson et al., 2004). The aforementioned critiquing-based
methods fall into this category. Besides, there are other kinds of
methods, we introduce some mainstream branches as below.
2.2.1. Fitting patterns from historical interaction
A conversation can be deemed as a sequence of entities including
consumed items and mentioned attributes, and the objective is to learn
to predict the next attribute to ask or the next item to recommend.
Therefore, the sequential neural network such as the gated recurrent
unit (GRU) model (ChoBart van Merri¨enboer et al., 2014) and the long
short term memory (LSTM) model (HochreiterJürgen Schmidhuber,
1997) can be naturally adopted in this setting, due to its ability to
capture long and short term dependency in user behavioral patterns.
An exemplar work is the question & recommendation (Q&R) model
proposed by Christakopoulou et al. (2018), where the interaction be-
tween the system and a user is implemented as a selection system. In
each turn, the system asks the user to choose one or more distinct topics
(e.g., NBA, Comics, or Cooking) from the given list, and then recom-
mends items in these topics to the user. It contains a trigger module to
decide whether to ask a question about attributes or to make a recom-
mendation. The triggering mechanism can be as simple as a random
mechanism or can be more sophisticated, i.e., using criteria capturing
the user’s state, or even be user-initiated. At the t-th time step, the next
topic q that user click can be predicted based on the user’s watching
history e1, …, eT as: P(q|e1, …, eT ). After user clicking a topic q, the model
can recommend an item r based on the conditional probability written
as: P(r|e1, …, eT , q). Both of the two conditional probabilities are
implemented as the GRU architecture (ChoBart van Merri¨enboer et al.,
2014). This algorithm is deployed on YouTube, for obtaining prefer-
ences from cold-start users.
Zhang et al. (2018) propose a “System Ask User Response” (SAUR)
paradigm. For each item, they utilize the rich review information and
convert a sentence containing an aspect-value pair to a latent vector via
the GRU model. Then they adopt a memory module with attention
mechanism (SukhbaatarArthur Szlam et al., 2015; Kumar et al., 2016;
Miller et al., 2016) to perform both the next question generation task
(determining which attribute to ask) and the next item recommendation
task. Again, they also develop a heuristic trigger to decide whether it is
the time to display the top-n recommended items to users or to keep
asking questions about attributes. One limitation of the work is that the
authors assume all information in reviews can support the purchasing
behavior, however it is not true as users may complain certain aspects of
the purchased items, e.g., a user may write “64 Gigabytes is not enough”.
Using information without discrimination will mislead the model and
deteriorate the performance.
The utterances produced by the system, i.e., the questions, are con-
structed with predefined language patterns or templates, meaning that
what the system needs to pay attention to are only the aspect and the
value. This is a common setting in state-of-the-art CRS studies because
the core task here is recommendation instead of language generation
(Christakopoulou et al., 2018; Lei et al., 2020a, 2020b).
Note that these kinds of methods have a common disadvantage:
learning from historical user behaviors cannot aid understanding the
C. Gao et al.
AI Open 2 (2021) 100–126
106logic behind the interaction. As interactive systems, these models do not
consider how to react to feedback when users reject the recommenda-
tion, i.e., they just try to fit the preferences in historical interaction and
do not consider an explicit strategy to deal with different feedback.
2.2.2. Reducing uncertainty
Unlike sequential neural network-based methods that do not have an
explicit strategy to handle all kinds of user feedback, some studies try to
build a straightforward logic to narrow down item candidates.
Critiquing-based Methods. The aforementioned critiquing model is
typically equipped with a heuristic tactic to elicit user preference on
attributes (Chen and Pu, 2012; Wu et al., 2019a; Luo et al., 2020b;
LuoScott et al., 2020). In traditional critiquing models, where the
critique on an attribute value (e.g., “not red” for color or “less expensive”
for price) is used for reconstructing the candidate set by removing the
items with unsatisfied attributes (Chen and Pu, 2012; McCarthy et al.,
2004; Smyth et al., 2004; Viappiani et al., 2007; Burke et al., 1997;
Smyth and McGinty, 2003). The neural vector-based methods take the
criticism into the latent vector, which is responsible for generating both
the recommended items and the explained attributes. For example, Wu
et al. (2019a) propose an explainable neural collaborative filtering
(CE-NCF) model for critiquing. They use the NCF model (He et al., 2017)
to encode the preference of a user i for an item j as a latent vector ̂ zi,j,
then ̂ zi,j is used for producing the rating score ̂ ri,j as well as the explained
attribute vector ̂ si,j. The attributes are composed of a set of key-phrases
such as “golden, copper, orange, black, yellow,” and each dimension of ̂
si,j corresponds to a certain attribute. When a user dislikes an attribute
and critique it in real-time feedback, the system updates the explained
attribute vector ̂ si,j by setting the corresponding dimension to zero. Then
the updated vector ̃ si,j is used to update the latent vector ̂ zi,j to be ̃ zi,j.
Consequently, the recommendation score is updated to be ̃ ri,j. Following
this setting, Luo et al. (2020b) change the base NCF model (He et al.,
2017) to be a variational autoencoder (VAE) model, and this generative
model can help the critiquing system have better computational effi-
ciency, improved stability, and faster convergence.
Reinforcement Learning-driven Methods. Reinforcement learning
is also used in CRSs to select the appropriate attributes to ask (Sun and
Zhang, 2018; Lei et al., 2020a, 2020b). Empowered by a deep policy
network, the system not only selects the attributes but also determine a
controlling strategy on when to change the topic of the current con-
versation; we will elaborate this in Section 3.1 where we describe how
reinforcement learning helps the system form a multi-turn conversa-
tional strategy.
Graph-constrained Candidates. Graph is a prevalent structure to
represent relationship of different entities. It is natural to utilize graphs
to sift items given a set of attributes. For example, Lei et al. (2020b)
propose an interactive path reasoning algorithm on a heterogeneous
graph on which users, items, and attributes are represented as nodes and
an edge connected two nodes represented a relationship between two
nodes, e.g., a user purchased an item, or an item has a certain value for
an attribute. With the help of the graph, a conversation can be converted
to a path on the graph, as illustrated in Fig. 4. The authors compare the
uncertainty of preference for attributes and choose the attributes with
the maximum uncertainty to ask. Here, the preference for a certain
attribute is modeled by the average preference for items that have this
attribute. Hence, the searching space and overhead of the algorithm can
be significantly reduced by utilizing the graph information. There are
other studies that apply graph neural networks (GNNs) to learn a
powerful representation of both items and attributes, so the semantic
information in the learned embedding vectors can help end-to-end CRS
models generate appropriate recommendations. For example, the GCN
model and its variants (Kipf and Welling, 2017; Schlichtkrull et al.,
2018) are adopted on the knowledge graph in recent CRS models (Chen
et al., 2019bb; Zhou et al., 2020a; Xu et al., 2020; Liao et al., 2020).
Other Methods. There are other attempts to make recommendations
based on user feedback on attributes. For example, Zou et al. (Zou et al.,
2020) proposed a question-driven recommender system based on an
extended matrix factorization model, which merely considers the user
rating data, to combine real-time feedback from users.
The basic assumption is that if a user likes an item, then he/she will
like the attributes of this item. Thereby, in each turn, the system will
select the attribute that carries the maximum amount of uncertainty to
ask. In other words, if an attribute is known to be shared by most items
that a user likes, then it does not need to ask about this attribute.
Similarly, there is no need to ask about the attributes that users dislike.
Only if it is not sure whether a user likes an attribute, then asking about
this attribute can provide the most amount of information. The param-
eters in matrices can be updated after users providing feedback. Besides,
using ideas similar to aforementioned models based on asking items,
MAB-based models (Zhang et al., 2020c; Li et al., 2021b) and Bayesian
approaches (Mangili et al., 2020) are also developed in attribute-asking
CRSs.
2.3. Section summary
We list the common CRS models in Table 2, where the models are
characterized by different dimensions, which are the asking entity (item
or attribute), the asking mechanism, the type of user feedback, and the
multi-turn strategy that we will describe in the next section.
In most interactive recommendations (Zou et al., 2020a; Wang et al.,
2020c; ZhangTong et al., 2019b; Ding et al., 2020) and critiquing
methods (Chen and Pu, 2012; Wu et al., 2019a; Luo et al., 2020b;
LuoScott et al., 2020), the system keeps asking questions, and each
question is followed by a recommendation. This process will only
terminate when users quit with either being satisfied or impatient. The
setting is unnatural and will likely hurt the user experience during the
interaction process. Asking too many questions may let the interaction
become an interrogation. Moreover, during the early stages of interac-
tion, when the system has not confidently modeled the user preferences
yet, recommendations with low confidence should not be exposed to the
user (Schnabel et al., 2018). In other words, there should be a multi-turn
conversational strategy to control how to switch between asking and
recommending, and this strategy should change dynamically in the
interaction process.
3. Multi-turn conversational strategies for CRSs
Question-driven methods focus on the problem of “What to ask”, and
the multi-turn conversational strategies discussed in this section focus
on “When to ask” or a broader perspective, “How to maintain the con-
versation”. A good strategy cannot only make the recommendation at the
proper time (with high confidence) and adapt flexibly to users’ feed-
back, but also maintain the conversation topics and adapt to different
scenarios to make users feel comfortable in the interaction.
3.1. Conversation strategies for determining when to ask and recommend
Most CRS models do not carefully consider a strategy to determine
whether to continue interrogating users by asking questions or to make a
recommendation. However, a good strategy is essential in the interac-
tion process so as to improve the user experience. The strategy can be a
rule-based policy, i.e., making recommendations every k turns of asking
questions (Zhang et al., 2020c), or a random policy (Christakopoulou
et al., 2018), or a model-based policy (Christakopoulou et al., 2018).
In the SAUR model (Zhang et al., 2018), a trigger is set to activate the
recommendation module when the confidence is high. The trigger is
simply implemented as a sigmoid function on the score of the most
probable item, i.e., if the score of the candidate item is high enough, then
the recommendation step is triggered, else the system will keep asking
questions.
Though straightforward and easy to control, these strategies cannot
C. Gao et al.
AI Open 2 (2021) 100–126
107capture rich semantic information, e.g., what topics are talking about
now or how deep the topics have been explored. This information can
directly affect the conversation topic. Thereby, a sophisticated strategy
is necessary. Recently, reinforcement learning (RL) has been adopted by
many interactive recommendation models for its potential of modeling
the complex environment (Zhao et al., 2018; Chen et al., 2019ba; Xian
Fig. 4. An illustration of interactive path reasoning in the CPR model. Credits: Lei et al. (Lei et al., 2020b).
Table 2
Characteristics of common CRS models in different dimensions. The strategy indicates whether the work considers an explicit strategy to control multi-turn con-
versations, e.g., whether to ask or recommend in the current turn.
Asking Asking Mechanism Basic Model Type of User Feedback Strategy Publications
Items Exploitation &
Exploration
Multi-armed bandit Rating on the given
item(s)
No (Zhao et al., 2013; Christakopoulou et al., 2016; Zhou et al., 2020e;
WangSteven et al., 2017; Yu et al., 2019b)
Exploitation &
Exploration
Meta learning Rating on the given
item(s)
No (Zou et al., 2020b; LeeJinbae et al., 2019)
Maximal posterior user
belief
Bayesian methods Rating on the given
item(s)
No Vendrov et al. (2020)
Reducing uncertainty Choice-based
methods
Choosing an item or a
set of items
No (LoeppTim Hussein and Ziegler, 2014; Jiang and QiHe, 2014; Graus and
Willemsen, 2015; Saavedra et al., 2016; Rana and Bridge, 2020)
Attributes Exploitation &
Exploration
Multi-armed bandit Rating on the given
attribute(s)
Yes (Zhang et al., 2020c; Li et al., 2021b)
Reducing uncertainty Bayesian approach Providing preferred
attribute values
No (Mangili et al., 2020; YangScott et al., 2021)
Critiquing-based
methods
Critiquing one/
multiple attributes
No (McCarthy et al., 2004; Smyth et al., 2004; Viappiani et al., 2007; Burke
et al., 1997; Smyth and McGinty, 2003)
(Pu and Faltings, 2004; Chen and Pu, 2012; Wu et al., 2019a; Luo et al.,
2020b; LuoScott et al., 2020)
Matrix factorization Answering Yes/No for
an attributes
No Zou et al. (2020)
Fitting historical patterns Sequential neural
network
Providing preferred
attribute values
Yes (Christakopoulou et al., 2018; Zhang et al., 2018)
Providing an utterance No (Li et al., 2018; Chen et al., 2019bb)
Maximal reward Reinforcement
learning
Answering Yes/No for
an attributes
Yes (Lei et al., 2020a, 2020b)
Providing an utterance Yes (Kang et al., 2019; Sun and Zhang, 2018; Tsumita and Takagi, 2019)
No Ren et al. (2020)
Exploring graph-
constrained candidates
Graph reasoning Answering Yes/No for
an attributes
Yes Lei et al. (2020b)
Providing an utterance Yes (Chen et al., 2019bb; Liu et al., 2020ab)
No (Zhou et al., 2020a; Liao et al., 2020)
Providing preferred
attribute values
Yes Xu et al. (2020)
No Moon et al. (2019)
C. Gao et al.
AI Open 2 (2021) 100–126
108et al., 2019; Zheng et al., 2018; Zou et al., 2019; Chen et al., 2019aa; Ie
et al., 2019; Liao et al., 2018; Pecune et al., 2019; ZhangTong et al.,
2019b; Zhou et al., 2020b). Therefore, it is natural to incorporate RL into
the CRS framework (Sun and Zhang, 2018; Lei et al., 2020a, 2020b;
Tsumita and Takagi, 2019; Ren et al., 2020; Kang et al., 2019). For
instance, Sun and Zhang (2018) propose a model called conversational
recommender model (CRM) that uses the architecture of task-oriented
dialogue system. In CRM, a belief tracker is used to track the users’
input, and it outputs a latent vector representing the current state of the
dialogue and the user preferences that have so far been captured. Af-
terward, the state vector of the belief tracker is input into a deep policy
network to decide whether to recommend an item or to keep asking
questions. Specifically, there are l + 1 actions: l actions for choosing one
facet to ask and the last one is to yield a recommendation. The deep
policy network uses the policy gradient method to make decisions.
Finally, the model gets rewards from the environment, which includes
user feedback towards the questions and the reward from the automatic
evaluation of recommendation results.
However, the state modeled in CRM is a latent vector capturing the
information of facet-values, which is hard to interpretable. In this
respect, some studies explore better ways to construct the state of RL to
make the multi-turn conversation strategy better adapt to an dynamic
environment. For example, Lei et al. (2020a) propose an
Estimation-Action-Reflection (EAR) framework, which assumes that the
model should only ask questions at the right time. The right time, in
their definition, is when (1) the item candidate space is small enough;
(2) asking additional questions is determined to be less useful or helpful,
from the perspective of either information gain or user patience; and (3)
the recommendation engine is confident that the top recommendations
will be accepted by the user.
The workflow of the EAR framework is illustrated in Fig. 5, where the
system has to decide whether to continue to ask questions about attri-
butes or to make a recommendation based on available information. To
determine when to ask a question, they construct the state of the RL
model to take into account four factors:
● Entropy information of each attribute among the attributes of the
current candidate items. Asking attributes with a large entropy helps
to reduce the candidate space, thus benefits finding desired items in
fewer turns.
● User preference on each attribute. The attribute with a high pre-
dicted preference is likely to receive positive feedback, which also
helps to reduce the candidate space.
● Historical user feedback. If the system has asked about a number of
attributes for which the user gives approval, it may be a good time to
recommend.
● Number of rest candidates. If the candidate list is short enough, the
system should turn to recommend to avoid wasting more turns.
Building on these vectors capturing the current state, the RL model
learns the proper timing to ask or recommend, which is more intelligent
than a fixed heuristic strategy.
During the conversation, the recommendation module takes the
items in the previous list of recommendations that are not chosen by
users as the negative samples. However, Lei et al. (2020a) mention that
this setting deteriorates the performance of the recommendation results.
The reason, as they analyze it, is that rejecting the produced attribute
does not mean that the user dislikes it: maybe the user does like it but
overlooks it or just wants to try other new things.
Furthermore, Lei et al. (2020b) extend the EAR model by proposing
the CPR model. By integrating the knowledge graph consisted of users,
items, and attributes, they model conversational recommendation as an
interactive path reasoning problem on the graph. A toy example of the
generated conversation of the CPR model is shown in Fig. 4. Unlike the
EAR model where the attributes to be asked are selected irregular and
unpredictable from all attribute candidates, CPR chooses attributes to be
asked and items to be recommended strictly following the paths on the
knowledge graph, which renders interpretable results.
In terms of the timing to ask or recommend, CRP makes an important
improvement: the action space of the RL policy is only two — asking an
attribute or making item recommendations. This largely reduces the
difficulty of learning the RL policy. The CPR model is much more effi-
cient than the EAR model due to the fact that the searching space of
attributes in CPR is constrained by the graph. The integration of
knowledge improves the multi-turn conversational reasoning ability.
3.2. Conversation strategies from a broader perspective
Although learning from the query-answering interactions can enable
the system to understand and respond to human query directly, the
system still lacks intelligence. One reason is that most CRS models as-
sume that users always bear in mind what they want, and the task is to
obtain the preference through asking questions. However, users who
resort to recommendation might not have a clear idea about what they
really want. Just like a human asks a friend for suggestions on restau-
rants. Before that, he may not have a certain target in mind, and his
decision can be affected by his friend’s opinions. Therefore, CRSs should
not only ask clarification questions and interrogate users, but also take
responsibility for leading the topics and affecting users’ mind. Towards
this objective, some studies try to enrich CRSs certain personalities or
endow CRSs the ability to lead the conversation, which can make the
dialogues more attractive and more engaging. These efforts can also be
found in the field of proactive conversation (Mo et al., 2018; Wu et al.,
2019; Balaraman and Magnini, 2020).
3.2.1. Multi-topic learning in conversations
Borrowing the idea from the proactive conversation, Liu et al. (Liu
et al., 2020ab) present a new task which places conversational recom-
mendation in the context of multi-type dialogues. In their model, the
system can proactively and naturally lead a conversation from a
non-recommendation dialogue (e.g., question answering or chitchat) to
a recommendation dialogue, taking into account the user’s interests and
feedback. And during the interaction, the system can learn to flexibly
switch between multiple goals. To address this task, they propose a
multi-goal driven conversation generation (MGCG) framework, which
consists of a goal planning module and a goal-guided responding mod-
ule. The goal-planning module can conduct dialogue management to
control the dialogue flow, which takes recommendation as the main goal
and complete the natural topic transitions as the short-term goals.
Specifically, given a user’s historical utterances as context X and the last
goal gt 1, the module estimates the probability of changing the goal gt of
the current task as PGC(gt ∕= gt 1|X, gt 1). The goal gt of the current task is
Fig. 5. The estimation-action-reflection workflow. Credits: Lei et al. (Lei
et al., 2020a).
C. Gao et al.
AI Open 2 (2021) 100–126
109changed when the probability PGC > 0.5 and remains to be gt 1 if PGC ≤
0.5. Based on the current goal, the framework can produce responses
from an end-to-end neural network.
Learning a multi-type conversational model requires a dataset that
supports multi-type dialogues. Therefore, Liu et al. (Liu et al., 2020ab)
create a dataset, denoted as DuRecDial, with various types of interac-
tion. In DuRecDial, two human workers are asked to conduct the con-
versation based on a given profile, which contains the information of
age, gender, occupation, preferred domains, and entities. The workers
must produce utterances that are consistent with their given profiles,
and they are encouraged to produce utterances with diverse goals, e.g.,
question answering, chitchat, or recommendation. Then these dialogue
data are labeled with goals and goal descriptions by templates and
human annotation.
Further, Zhou et al. (2020c) release a topic-guided conversational
recommendation dataset. They collect the review data from Douban
Movie ,2 a movie review website, to construct the recommended movies,
topic threads, user profiles, and utterances. And they associate each
movie with the concepts in ConceptNet (Speer et al., 2017), a
commonsense knowledge graph, for providing rich topic candidates.
Then they use rules to generate multi-turn conversations with diverse
topics based on the user profile and topic candidates. Based on the
proposed dataset, a new task of topic-guided conversational recom-
mendation is defined as follows: given the user profile Pu, user interac-
tion sequence Iu, historical utterances s1, …, sk 1, and corresponding
topic sequence {t1, …, tk 1}, the system should: (1) predict the next topic
tk, or (2) recommend the movie ik, and finally (3) produce a proper
response sk about the topic and with persuasive reasons.
3.2.2. Special ability: suggesting, negotiating, and persuading
There are miscellaneous tasks beyond the preference elicitation and
recommendation for an intelligent interactive system, which require the
CRS to possess different abilities to react in different scenarios. This is a
high-level and abstract requirement. A lot of effort have put into helping
the machine improve the topic’s guiding ability. For instance, in
conversational search (Voskarides et al., 2020; ter Hoeve et al., 2020;
Ren et al., 2020b; Vakulenko et al., 2020b; Vakulenko et al., 2021; Ren
et al., 2021), where traditional work has mainly attempted to better
understand a user’s information needs by resolving ambiguity, the
conversational search engine aims to lead the conversation with ques-
tions that a user may want to ask in the next step. For example, if a user
queried “Nissan GTR Price,” then the system can provide question sug-
gestions include those that help the user complete a task (“How much
does it cost to lease a Nissan GT-R?”), weigh options (“What are the pros
and cons of the Nissan GT-R?”), explore an interesting related topic (“Is
the Nissan GT-R the ultimate streetcar?”), or learn more details (“How
much does 2020 Nissan GTR cost?”) (Rosset et al., 2020). These question
suggestions can lead the user to an immersive search experience with
diverse and fruitful future outcomes.
In addition, Lewis et al. (2017) propose a system that is capable of
engaging in the negotiations with users. They define the problem as an
allocation problem: there are some items that need to be allocated to two
people, where each item has a different value to a different person and
people do not know the value of others. Hence, the two people have to
converse and negotiate with each other to reach an agreement about the
division of these items. Instead of optimizing relevance-based likeli-
hood, the model should pursue a maximal profit for both parties. The
authors use RL to tackle this problem. And they interleave RL updates
with supervised updates to avoid that the models diverges from human
language.
Wang et al. (2019) develop a model that tries to persuade users to
take certain actions, which is very promising for conversational
recommendation. They train the model, according to conversational
contexts, to learn and predict the 10 persuasion strategies (e.g., logical
appeal or emotion appeal) used in the corpus. And they analyze which
strategies are better conditioned on the background (personality, mo-
rality, value systems, willingness) of the user being persuaded.
Though some of these efforts are applied to specific application
scenarios in dialogue systems, these techniques can be adopted in the
multi-turn strategy in CRSs and thus push the development of CRSs.
3.3. Section summary
The multi-turn conversation strategies of CRSs discussed in this
section are summarized in Table 3. The main focus of the conversation
strategy is to determine when to elicit user preference by asking ques-
tions and when to make recommendations. As a recommendation should
only be made when the system is confident, an adaptive strategy can be
more promising compared to a static one. Besides this core function, we
introduce some strategies from a broader perspective. These strategies
can extend the capability of CRSs by means of leading multi-topic con-
versations (Liu et al., 2020ab; Zhou et al., 2020c) or showing special
ability such as suggesting (Rosset et al., 2020), negotiating (Lewis et al.,
2017), and persuading (Wang et al., 2019).
4. Dialogue understanding and generation in CRSs
An important direction of CRSs is to converse with humans in natural
languages, thus understanding human intentions and generating
human-understandable responses are critical. However, most CRSs only
extract key information from processed structural data and present the
result via rule-based template responses (Zhang et al., 2018; Zou et al.,
2020; Lei et al., 2020a, 2020b). This not only requires lots of labor to
construct the rule or template but also make the result rely on the pre-
processing. It also hurt user experience as the constrained interaction is
unnatural in real-world applications. Recently, we have witnessed the
development of end-to-end learning frameworks in dialogue systems,
which have been studying for years to automatically handle the se-
mantic information in raw natural language (Gao et al., 2019a; Lei et al.,
2018; Jin et al., 2018). We will introduce these natural language pro-
cessing (NLP) technologies in dialogue systems and describe how they
help CRSs understand user intention and sentiment and generate
meaningful responses.
4.1. Dialogue understanding
Understanding users’ intention is the key requirement for the user
interface of a CRS, as downstream tasks, e.g., recommendation, rely
heavily on this information. However, most CRSs pay attention to the
core recommendation logic and the multi-turn strategy, while they
circumvent extracting user intention from raw utterances and requires
the preprocessed input such as rating scores (Zhao et al., 2013; Chris-
takopoulou et al., 2016; Zou et al., 2020b; LeeJinbae et al., 2019),
YES/NO answers (Zou et al., 2020; Lei et al., 2020a, 2020b), or another
type of value or orientation (Christakopoulou et al., 2018; Zhang et al.,
2018) towards the queried items or attributes. This is unnatural in
real-life human conversation and imposes constraints on user expres-
sion. Thereby, it is necessary to develop methods to extract semantic
information in users’ raw language input, either in an explicit or implicit
way.
We introduce how dialogue systems use NLP technologies to address
this problem and give the examples of CRSs that use these technology to
understand user intention.
4.1.1. Slot filling
A common way used in dialogue systems to extract useful informa-
tion is to predefine some aspects of interest and use a model to fill out the
values of these aspects from users’ input, a.k.a, slot filling (Deng et al.,
2012; Deoras and Sarikaya, 2013; Yao et al., 2013, 2014; Mesnil et al.,2 https://movie.douban.com/.
C. Gao et al.
AI Open 2 (2021) 100–126
1102013; Pecune et al., 2020). Sun and Zhang (2018) first consider
extracting the semantic information from the raw dialogue in CRSs.
They propose a belief tracker to capture the facet-value pairs, e.g., (color,
red), from user utterances. Specifically, given a user utterance et at time
step t, the input to the belief tracker is the n-gram vector zt, which is
written as zt = n-gram(et), where the dimension of zt is the corpus size.
This means that only the positions corresponding to the words in ut-
terance et are set to 1, other positions will be set to 0. Suppose there are K
types of facet-value pairs, for a given facet m ∈{1, 2, …, K}, the user’s
sequential utterances z1, z2, …, zt are encoded by a LSTM model
(HochreiterJürgen Schmidhuber, 1997) to learn the latent vector fm for
this facet m. The size of vector fm is set to the number of values, e.g., the
number of available colors. The vector fm capturing the facet-value in-
formation will be used in the recommendation module and policy
network later. Besides, Ren et al. (2020), Tsumita and Takagi (2019)
also employ recurrent neural networks (RNN)-based methods to extract
the facet-value information as input for in downstream tasks in their
CRSs.
However, explicitly modeling semantic information as aspect-value
pairs can be a limitation in some scenarios where it is difficult and
also unnecessary to do that. Besides, aspect-value pairs cannot precisely
express information such as user intent or sentiment. Therefore, some
recent CRSs use end-to-end neural frameworks to implicit learning the
representation of users’ intentions and sentiment.
4.1.2. Intentions and sentiment learning
Neural networks are famous for extracting features automatically, so
it can be used to extract users’ intentions and sentiment in CRSs. An
classic example in CRSs is the end-to-end framework that proposed by Li
et al. (2018), which takes the user’s raw utterances as input and directly
produces the responses in the interaction. They collect the REDIAL
dataset 3 through the crowdsourcing platform Amazon Mechanical Turk
(AMT) .4 They pair up AMT workers and give each of them a role. The
movie seeker has to explain what kind of movie he/she likes, and asks
for movie suggestions. The recommender tries to understand the
seeker’s movie tastes and recommends movies. All exchanges of infor-
mation and recommendations are made using natural language; every
movie mention is tagged using the “@” symbol to let the machine know
it is a named entity. In this way, the dialogues in the REDIAL data
contain the required semantic information that can help the model learn
to answer users with recommendations and reasonable explanations. In
addition, three questions are asked to provide labels for supervised
learning: (1) Whether the movie was mentioned by the seeker, or was a
suggestion from the recommender (“suggested” label). (2) Whether the
seeker has seen the movie (“seen” label): one of Have seen it, Haven’t seen
it, or Didn’t say. (3) Whether the seeker liked the movie or the suggestion
(“liked” label): one of Liked, Didn’t like, Didn’t say. The three labels are
collected from both the seeker and the recommender.
In this way, although the facet-value constraints are removed, all
kinds of information including mentioned items and attributes, user
attitude, and user interest are preserved and labeled in the raw utter-
ance. And the CRS model needs to directly learn users’ sentiment (or
preferences), and it will make recommendations and generate responses
based on the learned sentiment. The deep neural network-based model
consists of four parts: (1) A hierarchical recurrent encoder implemented
as a bidirectional GRU (ChoBart van Merri¨enboer et al., 2014) that
transforms the raw utterances into a latent vector with the key semantic
information remained. (2) At each time a movie entity is detected (with
the “@” identifier convention), an RNN model is instantiated to classify
the seeker’s sentiment or opinion regarding that entity. (3) An
autoencoder-based recommendation module that takes the sentiment
prediction as input and produces an item recommendation. (4) A
switching decoder generating the response and deciding whether the
name of the recommended item is included in the response. The model
generates a complete sentence that might contain a recommended item
to answer each user’s utterance.
Beside using the RNN-based neural networks, there are some CRSs
that adopt the convolutional neural network (CNN) model (Ren et al.,
2020; Liu et al., 2020ab), which has been proven to be very effective for
modeling the semantics from raw natural language (Kim, 2014). How-
ever, deep neural networks are often criticized to be non-transparent
and hard to interpretable (Buhrmester et al., 2019). It is not clear how
the deep language models can help CRSs in understanding user needs.
In order to answer this question, Penha and Hauff (2020) investigate
the bidirectional encoder representations from transformers (BERT)
(Devlin et al., 2019), a powerful technology for NLP pre-training
developed by Google, to analyze whether its parameters can capture
and store semantic information about items such as books, movies, and
music for CRSs. The semantic information includes two kinds of
knowledge needed for conducting conversational search and recom-
mendation, namely content-based and collaborative-based knowledge.
Content-based knowledge is knowledge that requires the model to
match the titles of items with their content information, such as textual
descriptions and genres. In contrast, collaborative-based knowledge
requires the model to match items with similar ones, according to
community interactions such as ratings. The authors use the three
probes on the BERT model (i.e., tasks to examine a trained model
regarding certain properties) to achieve the goal. And the result shows
that both collaborative-based and content-based knowledge can be
learned and remembered. Therefore, the end-to-end language model has
potential as part of CRS models to interact with humans directly in
real-world applications with complex contexts.
4.2. Response generation
A natural language-based response of a CRS should at least meet two
levels of standards. The lower level standard requires the generated
language to be proper and correct; the higher level standard requires the
Table 3
The commonly used multi-turn strategies in CRSs.
Main
Mechanism
Asking
Method
When to ask and recommend Determining X and
Y
Publications
Asking
questions
Explicit Asking 1 turn; recommending 1 turn Fixed (Christakopoulou et al., 2018; Yu et al., 2019b)
Asking X turn(s); recommending 1
turn
Fixed Zou et al. (2020)
Adaptive Sun and Zhang (2018)
Asking X turn(s); recommending Y
turn(s)
Adaptive (Zhang et al., 2018; Lei et al., 2020a, 2020b; Li et al., 2021b; Xu et al., 2021)
Implicit Contained in natural language Adaptive (Li et al., 2018; Chen et al., 2019bb; Zhou et al., 2020a; Zhou et al., 2020c)
Leading diverse topics or explore special abilities (Liu et al., 2020ab; Zhou et al., 2020c; Rosset et al., 2020; Lewis et al., 2017;
Wang et al., 2019)
3 https://redialdata.github.io/website/.
4 https://www.mturk.com/.
C. Gao et al.
AI Open 2 (2021) 100–126
111response contains meaningful and useful information about recom-
mended results.
4.2.1. Generating proper utterances in natural language
Many CRSs use template-based methods to generate responses in
conversations (Sun and Zhang, 2018; Lei et al., 2020a, 2020b). How-
ever, template-based methods suffer from producing repetitive and
inflexible output, and it require intense manual work. Besides,
template-based responses could make users uncomfortable and hurt user
experience. Hence, it is important to automate the response generation
in CRSs to produce proper and fluent responses. This is also the objective
of dialogue systems, so we introduce two veins of technologies for
producing responses in dialogue systems:
Retrieval-based Methods. The basic idea is to retrieve the appro-
priate response from a large collection of response candidate. This
problem can be formulated as a matching problem between an input
user query and the candidate responses. The most straightforward
method is to measure the inner-product of the feature vectors repre-
senting a query and a response (Wu and Yan, 2018). A key challenge is to
learn a proper feature representation (Wu and Yan, 2018). One strategy
is to use neural networks to learn the representation vectors from user
query and candidate response, respectively. Then, a matching function is
used to combine the two representations and output a matching prob-
ability (Hu et al., 2014; TanCicero dos Santos et al., 2016; Qiu and
Huang, 2015; Feng et al., 2015; Wang et al., 2016b). An alternative
strategy, in contrast, is to combine the representation vectors of query
and response first, and then a neural method is used on the combined
representation pair to further learn the interaction (Wang and Jiang,
2016; Wan et al., 2016; Pang et al., 2016; Lu and Li, 2013). These two
strategies have their own advantages: the former is more efficient and
suitable for online serving, while the latter is better at efficacy since the
matching information is sufficiently preserved and mined (Wu and Yan,
2018).
Generation-based Methods. Unlike retrieval-based methods, which
select existing responses from a database of template response,
generation-based methods directly produce a complete sentence from
the model. The basic generation model is a recurrent sequence-to-
sequence model, which sequentially feeds in each word in the query
as input, and then generates the output word one by one (SutskeverOriol
VinyalsQuoc, 2014). Compared to retrieval-based methods,
generation-based methods have some challenges. First, the generated
answer is not guaranteed to be a well-formed natural language utterance
(Yan et al., 2016). Second, even though the generated response may be
grammatically correct, we can still distinguish a machine-generated
utterance from a human-generated utterance, since the machine
response lacks basic commonsense (Young et al., 2018; Zhou et al.,
2018c; Ren et al., 2020a), personality (Qian et al., 2018; Zheng et al.,
2020), emotion (Zhou et al., 2018b), and the ability to perceive user
profiles (Pei et al., 2021). Even worse, generation models are prone to
produce a safe answer, such as “OK,” “I don’t understand what you are
talking about,” which can fit in almost all conversational contexts but
would only hurt the user experience (Li et al., 2016a; Qiu et al., 2019).
Ke et al. (2018) propose to explicitly control the function of the gener-
ated sentence, for example, for the same user query, the system can
answer with different tones: The interrogative tone can be used to ac-
quire further information; the imperative tone is used to make requests,
directions, instructions or invitations to elicit further interactions; and
the declarative tone is commonly used to make statements or explana-
tions. Another problem is how to evaluate the generated response, since
there is no standard answer; we will further discuss this in Section 6.
Researchers borrow the ideas from dialogue systems and apply the
technologies in the user inferface of CRSs. For instance, Li et al. (2018)
generate responses by a decoder where a GRU model (ChoBart van
Merri¨enboer et al., 2014) decodes the context from the previous
component (i.e., predicted sentiment towards items) to predict the next
utterance step by step. Liu et al. (Liu et al., 2020ab) adopt the
responding model in the work of Wu et al. (2019) and propose both a
retrieval-based model and a generation-based model to produce re-
sponses in their CRS.
However, a correct sentence does not mean it can fulfill the task of
recommendation; at least the name of the recommended entity should
be mentioned in generated sentences. Hence, Li et al. (2018) use a
switch to decide whether the next predicted word is a movie name or an
ordinal word; Liu et al. (Liu et al., 2020ab) introduce an external
memory module for storing all related knowledge, making the models
select appropriate knowledge to enable proactive conversations. Be-
sides, there are other efforts to guarantee the generated responses should
not only be proper and accurate but also be meaningful and useful.
4.2.2. Incorporating recommendation-oriented information
There is a major limitation CRSs that use the end-to-end frameworks
as the user interface: only items mentioned in the training corpus have a
chance of being recommended since items that have never been
mentioned are not modeled by the end-to-end model. Therefore, the
performance of this method is greatly limited by the quality of human
recommendations in the training data. To overcome this problem, Chen
et al. (Chen et al., 2019bb) propose to incorporate domain knowledge to
assist the recommendation engine. The incorporation of a knowledge
graph mutually benefits the dialogue interface and the recommendation
engine in the CRS. (1) the dialogue interface can help the recommender
engine by linking related entities in the knowledge graph; the recom-
mendation model is based on the R-GCN model (Schlichtkrull et al.,
2018) to extract information from the knowledge graph; (2) the
recommender system can also help the dialogue interface: by mining
words with high probability, the dialogue can connect movies with some
biased vocabularies, thus it can produce consistent and interpretable
responses.
Following this line, Zhou et al. (2020a) point out the remaining
problems in the dialogue interface in CRSs. Although Chen et al. (Chen
et al., 2019bb) have introduced an item-oriented knowledge graph to
enable the system to understand the movie-related concepts, the system
still cannot comprehend some words in the raw utterances. For example,
“thriller”, “scary”, “good plot”. In essence, the problem originates from
the fact that the dialogue component and the recommender component
correspond to two different semantic spaces, namely word-level and
entity-level semantic spaces. Therefore, Zhou et al. (2020a) incorporate
and fuse two special knowledge graphs, i.e., a word-oriented graph
(ConceptNet (Speer et al., 2017)), and an item-oriented graph (DBpedia
(Bizer et al., 2009)), to enhance understanding semantics in both the
components. The representations of the same concepts on the two
knowledge graphs are forced to be aligned with each other via the
mutual information maximization (MIM) technique (Veliˇckovi´c et al.,
2019; Yeh and Chen, 2019). Furthermore, a self-attention-based
recommendation model is proposed to learn the user preference and
adjust the representation of corresponding entities on the knowledge
graph. Then, equipped with these representations containing both se-
mantics and users’ historical preferences, the authors use an
encoder-decoder model to extract user intention from the raw utterances
and directly generate the responses containing recommended items.
Besides, some researchers try to improve the diversity or explain-
ability of generated responses in CRSs. For example, Liu et al. (Liu et al.,
2020ab) propose the multi-topic learning that can handle diverse dia-
logue types in CRSs. To enhance the interpretability of CRSs, Chen et al.
(2020b) design an incremental multi-task learning framework to inte-
grate review comments as side information. Hence, the CRS can simul-
taneously produce a recommendation as well as a sentence as an
explanation, e.g., “I recommend Mission Impossible, because it is by far
the best of the action series.” Moreover, Luo et al. (2020b) use a
VAE-based architecture to learn a latent representation for generating
recommendations and fitting user critiquing. Therefore, their model can
better understand users’ intentions from users’ raw comments, and thus
can generate more interpretable responses. Gao et al. (2020) consider
C. Gao et al.
AI Open 2 (2021) 100–126
112attributes and review information and rewrite a coherent and mean-
ingful answer from a selected prototype answer, which can address the
safe answer problem in the response (Li et al., 2016a; Qiu et al., 2019).
4.3. Section summary
In Table 4, we classify CRSs into two classes in terms of the forms of
input and output. Generally, interactive recommendations (Zou et al.,
2020a; Wang et al., 2020c; ZhangTong et al., 2019b; Ding et al., 2020),
critiquing methods (Chen and Pu, 2012; Wu et al., 2019a; Luo et al.,
2020b; LuoScott et al., 2020), and CRSs focusing on the multi-turn
conversation strategy (Christakopoulou et al., 2016, 2018; Lei et al.,
2020a, 2020b; Li et al., 2021b) are prone to use the pre-annotated input
and rule-based or template-based output; dialogue systems (Young et al.,
2018; Zhou et al., 2018c; Gao et al., 2020) and CRSs caring about the
dialogue ability (Li et al., 2018; Chen et al., 2019bb; Zhou et al., 2020a)
are more likely to use raw natural language as input and automatically
generate responses. In the future, user understanding and response
generation in CRSs will remain a critical research field, as they serve as
the interface of CRSs and directly impact the user experience.
5. Exploration-exploitation trade-offs
One challenge of CRSs is to handle the cold-start users that have few
historical interactions. A natural way to tackle this is through the idea of
the Exploration-Exploitation (E&E) trade-off. With exploitation, the
system takes advantage of the best option that is known; with explora-
tion, the system takes some risks to collect information about unknown
options. In order to achieve long-term optimization, one might make a
short-term sacrifice. In the early stages of E&E, an exploration trial could
be a failure, but it warns the model to not take that action too often in the
future. Although the E&E trade-off is mainly used for the cold-start
scenario in CRSs, it can also be used for improving the recommenda-
tion performance for any users (including cold users and warm-up users)
in recommendation systems.
MAB is a classic problem formulated to illustrate the E&E trade-off,
and many algorithms have been proposed to solve the problem. In CRSs,
the MAB-based algorithms are introduced to help the system improve its
recommendation. Besides, there are also CRSs that use meta-learning to
balance E&E. We first introduce MAB and common MAB-based algo-
rithms in recommender systems, then we present examples how CRSs
balance E&E in their models.
5.1. Multi-armed bandits in recommendation
We first introduce the general MAB problem and the classic methods
to solve it, then we introduce how recommender systems use MAB-based
methods to achieve the E&E balance.
5.1.1. Introduction to multi-armed bandits
MAB is a classic problem that well demonstrates the E&E dilemma
(Katehakis and Veinott, 1987; Auer et al., 2002). The name comes from
the story where a gambler at a row of slot machines (each of which is
known as a “one-arm bandit”) wants to maximize his expected gain and
has to decide which machines to play, how many times to play each
machine, in which order to play them, and whether to continue with the
current machine or try a different machine. The problem is difficult
because all of the slot machines are black boxes, whose properties, i.e.,
the probability of winning, can only be estimated by the rewards
observed in previous experiments.
Formally, the problem is to maximize the cumulative reward ∑T
t=1ra,t
after T rounds of arm selection. Here, ra,t is the reward with arm 0 ≤ a ≤
K selected at trial t, K is the total number of arms. Fig. 6 illustrates an
example in which a gambler decides which arm to choose now. For a
certain arm, a reward distribution is estimated based on previous
experiment results. The gambler can, naturally, select to exploit the
second arm which has the maximal mean reward μ(a). Or, he can take
some risks to explore the other arm, e.g., the third arm, which has a
higher uncertainty Δ(a) and thus has the maximal upper confidence
bound (UCB) of the reward μ(a) + Δ(a). After each time he plays an arm,
the new reward value is observed, and the estimated reward distribution
of this arm can be updated accordingly. With exploration, the gambler
hopes to find the potential arms that have higher rewards, though it can
also end up in lower rewards. In any case the gambler has a better
estimation of the rewards of those arms.
Equivalently, the problem can also be formulated as minimizing the
regret function, which is the difference between the theoretically
optimal expected cumulative reward and the estimated expected cu-
mulative reward:
E
[ ∑T
t=1
rt,a*
]
 E
[ ∑T
t=1
rt,a
]
, (5)
where a* is the theoretically optimal arm with the maximum expected
reward at all times.
The commonly used bandit strategies include the greedy strategy, i.
e., the exploit-only strategy that always selects the arm with the current
estimated highest reward; the random strategy, i.e., a trivial explore-
only strategy; and ε-greedy, which mixes the greedy and random stra-
tegies via a trigger with probability ε. Other classic models include
Upper Confidence Bound (UCB) (Auer, 2002; Auer et al., 2002) and
Thompson Sampling (TS) (Chapelle and Li, 2011) which are introduced
next.
Table 4
Mechanisms of language understanding and generation in CRSs.
Forms of Input & Output Publications
Pre-annotated Input &
Template-based Output
(Zhao et al., 2013; Zou et al., 2020; LoeppTim Hussein and Ziegler, 2014; Zhang et al., 2018; Sun and Zhang, 2018),
(Christakopoulou et al., 2016, 2018; Lei et al., 2020a, 2020b; Li et al., 2021b; Fu et al., 2021)
Raw Language Input &
Natural Language Generation
(Ren et al., 2020; Li et al., 2018; Chen et al., 2019bb),
(Zhou et al., 2020a; Ma et al., 2020; Liu et al., 2020ab)
Fig. 6. An illustration of the multi-armed bandit problem.
C. Gao et al.
AI Open 2 (2021) 100–126
1135.1.2. Recommendation via MAB-based methods
As the classic algorithm for E&E trade-offs, MAB-based models can
be seamlessly plugged into the online recommendation setting (Zeng
et al., 2016; Zheng et al., 2018), interactive recommendation (Zhao
et al., 2013; Wang et al., 2017), and CRSs (Christakopoulou et al., 2016;
Zhang et al., 2020c; Li et al., 2021b). In the online or interactive
recommendation tasks, the system aims to recommend the optimal item
(s) according to users’ previous feedback. This process can be deemed as
a MAB problem, where each arm corresponds to an item. Therefore, the
classical MAB-based methods can be plugged in this situation.
However, traditional bandit methods only consider treating items as
independent arms and ignore the item features (Li et al., 2010). Directly
estimating each item’s probability of being chosen based on the accu-
mulated rewards is rather inefficient due to a large number of items. In
recommendation, there is a rich set of features on users and items, and
whether a user ut would choose item at can be predicted by the features
of both ut and at. Motivated by this, Li et al. (2010) propose a linear
contextual bandit model called LinUCB, which is the first bandit model
that considers the contextual information (i.e., user/item features) in
recommendation systems.
For each trial t, they assume the expected reward rt of a user ut
choosing an arm (item) at is linear in its d-dimensional feature vector
xut ,at with the unknown coefficient vector θ*
a (which is determined on
this arm at rather than other arms); namely, for all trial t,
E[rt,a⃒⃒ xut ,at
] = x⊤
ut ,at θ*
a, (6)
where the feature vector xut ,at summarizes information of both user ut
and arm (item) at, and is referred to as the context. The coefficients θ*
a
can be learned from the historical interactions and feedback. Specif-
ically, let Da be a design matrix of dimension m × d at trial t, e.g., m
contexts that are observed previously for arm a, and ct ∈ Rm be the
corresponding reward vector, the coefficients θ*
a are estimated by
applying ridge regression to the training data (Da, ca) as: ̂
θa =  D⊤
a Da + Id
) 1D⊤
a ca,
where Id is the d × d identity matrix. When components in ca are in-
dependent conditioned on corresponding rows in Da, it can be shown
that with probability at least 1  δ, ⃒⃒⃒
x⊤
ut ,at̂ θa  E[rt,a⃒⃒ xut ,at
]⃒⃒⃒ ≤ α̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅ x⊤
ut ,at
 D⊤
a Da + Id
) 1xut ,at
√
,
for any δ > 0 and xut ,at ∈ Rd, where α = 1 +̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅ ln(2/δ)/2
√ is a constant.
Therefore, the inequality gives a reasonably tight UCB for the expected
reward of arm at, from which the arm-selection (recommendation)
strategy can be derived: at each trial t, choose
at =
def arg maxa∈𝒜t
(
x⊤
ut ,at̂ θa + α̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅̅ x⊤
ut ,at
 D⊤
a Da + Id
) 1xut ,at
√ )
.
Actually, the contextual bandit model improves the recommendation
by leveraging the user/item features through the idea of collaborative
filtering (SarwarGeorge et al., 2001; Schafer et al., 2007), i.e., those
items are more likely to be recommended to a user who showed pref-
erence for items with similar features.
There are also studies pointing out that exploration in recommen-
dations is important, i.e., the recommendations should be diverse
instead of being limited by similar items (Qin et al., 2014; Liu et al.,
2020bb; Ding et al., 2020). For instance, Ding et al. (2020) consider the
fact that users may have different preference with regard to the diversity
of items, e.g., a user with specific interest may prefer a relevant item set
than a diverse item set, while another user without specific interest may
prefer a diverse item set to explore his interests. Therefore, the authors
propose a bandit learning framework to consider the user’s preferences
on both the item relevance features and the diversity features. It is a way
to trade off the accuracy and diversity of recommendation results.
Besides, Yu et al. (2019b) use a cascading bandit in a visual dialogue
augmented interactive recommender system. In cascading bandits, the
user examines the recommended list from the first item to the last and
selects the first attractive one (Kveton et al., 2015; Zong et al., 2016).
This setting is practical to implement in online recommender systems or
search engines. It has an excellent advantage as it can provide reliable
negative samples, which are critical for recommendation, and the
problem has drawn a lot of research attention (Chen et al., 2019, Ding
et al., 2019; Wang et al., 2020c; LianQi and Chen, 2020; Chen et al.,
2019). Since the system can ensure that the items before the first
selected one are not attractive, thus it can easily obtain reliable negative
samples. Another contribution is the use of the item’s visual appearance
and user feedback to design more efficient exploration.
In addition, there are other efforts to enhance bandit methods in
different recommendation scenarios. For instance, Chou et al. (2015)
indicate that a user would only choose one or a few arms in the candi-
dates, leaving out the informative non-selected arms. They propose the
concept of pseudo-rewards, which embeds estimates to the hidden re-
wards of non-selected actions under the bandit setting. Wang et al.
(2018) consider dependencies among items and explicitly formulate the
item dependencies as clusters on arms, where arms within a single
cluster share similar latent topics. They adopt a generative process based
on a topic model to explicitly formulate the arm dependencies as the
clusters on arms, where dependent arms are assumed to be generated
from the same cluster. Yang et al. (2020) consider the situations where
there are exploration overheads, i.e., there are non-zero costs associated
with executing a recommendation (arm) in the environment, and hence,
the policy should be learned with a fixed exploration cost constraint.
They propose a hierarchical learning structure to address the problem.
Sakhi et al. (2020) state that the online bandit signal is sparse and un-
even, so they utilize the massive offline historical data. The difficulty is
that most of offline data is irrelevant to the recommendation task, and
the authors propose a probabilistic model to solve it.
The advantage of multi-armed bandit methods is their ability to
conduct online learning, enabling the model to learn the preferences of
cold users and adjust the strategy quickly after several trials to pursue a
global optimum.
5.2. Multi-armed bandits in CRSs
The ability to interact with users enables CRSs to directly use MAB-
based methods to help the recommendation. Christakopoulou et al.
(2016) propose a classic CRS based on MAB, which uses several naive
MAB-based methods to enhance the offline probabilistic matrix factor-
ization (PMF) model (Salakhutdinov and Mnih, 2007). They first
initialize the model parameters using offline data, then leverage
real-time user feedback to update parameters via several common
multi-armed bandit models, including the aforementioned greedy
strategy, random strategy, UCB (Auer, 2002; Auer et al., 2002), and TS
(Chapelle and Li, 2011). On the one hand, the performance improves on
the initialized model due to the online updating; on the other hand, the
offline initialization helps bandit methods reduce the computational
complexity.
As mentioned above, the original MAB methods ignore item features,
which could be very helpful in recommendation. Hence, Zhang et al.
(2020c) propose a conversational upper confidence bound (ConUCB)
algorithm to apply the LinUCB model (Li et al., 2010) in the CRS context.
Instead of asking items, ConUCB asks the user about one or more attri-
butes (key-terms in their work). Specifically, they make the assumption
that user preference on attributes can propagate to items, hence the
system can analyze user feedback on queried attributes to quickly nar-
row down the item candidates. The strategies to select the attributes and
arms depend on both the attribute-level and arm-level rewards, i.e., the
feedback on attributes and items will be absorbed into the model pa-
rameters for future use. In addition, the authors employ a hand-crafted
C. Gao et al.
AI Open 2 (2021) 100–126
114function to determine the timing to ask attributes or make recommen-
dation, e.g., making k conversations in every m rounds.
However, hand-crafted strategies are fragile and inflexible, as the
system should make recommendation only when the confidence is high.
Therefore, Li et al. (2021b) propose a Conversational Thompson Sam-
pling method (ConTS) to automatically alternate asking questions about
attributes with recommending items. They achieve this goal by unifying
all attributes and items in the same arm pool, thus an arm selected from
the arm pool can be either a recommendation about an item or a
question about an attribute. The flowchart of ConTS is illustrated in
Fig. 7. ConTS assumes each user’s preference vector ̃ u is sampled from a
prior Gaussian distribution as ̃ u ∼ 𝒩
(
μu, l2B 1
u
)
, where the μu, B, and l
are parameters.
For each new-coming user, the mean of prior Gaussian distribution,
μu, is initialized by the average of existing users’ preference vector 𝒰old
as:
μu = 1
|𝒰old|
∑|𝒰old |
i=1
ui, ui ∈ 𝒰old . (7)
The expected reward of arm a (which can either be an item or an
attribute) for user u is also formulated as a Gaussian distribution since
the Gaussian family is conjugate to itself. The expected reward is written
as:
r(a, u, ℘u) ∼ 𝒩
(̃
u⊤xa + ∑
pi ∈℘u
xT
a pi , l2
)
, (8)
where ℘u denotes the user’s currently known preferred attributes ob-
tained in historical conversations. And xa represents the embedding
vector of an arm. In the reward function, the term ̃ u⊤xa models the
general preference of user u to arm a, and the term ∑
pi ∈℘u xT
a pi models
the affinity between arm a and the user’s preferred attributes ℘u. Then
ConTS select an arm with the maximal reward as:
a(t) = argmaxa⊂𝒜ũ u⊤xa + ∑
pi ∈℘u
xT
a pi. (9)
Note that if the a(t) is an attribute, the system will query the user
about the preference on this attribute; if it is an item, the system will
make a recommendation using this item. After obtaining users’ feed-
back, parameters such as μu, ℘u, B will be updated accordingly.
5.3. Meta learning for CRSs
Beyond multi-armed bandits, there are work trying to balance
between exploration and exploitation via meta learning. For instance,
Zou et al. (2020b) formulate the interactive recommendation as a
meta-learning problem, where the objective is to learn a learning algo-
rithm that takes the user’s historical interactions as the input and out-
puts a model (policy function) that can be applied to new users. The
authors follow the idea of meta reinforcement learning (DuanJohn
Schulman et al., 2016) and use Q-Learning (Mnih et al., 2013) to learn
the recommendation policy. The exploration strategy is the aforemen-
tioned ε-greedy, where the model will select the items of maximum
Q-value with probability 1  ε, and choose random items with proba-
bility ε.
In addition, Lee et al. (LeeJinbae et al., 2019) address the cold-start
problem in recommendation via a model based on the Model-Agnostic
Meta-Learning (MAML) algorithm (Finn et al., 2017). The learned
recommendation model can quickly adapt to the cold user preference in
the fine-tuning stage by asking the cold user a few questions about
certain items (called the evidence candidates in the work). A drawback
of this work is that the evidence candidates are only selected once, and
the query process is conducted only at the beginning when cold users
arrived. It could be better to extend this strategy to a CRS setting and
develop a dynamic multi-round query strategy to further enhance the
recommendation.
5.4. Section summary
In this section, we introduce how a CRS can solve the cold-start
problem and trade off the E&E balance via the interactive models such
as MAB-based methods and meta learning methods. The solutions are
summarized in Table 5. It still has a lot of room for CRSs to develop
potential models to address the E&E problem, in order to improve the
user experience.
6. Evaluation and user simulation
In this section, we discuss how to evaluate CRSs, which is an
underexplored problem. We group attempts to evaluate CRSs into two
classes: (1) Turn-level evaluation, which evaluates a single turn of the
system output, including the recommendation task and response gen-
eration task, which are both supervised prediction tasks. (2)
Conversation-level evaluation, which evaluates the performance of the
multi-turn conversation strategy which is a sequential decision making
task. To achieve the goal, user simulation is important. We first intro-
duce the commonly used datasets in CRSs, and then we introduce the
metrics, methods, and problems in the turn-level and conversation-level
evaluation of CRSs. Finally, we discuss the strategies of user simulation
in CRSs.
6.1. Datasets and tools
We list the statistics of the commonly used CRS datasets in Table 6.
Some studies collect human-human and human-machine conversation
data by asking true users to converse using natural language under
certain rules. To guarantee the quality of the data, these users will be
rewarded after providing qualified data. There are crowdsourcing sites,
such as Amazon Mechanical Turk (AMT),5 where the researchers can
find participants to fulfill their data collection task (Li et al., 2018; Moon
et al., 2019; Liu et al., 2020ab; Hayati et al., 2020).
As mentioned earlier, a lot of studies of CRS focus on the interaction
policy and the recommendation strategy instead of language under-
standing and generation. Thus, all these studies need is the labeled en-
tities (including users, items, attributes, etc.) in the multi-turn
conversation (Zhang et al., 2018; Christakopoulou et al., 2018; Lei et al.,
2020a, 2020b; Li et al., 2021b; Fu et al., 2021). These studies mainly
Fig. 7. The flowchart of the ConTS algorithm. Credits: Li et al. (Li
et al., 2021b).
5 https://www.mturk.com/.
C. Gao et al.
AI Open 2 (2021) 100–126
115simulate and construct the user interaction from the historical records in
traditional recommendation datasets, e.g., MovieLens (Bertin-Mahieux
et al., 2011), LastFM (Bertin-Mahieux et al., 2011), Yelp,6 and Amazon
dataset (McAuley et al., 2015b).
Although it seems to be many datasets in CRSs, these datasets are not
qualified to develop the CRSs that can work in industrial applications.
The reason is twofold: first, the scale of these datasets is not enough to
cover the real-world entities and concepts; second, the conversation is
either constructed from the non-conversation data or generated under
certain rigorous constraints, so it is hard to generalize to the complex
and diverse real-world conversations. Therefore, more effort is needed
to develop large-scale, generalizable, natural datasets for CRSs. There-
fore, more effort is still needed to develop large-scale, generalizable,
diverse, and natural datasets for CRSs.
There are many different settings in CRSs, making comparison be-
tween different models difficult. Recently, Zhou et al. (2021) have
Table 6
Statistics of commonly used datasets of CRSs.
Dataset #Dialogs #Turns Dialogue
Type
Domains Dialogue Resource Related Publications
MovieLens (
Bertin-Mahieux et al.,
2011)
Depended on the dialogue simulation
process
Movie From item ratings (Zhao et al., 2013; LoeppTim Hussein and
Ziegler, 2014; Vendrov et al., 2020; Zou
et al., 2020b),
(LeeJinbae et al., 2019; Iovine et al., 2020;
Habib et al., 2020)
LastFM (Bertin-Mahieux
et al., 2011)
Music From item ratings (Lei et al., 2020a, 2020b; Zhou et al., 2020b)
Yelp Restaurant From item ratings (Sun and Zhang, 2018; Lei et al., 2020a,
2020b)
Amazon (McAuley et al.,
2015b)
E-commerce From item ratings (Zhang et al., 2018; Fu et al., 2020; Zou
et al., 2020; Penha and Hauff, 2020),
(Wu et al., 2019a; Luo et al., 2020b;
LuoScott et al., 2020a; Fu et al., 2021)
TG-ReDial (Zhou et al.,
2020c)
10,000 129,392 Rec.,
chichat
Movie, multi
topics
From item rating, and enhanced by
multi topics
Zhou et al. (2020c)
Facebook_Rec (Dodge
et al., 2016)
1M 6M Rec. Movie From item ratings Dodge et al. (2016)
COOKIE (Fu et al., 2020) Not
given
11,638,418 Rec. E-commerce From interactions and reviews on
Amazon dataset (McAuley et al.,
2015b)
Fu et al. (2020)
HOOPS (Fu et al., 2021) Not
given
11,638,418 Rec. E-commerce From interactions and reviews on
Amazon dataset (McAuley et al.,
2015b)
Fu et al. (2021)
DuRecDial (Liu et al.,
2020ab)
10,190 155,477 Rec., QA,
etc.
Movie,
restaurant, etc.
Generated by workers (Liu et al., 2020ab)
OpenDialKG (Moon et al.,
2019)
15,673 91,209 Rec.
chitchat
Movie, book,
sport, etc.
Generated by workers Moon et al. (2019)
ReDial (Li et al., 2018) 10,006 182,150 Rec.,
chitchat
Movie Generated by workers (Li et al., 2018; Chen et al., 2019bb; Zhou
et al., 2020a; Ma et al., 2020)
MGConvRex (Xu et al.,
2020)
7.6K+ 73K Rec. Restaurant Generated by workers Xu et al. (2020)
GoRecDial (Kang et al.,
2019; Ma et al., 2020)
9,125 170,904 Rec. Movie Generated by workers Kang et al. (2019)
INSPIRED (Hayati et al.,
2020)
1,001 35,811 Rec. Movie Generated by workers Hayati et al. (2020)
ConveRSE (Iovine et al.,
2019)
Not
given
9,276 Rec. Movie, books,
music
Generated by workers (Iovine et al., 2019, 2020)
Table 5
E&E-based methods adopted by interactive recommender systems (IRSs) and CRSs.
Mechanism Publications
MAB in IRSs Linear UCB considering item features Li et al. (2010)
Considering diversity of recommendation (Qin et al., 2014; Liu et al., 2020bb; Ding et al., 2020)
Cascading bandits providing reliable negative samples (Kveton et al., 2015; Zong et al., 2016)
Leveraging social information Yu et al. (2019b)
Combining offline data and online bandit signals Sakhi et al. (2020)
Considering pseudo-rewards for arms without feedback Chou et al. (2015)
Considering dependency among arms Wang et al. (2018)
Considering exploration overheads Yang et al. (2020)
MAB in CRSs Traditional bandit methods in CRSs Christakopoulou et al. (2016)
Conversational upper confidence bound Zhang et al. (2020c)
Conversational Thompson Sampling Li et al. (2021b)
Cascading bandits augmented by visual dialogues Yu et al. (2019b)
Meta learning for CRSs Learning to learn the recommendation model (LeeJinbae et al., 2019; Zou et al., 2020b; Wei et al., 2020)
6 https://www.yelp.com/dataset.
C. Gao et al.
AI Open 2 (2021) 100–126
116implemented an open-source toolkit, called CRSLab,7 for building and
evaluating CRSs. They unify the tasks in existing CRSs into three
sub-tasks: namely recommendation, conversation and policy, which
correspond to our three components in Fig. 3: recommendation engine,
user interface, and conversation strategy module, respectively. Some
models and metrics are implemented under the three tasks, and the
toolkit contains an evaluation module that can not only conduct the
automatic evaluation but also the human evaluation through an inter-
action interface, which makes the evaluation of CRSs more intuitive.
However, up to now, the majority of implemented methods are based on
end-to-end dialogue systems (Li et al., 2018; Chen et al., 2019bb; Zhou
et al., 2020a) or deep language models (Zhou et al., 2020c); the CRSs
that focus on the interaction policy and the multi-turn conversation
strategies ((Lei et al., 2020b; Lei et al., 2020a)) are absent.
6.2. Turn-level evaluation
The fine-grained evaluation of CRSs is conducted on the output of
each single turn, which contains two tasks: language generation and
recommendation.
6.2.1. Evaluation of language generation
For CRS models that generate natural language-based responses to
interact with users, the quality of the generated responses is critical.
Thus we can adopt the metrics used in dialogue response generation to
evaluate the output of CRS. Two example metrics are BLEU (Papineni
et al., 2002a) and Rouge (Lin, 2004). BLEU measures the precision of
generated words or n-grams compared to the ground-truth words, rep-
resenting how much the words in the machine-generated utterance
appeared in the ground-truth reference utterance. Rouge measures the
recall of it, i.e., how many of the words or n-grams in the ground-truth
reference utterance appear in the machine-generated utterance.
However, it is widely debated whether these metrics are suitable for
evaluating language generation (Liu et al., 2016a; Novikova et al.,
2017). Because those metrics are only sensitive to lexical variation, they
cannot appropriately assess semantic or syntactic variations of a given
reference. Meanwhile, the goal of the proposed system is not to predict
the highest probability response, but rather the long-term success of the
dialogue. Thus, other metrics reflecting user satisfaction are more suit-
able in evaluation, such as measuring fluency (CelikyilmazAntoine et al.,
2018; Narayan et al., 2018; Du et al., 2017), consistency (Gandhe and
Traum, 2008; Lapata and Barzilay, 2005), readability (Lapata, 2003),
informativeness (Huang et al., 2017), diversity (Li et al., 2016b; Ippolito
et al., 2019; Gao et al., 2019b), and empathy (Ghandeharioun et al.,
2019; Sharma et al., 2021). For more metrics and evaluation methods on
text generation, we refer the readers to the overviews (Celikyilmaz et al.,
2020; Deriu et al., 2021).
However, the CRSs based on end-to-end dialogue frameworks or
deep language models may have limitations regarding the usability in
practice. Recently, Jannach and Manzoor (2020) conducted an evalua-
tion on the two state-of-the-art end-to-end frameworks (Li et al., 2018;
Chen et al., 2019bb), and showed that both models face three critical
issues: (1) For each system, about one-third of the system utterances are
not meaningful in the given context and would probably lead to a
breakdown of the conversation in a human evaluation. (2) Less than
two-thirds of the recommendations were considered to be meaningful in
a human evaluation. (3) Neither of the two systems “generated” utter-
ances, as almost all system responses were already present in the
training data. Jannach and Manzoor (2020)’s analysis shows that human
assessment and expert analysis are necessary for evaluating CRS models
as there is no perfect metric to evaluate all aspects of a CRS. The CRS
models and their evaluation still have a long way to go.
6.2.2. Evaluation of recommendation
The performance of recommendation models is evaluated by
comparing the predicted results with the records in the test set. There
are two kinds of metrics in measuring the performance of recommender
systems:
● Rating-based Metrics. These metrics assume the user feedback is an
explicit rating score, e.g., an integer in the range of one to five.
Therefore, we can measure the divergence between the predicted
scores of models and the ground-truth scores given by users in the
test set. Conventional rating-based metrics include Mean Squared
Error (MSE) and Root Mean Squared Error (RMSE), where RMSE is
the square root of the MSE.
● Ranking-based Metrics. These metrics are more frequently used
than rating-based metrics. Ranking-based metrics require that the
relative order of predicted items should be consistent with the order
of items in the test set. Thereby, there is no need for explicit rating
scores from users, and the implicit interactions (e.g., clicks, plays)
can be used to evaluate models. For example, a good evaluation
result means that the model should only recommend the items in the
test set, or it means that the items with higher scores in the test set
should be recommended at higher ranks than the items with lower
scores. Frequently used ranking-based metrics include hits, preci-
sion, recall, F1-score, Mean Reciprocal Rank (MRR), Mean Average
Precision (MAP), and Normalized Discounted Cumulative Gain
(NDCG) (J¨arvelin and Kek¨al¨ainen, 2002).
Recently, it has become common for researchers to speed up evalu-
ation by sampling a small set of irrelevant items and calculate the
ranking-based metrics only on the small set (He et al., 2017; Ebesu et al.,
2018; Hu et al., 2018; Yang et al., 2018). However, Krichene and Rendle
(2020) point out and prove that some metrics, such as average precision,
recall, and NDCG, are inconsistent with the exact metrics when they are
calculated on the sampled set. This means that if a recommender A
outperforms a recommender B on the sampled metric, it does not imply
that A has a better metric than B when the metric is computed exactly.
Therefore, the authors suggest that sampling during evaluation should
be avoided; if it is necessary to sample, using the corrected metrics
proposed by the authors is a better choice.
The biggest problem in these evaluation methods is that real-world
user interactions are very sparse, and a large fraction of items never
have a chance of being consumed by a user. However, this does not mean
that the user does not like any of them. Perhaps the user has never seen
them, or the user just does not have resources to consume them (Liu
et al., 2020aa; Chen et al., 2020a). Hence, taking the consumed items in
the test set as the users’ ground-truth preferences can introduce evalu-
ation biases (Yang et al., 2018; Chen et al., 2020a). Unlike static
recommender systems, CRSs have the ability to ask real-time questions,
so the system can make sure whether a user is satisfied with an item by
collecting users’ online feedback. This online user test can avoid biases
and provide conversation-level assessments for the CRS model.
6.3. Conversation-level evaluation
Different from the turn-level evaluation which compares the pre-
diction results with the ground-truth labels in a supervised way, the
conversation-level evaluation is not a supervised prediction task. The
interaction process is not i.i.d. (independent and identically distributed)
since each observation is part of a sequential process and each action the
system makes can influence future observations. Plus, the conversation
heavily relies on the user feedback. Therefore, the evaluation of the
conversation requires either an online user test or leveraging historical
interaction data which can be conducted by the off-policy evaluation or
using user simulation.
7 https://github.com/RUCAIBox/CRSLab.
C. Gao et al.
AI Open 2 (2021) 100–126
1176.3.1. Online user test
The online user test, or A/B test, can directly evaluate the conver-
sation policy by leveraging true user feedback. To conduct the assess-
ment, the appropriate metrics should be designed. For example, the
average turn (AT) is a global metric to optimize in a CRS, as the model
should capture user intention and make successful recommendations
thus finish the conversation with as few turns as possible (Lei et al.,
2020a, 2020b; Li et al., 2021b). A similar metric is the recommendation
success rate (SR@t), which measures how many conversations have
ended with the successful recommendation by the t-th turn. Besides, the
ratio of failed attempts, e.g., how many of the questions asked by the
system are rejected or ignored by users, can be a feasible way to measure
whether a system makes decisions to the users’ satisfaction.
Besides these global statistics, the cumulative performance of each
turn of the conversation can also reflect the overall quality of the con-
versation. The expectation of the cumulative reward of a conversation
policy can be written as:
J(π) = Eτ∼pπ (τ)
[ ∑T
t=0
γt r(st , at )
]
, (10)
where the conversation trajectory τ is a sequence of states and actions of
length T, pπ(τ) is the trajectory distribution under policy π. γ ∈ (0, 1] is a
scalar discount factor. r(st , at ) is the immediate reward obtained by
performing action at at state st, e.g., it can be a feedback signal that
reflects user satisfaction such as user clicks or dwell time (Chen et al.,
2019aa; Ie et al., 2019).
Though effective, the online user evaluation has critical problems:
(1) The online interaction between humans and CRSs is slow and usually
takes weeks to collect sufficient data to make the assessment statistically
significant (Li et al., 2015; Gilotte et al., 2018; Zhao et al., 2019). (2)
Collecting users’ feedback is expensive in terms of engineering and lo-
gistic overhead (Jagerman et al., 2018, 2019; Xu et al., 2015) and may
hurt user experience as the recommendation may not satisfy them
(Schnabel et al., 2018; Li et al., 2015; Gilotte et al., 2018; Chen et al.,
2019ab). Therefore, a natural solution is to leverage the historical
interaction, where the off-policy evaluation and user simulation tech-
niques can be used.
6.3.2. Off-policy evaluation
Off-policy evaluation, also called counterfactual reasoning or coun-
terfactual evaluation, is designed to answer a counterfactual question:
what would have happened if instead of πβ we would have used πθ?
Specifically, when we want to evaluate the current target policy πθ but
we only have data under a behavior policy (or logging policy) πβ, we can
still evaluate the target policy πθ by introducing the importance sam-
pling or inverse propensity score (Gilotte et al., 2018; Jagerman et al.,
2019; Chen et al., 2019aa; McInerney et al., 2020; Levine et al., 2020) as:
J(πθ) = Eτ∼πβ (τ)
[
πθ (τ)
πβ(τ)
∑T
t=0
γt r(s, a)
]
. (11)
It is similar to Equation (10) except we use data logged under another
policy to evaluate the target policy. Where a weight w(τ) = πθ (τ)
πβ (τ) is used to
address the distribution mismatch between the two policy πβ and πθ.
Unfortunately, such an estimator suffers from high variance when πθ
deviates from πβ a lot. The variance reduction techniques are introduced
as the remedy. The common techniques include weight clipping (Chen
et al., 2019aa; Zou et al., 2020a) which limits w(τ) by an upper bound,
and trusted region policy optimization (TRPO) (Schulman et al., 2015;
Chen et al., 2019aa).
Another intuitive method is to directly simulate user behaviors just
like the online user test, where user feedback is provided by the user
simulators instead of true users. It is efficient and can avoid the high
variance problem. However, the challenge is that the preference of
simulated users may deviate from the true users, i.e., the user simulation
can avoid high variance, but it introduces bias. Therefore, creating
reliable user simulators is a crucial challenge.
6.3.3. User simulation
There are generally four types of strategies in simulating users: (1)
using the direct interaction history of users, (2) estimating user prefer-
ences on all items, (3) extracting from user reviews, and (4) imitating
human conversational corpora.
● Using Direct Interaction History of Users. The basic idea is similar to the
evaluation of traditional recommender systems, where a subset of
human interaction data is set aside as the test set. If the items rec-
ommended by a CRS are in the users’ test set, then this recommen-
dation is deemed to be a successful one. As user-machine interactions
are relatively rare, there is a need to generate/simulate interaction
data for training and evaluation. Sun and Zhang (2018) make a
strong assumption that users visit restaurants after chatting with a
virtual agent. Based on this assumption, they create a crowdsourcing
task to use a schema-based method to collect dialogue utterances
from the Yelp dataset. In total, they collect 385 dialogues, and
simulate 875, 721 dialogues based on the collected dialogues by a
process called delexicalization. For instance, “I’m looking for
Mexican food in Glendale” is converted to the template: “I’m looking
for <Category> in ”, then they use these templates to generate di-
alogues by using the rating data and the rich information on the Yelp
dataset. Lei et al., 2020a, Lei et al., 2020b use click data in the
LastFM and Yelp datasets to simulate conversational user in-
teractions. Given an observed user-item interaction, they treat the
item as the ground truth item to seek for and its attributes as the
oracle set of attributes preferred by the user in this session. First, the
authors randomly choose an attribute from the oracle set as the
user’s initialization to the session. The session goes into a loop of a
“model acts – simulator responses” process, in which the simulated
user will respond with “Yes” if the query entity is contained in the
oracle set and “No” otherwise. Most CRS studies adopt this simula-
tion method because of its simplicity (Zou et al., 2020; Christako-
poulou et al., 2018; Chen et al., 2019ba). However, the sparsity
problem in recommender systems still remains: only a few values in
the user-item matrix are known, while most elements are missing,
which forbids the simulation on these items.
● Estimating User Preferences on All Items. Using direct user interactions
to simulate conversations has the same drawbacks as we mentioned
above, i.e., a large number of items that have not been seen by a user
are treated as disliked items. To overcome this bias in the evaluation
process, some research proposes to obtain the user preferences on all
items in advance. Given an item and its auxiliary information, the
key to simulating user interaction is to estimate or synthesize pref-
erences for this item. For example, Christakopoulou et al. (2016) ask
28 participants to rate 10 selected items, and then they can estimate
the latent vectors of the 10 users’ preferences based on their matrix
factorization model. By adding noise to the latent vector, they
simulate 50 new user profiles and calculate these new users’ pref-
erences on any items based on the same matrix factorization model.
Zhang et al. (2020c) propose to use ridge regression to compute user
preferences based on these known rewards on historical interaction
and users’ features; they synthesize the user’s reaction (rewards) on
each item according to the computed preferences. This kind of
method can theoretically simulate a complete user preference
without the exposure bias. However, because the user preferences
are computed or synthesized, it could deviate from real user pref-
erences. Huang et al. (2020) analyze the phenomenon of popularity
bias (Steck, 2011; Pradel et al., 2012) and selection bias (Marlin
et al., 2007; Hern´andez-Lobato et al., 2014; Steck, 2013) in simula-
tors built on logged interaction data and try to alleviate model per-
formance degradation due to these biases; it remains to be seen to
C. Gao et al.
AI Open 2 (2021) 100–126
118which degree generated interactions of the type described above are
subject to similar bias phenomena.
● Extracting Information from User Reviews. Besides user behavior his-
tory, many e-commerce platforms have textual review data. Unlike
the consumption history, an item’s review data usually explicitly
mentions attributes, which can reflect the users’ personalized opin-
ions on this item. Zhang et al. (2018) transform each textual review
of part of the Amazon dataset into a question-answer sequence to
simulate the conversation. For example, when a user mentioned that
a blue Huawei phone with the Android system in a review of a mobile
phone X, then the conversation sequence constructed from this re-
view is (Category: mobile phone → System: Android → Color: blue →
Recommendation: X). Zhou et al. (2020c) also construct simulated
interactions by leveraging user reviews. Based on a given user profile
and its historical watching records, the authors construct a topic
thread that consists of topics (e.g., “family” or “job seeking”)
extracted from reviews of these watched movies. The topic thread is
organized by a rule and eventually leads to the target movie. And the
synthetic conversation is fleshed out by retrieving the most related
reviews under corresponding topics.
A noteworthy problem is that the aspects mentioned in reviews
may contain some drawbacks of the products, which does not aid
understanding why a user has chosen a product. For example, when a
user complains about the capacity of a phone of 64 Gigabytes is not
enough, and it should not be simply convert to (Storage capacity: 64
Gigabytes) for the CRS to learn. Thus, employing sentiment analysis
on the review data is necessary, and only the attribute with positive
sentiment should be considered as the reason in choosing the item
(Zhang et al., 2014a, 2014b).
● Imitating Humans’ Conversational Corpora. In order to generate
conversational data without biases, a feasible solution is to use real-
world two-party human conversations as the training data (Vaku-
lenko et al., 2020a). By using this type of data, a CRS system can
directly mimic human behavior by learning from a large number of
real human-human conversations. For example, Li et al. (2018) ask
workers from AMT to converse in terms of the topics on the movie
recommendation. Using these conversational corpora as training
data, the model can learn how to respond properly based on the
sentiment analysis result. Liu et al. (Liu et al., 2020ab) conduct a
similar data collection process. Except for collecting the dialogues
about the recommendation, they also collect and construct a
knowledge graph and define an explicit profile for each worker who
seeks recommendations. Therefore, the conversational topics can
contain many non-recommendation scenarios, e.g., question
answering or social chitchat, which are more common in real life. To
evaluate this kind of model, besides considering whether the user
likes the recommended item, we have to consider if the system re-
sponds properly and fluently. The BLEU score (Papineni et al.,
2002b) is used to measure the fluency of these models mimicking
human conversations (Budzianowski et al., 2018; Zhang and OuZ-
hou, 2020).
There are also drawbacks for this kind of method. First, when col-
lecting the human conversational corpus, two workers need to enter the
task at the same time, which is a rigorous setting and thus limits the scale
of the dataset. Second, designers usually have many requirements that
restrict the direction of the conversation. Therefore, the generated
conversation is constrained and cannot fully cover the real-world sce-
narios. By imitating a collected corpus, learning a conversation strategy
is very sensitive to the quality of the collected data. Vakulenko et al.
(2020a) analyze the characteristics of different human-human corpora,
e.g., in terms of initative taking, and show that there are important
differences between human-human and human-machine conversations.
Recently, Zhang and Balog (2020) have investigated using user
simulations in evaluating CRSs. They organize the action sequence of the
simulated user as a stack-like structure, called the user agenda. A dy-
namic update of the agenda is regarded as a sequence of pull or push
operations, where dialogue actions are removed from or added to the
top. Fig. 8 shows an example of a dialogue between the simulated user
and a CRS. At each turn, the simulated user updates its agenda by either
a push or a pull operation based on the dialogue state and the CRS’s
action. The authors define a set of actions and the transition rule on
these actions to let the simulated user imitate real users’ intentions. For
example, the Disclose action indicates that the user expresses its need
either actively, or in response to the agent’s question, e.g., “I would like
to arrange a holiday in Italy”. And after this action, the simulator can
either transit to the Inquire action or the Reveal section based on how the
CRS model acts.
Besides modeling the user preference in simulation, another branch
of studies considers modeling user behaviors in the slate, top-K, or list-
wise recommendation. A natural solution is to consider the combina-
torial action which contains a list of items instead of a single item
(Sunehag et al., 2015). However, this method is unable to scale to
problems of the size encountered in large, real-world recommender
systems. The feasible way is to assume a user only consumes a single
item from each slate and the obtained reward only depends on the item
Fig. 8. Example dialogue with agenda sequence and state transition. The agenda is shown in square brackets. The third agenda is a result of a push operation, all
other agendas updates are pull operations. Credits: Zhang and Balog (Zhang and Balog, 2020).
C. Gao et al.
AI Open 2 (2021) 100–126
119(Ie et al., 2019). Under the assumption, user choice behavior can be
modeled as the multinomial logit model (Louviere et al., 2000) or the
cascade model (Ie et al., 2019; Yu et al., 2019b; Kveton et al., 2015; Zong
et al., 2016). Despite the recent interest in developing reliable user
simulators, we believe that the research in this field is in its infancy and
needs a lot of advancements.
6.4. Section summary
In this section, we review the metrics, methods, and challenges in the
turn-level evaluation and conversation-level evaluation of CRSs. The
turn-level evaluation measures the performance of the supervised pre-
diction tasks, i.e., recommendation and language generation of the CRS
in a single round; the conversation-level evaluation measure how the
conversation strategy performs in the multi-turn conversation. Since an
online user test is expensive to conduct, researchers either leverage the
off-policy evaluation which assesses the target policy using the logged
data under the behavior policy, or directly introduce user simulators to
replace the true users in evaluation.
The evaluation of CRSs still needs a lot of effort. It ranges from
constructing large-scale dense conversational recommendation data, to
proposing uniform evaluation methods to compare different CRS
methods that integrate both recommendation and conversation aspects.
7. Future directions and opportunities
Having described key advances and challenges in the area CRSs, we
now envision some promising future directions.
7.1. Jointly optimizing three tasks
The recommendation task, language understanding and generation
task, and conversation strategies in CRSs are usually studied separately
in the three components in Fig. 3, respectively. The three components
share certain objectives and data with each other (Chen et al., 2019bb;
Ma et al., 2020; Lei et al., 2020a; Zhou et al., 2020a). For example, the
user interface feeds extracted aspect-value pairs to the recommendation
engine, and then integrates the entities produced by the recommenda-
tion engine into the generated response. However, they have the
exclusive data that does not benefit each other. For instance, the user
interface may use the rich semantic information in reviews but not
shares with a recommendation engine (Li et al., 2018). Besides, the two
components may work in the end-to-end framework that lacks an
explicit conversation strategy to coordinate them in the multi-turn
conversation (Li et al., 2018; Chen et al., 2019bb), thus the perfor-
mance is not satisfied in human evaluation (Jannach and Manzoor,
2020).
Thereby, the three tasks should be jointly learned and guided by an
explicit conversation strategy for their mutual benefit, for instance, what
if the conversation strategy module were able to plan future dialogue
acts based on item-item relationships (such as complementarity and
substitutability (McAuley et al., 2015a; Wan et al., 2018; Liu et al.,
2020ba))?
7.2. Bias and debiasing
It is inevitable that a recommender system could encounter various
types of bias (Chen et al., 2020a). Some types of biases, e.g., popularity
bias (Abdollahpouri and Mansoury, 2020; Steck, 2011) and conformity
bias (Zhang et al., 2014b; Liu et al., 2016b), can be removed with
introducing the interaction between the user and system. For example, a
static recommender may not be sure whether a user will follow the
crowd and like popular items. Therefore, the popularity bias is intro-
duced in the recommender system since popular items can have higher
probability of being recommended. This, however, could be avoided in
CRSs because a CRS can query about the user’s attitude towards popular
items in real time and avoid recommending them if the user gives
negative feedback.
Nevertheless, some types of bias persist. For example, even though a
recommender system may provide access to a large number of items, a
user can only interact with a small set of them. If these items are chosen
by a model or a certain exposure mechanism, users have no choices but
to keep consuming these items. That is the exposure bias (Liu et al.,
2020aa). Moreover, users often select or consume their liked items and
ignore these disliked ones even these items have been exposed to users,
which introduces the selection bias (Marlin et al., 2007; Hern´ande-
z-Lobato et al., 2014; Steck, 2013), also known as the positivity bias
(Huang et al., 2020; Pradel et al., 2012), i.e., rating data is often missing
not at random and the missing ones are more likely to be disliked by the
user (Hern´andez-Lobato et al., 2014). These types of bias can be
amplified in the feedback loop and may hurt the recommendation model
(Sinha et al., 2016; Sun et al., 2019). For instance, a CRS model polluted
by biased data might repeatedly generate the same items even through
users suggested they would like other ones.
There are relatively few efforts to study the bias problem in CRSs.
The exploration-exploitation methods introduced in Section 5 can alle-
viate some types of bias in CRSs. And Huang et al. (2020) make an
attempt to remove the positivity bias in the user simulation stage for the
interactive recommendation. Moreover, Chen et al. (2020a) present a
comprehensive survey of different types of bias and describe a number of
debiasing methods for recommender systems (RSs); it provides some
perspectives for debiasing CRSs.
7.3. Sophisticated multi-turn conversation strategies
The multi-turn strategy considered in current studies of CRSs are
relatively naive. For example, there is work using a hand-crafted func-
tion to determine the timing to ask attributes or make recommendation,
e.g., making k conversations in every m rounds (Zhang et al., 2020c).
These studies based on end-to-end dialogue systems or deep neural
language models are worse: they do not even have an explicit strategy to
control the multi-turn conversation (Li et al., 2018; Chen et al., 2019bb).
Besides, some strategies can be problematic in regard to handling users’
negative feedback. For instance, Lei et al. (2020a) consider updating the
model parameters when the user dislikes a recommended item. How-
ever, simply taking rejected items as negative samples would influence
the model’s judgement on the queried attributes. For example, a user’s
rejection of a recreation video might be due to the fact that they watched
it before, and it does not mean that they dislike recreation videos. To
overcome this problem, the model should consider more sophisticated
strategies such as recognizing reliable negative samples (Chen et al.,
2019, Ding et al., 2019; Wang et al., 2020c; LianQi and Chen, 2020;
Chen et al., 2019) as well as disentangling user preferences on items and
attributes (MaChang et al., 2019; Wang et al., 2020b).
We have witnessed some studies using RL as the multi-turn conver-
sation strategy by determining model actions such as whether to ask or
recommend (Sun and Zhang, 2018; Lei et al., 2020a, 2020b). However,
there is a lot of room for improvement in designing the state, action, and
reward in RL. For instance, more sophisticated actions can be taken into
consideration such as answering open-domain questions raised by users
(Zhu et al., 2021) or chatting non-task-oriented topics for entertainment
purposes (Wu and Yan, 2018; Liu et al., 2020ab). Besides, more
advanced and intuitive RL technologies can be considered to avoid the
difficulties, e.g., hard to train and converge, in basic RL models (Wang
et al., 2020a). For example, Inverse RL (IRL) (Ng and Russell, 2000) can
be considered to learn a proper reward function from the observed ex-
amples in certain CRS scenarios, where there are too many user behavior
patterns so the reward is hard to define. Meta-RL (DuanJohn Schulman
et al., 2016; Wang et al., 2016a) can be adopted in CRSs, where the
interaction is sparse and various, to speed up the training process and to
improve the learning efficiency for novel subsequent tasks.
C. Gao et al.
AI Open 2 (2021) 100–126
1207.4. Knowledge enrichment
A natural idea to improve CRSs is to introduce additional knowledge.
In early stages of the development of CRSs, only the recommended items
themselves were considered (Christakopoulou et al., 2016). Later, the
attribute information of items was introduced to assist in modeling user
preferences (Christakopoulou et al., 2018). Even more recent studies
consider the rich semantic information in knowledge graphs (Zhou et al.,
2020a; Lei et al., 2020b; Xu et al., 2020; Moon et al., 2019). For example,
to better understand concepts in a sentence such as “I am looking for
scary movies similar to Paranormal Activity (2007)”, Zhou et al. (2020a)
propose to incorporate two external knowledge graphs (KGs): one
word-oriented KG providing relations (e.g., synonyms, antonyms, or
co-occurrence) between words so as to comprehend the concept “scary”
in the sentence; one item-oriented KG carrying structured facts
regarding the attributes of items.
Besides knowledge graphs, multimodal data can also be integrated
into the original text-based CRSs since it can enrich the interaction from
new dimensions. There are some studies that exploit the visual modality,
i.e., images, in dialogue systems (Yu et al., 2019b; Liao et al., 2018; Cui
et al., 2019; Zhang et al., 2019). For example, Yu et al. (2019b) propose
a visual dialogue augmented CRS model. The model will recommend a
list of items in photos, and the user will give text-based comments as
feedback. The image not only helps the model learn a more informative
representation of entities, but also enable the system to better convey
information to the user. Except for the visual modality, other modalities
can benefit CRSs and could be integrated. For example, spoken natural
language can convey users’ emotions as well as sentiments towards
certain entities (Pittermann et al., 2010).
7.5. Better Evaluation and user simulation
The evaluation of CRSs still has a long way to go. As we introduced in
Section 6.3, evaluating the CRS requires real-time feedback, which is
expensive in real-world situations (Jagerman et al., 2019). Thus, most
CRSs adopt user simulation techniques to create an environment (Zhang
and Balog, 2020). However, simulated users cannot fully replace human
beings. How to simulate users with maximum fidelity still needs further
research. Feasible directions include designing systematic simulation
agenda (Zhang and Balog, 2020; Schatzmann et al., 2007), building
dense user interactions for reliable simulation (Zou et al., 2020a; Chen
et al., 2019ab; Bai et al., 2019), and modeling user choice behaviors over
the slate recommendation (Ie et al., 2019; McInerney et al., 2020; Afsar
et al., 2021).
In addition, CRSs work on different datasets and they have various
assumptions and settings. Therefore, developing comprehensive evalu-
ation metrics and procedures to assess the performance of CRSs remains
an open problem. Recently, Zhou et al. (2021) have implemented an
open-source CRS toolkit, enabling evaluation between different CRS
models. However, their implemented models are mainly based on
end-to-end dialogue systems (Li et al., 2018; Chen et al., 2019bb; Zhou
et al., 2020a) or deep language models (Zhou et al., 2020c), the models
focusing on the explicit conversation strategy (Lei et al., 2020a, 2020b)
are absent.
8. Conclusion
Recommender systems are playing increasingly important role in
information seeking and retrieval. Despite having been studied for de-
cades, traditional recommender systems estimate user preferences only
in a static manner like through historical user behaviours and profiles. It
offers no opportunities to communicate with users about their prefer-
ences. This inevitably suffers from a fundamental information asym-
metry problem: a system will never know precisely what a user likes
(especially when his/her preference drifts frequently) and why the user
likes an item. The envision of conversational recommender systems
(CRSs) brings a promising solution to such problems. With the interac-
tive ability as well as the natural language-based user interface, CRSs
can dynamically get explicit user feedback using natural languages,
while increasing user engagement and improving user experience. This
bold vision provides great potential for the future of recommender
system, hence actively contributes to the development of the next gen-
eration of information seeking techniques.
Although the build of CRS is an emerging field, we have spotted great
efforts from different perspectives. In this survey, we acknowledge those
efforts, with the aim to summarize the existing studies and to provide
insightful discussions. We tentatively gave a definition of the CRS and
introduced a general framework of CRSs that consists of three compo-
nents: a user interface, a conversation strategy module and a recom-
mender engine. Based on this decomposition, we distilled five existing
research directions, namely: (1) question-based user preference elicita-
tion; (2) multi-turn conversational recommendation strategies; (3) dia-
logue understanding and generation; (4) exploitation-exploration
tradeoffs for cold users; (5) evaluation and user simulation. For each
direction, we reviewed the existing efforts and their limitation in one
section, leading to the primary structure of this survey. Despite the
progresses on the above five directions, more interesting problems
remain to be explored in the field of CRSs, such as, (1) joint optimization
of three components; (2) bias and debiasing methods in CRSs; (3) multi-
turn conversational recommendation strategies; (4) multi-modal
knowledge enrichment; (5) evaluation and user simulation. Our dis-
cussions above provide a comprehensive retrospect of current progress
of CRSs which can serve as the basis for the further development of this
field. By providing this survey, we call arm to this emerging and inter-
esting field. We hope this survey can inspire the researchers and prac-
titioners from both industry and academia to push the frontiers of CRSs,
making the thoughts and techniques of CRSs more prevalent for the next
generation of information seeking techniques.
Declaration of competing interest
The authors declare that they have no known competing financial
interests or personal relationships that could have appeared to influence
the work reported in this paper.
Acknowledgments
This work is supported by the National Natural Science Foundation
of China (61972372, U19A2079) and the National Key Research and
Development Program of China (2020YFB1406703,
2020AAA0106000).
All content represents the opinion of the authors, which is not
necessarily shared or endorsed by their respective employers and/or
sponsors.
References
Abdollahpouri, Himan, Mansoury, Masoud, 2020. Multi-sided exposure bias in
recommendation. In: International Workshop on Industrial Recommendation
Systems (IRS2020) in Conjunction with ACM KDD ’2020 (2020).
Afsar, M Mehdi, Crump, Trafford, Far, Behrouz, 2021. Reinforcement Learning Based
Recommender Systems: A Survey arXiv preprint arXiv:2101.06286 (2021).
Auer, Peter, 2002. Using confidence bounds for exploitation-exploration trade-offs.
J. Mach. Learn. Res. 3 (Nov), 397–422, 2002.
Auer, Peter, Cesa-Bianchi, Nicolo, Fischer, Paul, 2002. Finite-time analysis of the
multiarmed bandit problem. Mach. Learn. 47, 2–3 (2002), 235–256.
Bai, Xueying, Guan, Jian, Wang, Hongning, 2019. Model-based reinforcement learning
with adversarial training for online recommendation. In: Advances in Neural
Information Processing Systems (NeurIPS ’19), vol. 32.
Balaraman, Vevake, Magnini, Bernardo, 2020. Proactive systems and influenceable
users: simulating proactivity in task-oriented dialogues. In: The 24th Workshop on
the Semantics and Pragmatics of Dialogue (WatchDial ’20).
Bar-Yossef, Ziv, Kraus, Naama, 2011. Context-sensitive query auto-completion. In:
Proceedings of the 20th International Conference on World Wide Web (WWW ’11),
pp. 107–116.
C. Gao et al.
AI Open 2 (2021) 100–126
121Bell, Robert, Koren, Yehuda, Volinsky, Chris, 2007. Modeling relationships at multiple
scales to improve accuracy of large recommender systems. In: Proceedings of the
13th ACM SIGKDD International Conference on Knowledge Discovery and Data
Mining (KDD ’07), pp. 95–104.
Bertin-Mahieux, Thierry, Ellis, Daniel P.W., Whitman, Brian, Paul, Lamere, 2011. The
million song dataset. In: Proceedings of the 12th International Conference on Music
Information Retrieval (ISMIR ’2011).
Bizer, Christian, Lehmann, Jens, Kobilarov, Georgi, Auer, S¨oren, Becker, Christian,
Cyganiak, Richard, Hellmann, Sebastian, 2009. DBpedia - a crystallization point for
the web of data. Web Semant. Sci. Serv. Agents World Wide Web 7 (Sept. 2009),
154–165, 3.
Boutilier, Craig, 2002. A POMDP formulation of preference elicitation problems. In:
Eighteenth National Conference on Artificial Intelligence. AAAI ’02, pp. 239–246.
Budzianowski, Paweł, Wen, Tsung-Hsien, Tseng, Bo-Hsiang, Casanueva, I˜nigo,
Ultes, Stefan, Ramadan, Osman, Gaˇsi´c, Milica, 2018. MultiWOZ - a large-scale multi-
domain wizard-of-oz dataset for task-oriented dialogue modelling. In: Proceedings of
the 2018 Conference on Empirical Methods in Natural Language Processing. EMNLP
’18, pp. 5016–5026.
Buhrmester, Vanessa, Münch, David, Arens, Michael, 2019. Analysis of Explainers of
Black Box Deep Neural Networks for Computer Vision: A Survey arXiv preprint
arXiv:1911.12116 (2019).
Burke, Robin D., Hammond, Kristian J., Young, Benjamin C., 1997. The FindMe
approach to assisted browsing. IEEE Expert: Intell. Syst. Their Appl. 12 (July 1997),
32–40, 4.
Cai, Fei, de Rijke, Maarten, 2016. A survey of query auto completion in information
retrieval. Found. Trends. Inf. Retr. 10 (September 2016), 273–363, 4.
Carenini, Giuseppe, Smith, Jocelyin, Poole, David, 2003. Towards more conversational
and collaborative recommender systems. In: Proceedings of the 8th International
Conference on Intelligent User Interfaces (IUI ’03), pp. 12–18.
Celikyilmaz, Asli, Antoine, Bosselut, He, Xiaodong, Choi, Yejin, 2018. Deep
communicating agents for abstractive summarization. In: Proceedings of the 2018
Conference of the North American Chapter of the Association for Computational
Linguistics: Human Language Technologies, vol. 1, pp. 1662–1675 (Long Papers)
(NAACL ’18).
Celikyilmaz, Asli, Clark, Elizabeth, Gao, Jianfeng, 2020. Evaluation of Text Generation: A
Survey arXiv preprint arXiv:2006.14799 (2020).
Cen, Yukuo, Zhang, Jianwei, Zou, Xu, Chang, Zhou, Yang, Hongxia, Tang, Jie, 2020.
Controllable multi-interest framework for recommendation. In: Proceedings of the
26th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining
(KDD ’20), pp. 2942–2951.
Chajewska, Urszula, Getoor, Lise, Norman, Joseph, Yuval Shahar, 1998. Utility
elicitation as a classification problem. In: Proceedings of the Fourteenth Conference
on Uncertainty in Artificial Intelligence (UAI ’98), pp. 79–88.
Chapelle, Olivier, Li, Lihong, 2011. An empirical evaluation of Thompson sampling. In:
Proceedings of the 24th International Conference on Neural Information Processing
Systems (NIPS ’11), pp. 2249–2257.
Chen, Haokun, Dai, Xinyi, Han, Cai, Zhang, Weinan, Wang, Xuejian, Tang, Ruiming,
Zhang, Yuzhou, Yu, Yong, 2019ba. Large-scale interactive recommendation with
tree-structured policy gradient. In: Proceedings of the AAAI Conference on Artificial
Intelligence (AAAI ’19), vol. 33, pp. 3312–3320.
Chen, Hongshen, Liu, Xiaorui, Yin, Dawei, Tang, Jiliang, 2017. A survey on dialogue
systems: recent advances and new frontiers. SIGKDD Explor. Newsl. 19 (Nov. 2017),
25–35, 2.
Chen, Jiawei, Dong, Hande, Wang, Xiang, Feng, Fuli, Wang, Meng, He, Xiangnan, 2020a.
Bias and Debias in Recommender System: A Survey and Future Directions arXiv
preprint arXiv:2010.03240 (2020).
Chen, Jiawei, Wang, Can, Zhou, Sheng, Shi, Qihao, Yan, Feng, Chen, Chun, 2019.
SamWalker: social recommendation with informative sampling strategy. In: The
World Wide Web Conference (WWW ’19), pp. 228–239.
Chen, Li, Pu, Pearl, 2012. Critiquing-based recommenders: survey and emerging trends.
User Model. User-Adapted Interact. 22, 1–2 (2012), 125–150.
Chen, Minmin, Beutel, Alex, Paul, Covington, Jain, Sagar, Belletti, Francois, Chi, Ed H.,
2019aa. Top-K off-policy correction for a REINFORCE recommender system. In:
Proceedings of the Twelfth ACM International Conference on Web Search and Data
Mining. WSDM ’19, pp. 456–464.
Chen, Qibin, Lin, Junyang, Zhang, Yichang, Ding, Ming, Cen, Yukuo, Yang, Hongxia,
Tang, Jie, 2019bb. Towards knowledge-based recommender dialog system. In:
Proceedings of the 2019 Conference on Empirical Methods in Natural Language
Processing and the 9th International Joint Conference on Natural Language
Processing. EMNLP-IJCNLP ’2019, pp. 1803–1813.
Chen, Xinshi, Li, Shuang, Li, Hui, Jiang, Shaohua, Qi, Yuan, Song, Le, 2019ab. Generative
adversarial user model for reinforcement learning based recommendation system. In:
International Conference on Machine Learning (ICML ’19). PMLR, pp. 1052–1061.
Chen, Zhongxia, Wang, Xiting, Xie, Xing, Parsana, Mehul, Soni, Akshay, Xiang, Ao,
Chen, Enhong, 2020b. Towards explainable conversational recommendation. In:
Proceedings of the 29th International Joint Conference on Artificial Intelligence,
IJCAI ’20, pp. 2994–3000.
Cheng, Heng-Tze, Koc, Levent, Harmsen, Jeremiah, Shaked, Tal, Chandra, Tushar,
Aradhye, Hrishi, Anderson, Glen, Corrado, Greg, Chai, Wei, Ispir, Mustafa,
Anil, Rohan, Haque, Zakaria, Hong, Lichan, Jain, Vihan, Liu, Xiaobing, Shah, Hemal,
2016. Wide & deep learning for recommender systems. In: Proceedings of the 1st
Workshop on Deep Learning for Recommender Systems (DLRS ’2016), pp. 7–10.
Cho, Kyunghyun, Bart van Merri¨enboer, Gulcehre, Caglar, Bahdanau, Dzmitry,
Bougares, Fethi, Schwenk, Holger, Bengio, Yoshua, 2014. Learning phrase
representations using RNN encoder–decoder for statistical machine translation. In:
Proceedings of the 2014 Conference on Empirical Methods in Natural Language
Processing. EMNLP ’14, pp. 1724–1734.
Chou, Ku-Chun, Lin, Hsuan-Tien, Chiang, Chao-Kai, Lu, Chi-Jen, 2015. Pseudo-reward
algorithms for contextual bandits with linear payoff functions. In: Asian Conference
on Machine Learning. ACML ’15, pp. 344–359.
Christakopoulou, Konstantina, Beutel, Alex, Li, Rui, Jain, Sagar, Chi, Ed H., 2018. Q&R: a
two-stage approach toward interactive recommendation. In: Proceedings of the 24th
ACM SIGKDD International Conference on Knowledge Discovery & Data Mining.
KDD ’18, pp. 139–148.
Christakopoulou, Konstantina, Radlinski, Filip, Hofmann, Katja, 2016. Towards
conversational recommender systems. In: Proceedings of the 22nd ACM SIGKDD
International Conference on Knowledge Discovery and Data Mining. KDD ’16,
pp. 815–824.
Covington, Paul, Adams, Jay, Sargin, Emre, 2016. Deep neural networks for YouTube
recommendations. In: Proceedings of the 10th ACM Conference on Recommender
Systems. RecSys ’16, pp. 191–198.
Cui, Chen, Wang, Wenjie, Song, Xuemeng, Huang, Minlie, Xu, Xin-Shun, Nie, Liqiang,
2019. User attention-guided multimodal dialog systems. In: Proceedings of the 42nd
International ACM SIGIR Conference on Research and Development in Information
Retrieval (SIGIR’19), pp. 445–454.
Dehghani, Mostafa, Rothe, Sascha, Alfonseca, Enrique, Fleury, Pascal, 2017. Learning to
attend, copy, and generate for session-based query suggestion. In: Proceedings of the
2017 ACM on Conference on Information and Knowledge Management. CIKM ’17,
pp. 1747–1756.
Deng, Li, Tur, Gokhan, He, Xiaodong, Hakkani-Tur, Dilek, 2012. Use of kernel deep
convex networks and end-to-end learning for spoken language understanding. In:
2012 IEEE Spoken Language Technology Workshop (SLT). IEEE, pp. 210–215.
Deoras, Anoop, Sarikaya, Ruhi, 2013. Deep belief network based semantic taggers for
spoken language understanding. In: ISCA Interspeech, pp. 2713–2717.
Deriu, Jan, Rodrigo, Alvaro, Otegi, Arantxa, Echegoyen, Guillermo, Rosset, Sophie,
Agirre, Eneko, Cieliebak, Mark, 2021. Survey on evaluation methods for dialogue
systems. Artif. Intell. Rev. 54 (1), 755–810, 2021.
Devlin, Jacob, Chang, Ming-Wei, Lee, Kenton, Toutanova, Kristina, 2019. BERT: pre-
training of deep bidirectional transformers for language understanding. In:
Proceedings of the 2019 Conference of the North American Chapter of the
Association for Computational Linguistics: Human Language Technologies, vol. 1,
pp. 4171–4186 (Long and Short Papers) (NAACL ’19).
Ding, Jingtao, Quan, Yuhan, He, Xiangnan, Li, Yong, Jin, Depeng, 2019. Reinforced
negative sampling for recommendation with exposure data. In: Proceedings of the
Twenty-Eighth International Joint Conference on Artificial Intelligence, IJCAI-19,
pp. 2230–2236.
Ding, Qinxu, Liu, Yong, Miao, Chunyan, Cheng, Fei, Tang, Haihong, 2020. A hybrid
bandit framework for diversified recommendation. In: Proceedings of the AAAI
Conference on Artificial Intelligence (2020).
Dodge, Jesse, Gane, Andreea, Zhang, Xiang, Antoine, Bordes, Chopra, Sumit,
Miller, Alexander H., Arthur Szlam, Weston, Jason, 2016. Evaluating prerequisite
qualities for learning end-to-end dialog systems. In: International Conference on
Learning Representations (ICLR ’16).
Du, Xinya, Shao, Junru, Cardie, Claire, 2017. Learning to ask: neural question generation
for reading comprehension. In: Proceedings of the 55th Annual Meeting of the
Association for Computational Linguistics (ACL ’17), pp. 1342–1352.
Duan, Yan, John Schulman, Chen, Xi, Bartlett, Peter L., Sutskever, Ilya, Abbeel, Pieter,
2016. RL2: Fast Reinforcement Learning via Slow Reinforcement Learning arXiv
preprint arXiv:1611.02779 (2016).
Ebesu, Travis, Shen, Bin, Fang, Yi, 2018. Collaborative memory network for
recommendation systems. In: The 41st International ACM SIGIR Conference on
Research & Development in Information Retrieval. SIGIR ’18, pp. 515–524.
Feng, Minwei, Xiang, Bing, Glass, Michael R., Wang, Lidan, Zhou, Bowen, 2015.
Applying deep learning to answer selection: a study and an open task. In: 2015 IEEE
Workshop on Automatic Speech Recognition and Understanding (ASRU). IEEE,
pp. 813–820.
Finn, Chelsea, Abbeel, Pieter, Levine, Sergey, 2017. Model-agnostic meta-learning for
fast adaptation of deep networks. In: Proceedings of the 34th International
Conference on Machine Learning (ICML ’17), vol. 70, pp. 1126–1135.
Fu, Zuohui, Xian, Yikun, Zhu, Yaxin, Xu, Shuyuan, Li, Zelong, de Melo, Gerard,
Zhang, Yongfeng, 2021. HOOPS: human-in-the-loop graph reasoning for
conversational recommendation. In: Proc Eedings of the 44th International ACM
SIGIR Conference on Research and Development in Information Retrieval (SIGIR
’21). ACM.
Fu, Zuohui, Xian, Yikun, Zhu, Yaxin, Zhang, Yongfeng, de Melo, Gerard, 2020. COOKIE:
A Dataset for Conversational Recommendation over Knowledge Graphs in E-
Commerce arXiv preprint arXiv:2008.09237 (2020).
Gandhe, Sudeep, Traum, David, 2008. An evaluation understudy for dialogue coherence
models. In: Proceedings of the 9th SIGDIAL Workshop on Discourse and Dialogue.
SIGDIAL ’08, pp. 172–181.
Gao, Chongming, Yuan, Shuai, Zhang, Zhong, Yin, Hongzhi, Shao, Junming, 2019c.
BLOMA: explain collaborative filtering via boosted local rank-one matrix
approximation. In: International Conference on Database Systems for Advanced
Applications (DASFAA ’19). Springer, pp. 487–490.
Gao, Jianfeng, Galley, Michel, Li, Lihong, 2019a. Neural approaches to conversational
AI: question answering, task-oriented dialogues and social chatbots. Now
Foundations and Trends. https://doi.org/10.1561/1500000074, p. 184. https
://ieeexplore.ieee.org/document/8649787.
Gao, Shen, Chen, Xiuying, Ren, Zhaochun, Zhao, Dongyan, Yan, Rui, 2020. Meaningful
Answer Generation of E-Commerce Question-Answering arXiv preprint arXiv:
2011.07307 (2020).
C. Gao et al.
AI Open 2 (2021) 100–126
122Gao, Xiang, Lee, Sungjin, Zhang, Yizhe, Brockett, Chris, Galley, Michel, Gao, Jianfeng,
Dolan, Bill, 2019b. Jointly optimizing diversity and relevance in neural response
generation. In: Proceedings of the 2019 Conference of the North American Chapter
of the Association for Computational Linguistics (NAACL ’19), pp. 1229–1238.
Ghandeharioun, Asma, Shen, Judy Hanwen, Jaques, Natasha, Ferguson, Craig,
Jones, Noah, Lapedriza, Agata, Picard, Rosalind, 2019. Approximating interactive
human evaluation with self-play for open-domain dialog systems. In: Wallach, H.,
Larochelle, H., Beygelzimer, A., d’ Alch´e-Buc, F., Fox, E., Garnett, R. (Eds.),
Advances in Neural Information Processing Systems (NeurIPS ’19), vol. 32.
Gilotte, Alexandre, Calauz`enes, Cl´ement, Nedelec, Thomas, Abraham, Alexandre,
Simon, Doll´e, 2018. Offline A/B testing for recommender systems. In: Proceedings of
the Eleventh ACM International Conference on Web Search and Data Mining. WSDM
’18, pp. 198–206.
Graus, MarkP., Willemsen, Martijn C., 2015. Improving the user experience during cold
start through choice-based preference elicitation. In: Proceedings of the 9th ACM
Conference on Recommender Systems. RecSys ’15, pp. 273–276.
Guo, Qingyu, Zhuang, Fuzhen, Qin, Chuan, Zhu, Hengshu, Xie, Xing, Xiong, Hui,
He, Qing, 2020. A survey on knowledge graph-based recommender systems. In: IEEE
Transactions on Knowledge and Data Engineering (2020).
Guo, Shengbo, Scott, Sanner, 2010. Real-time multiattribute bayesian preference
elicitation with pairwise comparison queries. In: Proceedings of the 13th
International Conference on Artificial Intelligence and Statistics. AISTATS ’10,
pp. 289–296.
Habib, Javeria, Zhang, Shuo, Balog, Krisztian, 2020. IAI MovieBot: a conversational
movie recommender system. In: Proceedings of the 29th ACM International
Conference on Information and Knowledge Management. CIKM ’20, pp. 3405–3408.
Hayati, Shirley Anugrah, Kang, Dongyeop, Zhu, Qingxiaoyang, Shi, Weiyan, Zhou, Yu,
2020. INSPIRED: toward sociable recommendation dialog systems. In: Conference on
Empirical Methods in Natural Language Processing. EMNLP ’20.
He, Chen, Parra, Denis, Verbert, Katrien, 2016. Interactive recommender systems: a
survey of the state of the art and future research challenges and opportunities. Expert
Syst. Appl. 56, 9–27, 2016.
He, Xiangnan, Deng, Kuan, Wang, Xiang, Li, Yan, Zhang, YongDong, Wang, Meng, 2020.
LightGCN: simplifying and powering graph convolution network for
recommendation. In: Proceedings of the 43rd International ACM SIGIR Conference
on Research and Development in Information Retrieval. SIGIR ’20, pp. 639–648.
He, Xiangnan, Liao, Lizi, Zhang, Hanwang, Nie, Liqiang, Hu, Xia, Chua, Tat-Seng, 2017.
Neural collaborative filtering. In: Proceedings of the 26th International Conference
on World Wide Web (WWW ’17), pp. 173–182.
Hern´andez-Lobato, Jos´e Miguel, Houlsby, Neil, Ghahramani, Zoubin, 2014. Probabilistic
matrix factorization with non-random missing data. In: Proceedings of the 31st
International Conference on International Conference on Machine Learning
(ICML’14). II–1512–II–1520.
Hochreiter, Sepp, Jürgen Schmidhuber, 1997. Long short-term memory. Neural Comput.
9 (8), 1735–1780, 1997.
Hu, Baotian, Lu, Zhengdong, Li, Hang, Chen, Qingcai, 2014. Convolutional neural
network architectures for matching natural language sentences. In: Proceedings of
the 27th International Conference on Neural Information Processing Systems, vol. 2,
pp. 2042–2050. NIPS ’14.
Hu, Binbin, Shi, Chuan, Zhao, Wayne Xin, Yu, Philip S., 2018. Leveraging meta-path
based context for top-N recommendation with A neural Co-attention model. In:
Proceedings of the 24th ACM SIGKDD International Conference on Knowledge
Discovery & Data Mining. KDD ’18, pp. 1531–1540.
Hu, Yujing, Qing, Da, Zeng, Anxiang, Yang, Yu, Xu, Yinghui, 2018. Reinforcement
learning to rank in E-commerce search engine: formalization, analysis, and
application. In: Proceedings of the 24th ACM SIGKDD International Conference on
Knowledge Discovery & Data Mining. KDD ’18, pp. 368–377.
Huang, Jin, Oosterhuis, Harrie, de Rijke, Maarten, Herke van Hoof, 2020. Keeping
dataset biases out of the simulation: a debiased simulator for reinforcement learning
based recommender systems. In: Fourteenth ACM Conference on Recommender
Systems. RecSys ’20, pp. 190–199.
Huang, Liang, Zhao, Kai, Ma, Mingbo, 2017. When to finish? Optimal beam search for
neural text generation (modulo beam size). In: Proceedings of the 2017 Conference
on Empirical Methods in Natural Language Processing. EMNLP ’17, pp. 2134–2139.
Ie, Eugene, Jain, Vihan, Wang, Jing, Narvekar, Sanmit, Agarwal, Ritesh, Wu, Rui,
Cheng, Heng-Tze, Lustman, Morgane, Gatto, Vince, Paul, Covington, others, 2019.
Reinforcement Learning for Slate-Based Recommender Systems: A Tractable
Decomposition and Practical Methodology arXiv preprint arXiv:1905.12767 (2019).
Iovine, Andrea, Narducci, Fedelucio, de Gemmis, Marco, 2019. A dataset of real
dialogues for conversational recommender systems. In: CLiC-It.
Iovine, Andrea, Narducci, Fedelucio, Semeraro, Giovanni, 2020. Conversational
recommender systems and natural language: a study through the ConveRSE
framework. Decis. Support Syst. 131 (2020), 113250.
Ippolito, Daphne, Kriz, Reno, Sedoc, Jo˜ao, Kustikova, Maria, Callison-Burch, Chris, 2019.
Comparison of diverse decoding methods from conditional language models. In:
Proceedings of the 57th Annual Meeting of the Association for Computational
Linguistics (ACL ’19), pp. 3752–3762.
Jagerman, Rolf, Balog, Krisztian, de Rijke, Maarten, 2018. Opensearch: lessons learned
from an online evaluation campaign. J. Data. Inf. Qual.(JDIQ) 10 (3), 1–15, 2018.
Jagerman, Rolf, Markov, Ilya, de Rijke, Maarten, 2019. When people change their mind:
off-policy evaluation in non-stationary recommendation environments. In:
Proceedings of the Twelfth ACM International Conference on Web Search and Data
Mining. WSDM ’19, pp. 447–455.
Jannach, Dietmar, Manzoor, Ahtsham, 2020. End-to-End learning for conversational
recommendation: a long way to go?. In: Proceedings of the 7th Joint Workshop on
Interfaces and Human Decision Making for Recommender Systems Co-located with
14th ACM Conference on Recommender Systems (RecSys 2020), 2020.
Jannach, Dietmar, Manzoor, Ahtsham, Cai, Wanling, Chen, Li’e, 2020. A Survey on
Conversational Recommender Systems arXiv abs/2004.00646 (2020).
J¨arvelin, Kalervo, Kek¨al¨ainen, Jaana, 2002. Cumulated gain-based evaluation of IR
techniques. ACM Trans. Inf. Syst. 20 (Oct. 2002), 422–446, 4.
Jiang, Hai, Qi, Xin, He, Sun, 2014. Choice-based recommender systems: a unified
approach to achieving relevancy and diversity. Oper. Res. 62 (5), 973–993, 2014.
Jin, Xisen, Lei, Wenqiang, Ren, Zhaochun, Chen, Hongshen, Liang, Shangsong,
Zhao, Yihong, Yin, Dawei, 2018. Explicit state tracking with semi-supervisionfor
neural dialogue generation. In: Proceedings of the 27th ACM International
Conference on Information and Knowledge Management. CIKM ’18, pp. 1403–1412.
Kang, Dongyeop, Balakrishnan, Anusha, Shah, Pararth, Paul, Crook, Lan Boureau, Y.-,
Weston, Jason, 2019. Recommendation as a communication game: self-supervised
bot-play for goal-oriented dialogue. In: Proceedings of the 2019 Conference on
Empirical Methods in Natural Language Processing and the 9th International Joint
Conference on Natural Language Processing. EMNLP-IJCNLP ’19, pp. 1951–1961.
Katehakis, Michael N., Veinott, Arthur F., 1987. The multi-armed bandit problem:
decomposition and computation. Math. Oper. Res. 12 (2), 262–268. May 1987.
Ke, Guolin, Meng, Qi, Finley, Thomas, Wang, Taifeng, Chen, Wei, Ma, Weidong,
Ye, Qiwei, Liu, Tie-Yan, 2017. LightGBM: a highly efficient gradient boosting
decision tree. In: Proceedings of the 31st International Conference on Neural
Information Processing Systems. NIPS ’17, pp. 3149–3157.
Ke, Pei, Guan, Jian, Huang, Minlie, Zhu, Xiaoyan, 2018. Generating informative
responses with controlled sentence function. In: Proceedings of the 56th Annual
Meeting of the Association for Computational Linguistics (ACL ’18), pp. 1499–1508.
Kim, Yoon, 2014. Convolutional neural networks for sentence classification. In:
Proceedings of the 2014 Conference on Empirical Methods in Natural Language
Processing. EMNLP ’14, pp. 1746–1751.
Kipf, Thomas N., Welling, Max, 2017. Semi-Supervised Classification with Graph
Convolutional Networks, 2017.
Krichene, Walid, Rendle, Steffen, 2020. On sampled metrics for item recommendation.
In: Proceedings of the 26th ACM SIGKDD International Conference on Knowledge
Discovery & Data Mining. KDD ’20, pp. 1748–1757.
Kumar, Ankit, Irsoy, Ozan, Peter, Ondruska, Iyyer, Mohit, James, Bradbury,
Gulrajani, Ishaan, Zhong, Victor, Paulus, Romain, Socher, Richard, 2016. Ask me
anything: dynamic memory networks for natural language processing. In:
Proceedings of the 33rd International Conference on International Conference on
Machine Learning. ICML ’16), pp. 1378–1387.
Kveton, Branislav, Szepesvari, Csaba, Zheng, Wen, Ashkan, Azin, 2015. Cascading
bandits: learning to rank in the cascade model. In: International Conference on
Machine Learning. ICML ’15), pp. 767–776.
Lapata, Mirella, 2003. Probabilistic text structuring: experiments with sentence ordering.
In: Proceedings of the 41st Annual Meeting on Association for Computational
Linguistics, vol. 1. ACL ’03, pp. 545–552.
Lapata, Mirella, Barzilay, Regina, 2005. Automatic evaluation of text coherence: models
and representations. In: Proceedings of the 19th International Joint Conference on
Artificial Intelligence. IJCAI ’05, pp. 1085–1090.
Lee, Hoyeop, Jinbae, Im, Jang, Seongwon, Cho, Hyunsouk, Chung, Sehee, 2019. MeLU:
meta-learned user preference estimator for cold-start recommendation. In:
Proceedings of the 25th ACM SIGKDD International Conference on Knowledge
Discovery & Data Mining. KDD ’19, pp. 1073–1082.
Lei, Wenqiang, He, Xiangnan, Miao, Yisong, Wu, Qingyun, Hong, Richang, Kan, Min-Yen,
Chua, Tat-Seng, 2020a. Estimation-action-reflection: towards deep interaction
between conversational and recommender systems. In: Proceedings of the 13th
International Conference on Web Search and Data Mining (WSDM’ 20). ACM,
pp. 304–312.
Lei, Wenqiang, Jin, Xisen, Kan, Min-Yen, Ren, Zhaochun, He, Xiangnan, Yin, Dawei,
2018. Sequicity: simplifying task-oriented dialogue systems with single sequence-to-
sequence architectures. In: Proceedings of the 56th Annual Meeting of the
Association for Computational Linguistics (ACL ’18), pp. 1437–1447.
Lei, Wenqiang, Zhang, Gangyi, He, Xiangnan, Miao, Yisong, Wang, Xiang, Chen, Liang,
Chua, Tat-Seng, 2020b. Interactive path reasoning on graph for conversational
recommendation. In: Proceedings of the 26th ACM SIGKDD International Conference
on Knowledge Discovery & Data Mining. KDD ’20, pp. 2073–2083.
Levine, Sergey, Kumar, Aviral, Tucker, George, Fu, Justin, 2020. Offline Reinforcement
Learning: Tutorial, Review, and Perspectives on Open Problems arXiv preprint arXiv:
2005.01643 (2020).
Lewis, Mike, Yarats, Denis, Dauphin, Yann, Parikh, Devi, Batra, Dhruv, 2017. Deal or No
deal? End-to-End learning of negotiation dialogues. In: Proceedings of the 2017
Conference on Empirical Methods in Natural Language Processing. EMNLP ’17,
pp. 2443–2453.
Li, Jiwei, Galley, Michel, Brockett, Chris, Gao, Jianfeng, Dolan, Bill, 2016a. A diversity-
promoting objective function for neural conversation models. In: Proceedings of the
2016 Conference of the North American Chapter of the Association for
Computational Linguistics (NAACL ’16, pp. 110–119.
Li, Jiwei, Monroe, Will, Ritter, Alan, Jurafsky, Dan, Galley, Michel, Gao, Jianfeng,
2016b. Deep reinforcement learning for dialogue generation. In: Proceedings of the
2016 Conference on Empirical Methods in Natural Language Processing. EMNLP ’16,
pp. 1192–1202.
Li, Lihong, Chu, Wei, Langford, John, Robert, E., Schapire, 2010. A contextual-bandit
approach to personalized news article recommendation. In: Proceedings of the 19th
International Conference on World Wide Web (WWW ’10), pp. 661–670.
Li, Lihong, Kim, Jin Young, Zitouni, Imed, 2015. Toward predicting the outcome of an A/
B experiment for search relevance. In: Proceedings of the Eighth ACM International
Conference on Web Search and Data Mining. WSDM ’15, pp. 37–46.
C. Gao et al.
AI Open 2 (2021) 100–126
123Li, Raymond, Kahou, Samira Ebrahimi, Schulz, Hannes, Vincent, Michalski,
Charlin, Laurent, Pal, Chris, 2018. Towards deep conversational recommendations.
In: Proceedings of the 32nd International Conference on Neural Information
Processing Systems. NeurIPS ’18, pp. 9748–9758.
Li, Shijun, Lei, Wenqiang, Wu, Qingyun, He, Xiangnan, Jiang, Peng, Chua, Tat-Seng,
2021b. Seamlessly Unifying Attributes and Items: Conversational Recommendation
for Cold-Start Users. ACM Transactions on Information Systems, 2021.
Li, Ziming, Kiseleva, Julia, de Rijke, Maarten, 2021a. Improving response quality with
backward-reasoning in open-domain dialogue systems. In: SIGIR 2021: 44th
International ACM SIGIR Conference on Research and Development in Information
Retrieval.
Lian, Defu, Qi, Liu, Chen, Enhong, 2020. Personalized ranking with importance
sampling. In: Proceedings of the Web Conference 2020 (WWW ’20), pp. 1093–1103.
Liao, Lizi, Ma, Yunshan, He, Xiangnan, Hong, Richang, Chua, Tat-Seng, 2018.
Knowledge-aware multimodal dialogue systems. In: Proceedings of the 26th ACM
International Conference on Multimedia. MM ’18, pp. 801–809.
Liao, Lizi, Takanobu, Ryuichi, Ma, Yunshan, Yang, Xun, Huang, Minlie, Chua, Tat-Seng,
2020. Topic-guided relational conversational recommender in multi-domain. In:
IEEE Transactions on Knowledge and Data Engineering. TKDE, 2020.
Lin, Chin-Yew, 2004. ROUGE: a package for automatic evaluation of summaries. In: Text
Summarization Branches Out, pp. 74–81.
Liu, Chia-Wei, Lowe, Ryan, Serban, Iulian, Noseworthy, Mike, Charlin, Laurent,
Pineau, Joelle, 2016a. How NOT to evaluate your dialogue system: an empirical
study of unsupervised evaluation metrics for dialogue response generation. In:
Proceedings of the 2016 Conference on Empirical Methods in Natural Language
Processing. EMNLP ’16, pp. 2122–2132.
Liu, Dugang, Cheng, Pengxiang, Dong, Zhenhua, He, Xiuqiang, Pan, Weike, Zhong, Ming,
2020aa. A general knowledge distillation framework for counterfactual
recommendation via uniform data. In: Proceedings of the 43rd International ACM
SIGIR Conference on Research and Development in Information Retrieval. SIGIR ’20,
pp. 831–840.
Liu, Weiwen, Liu, Qing, Tang, Ruiming, Chen, Junyang, He, Xiuqiang, Ann Heng, Pheng,
2020ba. Personalized Re-ranking with item relationships for E-commerce. In:
Proceedings of the 29th ACM International Conference on Information & Knowledge
Management. CIKM ’20, pp. 925–934.
Liu, Yiming, Cao, Xuezhi, Yu, Yong, 2016b. Are you influenced by others when rating?
Improve rating prediction by conformity modeling. In: Proceedings of the 10th ACM
Conference on Recommender Systems. RecSys ’16, pp. 269–272.
Liu, Yong, Xiao, Yingtai, Wu, Qiong, Miao, Chunyan, Zhang, Juyong, Zhao, Binqiang,
Tang, Haihong, 2020bb. Diversified interactive recommendation with implicit
feedback. In: Proceedings of the Thirty-Fourth AAAI Conference on Artificial
Intelligence. AAAI ’20, pp. 4932–4939.
Liu, Zeming, Wang, Haifeng, Niu, Zheng-Yu, Wu, Hua, Che, Wanxiang, Liu, Ting,
2020ab. Towards conversational recommendation over multi-type dialogs. In:
Proceedings of the 58th Annual Meeting of the Association for Computational
Linguistics (ACL ’20), pp. 1036–1049.
Loepp, Benedikt, Tim Hussein, Ziegler, Jüergen, 2014. Choice-based preference
elicitation for collaborative filtering recommender systems. In: Proceedings of the
SIGCHI Conference on Human Factors in Computing Systems. CHI ’14,
pp. 3085–3094.
Louviere, Jordan J., Hensher, David A., Swait, Joffre D., 2000. Stated Choice Methods:
Analysis and Applications. Cambridge University Press.
Lu, Zhengdong, Li, Hang, 2013. A deep architecture for matching short texts. In:
Proceedings of the 26th International Conference on Neural Information Processing
Systems. NIPS ’13, pp. 1367–1375.
Luo, Kai, Scott, Sanner, Wu, Ga, Li, Hanze, Yang, Hojin, 2020. Latent linear critiquing for
conversational recommender systems. In: Proceedings of the Web Conference 2020
(WWW’ 20), pp. 2535–2541.
Luo, Kai, Yang, Hojin, Wu, Ga, Scott, Sanner, 2020. Deep critiquing for VAE-based
recommender systems. In: Proceedings of the 43rd International ACM SIGIR
Conference on Research and Development in Information Retrieval. SIGIR ’20,
pp. 1269–1278.
Ma, Hao, Yang, Haixuan, King, Irwin, Lyu, Michael R., 2008. Learning latent semantic
relations from clickthrough data for query suggestion. In: Proceedings of the 17th
ACM Conference on Information and Knowledge Management. CIKM ’08,
pp. 709–718.
Ma, Jianxin, Chang, Zhou, Cui, Peng, Yang, Hongxia, Zhu, Wenwu, 2019. Learning
disentangled representations for recommendation. In: Proceedings of the 34th
International Conference on Neural Information Processing Systems. NeurIPS ’20,
pp. 5711–5722.
Ma, Wenchang, Takanobu, Ryuichi, Tu, Minghao, Huang, Minlie, 2020. Bridging the Gap
between Conversational Reasoning and Interactive Recommendation arXiv preprint
arXiv:2010.10333 (2020).
Ma, Zhengyi, Dou, Zhicheng, Zhu, Yutao, Zhong, Hanxun, Wen, Ji-Rong, 2021. One
chatbot per person: creating personalized chatbots based on implicit user profiles. In:
SIGIR 2021: 44th International ACM SIGIR Conference on Research and
Development in Information Retrieval (SIGIR ’21).
Mahmood, Tariq, Ricci, Francesco, 2007. Learning and adaptivity in interactive
recommender systems. In: Proceedings of the Ninth International Conference on
Electronic Commerce. ICEC ’07, pp. 75–84.
Mangili, Francesca, Broggini, Denis, Antonucci, Alessandro, Alberti, Marco,
Cimasoni, Lorenzo, 2020. A bayesian approach to conversational recommendation
systems. In: AAAI 2020 Workshop on Interactive and Conversational
Recommendation Systems, 2020.
Marlin, Benjamin M., Zemel, Richard S., Roweis, Sam, Slaney, Malcolm, 2007.
Collaborative filtering and the missing at random assumption. In: Proceedings of the
Twenty-Third Conference on Uncertainty in Artificial Intelligence. UAI ’07,
pp. 267–275.
McAuley, Julian, Pandey, Rahul, Leskovec, Jure, 2015a. Inferring networks of
substitutable and complementary products. In: Proceedings of the 21th ACM
SIGKDD International Conference on Knowledge Discovery and Data Mining. KDD
’15, pp. 785–794.
McAuley, Julian, Targett, Christopher, Shi, Qinfeng, van den Hengel, Anton, 2015b.
Image-based recommendations on styles and substitutes. In: Proceedings of the 38th
International ACM SIGIR Conference on Research and Development in Information
Retrieval. SIGIR ’15, pp. 43–52.
McCarthy, Kevin, Reilly, James, McGinty, Lorraine, Smyth, Barry. Thinking positively-
explanatory feedback for conversational recommender systems. In: Proceedings of
the European Conference on Case-Based Reasoning (ECCBR-04) Explanation
Workshop, pp. 115–124.
McInerney, James, Brost, Brian, Chandar, Praveen, Mehrotra, Rishabh,
Carterette, Benjamin, 2020. Counterfactual evaluation of slate recommendations
with sequential reward interactions. In: Proceedings of the 26th ACM SIGKDD
International Conference on Knowledge Discovery & Data Mining. KDD ’20,
pp. 1779–1788.
Mesnil, Gr´egoire, He, Xiaodong, Deng, Li, Bengio, Yoshua, 2013. Investigation of
recurrent-neural-network architectures and learning methods for spoken language
understanding. Interspeech 3771–3775.
Miller, Alexander, Fisch, Adam, Dodge, Jesse, Karimi, Amir-Hossein, Bordes, Antoine,
Weston, Jason, 2016. Key-value memory networks for directly reading documents.
In: Proceedings of the 2016 Conference on Empirical Methods in Natural Language
Processing. EMNLP ’16, pp. 1400–1409.
Mirzadeh, Nader, Ricci, Francesco, Bansal, Mukesh, 2005. Feature selection methods for
conversational recommender systems. In: Proceedings of the 2005 IEEE International
Conference on E-Technology, E-Commerce and E-Service (EEE’05) on E-Technology,
E-Commerce and E-Service (EEE ’05), pp. 772–777.
Mnih, Volodymyr, Kavukcuoglu, Koray, Silver, David, Graves, Alex, Antonoglou, Ioannis,
Wierstra, Daan, Riedmiller, Martin, 2013. Playing atari with deep reinforcement
learning. In: Proceedings of the 27th International Conference on Neural Information
Processing Systems (NIPS) Deep Learning Workshop (2013).
Mo, Kaixiang, Zhang, Yu, Li, Shuangyin, Li, Jiajun, Yang, Qiang, 2018. Personalizing a
dialogue system with transfer reinforcement learning. In: Proceedings of the Thirty-
Second AAAI Conference on Artificial Intelligence (AAAI ’18).
Moon, Seungwhan, Shah, Pararth, Kumar, Anuj, Rajen Subba, 2019. OpenDialKG:
explainable conversational reasoning with attention-based walks over knowledge
graphs. In: Proceedings of the 57th Annual Meeting of the Association for
Computational Linguistics (ACL ’19), pp. 845–854.
Narayan, Shashi, Cohen, Shay B., Lapata, Mirella, 2018. Ranking sentences for extractive
summarization with reinforcement learning. In: Proceedings of the 2018 Conference
of the North American Chapter of the Association for Computational Linguistics:
Human Language Technologies (NAACL ’18), pp. 1747–1759.
Nelder, John Ashworth, Wedderburn, Robert WM., 1972. Generalized linear models.
J. Roy. Stat. Soc. 135 (3), 370–384, 1972.
Ng, Andrew Y., Russell, Stuart J., 2000. Algorithms for inverse reinforcement learning.
In: Proceedings of the Seventeenth International Conference on Machine Learning.
ICML ’00), pp. 663–670.
Novikova, Jekaterina, Duˇsek, Ondˇrej, Curry, Amanda Cercas, Rieser, Verena, 2017. Why
we need new evaluation metrics for NLG. In: Proceedings of the 2017 Conference on
Empirical Methods in Natural Language Processing. EMNLP ’16, pp. 2241–2252.
Pang, Liang, Lan, Yanyan, Guo, Jiafeng, Xu, Jun, Wan, Shengxian, Cheng, Xueqi, 2016.
Text matching as image recognition. In: Proceedings of the Thirtieth AAAI
Conference on Artificial Intelligence. AAAI ’16, pp. 2793–2799.
Papineni, Kishore, Roukos, Salim, Ward, Todd, Zhu, Wei-Jing, 2002a. Bleu: a method for
automatic evaluation of machine translation. In: Proceedings of the 40th Annual
Meeting of the Association for Computational Linguistics, pp. 311–318.
Papineni, Kishore, Roukos, Salim, Ward, Todd, Zhu, Wei-Jing, 2002b. Bleu: a method for
automatic evaluation of machine translation. In: Proceedings of the 40th Annual
Meeting of the Association for Computational Linguistics (ACL ’02), pp. 311–318.
Pecune, Florian, Callebert, Lucile, Marsella, Stacy, 2020. A socially-aware conversational
recommender system for personalized recipe recommendations. In: Proceedings of
the 8th International Conference on Human-Agent Interaction (HAI ’20), pp. 78–86.
Pecune, Florian, Murali, Shruti, Tsai, Vivian, Matsuyama, Yoichi, Cassell, Justine, 2019.
A model of social explanations for a conversational movie recommendation system.
In: Proceedings of the 7th International Conference on Human-Agent Interaction
(HAI ’19), pp. 135–143.
Pei, Jiahuan, Ren, Pengjie, de Rijke, Maarten, 2021. A cooperative memory network for
personalized task-oriented dialogue systems with incomplete user profiles. In: The
Web Conference 2021.
Penha, Gustavo, Hauff, Claudia, 2020. What does BERT know about books, movies and
music? Probing BERT for conversational recommendation. In: Fourteenth ACM
Conference on Recommender Systems. RecSys ’20, pp. 388–397.
Pittermann, Johannes, Pittermann, Angela, Minker, Wolfgang, 2010. Emotion
recognition and adaptation in spoken dialogue systems. Int. J. Speech Technol. 13
(1), 49–60, 2010.
Pradel, Bruno, Usunier, Nicolas, Gallinari, Patrick, 2012. Ranking with non-random
missing ratings: influence of popularity and positivity on evaluation metrics. In:
Proceedings of the Sixth ACM Conference on Recommender Systems (RecSys ’12),
pp. 147–154.
Pu, Pearl, Faltings, Boi, 2004. Decision tradeoff using example-critiquing and constraint
programming. Constraints 9 (4), 289–310, 2004.
Qian, Qiao, Huang, Minlie, Zhao, Haizhou, Xu, Jingfang, Zhu, Xiaoyan, 2018. Assigning
personality/profile to a chatting machine for coherent conversation generation. In:
C. Gao et al.
AI Open 2 (2021) 100–126
124Proceedings of the Twenty-Seventh International Joint Conference on Artificial
Intelligence. IJCAI ’18, pp. 4279–4285.
Qin, Lijing, Chen, Shouyuan, Zhu, Xiaoyan, 2014. Contextual combinatorial bandit and
its application on diversified online recommendation. In: Proceedings of the 2014
SIAM International Conference on Data Mining (SDM ’14). SIAM, pp. 461–469.
Qiu, Lisong, Li, Juntao, Bi, Wei, Zhao, Dongyan, Yan, Rui, 2019. Are training samples
correlated? Learning to generate dialogue responses with multiple references. In:
Proceedings of the 57th Annual Meeting of the Association for Computational
Linguistics (ACL ’19), pp. 3826–3835.
Qiu, Xipeng, Huang, Xuanjing, 2015. Convolutional neural tensor network architecture
for community-based question answering. In: Proceedings of the 24th International
Conference on Artificial Intelligence. IJCAI ’15, pp. 1305–1311.
Rana, Arpit, Bridge, Derek, 2020. Navigation-by-Preference: a new conversational
recommender with preference-based feedback. In: Proceedings of the 25th
International Conference on Intelligent User Interfaces. IUI ’20, pp. 155–165.
Ren, Pengjie, Chen, Zhumin, Monz, Christof, Ma, Jun, de Rijke, Maarten, 2020a.
Thinking globally, acting locally: distantly supervised global-to-local knowledge
selection for background based conversation. In: Proceedings of the Thirty-Fourth
AAAI Conference on Artificial Intelligence. AAAI ’20.
Ren, Pengjie, Chen, Zhumin, Ren, Zhaochun, Kanoulas, Evangelos, Monz, Christof, de
Rijke, Maarten, 2020b. Conversations with search engines. arXiv preprint arXiv:
2004.14162 (April 2020). https://arxiv.org/pdf/2004.14162.
Ren, Pengjie, Liu, Zhongkun, Song, Xiaomeng, Tian, Hongtao, Chen, Zhumin,
Ren, Zhaochun, de Rijke, Maarten, 2021. Wizard of search engine: access to
information through conversations with search engines. In: SIGIR 2021: 44th
International ACM SIGIR Conference on Research and Development in Information
Retrieval.
Ren, Xuhui, Yin, Hongzhi, Chen, Tong, Wang, Hao, Nguyen, Quoc Viet Hung, Huang, Zi,
Zhang, Xiangliang, 2020. CRSAL: conversational recommender systems with
adversarial learning. ACM Trans. Inf. Syst. 38 (4).
Rendle, Steffen, 2010. Factorization machines. In: Proceedings of the 2010 IEEE
International Conference on Data Mining. ICDM ’10), pp. 995–1000.
Rosset, Corbin, Xiong, Chenyan, Song, Xia, Campos, Daniel, Craswell, Nick,
Tiwary, Saurabh, Bennett, Paul, 2020. Leading conversational search by suggesting
useful questions. In: Proceedings of the Web Conference 2020 (WWW ’20),
pp. 1160–1170.
Saavedra, Paula, Barreiro, Pablo, Duran, Roi, Crujeiras, Rosa, Loureiro, María,
Vila, Eduardo S´anchez, 2016. Choice-based recommender systems. In: ACM RecSys
Workshop on Recommenders in Tourism, pp. 38–46.
Sakhi, Otmane, Bonner, Stephen, Rohde, David, Vasile, Flavian, 2020. BLOB: a
probabilistic model for recommendation that combines organic and bandit signals.
In: Proceedings of the 26th ACM SIGKDD International Conference on Knowledge
Discovery & Data Mining. KDD ’20, pp. 783–793.
Salakhutdinov, Ruslan, Mnih, Andriy, 2007. Probabilistic matrix factorization. In:
Proceedings of the 20th International Conference on Neural Information Processing
Systems. NIPS ’07, pp. 1257–1264.
Sarwar, Badrul, George, Karypis, Joseph, Konstan, Riedl, John, 2001. Item-based
collaborative filtering recommendation algorithms. In: Proceedings of the 10th
International Conference on World Wide Web. WWW ’01, pp. 285–295.
Schafer, J Ben, Frankowski, Dan, Herlocker, Jon, Sen, Shilad, 2007. Collaborative
filtering recommender systems. In: The Adaptive Web. Springer, pp. 291–324.
Schatzmann, Jost, Thomson, Blaise, Weilhammer, Karl, Ye, Hui, Young, Steve, 2007.
Agenda-based user simulation for bootstrapping a POMDP dialogue system. In:
Human Language Technologies 2007: the Conference of the North American Chapter
of the Association for Computational Linguistics; Companion Volume, Short Papers
(NAACL-Short ’07), pp. 149–152.
Schlichtkrull, Michael, Kipf, Thomas N., Peter Bloem, Van Den Berg, Rianne, Titov, Ivan,
Welling, Max, 2018. Modeling relational data with graph convolutional networks.
In: European Semantic Web Conference (ESWC ’18). Springer, pp. 593–607.
Schnabel, Tobias, Bennett, Paul N., Dumais, Susan T., Joachims, Thorsten, 2018. Short-
term satisfaction and long-term coverage: understanding how users tolerate
algorithmic exploration. In: Proceedings of the Eleventh ACM International
Conference on Web Search and Data Mining. WSDM ’18, pp. 513–521.
Schulman, John, Levine, Sergey, Moritz, Philipp, Jordan, Michael, Abbeel, Pieter, 2015.
Trust region policy optimization. In: Proceedings of the 32nd International
Conference on International Conference on Machine Learning. ICML ’15,
pp. 1889–1897.
Sepliarskaia, Anna, Kiseleva, Julia, Radlinski, Filip, de Rijke, Maarten, 2018. Preference
elicitation as an optimization problem. In: Proceedings of the 12th ACM Conference
on Recommender Systems. RecSys ’18, pp. 172–180.
Sharma, Ashish, Lin, Inna W., Miner, Adam S., Atkins, David C., Tim Althoff, 2021.
Towards facilitating empathic conversations in online mental health support: a
reinforcement learning approach. In: Proceedings of the Web Conference 2021
(WWW ’21).
Shi, Yue, Larson, Martha, Hanjalic, Alan, 2014. Collaborative filtering beyond the user-
item matrix: a survey of the state of the art and future challenges. Comput. Surv. 47
(1), 45. Article 3 (May 2014).
Silveira, Thiago, Zhang, Min, Lin, Xiao, Liu, Yiqun, Ma, Shaoping, 2019. How good your
recommender system is? A survey on evaluations in recommendation. Int. J.Mach.
Learn. Cybern 10 (5), 813–831, 2019.
Sinha, Ayan, Gleich, David F., Ramani, Karthik, 2016. Deconvolving feedback loops in
recommender systems. In: Proceedings of the 30th International Conference on
Neural Information Processing Systems. NIPS ’16, pp. 3251–3259.
Smyth, Barry, McGinty, Lorraine, 2003. An analysis of feedback strategies in
conversational recommenders. In: The Fourteenth Irish Artificial Intelligence and
Cognitive Science Conference. AICS, Citeseer, 2003.
Smyth, Barry, McGinty, Lorraine, Reilly, James, McCarthy, Kevin, 2004. Compound
critiques for conversational recommender systems. In: Proceedings of the 2004
IEEE/WIC/ACM International Conference on Web Intelligence (WI ’04),
pp. 145–151.
Speer, Robyn, Chin, Joshua, Havasi, Catherine, 2017. ConceptNet 5.5: an open
multilingual graph of general knowledge. In: Proceedings of the Thirty-First AAAI
Conference on Artificial Intelligence. AAAI ’17, pp. 4444–4451.
Steck, Harald, 2011. Item popularity and recommendation accuracy. In: Proceedings of
the Fifth ACM Conference on Recommender Systems. RecSys ’11, pp. 125–132.
Steck, Harald, 2013. Evaluation of recommendations: rating-prediction and ranking. In:
Proceedings of the 7th ACM Conference on Recommender Systems. RecSys ’13,
pp. 213–220.
Sukhbaatar, Sainbayar, Arthur Szlam, Weston, Jason, Fergus, Rob, 2015. End-to-End
memory networks. In: Proceedings of the 28th International Conference on Neural
Information Processing Systems (NIPS ’15), pp. 2440–2448.
Sun, Wenlong, Khenissi, Sami, Nasraoui, Olfa, Shafto, Patrick, 2019. Debiasing the
human-recommender system feedback loop in collaborative filtering. In: Companion
Proceedings of the 2019 World Wide Web Conference (WWW ’19), pp. 645–651.
Sun, Weiwei, Zhang, Shuo, Balog, Krisztian, Ren, Zhaochun, Ren, Pengjie, Chen, Zhumin,
de Rijke, Maarten, 2021. Simulating user satisfaction for the evaluation of task-
oriented dialogue systems. In: SIGIR 2021: 44th International ACM SIGIR
Conference on Research and Development in Information Retrieval.
Sun, Yueming, Zhang, Yi, 2018. Conversational recommender system. In: The 41st
International ACM SIGIR Conference on Research & Development in Information
Retrieval. SIGIR ’18, pp. 235–244.
Sunehag, Peter, Evans, Richard, Dulac-Arnold, Gabriel, Zwols, Yori, Visentin, Daniel,
Ben, Coppin, 2015. Deep Reinforcement Learning with Attention for Slate Markov
Decision Processes with High-Dimensional States and Actions arXiv preprint arXiv:
1512.01124 (2015).
Sutskever, Ilya, Oriol Vinyals, Quoc, V. Le, 2014. Sequence to sequence learning with
neural networks. In: Proceedings of the 27th International Conference on Neural
Information Processing Systems, vol. 2, pp. 3104–3112. NIPS ’14.
Sutton, Richard S., Barto, Andrew G., 2018. Reinforcement Learning: an Introduction.
Tan, Ming, Cicero dos Santos, Xiang, Bing, Zhou, Bowen, 2016. Improved representation
learning for question answer matching. In: Proceedings of the 54th Annual Meeting
of the Association for Computational Linguistics (ACL ’16), pp. 464–473.
ter Hoeve, Maartje, Sim, Robert, Nouri, Elnaz, Adam, Fourney, de Rijke, Maarten,
White, Ryen, 2020. Conversations with documents: an exploration of document-
centered assistance. In: 2020 ACM SIGIR Conference on Human Information
Interaction & Retrieval. CHIIR, pp. 43–52, 2020.
Thompson, Cynthia A., G¨oker, Mehmet H., Langley, Pat, 2004. A personalized system for
conversational recommendations. J. Artif. Intell. Res. 21 (March 2004), 393–428, 1.
Tou, Frederich N., Williams, Michael D., Fikes, Richard, Henderson Jr., D Austin,
Malone, Thomas W., 1982. RABBIT: an intelligent database assistant. In: Proceedings
of the National Conference on Artificial Intelligence. AAAI ’82, pp. 314–318.
Tsumita, Daisuke, Takagi, Tomohiro, 2019. Dialogue based recommender system that
flexibly mixes utterances and recommendations. In: IEEE/WIC/ACM International
Conference on Web Intelligence (WI ’19), pp. 51–58.
Tversky, Amos, Simonson, Itamar, 1993. Context-dependent preferences. Manag. Sci. 39
(Oct. 1993), 1179–1189, 10.
Vakulenko, Svitlana, Kanoulas, Evangelos, de Rijke, Maarten, 2020a. An analysis of
mixed initiative and collaboration in information-seeking dialogues. In: Proceedings
of the 43rd International ACM SIGIR Conference on Research and Development in
Information Retrieval. SIGIR ’20), pp. 2085–2088.
Vakulenko, Svitlana, Kanoulas, Evangelos, de Rijke, Maarten, 2021. A Large-Scale
Analysis of Mixed Initiative in Information-Seeking Dialogues for Conversational
Search arXiv preprint arXiv:2104.07096 (April 2021). https://arxiv.org/pdf/2
104.07096.
Vakulenko, Svitlana, Savenkov, Vadim, de Rijke, Maarten, 2020b. Conversational
Browsing arXiv preprint arXiv:2012.03704 (December 2020). https://arxiv.
org/pdf/2012.03704.
Veliˇckovi´c, Petar, Fedus, William, Hamilton, William L., Pietro, Li`o, Bengio, Yoshua,
Hjelm, R Devon, 2019. Deep graph infomax. In: International Conference on
Learning Representations. ICLR ’19.
Vendrov, Ivan, Lu, Tyler, Huang, Qingqing, Craig, Boutilier, 2020. Gradient-based
optimization for bayesian preference elicitation. In: Proceedings of the Thirty-Fourth
AAAI Conference on Artificial Intelligence. AAAI ’20.
Viappiani, Paolo, Pu, Pearl, Faltings, Boi, 2007. Conversational recommenders with
adaptive suggestions. In: Proceedings of the 2007 ACM Conference on Recommender
Systems. RecSys ’07), pp. 89–96.
Voskarides, Nikos, Li, Dan, Ren, Pengjie, Kanoulas, Evangelos, de Rijke, Maarten, 2020.
Query resolution for conversational search with limited supervision. In: SIGIR 2020:
43rd International ACM SIGIR Conference on Research and Development in
Information Retrieval, pp. 921–932.
Wan, Mengting, Wang, Di, Liu, Jie, Bennett, Paul, McAuley, Julian, 2018. Representing
and recommending shopping baskets with complementarity, compatibility and
loyalty. In: Proceedings of the 27th ACM International Conference on Information
and Knowledge Management. CIKM ’18, pp. 1133–1142.
Wan, Shengxian, Lan, Yanyan, Guo, Jiafeng, Xu, Jun, Pang, Liang, Cheng, Xueqi, 2016.
A deep architecture for semantic matching with multiple positional sentence
representations. In: Proceedings of the Thirtieth AAAI Conference on Artificial
Intelligence. AAAI ’16, pp. 2835–2841.
Wang, Bingning, Liu, Kang, Zhao, Jun, 2016b. Inner attention based recurrent neural
networks for answer selection. In: Proceedings of the 54th Annual Meeting of the
Association for Computational Linguistics (ACL ’16), pp. 1288–1297.
C. Gao et al.
AI Open 2 (2021) 100–126
125Wang, Chaoyang, Guo, Zhiqiang, Li, Jianjun, Pan, Peng, Li, Guohui, 2020c. A text-based
deep reinforcement learning framework for interactive recommendation. In:
Proceedings of the 24th European Conference on Artificial Intelligence, vol. 325.
ECAI ’20, pp. 537–544.
Wang, Huazheng, Wu, Qingyun, Wang, Hongning, 2017. Factorization bandits for
interactive recommendation. In: Proceedings of the Thirty-First AAAI Conference on
Artificial Intelligence. AAAI ’17, pp. 2695–2702.
Wang, Hao-nan, Liu, Ning, Zhang, Yi-yun, Feng, Da-wei, Huang, Feng, Li, Dong-sheng,
Zhang, Yi-ming, 2020a. Deep reinforcement learning: a survey. Front. Inf. Technol.
Electron. Eng. 1–19, 2020.
Wang, Jane X., Kurth-Nelson, Zeb, Tirumala, Dhruva, Hubert Soyer, Joel, Z Leibo,
Munos, Remi, Blundell, Charles, Kumaran, Dharshan, Botvinick, Matt, 2016a.
Learning to Reinforcement Learn arXiv preprint arXiv:1611.05763 (2016).
Wang, Qing, Zeng, Chunqiu, Zhou, Wubai, Li, Tao, Sitharama Iyengar, S.,
Shwartz, Larisa, Ya Grabarnik, Genady, 2018. Online interactive collaborative
filtering using multi-armed bandit with dependent arms. IEEE Trans. Knowl. Data
Eng. 31 (2018), 1569–1580, 8.
Wang, Shuohang, Jiang, Jing, 2016. Learning natural language inference with LSTM. In:
Proceedings of the 2016 Conference of the North American Chapter of the
Association for Computational Linguistics: Human Language Technologies (NAACL
’16, pp. 1442–1451.
Wang, Weiquan, Benbasat, Izak, 2013. Research note—a contingency approach to
investigating the effects of user-system interaction modes of online decision aids. Inf.
Syst. Res. 24 (3), 861–876, 2013.
Wang, Wenjie, Feng, Fuli, He, Xiangnan, Nie, Liqiang, Chua, Tat-Seng, 2020a. Denoising
Implicit Feedback for Recommendation arXiv preprint arXiv:2006.04153 (2020).
Wang, Wenjie, Feng, Fuli, He, Xiangnan, Zhang, Hanwang, Chua, Tat-Seng, 2020b.
“Click” Is Not Equal to “Like”: Counterfactual Recommendation for Mitigating
Clickbait Issue arXiv preprint arXiv:2009.09945 (2020).
Wang, Xin, Steven, C., Hoi, H., Liu, Chenghao, Ester, Martin, 2017. Interactive social
recommendation. In: Proceedings of the 2017 ACM on Conference on Information
and Knowledge Management. CIKM ’17, pp. 357–366.
Wang, Xiang, Jin, Hongye, Zhang, An, He, Xiangnan, Xu, Tong, Chua, Tat-Seng, 2020b.
Disentangled graph collaborative filtering. In: Proceedings of the 43rd International
ACM SIGIR Conference on Research and Development in Information Retrieval
(SIGIR ’20).
Wang, Xuewei, Shi, Weiyan, Kim, Richard, Oh, Yoojung, Yang, Sijia, Zhang, Jingwen,
Zhou, Yu, 2019. Persuasion for good: towards a personalized persuasive dialogue
system for social good. In: Proceedings of the 57th Annual Meeting of the Association
for Computational Linguistics. ACL ’19, pp. 5635–5649.
Wang, Xiang, Xu, Yaokun, He, Xiangnan, Cao, Yixin, Wang, Meng, Chua, Tat-Seng,
2020c. Reinforced negative sampling over knowledge graph for recommendation. In:
Proceedings of the Web Conference 2020. WWW ’20, pp. 99–109.
Wei, Tianxin, Wu, Ziwei, Li, Ruirui, Hu, Ziniu, Feng, Fuli, He, Xiangnan, Sun, Yizhou,
Wang, Wei, 2020. Fast adaptation for cold-start collaborative filtering with meta-
learning. In: 2019 IEEE International Conference on Data Mining (ICDM ’19).
Wu, Felix, Souza, Amauri, Zhang, Tianyi, Fifty, Christopher, Tao, Yu, Weinberger, Kilian,
2019b. Simplifying graph convolutional networks. In: Proceedings of the 36th
International Conference on Machine Learning (Proceedings of Machine Learning
Research), vol. 97, pp. 6861–6871.
Wu, Ga, Luo, Kai, Scott, Sanner, Soh, Harold, 2019a. deep language-based critiquing for
recommender systems. In: Proceedings of the 13th ACM Conference on
Recommender Systems. RecSys ’19, pp. 137–145.
Wu, Le, He, Xiangnan, Wang, Xiang, Zhang, Kun, Wang, Meng, 2021. A Survey on Neural
Recommendation: from Collaborative Filtering to Content and Context Enriched
Recommendation arXiv preprint arXiv:2104.13030 (2021).
Wu, Shiwen, Sun, Fei, Zhang, Wentao, Cui, Bin, 2020. Graph Neural Networks in
Recommender Systems: A Survey arXiv preprint arXiv:2011.02260 (2020).
Wu, Wenquan, Guo, Zhen, Zhou, Xiangyang, Wu, Hua, Zhang, Xiyuan, Lian, Rongzhong,
Wang, Haifeng, 2019. Proactive human-machine conversation with explicit
conversation goal. In: Proceedings of the 57th Annual Meeting of the Association for
Computational Linguistics. (ACL ’19), pp. 3794–3804.
Wu, Wei, Yan, Rui, 2018. Deep chit-chat: deep learning for ChatBots. In: Proceedings of
the 2018 Conference on Empirical Methods in Natural Language Processing: Tutorial
Abstracts.
Xian, Yikun, Fu, Zuohui, Muthukrishnan, S., de Melo, Gerard, Zhang, Yongfeng, 2019.
Reinforcement knowledge graph reasoning for explainable recommendation. In:
Proceedings of the 42nd International ACM SIGIR Conference on Research and
Development in Information Retrieval. SIGIR ’19, pp. 285–294.
Xu, Hu, Moon, Seungwhan, Liu, Honglei, Liu, Bing, Shah, Pararth, Liu, Bing, Yu, Philip,
2020. User memory reasoning for conversational recommendation. In: Proceedings
of the 28th International Conference on Computational Linguistics. COLING ’20,
pp. 5288–5308.
Xu, Kerui, Yang, Jingxuan, Xu, Jun, Gao, Sheng, Guo, Jun, Wen, Ji-Rong, 2021. Adapting
user preference to online feedback in multi-round conversational recommendation.
In: Proceedings of the 14th ACM International Conference on Web Search and Data
Mining. WSDM ’21, pp. 364–372.
Xu, Ya, Chen, Nanyu, Fernandez, Addrian, Omar, Sinno, Bhasin, Anmol, 2015. From
infrastructure to culture: A/B testing challenges in large scale social networks. In:
Proceedings of the 21th ACM SIGKDD International Conference on Knowledge
Discovery and Data Mining. KDD ’15), pp. 2227–2236.
Yan, Rui, Song, Yiping, Wu, Hua, 2016. Learning to respond with deep neural networks
for retrieval-based human-computer conversation system. In: Proceedings of the
39th International ACM SIGIR Conference on Research and Development in
Information Retrieval. SIGIR ’16, pp. 55–64.
Yang, Hojin, Scott, Sanner, Wu, Ga, Zhou, Jin Peng, 2021. Bayesian preference elicitation
with keyphase-item coembeddings for interactive recommendation. In: Proceedings
of the 29th International Conference on User Modeling, Adaptation, and
Personalization (UMAP-21).
Yang, Longqi, Cui, Yin, Yuan, Xuan, Wang, Chenyang, Belongie, Serge, Estrin, Deborah,
2018. Unbiased offline recommender evaluation for missing-not-at-random implicit
feedback. In: Proceedings of the 12th ACM Conference on Recommender Systems.
RecSys ’18, pp. 279–287.
Yang, Mengyue, Li, Qingyang, Qin, Zhiwei, Ye, Jieping, 2020. Hierarchical adaptive
contextual bandits for resource constraint based recommendation. In: Proceedings of
the Web Conference 2020 (WWW’ 20), pp. 292–302.
Yao, Kaisheng, Peng, Baolin, Zhang, Yu, Dong, Yu, Zweig, Geoffrey, Shi, Yangyang, 2014.
Spoken language understanding using long short-term memory neural networks. In:
2014 IEEE Spoken Language Technology Workshop (SLT Workshop). IEEE,
pp. 189–194.
Yao, Kaisheng, Zweig, Geoffrey, Hwang, Mei-Yuh, Shi, Yangyang, Dong, Yu, 2013.
Recurrent neural networks for language understanding. Interspeech 2524–2528.
Yeh, Yi-Ting, Chen, Yun-Nung, 2019. QAInfomax: learning robust question answering
system by mutual information maximization. In: Proceedings of the 2019 Conference
on Empirical Methods in Natural Language Processing and the 9th International
Joint Conference on Natural Language Processing. EMNLP-IJCNLP, pp. 3370–3375.
Ying, Rex, He, Ruining, Chen, Kaifeng, Eksombatchai, Pong, Hamilton, William L.,
Leskovec, Jure, 2018. Graph convolutional neural networks for web-scale
recommender systems. In: Proceedings of the 24th ACM SIGKDD International
Conference on Knowledge Discovery & Data Mining. KDD ’18, pp. 974–983.
Young, Tom, Cambria, Erik, Chaturvedi, Iti, Zhou, Hao, Biswas, Subham, Huang, Minlie,
2018. Augmenting end-to-end dialogue systems with commonsense knowledge. In:
Proceedings of the Thirty-Second AAAI Conference on Artificial Intelligence (AAAI
’18).
Yu, Junliang, Gao, Min, Yin, Hongzhi, Li, Jundong, Gao, Chongming, Wang, Qinyong,
2019a. Generating reliable friends via adversarial training to improve social
recommendation. In: 2019 IEEE International Conference on Data Mining (ICDM
’19). IEEE, pp. 768–777.
Yu, Tong, Shen, Yilin, Jin, Hongxia, 2019b. A visual dialog augmented interactive
recommender system. In: Proceedings of the 25th ACM SIGKDD International
Conference on Knowledge Discovery & Data Mining. KDD ’19, pp. 157–165.
Zeng, Chunqiu, Wang, Qing, Mokhtari, Shekoofeh, Li, Tao, 2016. Online context-aware
recommendation with time varying multi-armed bandit. In: Proceedings of the 22nd
ACM SIGKDD International Conference on Knowledge Discovery & Data Mining.
KDD ’16, pp. 2025–2034.
Zhang, Ruiyi, Tong, Yu, Shen, Yilin, Jin, Hongxia, Chen, Changyou, Lawrence, Carin,
2019. Reward constrained interactive recommendation with natural language
feedback. In: Proceedings of the 33rd International Conference on Neural
Information Processing Systems. NeurIPS ’19.
Zhang, Shuo, Balog, Krisztian, 2020. Evaluating conversational recommender systems
via user simulation. In: Proceedings of the 26th ACM SIGKDD International
Conference on Knowledge Discovery & Data Mining. KDD ’20, pp. 1512–1520.
Zhang, Shuai, Yao, Lina, Sun, Aixin, Tay, Yi, 2019a. Deep learning based recommender
system: a survey and new perspectives. ACM Computing Surveys (CSUR) 52 (1), 38.
Article 5 (2019).
Zhang, Xiaoying, Xie, Hong, Li, Hang, John, C., Lui, S., 2020c. Conversational contextual
bandit: algorithm and application. In: Proceedings of the Web Conference (WWW
’20), pp. 662–672.
Zhang, Yongfeng, Chen, Xu, 2020. Explainable recommendation: a survey and new
perspectives. Foundations and Trends® in Information Retrieval 14 (2020), 1–101,
1.
Zhang, Yongfeng, Chen, Xu, Ai, Qingyao, Liu, Yang, Bruce Croft, W., 2018. Towards
conversational search and recommendation: system Ask, user respond. In:
Proceedings of the 27th ACM International Conference on Information and
Knowledge Management. CIKM ’18, pp. 177–186.
Zhang, Yongfeng, Lai, Guokun, Zhang, Min, Zhang, Yi, Liu, Yiqun, Ma, Shaoping, 2014a.
Explicit factor models for explainable recommendation based on phrase-level
sentiment analysis. In: Proceedings of the 37th International ACM SIGIR Conference
on Research & Development in Information Retrieval (SIGIR ’14), pp. 83–92.
Zhang, Yichi, Ou, Zhijian, Zhou, Yu, 2020. Task-oriented dialog systems that consider
multiple appropriate responses under the same context. In: Proceedings of the
Thirty-Fourth AAAI Conference on Artificial Intelligence. AAAI ’20.
Zhang, Yongfeng, Zhang, Haochen, Zhang, Min, Liu, Yiqun, Ma, Shaoping, 2014b. Do
users rate or review? Boost phrase-level sentiment labeling with review-level
sentiment classification. In: Proceedings of the 37th International ACM SIGIR
Conference on Research & Development in Information Retrieval. SIGIR ’14,
pp. 1027–1030.
Zhang, Zheng, Liao, Lizi, Huang, Minlie, Zhu, Xiaoyan, Chua, Tat-Seng, 2019. Neural
multimodal belief tracker with adaptive attention for dialogue systems. In: The
World Wide Web Conference (WWW ’19), pp. 2401–2412.
Zhang, Zheng, Takanobu, Ryuichi, Huang, Minlie, Zhu, Xiaoyan, 2020b. Recent
Advances and Challenges in Task-Oriented Dialog System arXiv preprint arXiv:
2003.07490 (2020).
Zhao, Xiangyu, Xia, Long, Ding, Zhuoye, Yin, Dawei, Tang, Jiliang, 2019. Toward
Simulating Environments in Reinforcement Learning Based Recommendations arXiv
preprint arXiv:1906.11462 (2019).
Zhao, Xiangyu, Zhang, Liang, Ding, Zhuoye, Xia, Long, Tang, Jiliang, Yin, Dawei, 2018.
Recommendations with negative feedback via pairwise deep reinforcement learning.
In: Proceedings of the 24th ACM SIGKDD International Conference on Knowledge
Discovery & Data Mining. KDD ’18, pp. 1040–1048.
C. Gao et al.
AI Open 2 (2021) 100–126
126Zhao, Xiaoxue, Zhang, Weinan, Wang, Jun, 2013. Interactive collaborative filtering. In:
Proceedings of the 22nd ACM International Conference on Information & Knowledge
Management. CIKM ’13, pp. 1411–1420.
Zheng, Guanjie, Zhang, Fuzheng, Zheng, Zihan, Yang, Xiang, Yuan, Nicholas Jing,
Xie, Xing, Li, Zhenhui, 2018. DRN: a deep reinforcement learning framework for
news recommendation. In: Proceedings of the 2018 World Wide Web Conference
(WWW ’18, pp. 167–176.
Zheng, Yinhe, Zhang, Rongsheng, Huang, Minlie, Mao, Xiaoxi, 2020. A pre-training
based personalized dialogue generation model with persona-sparse data. In:
Proceedings of the Thirty-Fourth AAAI Conference on Artificial Intelligence. AAAI
’20, pp. 9693–9700.
Zhou, Chunyi, Jin, Yuanyuan, Wang, Xiaoling, Zhang, Yingjie, 2020e. Conversational
music recommendation based on bandits. In: 2020 IEEE International Conference on
Knowledge Graph (ICKG ’20). IEEE, pp. 41–48.
Zhou, Guorui, Zhu, Xiaoqiang, Song, Chenru, Fan, Ying, Han, Zhu, Xiao, Ma,
Yan, Yanghui, Jin, Junqi, Han, Li, Gai, Kun, 2018a. Deep interest network for click-
through rate prediction. In: Proceedings of the 24th ACM SIGKDD International
Conference on Knowledge Discovery & Data Mining. KDD ’18, pp. 1059–1068.
Zhou, Hao, Huang, Minlie, Zhang, Tianyang, Zhu, Xiaoyan, Liu, Bing, 2018b. Emotional
chatting machine: emotional conversation generation with internal and external
memory. In: Proceedings of the Thirty-Second AAAI Conference on Artificial
Intelligence (AAAI ’18).
Zhou, Hao, Young, Tom, Huang, Minlie, Zhao, Haizhou, Xu, Jingfang, Zhu, Xiaoyan,
2018c. Commonsense knowledge aware conversation generation with graph
attention. In: Proceedings of the 27th International Joint Conference on Artificial
Intelligence. IJCAI ’18, pp. 4623–4629.
Zhou, Kun, Wang, Xiaolei, Zhou, Yuanhang, Shang, Chenzhan, Cheng, Yuan,
Zhao, Wayne Xin, Li, Yaliang, Wen, Ji-Rong, 2021. CRSLab: an Open-Source Toolkit
for Building Conversational Recommender System arXiv preprint arXiv:2101.00939
(2021).
Zhou, Kun, Zhao, Wayne Xin, Bian, Shuqing, Zhou, Yuanhang, Wen, Ji-Rong,
Yu, Jingsong, 2020a. Improving conversational recommender systems via
knowledge graph based semantic fusion. In: Proceedings of the 26th ACM SIGKDD
International Conference on Knowledge Discovery & Data Mining (SIGKDD’ 20),
pp. 1006–1014.
Zhou, Kun, Zhao, Wayne Xin, Wang, Hui, Wang, Sirui, Zhang, Fuzheng,
Wang, Zhongyuan, Wen, Ji-Rong, 2020b. Leveraging historical interaction data for
improving conversational recommender system. In: Proceedings of the 29th ACM
International Conference on Information & Knowledge Management. CIKM ’20,
pp. 2349–2352.
Zhou, Kun, Zhou, Yuanhang, Zhao, Wayne Xin, Wang, Xiaoke, Wen, Ji-Rong, 2020c.
Towards topic-guided conversational recommender system. In: Proceedings of the
28th International Conference on Computational Linguistics (COLING ’2020).
Zhou, Sijin, Dai, Xinyi, Chen, Haokun, Zhang, Weinan, Ren, Kan, Tang, Ruiming,
He, Xiuqiang, Yu, Yong, 2020d. Interactive recommender system via knowledge
graph-enhanced reinforcement learning. In: Proceedings of the 43rd International
ACM SIGIR Conference on Research and Development in Information Retrieval.
SIGIR’ 20, pp. 179–188.
Zhu, Fengbin, Lei, Wenqiang, Wang, Chao, Zheng, Jianming, Poria, Soujanya, Chua, Tat-
Seng, 2021. Retrieving and Reading: A Comprehensive Survey on Open-Domain
Question Answering arXiv preprint arXiv:2101.00774 (2021).
Zhu, Han, Li, Xiang, Zhang, Pengye, Li, Guozheng, He, Jie, Han, Li, Gai, Kun, 2018.
Learning tree-based deep model for recommender systems. In: Proceedings of the
24th ACM SIGKDD International Conference on Knowledge Discovery & Data
Mining. KDD ’18, pp. 1079–1088.
Zong, Shi, Ni, Hao, Sung, Kenny, Nan, Rosemary Ke, Zheng, Wen, Kveton, Branislav,
2016. Cascading bandits for large-scale recommendation problems. In: Proceedings
of the Thirty-Second Conference on Uncertainty in Artificial Intelligence. UAI ’16,
pp. 835–844.
Zou, Jie, Chen, Yifan, Kanoulas, Evangelos, 2020. Towards question-based recommender
systems. In: Proceedings of the 43rd International ACM SIGIR Conference on
Research and Development in Information Retrieval. SIGIR ’20, pp. 881–890.
Zou, Lixin, Xia, Long, Ding, Zhuoye, Song, Jiaxing, Liu, Weidong, Yin, Dawei, 2019.
Reinforcement learning to optimize long-term user engagement in recommender
systems. In: Proceedings of the 25th ACM SIGKDD International Conference on
Knowledge Discovery & Data Mining. KDD ’19, pp. 2810–2818.
Zou, Lixin, Xia, Long, Pan, Du, Zhang, Zhuo, Bai, Ting, Liu, Weidong, Nie, Jian-Yun,
Yin, Dawei, 2020a. Pseudo dyna-Q: a reinforcement learning framework for
interactive recommendation. In: Proceedings of the 13th International Conference
on Web Search and Data Mining. WSDM ’20, pp. 816–824.
Zou, Lixin, Xia, Long, Gu, Yulong, Zhao, Xiangyu, Liu, Weidong, Huang, Jimmy Xiangji,
Yin, Dawei, 2020b. Neural interactive collaborative filtering. In: Proceedings of the
43rd International ACM SIGIR Conference on Research and Development in
Information Retrieval. SIGIR ’20, pp. 749–758.
C. Gao et al.
