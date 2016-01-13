# ESWC-16 Open Knowledge Extraction (OKE) Challenge

This folder contains guidelines and materials for the Open Knowledge Extraction challenge at ESWC2016.


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

- Challenge **papers** submission deadline: Friday March 11th, 2016
- Challenge paper **reviews due**: Tuesday April 5th, 2016
- **Notifications** sent to participants: Friday April 8th, 2016
- **Test data** published: Friday April 8th, 2016
- **Camera ready** papers due: Sunday April 24th, 2016​


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

Entities also include anaphorically related discourse referents. Hence, anaphora resolution has to be take into account for addressing the task.

As an example, for the sentence: 

> Florence May Harding studied at a school in Sydney, and with Douglas Robert Dundas , but in effect had no formal training in either botany or art.	

we want the system to recognize four entities:

| Recognized Entity    | generated URI | Type     | SameAs|
| ------------- |:-------------|:-------------:| -----:|
| Florence May Harding      |oke:Florence_May_Harding| dul:Person | dbpedia:Florence_May_Harding |
| school      | oke:School|dul:Organization    |    |
| Sydney | oke:Sydney| dul:Place      |    dbpedia:Sydney  |
| Douglas Robert Dundas |oke:Douglas_Robert_Dundas| dul:Person      |      |

The results must be provided in [NIF](http://persistence.uni-leipzig.org/nlp2rdf/) format, including the offsets of recognized entities. The expected output for the example sentence can be found in [task1.ttl](./example_data/task1.ttl).

In the above example we use

```
@prefix oke: <http://www.ontologydesignpatterns.org/data/oke-challenge/task-1/>
```


We will evaluate three aspects on this task, independently:

- Ability to recognize entities: we will check if all strings recognizing entities are identified, using the offsets returned by the systems. **Only full matches are counted as correct** (e.g. if the system returns "Art School" instead of "National Art School" is counted as a miss).
- Ability to **assign the correct type**: evaluation will be carried out only on the 4 target DOLCE types.
- Ability to **link individuals to DBpedia 2014**: participants must **link** entities to DBpedia **only when relevant** (in the example sentence, the referred Douglas Robert Dundas is not present in DBpedia)

We will calculate Precision, recall and F1 for the three subtasks and the winner for task 1 will be the system with higher average F1 for all three.

##Task 2

*Class Induction and entity typing for Vocabulary and Knowledge Base enrichment.*

This task consists in producing rdf:type statements, given definition texts. The participants will be given a dataset of sentences, each defining an entity (known a priori), e.g. the entity: dpedia:Skara_Cathedral and its definition "Skara Cathedral is a church in the Swedish city of Skara".

Participants are expected to (i) identify the type(s) of the given entity as they are expressed in the given definition, (ii) create a owl:Class statement for defining each of them as a new class in the target knowledge base, (iii) create a rdf:type statement between the given entity and the new created classes, and (iv) align the identified types, if a correct alignment is available, to a set of given types.

In the task we will evaluate the extraction of all strings describing a type and the alignment to any of the subset of [DOLCE+DnS Ultra Lite classes](http://ontologydesignpatterns.org/ont/wikipedia/d0.owl)


As an example, for the sentence: 

> Brian Banner is a fictional villain from the Marvel Comics Universe created by Bill Mantlo and Mike Mignola and first appearing in print in late 1985..

*Brian Banner* will be given as the *input target entity*. We want the system to recognize any possible type for it. Correct answers include:


| Recognized string for the type    | Generated Type     | rsubClassOf|
| ------------- |:-------------| -----:|
| fictional villain      |oke:FictionalVillain| dul:Personification | 
| villain      | oke:Villain|dul:Person    |  

The results must be provided in [NIF](http://persistence.uni-leipzig.org/nlp2rdf/) format, including the offsets of recognized string describing the type. The expected output for the example sentence can be found in [task2.ttl](./example_data/task2.ttl).

In the above example we use

```
@prefix oke: <http://www.ontologydesignpatterns.org/data/oke-challenge/task-2/>
```


We will evaluate two aspects on this task, independently:

- Ability to **recognize strings that describe the type of a target entity**. As string describing types often include adjectives as modifiers (in the example above, "fictional" is a modifier for villain), in the Gold Standard we will include all possible options; the system answer will be counted **correct as long as at least one of the possibility is returned**.
- Ability to align the identified type with a reference ontology, which for this evaluation will be the subset of [DOLCE+DnS Ultra Lite classes](http://ontologydesignpatterns.org/ont/wikipedia/d0.owl). 

We will calculate Precision, recall and F1 for the two subtasks and the winner for task 2 will be the system with higher average F1 for the two of them.


##Task 3



# Organising Committee

- [Andrea Giovanni Nuzzolese](http://www.cs.unibo.it/~nuzzoles/), STLab-CNR, Italy
- [Valentina Presutti](http://stlab.istc.cnr.it/stlab/User:ValentinaPresutti), STLab-CNR, Italy
- [Anna Lisa Gentile](http://dws.informatik.uni-mannheim.de/en/people/researchers/annalisa/), University of Mannheim, Germany
- [Robert Meusel](http://dws.informatik.uni-mannheim.de/en/people/researchers/robert-meusel/), University of Mannheim, Germany
- [Heiko paulheim](http://dws.informatik.uni-mannheim.de/en/people/professors/dr-heiko-paulheim/), University of Mannheim, Germany
- [Aldo Gangemi](http://www.istc.cnr.it/people/aldo-gangemi), Université Paris 13, France


# Program Committee

- [Davide Buscaldi](https://sites.google.com/site/davidebuscaldi/home), Université Paris 13, France
- [Mauro Dragoni](http://shell.fbk.eu/people/profile/dragoni), Fondazione Bruno Kessler, Trento, Italy
- [Adeline Nazarenko](https://lipn.univ-paris13.fr/fr/component/content/article?id=3064&nom=Nazarenko), Université Paris 13, France
- [Silvio Peroni](http://www.essepuntato.it/), Univeristy of Bologna, Italy
- [Francesco Poggi](https://www.unibo.it/sitoweb/francesco.poggi5/en), Univeristy of Bologna, Italy
- [Diego Reforgiato](http://www.istc.cnr.it/people/diego-reforgiato-recupero), STLab-CNR, Italy


