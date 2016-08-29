Origin
==========
This crawler is a modified vresion of Google Scholar crawler acceisble here: https://github.com/ckreibich/scholar.py
The original source code is also copied in this repository under name scholar-original.py
<br>
Our crawling procedure:
==========

The publications list is imported from sample.xlsx file. For every publication is searched on Google Scholar to find either exact match in title, year, author names or the first result which has the title words somewhere in the content. For each publication result, the citing artciles are fetched with the link provided at Google scholar beneath the search result. These extracted results are saved in an sqlite database with the following schema. The publications are stored in a table called pub, and citations between publications are represented by a table called cite.
<br>

Table('pub')<br>
fields
--------

* id # the unique identifer of publiations in our database. It's highly recommended to use this id for referig to a publication.
* 'cluster_id', INT, index=True, # publication id in Google scholar. It's the unique identifier of a publication in Google Scholar. Note: Some publications do not have cluster_id because Google could not find an online version of them somewhere on the Internet. For these pubs, cluster_id left blank in our database as well. Therefore, it's necessary to only use "id" field (which is the unique identifer of pubs in our database) for refering to a publication in future analysis. 
* 'title', STRING(200), index=True 
* 'authors', STRING(300), # name of the authors separated by comma. For example: A Epstein, S Johnson, P Lafferty
* 'year', INT, index=True
* 'journal', STRING(200)
* 'url', STRING(200),  # url where publication can be accessed online
* 'num_citations', INT
* 'num_versions', INT, # number of different version of the this publication which Google scholar could detect and merge under one cluster_id
* 'excerpt', STRING(2000), # a breif summary of publication abstract
* 'url_pdf', STRING(500) # url where pdf file of publication can be accessed online


<br>
Table('cite')<br>
fields
--------

* id
* 'pub_id', INT, index=True, # id of the publication which is citing the publication with id of cited_pub_id
* 'cited_pub_id', INT, index=True # id of the publication which is being cited by pub_id publication   
 