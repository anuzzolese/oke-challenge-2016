# ESWC-16 Open Knowledge Extraction (OKE) Challenge

This folder contains guidelines and materials for the Open Knowledge Extraction challenge at [ESWC 2016](http://2016.eswc-conferences.org/).


The OKE challenge, launched as [first edition](https://github.com/anuzzolese/oke-challenge) at last year Extended Semantic Web Conference, ESWC2015, has the ambition to provide a reference framework for research on Knowledge Extraction from text for the Semantic Web by re-defining a number of tasks (typically from information and knowledge extraction), taking into account specific SW requirements. 
The OKE challenge defines three tasks, each one having a separate dataset: 
- Entity Recognition, Linking and Typing for Knowledge Base population
- Class Induction and entity typing for Vocabulary and Knowledge Base enrichment
- Web-scale Knowledge Extraction by Exploiting Structured Annotation. 

Task 1 consists of identifying Entities in a sentence and create an OWL individual representing it, link to a reference KB (DBpedia) when possible and assigning a type to such individual. 
Task 2 consists in producing rdf:type statements, given definition texts. The participants will be given a dataset of sentences, each defining an entity (known a priori). 
Task 3 will be based on one of the largest, publicly available collections of triples extracted from HTML pages (provided by the Web Data Commons project). Participants will use annotated Web pages as stimuli for training a Web-scale extraction system which is capable of extracting structured data from non-annotated pages.


The **example data** used in the following description of tasks is available in folder [example_data](./example_data)

The **Gold Standard data** is available in folder [GoldStandard_sampleData](./GoldStandard_sampleData)

# Important Dates

- Challenge **papers** submission deadline: Monday March 21st, 2016
- Challenge paper **reviews due**: Tuesday April 5th, 2016
- **Notifications** sent to participants: Friday April 8th, 2016
- **Camera ready** papers due: Sunday April 24th, 2016​
- **System evaluation and Test data** published: during [ESWC 2016](http://2016.eswc-conferences.org/) conference
- [ESWC 2016](http://2016.eswc-conferences.org/) conference: May 29th - June 2nd, 2016

Participants must:

1. Submit a paper describing their system, via [EasyChair](https://easychair.org/conferences/?conf=oke2016), no later then **March 21st, 2016**. The paper should contain the details of the system, including why the system is innovative, how it uses Semantic Web, which features or functions the system provides, what design choices were made and what lessons were learned. The description should also summarise how participants have addressed the evaluation task(s). Papers must be submitted in PDF format, following the style of the Springer's Lecture Notes in Computer Science (LNCS) series (http://www.springer.com/computer/lncs/lncs+authors), and not exceeding 12 pages in length. 
Accepted challenge papers will be published by Springer in the [CCIS series](http://www.springer.com/series/7899). 
The best challenge papers will be also published in the Satellite Event proceedings (a separate Springer LNCS Volume) of [ESWC2016](http://2016.eswc-conferences.org).

2. For task 1 and 2 provide access to the application as webservice (specifications [here](https://github.com/anuzzolese/oke-challenge-2016/blob/master/example_data/GERBIL_integration.md)), with input/output provided in [NIF](http://persistence.uni-leipzig.org/nlp2rdf/) format. The final evaluation  will be carried out via [GERBIL](http://gerbil.aksw.org). The implementation of the evaluation for tasks 1-2 is already available on GERBIL (also accessible as open source code) as [Web demo](http://gerbil.aksw.org/gerbil/config). Participants can autonomously test their system using GERBIL (selecting as Experiment Type either OKE Challenge 2015 - Task 1 or OKE Challenge 2015 - Task 2).
For task 3 participants will have to produce results in the specified format.
3. The URI for the final system (for task1-2) / results on test set (for task 3)  must be provided in the Camera Ready version of the paper. The organizers will evaluate the systems against the **evaluation dataset** during [ESWC2016](http://2016.eswc-conferences.org). The dataset will be publicly released after the announcement of results.


# Tasks Overview

##Task 1

*Entity Recognition, Linking and Typing for Knowledge Base population.*

This task consists of (i) identifying Entities in a sentence and create an OWL individual (owl:Individual statement) representing it, (ii) link (owl:sameAs statement) such individual, when possible, to a reference KB (DBpedia) and (iii) assigning a type to such individual (rdf:type statement) selected from a set of given types.

In this task by Entity we mean any discourse referent (the actors and
objects around which a story unfolds), either named or anonymous that is an individual of one of the following [DOLCE Ultra Lite classes](http://stlab.istc.cnr.it/stlab/WikipediaOntology/): 

- Person
- Place
- Organization
- Role

Entities also include anaphorically related discourse referents. Hence, anaphora resolution has to be taken into account for addressing the task.

As an example, for the sentence: 

> Florence May Harding studied at a school in Sydney, and with Douglas Robert Dundas , but in effect had no formal training in either botany or art.	

we want the system to recognize four entities:

| Recognized Entity    | generated URI | Type     | SameAs|
| ------------- |:-------------|:-------------:| -----:|
| Florence May Harding      |oke:Florence_May_Harding| dul:Person | dbpedia:Florence_May_Harding |
| school      | oke:School|dul:Organization    |    |
| Sydney | oke:Sydney| dul:Place      |    dbpedia:Sydney  |
| Douglas Robert Dundas |oke:Douglas_Robert_Dundas| dul:Person      |      |

###Expected Output

The results must be provided in [NIF](http://persistence.uni-leipzig.org/nlp2rdf/) format, including the offsets of recognized entities. The expected output for the example sentence can be found in [task1.ttl](./example_data/task1.ttl).

In the above example we use

```
@prefix oke: <http://www.ontologydesignpatterns.org/data/oke-challenge/task-1/>
```

###Evaluation

We will evaluate three aspects on this task, independently:

- Ability to recognize entities: we will check if all strings recognizing entities are identified, using the offsets returned by the systems. **Only full matches are counted as correct** (e.g. if the system returns "Art School" instead of "National Art School" is counted as a miss).
- Ability to **assign the correct type**: evaluation will be carried out only on the 4 target DOLCE types.
- Ability to **link individuals to DBpedia 2014**: participants must **link** entities to DBpedia **only when relevant** (in the example sentence, the referred Douglas Robert Dundas is not present in DBpedia)

We will calculate Precision, recall and F1 for the three subtasks and the winner for task 1 will be the system with higher average F1 for all three.

##Task 2

*Class Induction and entity typing for Vocabulary and Knowledge Base enrichment.*

This task consists in producing rdf:type statements, given definition texts. The participants will be given a dataset of sentences, each defining an entity (known a priori), e.g. the entity: dpedia:Skara_Cathedral and its definition "Skara Cathedral is a church in the Swedish city of Skara".

Participants are expected to (i) identify the type(s) of the given entity as they are expressed in the given definition, (ii) create a owl:Class statement for defining each of them as a new class in the target knowledge base, (iii) create a rdf:type statement between the given entity and the new created classes, and (iv) align the identified types, if a correct alignment is available, to a set of given types.

In the task we will evaluate the extraction of all strings describing a type and the alignment to any of the subset of [DOLCE+DnS Ultra Lite classes](http://ontologydesignpatterns.org/ont/wikipedia/d0.owl).
The list of the classes part of this subset is the following:

| Ontology class |	Label | Gloss | Examples |
|:---------------|:-------|:------|:---------|
| dul:Abstract	 | Abstract | Anything that cannot be located in space-time. | Vectors, sets, fractals, equations, etc. |
|d0:Activity | Action, activity or task | Any action or task planned or executed by an agent intentionally causing and participating in it. | Swimming, shopping, knowledge sharing, etc. |
| dul:Amount | Amount, quantity | Any quantity, independently from how it is measured, computed, etc. | kelvin, angstrom, quarter mile, silver dollar, deadline, etc. |
| d0:Characteristic | Quality, feature, attribute | An aspect or quality of a thing. | radial symmetry, poker face, alkalinity, attractiveness, darkness, etc. |
| dul:Collection | Collection or social group | A container or group of things (or agents) that share one or more common properties. | coin collection, checkout line, public library, Milky Way, etc. |
| d0:CognitiveEntity | Cognitive entity | Attitudes, cognitive abilities, ideologies, psychological phenomena, mind, etc. | discernment, homophobia, precognition, etc. |
| dul:Description | Conceptualization, description, context | A descriptive context that creates a relational view on a set of data or observations. | hypothesis, danger, Avogadro's law, string theory, utopia, etc. |
| d0:Event | Any natural event | Any natural event, independently of its possible causes. | avalanche, earthquake, brainwave, bonfire, etc. |
| dul:Goal | Goal, aim, achievement | The description of a situation that is desired by an agent. | destination, purpose, intention |
| dul:InformationEntity | Information entity, creative work, knowledge | A piece of information, be it concretely realized or not: linguistic expressions, works of art, knowledge objects. | data, string, message, novel, song, etc. |
| d0:Location | Place or space | A location, in a very generic sense e.g. geo-political entities, or physical object that are inherently located. | Oslo, Australia, Inner Mongolia, resort area, intergalactic space, tundra, tunnel, etc. |
| dul:Organism | Organism, animal, plant | A physical object with biological characteristics, typically able to self-reproduce. | Japanese banana, fox, fungus, etc. |
| dul:Organization | Organization | An internally structured, conventionally created social entity such as enterprises, bands, political parties, etc. | mathematics department, headquarters, yakuza, The Beatles, etc. |
| dul:Person | Person | Persons in commonsense intuition. | John Doe, Aristotle, Armenian, house guest, etc. |
| dul:Personification | Fictional or imaginary agent | A social entity with agentive features, invented or conceived through a cultural process. | holy grail, deus ex machina, God, magic wands, etc. |
| dul:PhysicalObject | Physical object | Any object that has a proper space region, and an associated mass: natural bodies, artifacts, substances. | Kleenex, beard, building, etc. |
| dul:Process | Natural or social process | Any natural process, independently of its possible causes. | absorption, acidification, chemical process, condensation, etc. |
| dul:Relation | Social, logical, or other relations | Any social, logical, or quantitative relation (usually quite elementary). | part, identity, homonymy, causality, reciprocality, etc. |
| dul:Role | Role | A concept that classifies some entity: social positions, roles, statuses. | soldier, eminence, legal status, etc. |
| dul:Situation | Case, condition, circumstance, state, situation | A unified view on a set of entities, e.g. physical or social facts or conditions, configurations, etc. | breaking point, circulatory failure, start topology, inflammation, alienation, etc. |
| d0:System | System | Physical, social, political systems. | viticulture, non-linear system, democracy, water system, etc. |
| dul:TimeInterval | Time interval | A time span. | January, Friday, 2011, Modern era, etc. |
| d0:Topic | Area of knowledge | Any area, discipline, subject of knowledge. | algebra, avionics, ballet, theology, engineering, etc. |



As an example, for the sentence: 

> Brian Banner is a fictional villain from the Marvel Comics Universe created by Bill Mantlo and Mike Mignola and first appearing in print in late 1985..

*Brian Banner* will be given as the *input target entity*. We want the system to recognize any possible type for it. Correct answers include:


| Recognized string for the type    | Generated Type     | rsubClassOf|
| ------------- |:-------------| -----:|
| fictional villain      |oke:FictionalVillain| dul:Personification | 
| villain      | oke:Villain|dul:Person    |  

###Expected Output

The results must be provided in [NIF](http://persistence.uni-leipzig.org/nlp2rdf/) format, including the offsets of recognized string describing the type. The expected output for the example sentence can be found in [task2.ttl](./example_data/task2.ttl).

In the above example we use

```
@prefix oke: <http://www.ontologydesignpatterns.org/data/oke-challenge/task-2/>
```

###Evaluation

We will evaluate two aspects on this task, independently:

- Ability to **recognize strings that describe the type of a target entity**. As string describing types often include adjectives as modifiers (in the example above, "fictional" is a modifier for villain), in the Gold Standard we will include all possible options; the system answer will be counted **correct as long as at least one of the possibility is returned**.
- Ability to align the identified type with a reference ontology, which for this evaluation will be the subset of [DOLCE+DnS Ultra Lite classes](http://ontologydesignpatterns.org/ont/wikipedia/d0.owl). 

We will calculate Precision, recall and F1 for the two subtasks and the winner for task 2 will be the system with higher average F1 for the two of them.


##Task 3
In the last years, more and more websites started making use of
markup languages as Microdata, RDFa, and Microformats to annotate
information on their pages. In 2014, over 17.3% of the popular
websites made use of at least one of those three markup languages,
with [schema.org and Microdata being among the most widely deployed
standards](http://dl.acm.org/citation.cfm?id=2797124). Using tools like [Any23](https://code.google.com/p/any23/) allows the extraction of such
annotated information from those web pages and returning them as
RDF triples.
One of the largest, publicly available collections of such triples
extracted from HTML pages is provided by the [Web Data Commons
project](http://webdatacommons.org/structureddata). The triples were extracted by the project using Any23
and Web crawls curated by the [Common Crawl Foundation](http://commoncrawl.org/),
which
maintains one of the largest, publicly available Web crawl corpora.
Structured annotations provide a very large corpus of training data
for knowledge extraction from the Web. In this task, we ask participants
to use annotated Web pages as stimuli for training a Web-scale
extraction system which is capable of extracting structured data from
non-annotated pages.



###Provided input 

Both training and test datasets are already available for [download](http://data.dws.informatik.uni-mannheim.de/LD4IE/2016/data/).

[Training data](http://data.dws.informatik.uni-mannheim.de/LD4IE/2016/data/train):
- The input consists of pairs of Web pages with structured annotations,
and the corresponding RDF statements extracted from
the annotations.
- For validating trained system, the Web pages are also provided
with annotations removed.


[Evaluation data](http://data.dws.informatik.uni-mannheim.de/LD4IE/2016/data/test):
- The evaluation data consists of Web pages (not contained in the
training corpus) which originally contained structured annotations.
- For those Web pages, the extracted triples are not published,
and as the annotations are removed, they cannot be trivially
reconstructed.



###Expected Output
- For each Web page, we expect the users to extract a set of RDF
statements. Following the [Microdata to RDF specification](http://www.w3.org/TR/microdata-rdf/),
each resource is represented by a blank node.
- The submission consists of RDF quads, where the fourth component
is the URL of the Web page from which the statement
has been extracted.

Both the training and evaluation
dataset are samples from the 2014 Web Data Commons corpus,
which contain exactly one root entity from a given class (e.g., Music
Recording). We provide training and evaluation datasets for five different
classes, as well as a mixed set containing instances from all five
classes. 

The size of the five datasets is the following:

| Class Avg. | instances per page | Avg. properties per page | # uniq. Hosts|
| ------------- |------------- |-------------| -----|
|MusicRecording| 2.52 | 11.77 | 154 |
|Person | 1.56 | 7.71 | 2,970 |
|Recipe | 1.76 | 21.95 | 2,387 |
|Restaurant | 3.15 | 14.69 | 1,786 |
|SportsEvent | 4.00 | 14.28 | 135 |
|Mixed | 2.26 | 14.42 |7,398|

Note that while
there is only one root entity, most pages describe more than one entity
(e.g., for music recordings, there may also be entities for the artist and
the record label).
In addition, the cleaning of the HTML pages might remove information (e.g. within <meta>-tags) 
which originally could be used to gather structured information from. Information, annotated 
in such a way would not be included in HTML pages which do not use schema.org as markup, as they 
are not visible to the visitor of the page at all. We there remove those information and therefore 
a recall of 1.0 might not be possible in such cases.

###Evaluation

For each of the six datasets, recall, precision,
and F-measure (of triples in the gold standard) are computed,
with the macro average F-measure across all six datasets being used
to determine the best-performing solution.
As stated above, entities are represented by blank nodes, therefore,
blank node identifiers are not taken into account when computing
the triple overlap. Furthermore, when comparing literals, those are
trimmed before (i.e., leading and trailing whitespace is removed).
Moreover:
- For the evaluation of literals, we will trim the literals (remove spaces), meaning " University of   Mannheim" is equal to "University of Mannheim". In addition language tags will not be considered, meaning "Hello"@en-us is equal to "Hello"@it and "Hello"
- For the evaluation of URLs (as there is a BUG within Any23) we apply the following strategy. In case the URL is absolute in the HTML, we expect an absolute URL also in the quads. If the URL is relative, we expect a relative URL in the quads. As ANY23 has in some cases problems extracting the relative URL we normalize those according to the strategy above.
- We will apply a de-duplication step before the evaluation, as in some cases data providers maintain the same information (e.g. the price or the name of a product) multiple times within the page. This means the following quads:
```
_:1    schema.org/url    google.com    yahoo.com
_:1    schema.org/url    google.com    yahoo.com
```
will be de-duplicated to:
```
_:1    schema.org/url    google.com    yahoo.com 
```

# OKE challenge at ESWC2016

On **Tuesday 31st of May 2016** there will be a full day Challenge track running in parallel with other tracks.
The OKE challenge (unless possible last minutes adjusting of the schedule) will run from 11.00 to 11.45.
The provisional program is the following.

- 11.00 to 11.05 - OKE challenge overview. Description of task 1 and 2
- 11.05 to 11.15 - Task 1: Julien Plu, Giuseppe Rizzo and Raphaël Troncy. Enhancing Entity Linking by Combining Models.
- 11.15 to 11.25 - Task 1: Mohamed Chabchoub, Michel Gagnon and Amal Zouaq. Collective disambiguation and Semantic Annotation for Entity Linking and Typing
- 11.25 to 11.35 - Task 2: Stefano Faralli and Simone Paolo Ponzetto. Open Knowledge Extraction Challenge (2016) a Hearst-like Pattern-Based approach to Hypernym Extraction and Class Induction
- 11.35 to 11.45 - Task 2: Lara Haidar-Ahmad, Ludovic Font, Amal Zouaq and Michel Gagnon. Entity Typing and Linking using SPARQL Patterns and DBpedia   

Challenge papers will also have a slot during the Poster and Demo session on Wednesday.


Results
=========

Participating systems have been evaluated on the **Evaluation data** available in folder [evaluation-data](./evaluation-data)

For Task 1 the participants are:

- Mohamed Chabchoub, Michel Gagnon and Amal Zouaq. [Collective disambiguation and Semantic Annotation for Entity Linking and Typing](./participating systems/paper_3.pdf)
- Julien Plu, Giuseppe Rizzo and Raphaël Troncy. [Enhancing Entity Linking by Combining NER Models](./participating systems/paper_4.pdf)


For Task 2 the participants are:

- Stefano Faralli and Simone Paolo Ponzetto. [DWS at the 2016 Open Knowledge Extraction Challenge: A Hearst-like Pattern-Based Approach to Hypernym Extraction and Class Induction]	(./participating systems/paper_1.pdf)
- Lara Haidar-Ahmad, Ludovic Font, Amal Zouaq and Michel Gagnon. [Entity Typing and Linking using SPARQL Patterns and Dbpedia] (./participating systems/paper_2.pdf)


**Task 1**

| Annotator	 | 	 	sub-task	 | 	Micro F1	 | 	Micro Precision	 | 	Micro Recall	 | 	Macro F1	 | 	Macro Precision	 | 	Macro Recall| 
|-----------|----------|-----------------|--------------|----------|-----------------|--------------|--------------|--------------|
| WestLab	 | 	 	Entity Recognition	 | 	0.7738	 | 	0.7403	 | 	0.8105	 | 	0.7627	 | 	0.7571	 | 	0.8088| 
| WestLab	 | 	 	Entity Typing	 | 	0.6283	 | 	0.6307	 | 	0.6258	 | 	0.611	 | 	0.6129	 | 	0.6093| 
| WestLab	 | 	 	D2KB	 | 	0.6008	 | 	0.7182	 | 	0.5163	 | 	0.5811	 | 	0.7007	 | 	0.5137| 
| WestLab	 | 	 	Global	 | 	0.6676	 | 	0.6964	 | 	0.6509	 | 	0.6516	 | 	0.6902	 | 	0.6439| 
| ADEL	 | 	 	Entity Recognition	 | 	0.8073	 | 	0.8209	 | 	0.7941	 | 	0.792	 | 	0.8277	 | 	0.789| 
| ADEL	 | 	 	Entity Typing	 | 	0.6038	 | 	0.6046	 | 	0.6029	 | 	0.5806	 | 	0.5813	 | 	0.58|
| ADEL	 | 	 	D2KB	 | 	0.4637	 | 	0.5813	 | 	0.3856	 | 	0.4465	 | 	0.5729	 | 	0.3849| 
| ADEL	 | 	 	Global	 | 	0.6249	 | 	0.6689	 | 	0.5942	 | 	0.6064	 | 	0.6606	 | 	0.5846| 


**Task 2**

| Annotator	 | 	sub-task	 | 	Micro F1	 | 	Micro Precision	 | 	Micro Recall	 | 	Macro F1	 | 	Macro Precision	 | 	Macro Recall| 
|-----------|----------|-----------------|--------------|----------|-----------------|--------------|--------------|--------------|
| Mannheim	 | 	 	Entity Recognition	 | 	0.7234	 | 	0.7727	 | 	0.68	 | 	0.6467	 | 	0.63	 | 	0.68| 
| Mannheim	 | 	 	Entity Typing	 | 	0	 | 	0	 | 	0	 | 	0	 | 	0	 | 	0| 
| Mannheim	 | 	 	Global	 | 	0.3617	 | 	0.3864	 | 	0.34	 | 	0.3233	 | 	0.315	 | 	0.34| 
| WestLab	 | 	 	Entity Recognition	 | 	0.86	 | 	0.86	 | 	0.86	 | 	0.86	 | 	0.86	 | 	0.86| 
| WestLab	 | 	 	Entity Typing	 | 	0.0727	 | 	0.08	 | 	0.0667	 | 	0.07	 | 	0.08	 | 	0.0667| 
| WestLab	 | 	 	Global	 | 	0.4664	 | 	0.47	 | 	0.4633	 | 	0.465	 | 	0.47	 | 	0.4633| 
| *(BASELINE from OKE2015) Cetus*	 | 	Entity Recognition	 | 	0.7719	 | 	0.6875	 | 	0.88	 | 	0.7733	 | 	0.72	 | 	0.88|
| *(BASELINE from OKE2015) Cetus*	 | 	Entity Typing	 | 	0.2326	 | 	0.2217	 | 	0.2447	 | 	0.1989	 | 	0.2217	 | 	0.2447| 
| *(BASELINE from OKE2015) Cetus*	 | 	Global	 | 	0.5023	 | 	0.4546	 | 	0.5624	 | 	0.4861	 | 	0.4708	 | 	0.5624| 



*Powered by*

![GERBIL](./img/gerbil.png)

Winners
=========

**Task 1**
- Mohamed Chabchoub, Michel Gagnon and Amal Zouaq. [Collective disambiguation and Semantic Annotation for Entity Linking and Typing](./participating systems/paper_3.pdf)

**Task 2**
- Lara Haidar-Ahmad, Ludovic Font, Amal Zouaq and Michel Gagnon. [Entity Typing and Linking using SPARQL Patterns and Dbpedia] (./participating systems/paper_2.pdf)
 
 
# Organising Committee

- [Andrea Giovanni Nuzzolese](http://www.cs.unibo.it/~nuzzoles/), STLab-CNR, Italy
- [Anna Lisa Gentile](http://dws.informatik.uni-mannheim.de/en/people/researchers/annalisa/), University of Mannheim, Germany
- [Valentina Presutti](http://stlab.istc.cnr.it/stlab/User:ValentinaPresutti), STLab-CNR, Italy
- [Robert Meusel](http://dws.informatik.uni-mannheim.de/en/people/researchers/robert-meusel/), University of Mannheim, Germany
- [Heiko Paulheim](http://dws.informatik.uni-mannheim.de/en/people/professors/dr-heiko-paulheim/), University of Mannheim, Germany
- [Aldo Gangemi](http://www.istc.cnr.it/people/aldo-gangemi), Université Paris 13, France


# Program Committee

- [Luigi Asprino](http://www.istc.cnr.it/people/luigi-asprino), STLab-CNR, Italy
- [Davide Buscaldi](https://sites.google.com/site/davidebuscaldi/home), Université Paris 13, France
- [Sergio Consoli](https://www.researchgate.net/profile/Sergio_Consoli), Philips Research, The Netherlands
- [Mauro Dragoni](http://shell.fbk.eu/people/profile/dragoni), Fondazione Bruno Kessler, Trento, Italy
- [Oliver Lehmberg](http://dws.informatik.uni-mannheim.de/en/people/researchers/oliverlehmberg/), University of Mannheim, Germany
- [Adeline Nazarenko](https://lipn.univ-paris13.fr/fr/component/content/article?id=3064&nom=Nazarenko), Université Paris 13, France
- [Silvio Peroni](http://www.essepuntato.it/), Univeristy of Bologna, Italy
- [Francesco Poggi](https://www.unibo.it/sitoweb/francesco.poggi5/en), Univeristy of Bologna, Italy
- [Diego Reforgiato](http://www.istc.cnr.it/people/diego-reforgiato-recupero), STLab-CNR, Italy
- [Dominique Ritze](http://dws.informatik.uni-mannheim.de/en/people/researchers/dominiqueritze/), University of Mannheim, Germany
- [Sanja Štajner](http://dws.informatik.uni-mannheim.de/en/people/researchers/dr-sanja-stajner/), University of Mannheim, Germany
- [Alessandro Russo](http://www.istc.cnr.it/people/alessandro-russo), STLab-CNR, Italy
- [Ziqi Zhang](http://staffwww.dcs.shef.ac.uk/people/Z.Zhang/), University of Sheffield, UK

