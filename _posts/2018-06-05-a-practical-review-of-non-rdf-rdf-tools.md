---
title: A practical review of non-RDF to RDF converters
layout: post
date: 2018-04-28 14:32:39 +0000
---

I believe, converting bunch of CSV, JSON or XML files to an RDF format is a very popular task for people dealing with Semantic Web, Linked Data, RDF and such stuff. However, I can't easily name a ready-to-use converter that I'd recommend, so I've decided to review some  of them to use the most suitable one next time.

---

## Tools

I found more than a dozen of converters, but included in the review only those which:

* are alive and well maintained,
* I've managed to install and use.

In the result we have six tools. I also classified them based on the functions they can perform: **e**xtract, **t**ransform, **l**oad.

| # | Name & Link      | Functions | Version | License |
| - | ---------------- | --------- | ------- | ------- |
| 1 | [Tarql](http://tarql.github.io/) | T | [master@f616414](https://github.com/tarql/tarql/commit/f6164142ead91fb66c60df7ddd6dd35954f70f35) | [BSD 2-Clause](https://github.com/tarql/tarql/blob/master/LICENSE) |
| 2 | [RML-Mapper](http://rml.io/)     | T | [master@cdd2464](https://github.com/RMLio/RML-Mapper/commit/cdd24647afb0b2fec7790e03bf2a60156315764b) | No |
| 3 | [SPARQL-Generate](https://github.com/thesmartenergy/sparql-generate) | T | [1.2.2](https://github.com/sparql-generate/sparql-generate/releases/tag/1.2.2) | [Apache License 2.0](https://github.com/thesmartenergy/sparql-generate/blob/master/LICENSE) |
| 4 | [SETLr](https://github.com/tetherless-world/setlr) | TL | [master@873f8da](https://github.com/tetherless-world/setlr/commit/873f8dab7a8bf64cc73f0425a51eab86495cd420) | [Apache License 2.0](https://github.com/tetherless-world/setlr/blob/master/LICENSE) |
| 5 | [Karma](http://usc-isi-i2.github.io/karma/) | TL | [2.1](https://github.com/usc-isi-i2/Web-Karma/releases/tag/v2.1) | [Apache License 2.0](https://github.com/usc-isi-i2/Web-Karma/blob/master/LICENSE.txt)|
| 6 | [Linked Pipes ETL](https://etl.linkedpipes.com/) | ETL | [master@d446615a](https://github.com/linkedpipes/etl/commit/d446615ac4036944bebbaa945eb40669194823ff) | [MIT License](https://github.com/linkedpipes/etl/blob/master/LICENSE) |

Those that support only the transform function may be useful for occasional runs or as a part of a bigger pipeline. The others may be used to build the whole pipeline.

---

## The review

### Feature comparison

I've looked at seven features I find important to know about when selecting such tool. The first three ones are:

* **Processing mode** - Input file is loaded in the memory and processed as one piece (*batch*). It's split in parts and each part loaded and processed seperatelly (*stream*). Or each file as whole or splitted in parts is processed on a cluster (*cluster*).
* **Input formats** - In this review I'm considering only file-based formats, so you won't see the RDB-to-RDF and such stuff.
* **Sources** - Where input file may be loaded from, from a local path, by URL, etc.

Here is the table with information about these three features:

| # |       Name      | Processing mode | Input formats | Sources |
| - | --------------- | --------------- | ------------- | ------- |
| 1 |       Tarql     | Batch, Stream | CSV | Local |
| 2 |    RML-Mapper   | Batch | CSV, JSON, XML, CSS | Local, URL |
| 3 | SPARQL-Generate | Batch,<br/> Stream *(CSV only)* | CSV, JSON, XML, HTML, [CBOR](http://cbor.io/), plain text | Local, URL |
| 4 |       SETLr     | Batch | CSV, Excel | Local |
| 5 |       Karma     | Batch,<br/> Cluster *(MapReduce)* | CSV, JSON, XML, Excel | Local, URL |
| 6 |   LinkedPipes   | Batch | CSV, JSON, XML, Excel, HTML | Local, URL |

Most of the popular file formats are supported, except the tools specialised for the column-based formats. Sad to say, but the stream-based processing isn't well supported, so you'll have to control the file sizes.

Other four features are:

* **Mapping language** - a declarative language which describes extraction, transformation and loading (aka ETL steps).
* **Graphical editor** - an UI which allows to define the ETL steps visually.
* **Joins** - a mechanism to define a join function between two or more input files and run the transformation on top of resulted data.
* **Custom functions** - an ability to implement a function which aren't built-in into the standard distribution of the tool. E.g. Jena ARQ engine allows to implement a custom function for SPARQL as a Java class and include it in the distribution.

| # |       Name      | Mapping language | Graphical editor | Joins | Custom functions |
| - | --------------- | ---------------- | ---------------- | ----- | ---------------- |
| 1 |       Tarql     | SPARQL | No | No | Yes ([Jena ARQ](https://jena.apache.org/documentation/query/)) |
| 2 |    RML-Mapper   | [R2RML](https://www.w3.org/TR/r2rml/)<br/> with extensions | Non public | Yes | Yes ([FnO](https://github.com/fnoio)) |
| 4 | SPARQL-Generate | SPARQL<br/> with extensions | No | Yes | Yes ([Jena ARQ](https://jena.apache.org/documentation/query/)) |
| 2 |       SETLr     | Turtle<br/> + template language | No | No | No |
| 5 |       Karma     | No | Yes | No | Yes ([Python](https://github.com/usc-isi-i2/Web-Karma/wiki/Transforming-Data#transformations-using-python)) |
| 6 |   LinkedPipes   | No<br/> (depends on component) | Yes | No | No |

So the ones that don't have a graphical editor support a declarative mapping language, the rest provide an UI instead. The joining is a powerful mechanism which is especially useful when it's not possible to create stable URIs for entities that bind data from different input files. Unfortunatelly only two tools support it, RML-Mapper and SPARQL-Generate.

### Communities comparison

Communities are compared based on the activity on Github, since all the source codes are published on it. Meaning of the columns should be clear for everyone, so I skip their description :)

| # |       Name      | Contributors | Forks | Stars | Releases | Last commit |
| - | --------------- | ---------------- | ---------------- | ----- | ---------------- |
| 1 |       Tarql     | 5 | 34 | 90 | 3 | [03.10.2017](https://github.com/tarql/tarql/commits/master) |
| 2 |    RML-Mapper   | 7 | 26 | 18 | 6 | [04.05.2018](https://github.com/RMLio/RML-Mapper/commits/master) |
| 4 | SPARQL-Generate | 3 | 3 | 6 | 5 | [10.04.2018](https://github.com/thesmartenergy/sparql-generate/commits/master) |
| 2 |       SETLr     | 2 | 4 | 7 | 1 | [19.04.2018](https://github.com/tetherless-world/setlr/commits/master) |
| 5 |       Karma     | 34 | 138 | 310 | 25|  [19.04.2018](https://github.com/usc-isi-i2/Web-Karma/commits/master) |
| 6 |   LinkedPipes   | 5 | 8 | 30 | 0 | [03.05.2018](https://github.com/linkedpipes/etl/commits/master) |

*(Updated on 25.05.2018)*

As you can see Karma stands out among others, it had much more contributors, forks, starts, etc. than for other tools all together. These numbers may partially be expained with that it's the oldest tool among the reviewed ones. The first commit to the repo dates to [Sep 23 2011](https://github.com/usc-isi-i2/Web-Karma/commit/ca876a532cda07dc6a106a687e2f52428cc98357).

### Performance comparison

For this part I took a dataset published by [ClearSpending.ru](https://clearspending.ru/opendata/) that contains public contracts from [the Russian Tender System](http://zakupki.gov.ru/). From all the archives I've selected one with contracts published in March 2018, it contains a single big JSON file (~560MB). To make the comparison more complete, I converted the original JSON file to CSV and XML formats, and for each format files with 100, 500, 1000, 5000, 10000 and 100000 contracts were generated.

To converted JSON to XML I've adapted the [json2xml](https://github.com/lukas-krecan/json2xml) library. It's shipped without a ready-to-use `.jar` file and doesn't support the Cyrillic encoding, but it was reletevaly easy to extend with the needed functionality.

The [json2csv](https://github.com/evidens/json2csv) converter were used to produce CSV files. Fields with large arrays were dropped from the original JSON before converting it in CSV, otherwise the CSV may contain hundreds of thousands of columns, since the json2csv tool maps each array element to a seperate column.

Here are the file sizes for each format:

| **# of contracts** | **Files sizes** |
| | **JSON** | **CSV** | **XML** |
| 100 | 556 KB | 303 KB | 653 KB |
| 500 | 2.4 MB | 1.4 MB | 2.9 MB |
| 1000 | 4.5 MB | 2.6 MB | 5.4 MB |
| 5000 | 21 MB | 13 MB | 26 MB |
| 10000 | 40 MB | 24 MB | 49 MB |
| 100000 | 412 MB | 214 MB  | 497 MB |

From all the data only five fields were taken to generate RDF data: `id`, `number`, `contractCreateDate`, `customer.OGRN`, `suppliers.[0].ogrn`. Example of these fields in the JSON files (the rest of the fields aren't shown):

```
[{
	"contractCreateDate":"2018-03-06",
	"customer":{ "OGRN":"1057746394155" },
	"id":"e5b9beda-742a-45c5-a5f7-08fb9166ab31",
	"number":"31200010721-03",
	"suppliers":[{ "ogrn":"5087746669225" }]
}]
```

And the outputted RDF data look like that:

```
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix ex: <http://example.com/> .

<http://example.com/contract/e5b9beda-742a-45c5-a5f7-08fb9166ab31> a ex:Contract ;
	ex:number “31200010721-03” ;
	ex:created “2018-03-06”^^xsd:date ;
	ex:customer <http://example.com/org/1057746394155> ;
	ex:supplier <http://example.com/org/5087746669225> .
```

In the table below you can find links to the mapping files for each tool:

| # |       Name      | JSON | CSV | XML |
| - | --------------- | ---- | --- | --- |
| 1 |       Tarql     | - | [contract.query](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/tarql/contract.query) | - |
| 2 |    RML-Mapper   | [contract.json.rml.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/rml-mapper/contract.json.rml.ttl) | [contract.csv.rml.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/rml-mapper/contract.csv.rml.ttl) | [contract.xml.rml.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/rml-mapper/contract.xml.rml.ttl) |
| 4 | SPARQL-Generate | [contract.json.query](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/sparql-generate/contract.json.query) | [contract.csv.query](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/sparql-generate/contract.csv.query) | [contract.xml.query](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/sparql-generate/contract.xml.query) |
| 2 |       SETLr     | - | [contract.setl.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/setlr/contract.setl.ttl) | - |
| 5 |       Karma     | [model.json.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/karma/model.json.ttl) | [model.csv.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/karma/model.csv.ttl) | [model.xml.ttl](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/karma/model.xml.ttl) |
| 6 |   LinkedPipes   | [contract.json.trig](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/linkedpipes/contract.json.trig) | [contract.csv.trig](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/linkedpipes/contract.csv.trig) | [contract.xml.trig](https://github.com/KMax/non-rdf-to-rdf-tools-review/blob/master/linkedpipes/contract.xml.trig) |

When the data and mappings were ready, the tools were run sequentially on a machine (in Google Cloud) with the following parameters:

* 2 vCPUs
* 7.5 GB memory

* OpendJDK 1.8.0_171, JVM Options: -Xmx6G
* Python 2.7


The following charts present the results of the performance comparation. If there is no result on a chart for a particular tool then it means that either this tool doesn't support the format or it crashed because of the out-of-memory error. The execution times were measured with the [time](https://en.wikipedia.org/wiki/Time_(Unix)) tool.

<iframe width="703" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRqVjJct_08UgLBeIs_B3-xiZWQ-5zdsziQdBSjQNHpZ-7SUH8H69qO3l4HAz5qu1A_OlYYvtPoNftT/pubchart?oid=409864852&amp;format=interactive"></iframe>

<iframe width="704" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRqVjJct_08UgLBeIs_B3-xiZWQ-5zdsziQdBSjQNHpZ-7SUH8H69qO3l4HAz5qu1A_OlYYvtPoNftT/pubchart?oid=18459260&amp;format=interactive"></iframe>

<iframe width="704" height="373" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRqVjJct_08UgLBeIs_B3-xiZWQ-5zdsziQdBSjQNHpZ-7SUH8H69qO3l4HAz5qu1A_OlYYvtPoNftT/pubchart?oid=1016046163&amp;format=interactive"></iframe>

So what do we see on these charts? Let's first look at the tools which support only CSV format.

* **Tarql** performed very good, it succesfully converted all the CSV files and it was the fastest one with the large files.
* **SETLr** wasn't able to convert the largest CSV file, because it failed with the out-of-memory error, but in other cases it had similar to Tarql results.

And now the tools which support all three formats:

* **RML-Mapper** has the worst results, except only the smallest files where it was the penultimate with all three formats. But it's the only tool which was able to complete process all the files.
* **SPARQL-Generate** was the fastest one with the JSON files, but has relatively average results with other formats and even failed to process the largest CSV and XML files.
* **Karma**, quite similar to SPARQL-Generate, it has average results and wasn't able to complete processing of the largest files.
* **LinkedPipes** was the fastest one with the CSV and XML files, but has one of the worse results with the JSON files.

To summarise, I'd say that LinkedPipes is the fastest tool in terms of the execution time, if it'd not fail to process the largest JSON file and wouldn't have quite worse results with JSON in general.

---

## Which one of them is the best?

Ahh, when I just started working on this review, I naively though that I'll definitely find the best tool for my tasks...But unfortunately I've not found such tool :(. Each of them has it's own limitations and features which I don't really like. Anyway let me summarise my thoughts about each of the tools.

As you may guess I'm skipping Tarql and SETLr here, because their only support column-based formats and can't be adapted to work with other formats. But if I'd work only with CSV, then I'd use Tarql, because of it's performance and usage of SPARQL as a mapping language.

Karma is quite similar to [OpenRefine](http://openrefine.org) in terms of UI and usage patterns. It has performance problems, but looks like it support the MapReduce approach, so they may be mitigated. The main limitations for me is that it couldn't easily integrate with a VCS and the mappings can't be written without the UI.

RML-Mapper has bad performance results, so it's not suitable for real-world datasets, although it's the only tool which successfully complete processing of all the files.

LinkedPipes and SPARQL-Generate aren't comparable with each other, because the first one is an ETL tool, but the last one can only transforms data, but can't extract or load it in/from sources. 

Although I've said above that I couldn't identify a clear winner, but as a result of this review, I found that I'd like to try SPARQL-Generate as my next non-RDF-to-RDF converter. There are several reasons for that: 

* its mapping language is based on SPARQL, so it's easy to get started;
* the mappings can't be edited in a text editor;
* it has an extandable architecture, so I can implement, e.g. the streaming support and etc., myself;
* it can be embedded in a more complex pipelines, such as built on top of [Apache Beam](http://beam.apache.org/) and others.

