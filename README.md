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

Participants must:

1. Submit a paper describing their system, via [EasyChair](https://easychair.org/conferences/?conf=oke2015), no later then **March 11th, 2016**. The paper should contain the details of the system, including why the system is innovative, how it uses Semantic Web, which features or functions the system provides, what design choices were made and what lessons were learned. The description should also summarise how participants have addressed the evaluation task(s). Papers must be submitted in PDF format, following the style of the Springer's Lecture Notes in Computer Science (LNCS) series (http://www.springer.com/computer/lncs/lncs+authors), and not exceeding 12 pages in length. 
2. For task 1 and 2 provide access to the application as webservice, with input/output provided in [NIF](http://persistence.uni-leipzig.org/nlp2rdf/) format. The final evaluation  will be carried out via [GERBIL](http://gerbil.aksw.org). The implementation of the evaluation for tasks 1-2 is already available on GERBIL (also accessible as open source code) as [Web demo](http://gerbil.aksw.org/gerbil/config). Participants can autonomously test their system using GERBIL (selecting as Experiment Type either OKE Challenge 2015 - Task 1 or OKE Challenge 2015 - Task 2).
For task 3 participants will have to produce results in the specified format.
3. The URI for the final system (for task1-2) / results on test set (for task 3)  must be provided by **April 8th, 2016** when the organizers will evaluate the systems against the **evaluation dataset**, which will be publicly released after announcement of results.


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

In the task we will evaluate the extraction of all strings describing a type and the alignment to any of the subset of [DOLCE+DnS Ultra Lite classes](http://ontologydesignpatterns.org/ont/wikipedia/d0.owl)


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
training corpus) with structured annotations.
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
- [Ziqi Zhang](http://staffwww.dcs.shef.ac.uk/people/Z.Zhang/), University of Sheffield, UK

