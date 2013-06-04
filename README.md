Berlin Buzzwords 2013 - linguistics-demo
========================================

Demo examples for linguistics in Lucene, Solr, ElasticSearch and OpenNLP.

The demo consists of the following modules:

- lucene-analyzer-example
- opennlp-example
- elasticsearch-multilang-example
- solr-multilang-example

Each example demo can be run as described below.


lucene-analyzer-example
-----------------------

The Lucene analyzer example consists of two demos, AnalyzerExampleTest
and FrenchSynonymExampleTest.

Run both demos with mvn test.

  $ cd lucene-analyzer-example

  $ mvn test

The demos can be run individually as well. For example:

  $ mvn -Dtest=AnalyzerExampleTest test


opennlp-example
---------------

The OpenNLP example consists of examples demonstrating sentence
segmentation, tokenization, person name extraction as well as
part-of-speech tagging.

Execute the following commands to run the examples.

  $ cd opennlp-example

  $ mvn -Dget-models test


elasticsearch-multilang-example
-------------------------------

The ElasticSearch multilangauge example demonstrates how to do basic
multilanguage analysis with ElasticSearch

Download and unpack ElasticSearch (We are using 0.90.1 in this example)

  $ cd elasticsearch-multilang-example

  $ tar zxvf elasticsearch-0.90.1.tar.gz

Start up Elastic Search

  $ ./elasticsearch-0.90.1/bin/elasticsearch -f

Install Kuromoji plugin

  $ ./elasticsearch-0.90.1/bin/plugin -install elasticsearch/elasticsearch-analysis-kuromoji/1.4.0

Create index with mappings

  $ curl -XPUT 'localhost:9200/wiki' -d @mappings.json

Analyze French and Japanese

  $ curl -XGET 'http://localhost:9200/wiki/_analyze?analyzer=french&pretty' -d "Le champagne est un vin pétillant français protégé appelation d'origine contrôlée."

  $ curl -XGET 'http://localhost:9200/wiki/_analyze?analyzer=japanese&pretty' -d 'ＪＲ新宿駅の近くにビールを飲みに行こうか？'

Post documents

  $ curl -XPOST 'http://localhost:9200/wiki/article' -d @test_en.json

  $ curl -XPOST 'http://localhost:9200/wiki/article' -d @test_de.json

  $ curl -XPOST 'http://localhost:9200/wiki/article' -d @test_fr.json

  $ curl -XPOST 'http://localhost:9200/wiki/article' -d @test_ar.json

  $ curl -XPOST 'http://localhost:9200/wiki/article' -d @test_ja.json


Search for Shinjuku

  $ curl -XGET 'http://localhost:9200/wiki/article/_search?pretty' -d '{ "query" : { "match" : { "body" : { "query" : "新宿", "analyzer" : "japanese" } } } }' 



solr-multilang-example
----------------------

The Solr multilanguage example demonstrates how language can be
detected automatically based on content in fields title and body of
Wikipedia documents.

Download and unpack Solr (we are using 4.3.0 in this example)

  $ cd solr-multilang-example

  $ tar zxvf solr-4.3.0.tgz

Copy the demo schema.xml and solrconfig.xml to Solr's example config
as follows

  $ cp cp conf/schema.xml \
       conf/solrconfig.xml \
       solr-4.3.0/example/solr/collection1/conf/

Start up Solr

  $ cd solr-4.3.0/example

  $ java -jar start.jar

In a different directory, post the Wikipedia documents

  $ ./posh.sh

The below query gives an overview of the documents now searchable from
the various Wikipedia language editions

  $ curl 'http://localhost:8983/solr/collection1/select?q=*%3A*&rows=0&wt=xml&indent=true&facet=true&facet.field=wiki

The below query gives the distribution of languages detected

  $ curl 'http://localhost:8983/solr/collection1/select?q=*%3A*&rows=0&wt=xml&indent=true&facet=true&facet.field=languages

The below query gives the distribution of languages detected
in the Japanese Wikipedia

  $ curl 'http://localhost:8983/solr/collection1/select?q=wiki%3Ajawiki&rows=0&wt=xml&indent=true&facet=true&facet.field=language'


Contact us
----------

Contact us on hello@atilika.com if you have questions or problems.
