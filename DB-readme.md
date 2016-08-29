Crawling procedure:
    The publications list is imported from sample.xlsx file. For every publication is searched on Google Scholar to find either exact match in title, year, author names or the first result which has the title words somewhere in the content. For each publication result, the citing artciles are fetched with the link provided at Google scholar beneath the search result. These extracted results are saved in an sqlite database with the following schema. The publications are stored in a table called pub, and citations between publications are represented by a table called cite.
    Table('pub', fields=[
            id(), # the unique identifer of publiations in our database. It's highly recommended to use this id for referig to a publication.
            field('cluster_id', INT, index=True), #publication id in Google scholar. It's the unique identifier of a publication in Google Scholar. Note: Some publications do not have cluster_id because Google could not find an online version of them somewhere on the Internet. For these pubs, cluster_id left blank in our database as well. Therefore, it's necessary to only use "id" field (which is the unique identifer of pubs in our database) for refering to a publication in future analysis. 
            field('title', STRING(200), index=True), 
            field('authors', STRING(300)), # name of the authors separated by comma. For example: A Epstein, S Johnson, P Lafferty
            field('year', INT, index=True),
            field('journal', STRING(200)),
            field('url', STRING(200)),  # url where publication can be accessed online
            field('num_citations', INT),
            field('num_versions', INT), # number of different version of the this publication which Google scholar could detect and merge under one cluster_id
            field('excerpt', STRING(2000)), # a breif summary of publication abstract
            field('url_pdf', STRING(500)) # url where pdf file of publication can be accessed online
        ])
    Table('cite', fields=[
            id(),
            field('pub_id', INT, index=True), # id of the publication which is citing the publication with id of cited_pub_id
            field('cited_pub_id', INT, index=True) # id of the publication which is being cited by pub_id publication
        ])   
 