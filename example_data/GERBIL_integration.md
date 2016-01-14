# GERBIL integration for the evaluation

The following is an example of a cURL POST request that GERBIL performs to an annotation system.<br>
The URL http://system_end_point should be replaced with the actual publicly accessible URL of the system.<br>
The NIF-compliant turtle provided as input contains the sentence to be annotated.

```
curl -H "Content-Type:application/x-turtle" -H "Accept:application/x-turtle" 
  -d "@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
      @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
      @prefix owl: <http://www.w3.org/2002/07/owl#> .
      @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
      @prefix nif: <http://persistence.uni-leipzig.org/nlp2rdf/ontologies/nif-core#> .
      @prefix d0: <http://ontologydesignpatterns.org/ont/wikipedia/d0.owl#> .
      @prefix dul: <http://www.ontologydesignpatterns.org/ont/dul/DUL.owl#> .
      @prefix oke: <http://www.ontologydesignpatterns.org/data/oke-challenge/task-1/> .
      @prefix dbpedia: <http://dbpedia.org/resource/> .
      @prefix itsrdf: <http://www.w3.org/2005/11/its/rdf#> .

      <http://www.ontologydesignpatterns.org/data/oke-challenge/task-1/sentence-1#char=0,146>
              a                     nif:RFC5147String , nif:String , nif:Context ;
              nif:beginIndex        \"0\"^^xsd:nonNegativeInteger ;
              nif:endIndex          \"146\"^^xsd:nonNegativeInteger ;
              nif:isString          \"Florence May Harding studied at a school in Sydney, and with Douglas Robert Dundas , but in effect had no formal training in either botany or art.\"@en ." 
          
  http://system_end_point
```
  
An annotation system should produce a valid NIF-compliant turtle as output. Such a turtle contains the annotations produced by 
an annotation system, cf. https://github.com/anuzzolese/oke-challenge/blob/master/example_data/task1.ttl for details.

So the participants to the challenge should take care that their systems:
* expose REST interface compliant with the cURL example above;
* produce NIF-compliant outputs for their annotations.
