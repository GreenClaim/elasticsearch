### Flexible elasticseach builder to run complex queries with an easier way.

#####1) Install package via composer:

	composer require basemkhirat/elasticsearch

#####2) Add package service provider:

	Basemkhirat\Elasticsearch\ElasticsearchServiceProvider::class
	
#####3) Add package alias:

	'ES' => Basemkhirat\Elasticsearch\Facades\ES::class
	
#####4) Publishing:
    
    php artisan vendor:publish --provider="Basemkhirat\Elasticsearch\ElasticsearchServiceProvider"
	
### Usage:

#### Setting your connections

  
  After publishing, the config file is placed here `config/es.php`
  where you can add more than one elasticsearch server.


#### Running queries:

    $documents = ES::connection("default")
                    ->index("my_index")
                    ->type("my_type")
                    ->get();    // return collection of results

you can rewite the above query to

    $documents = ES::get();    // return collection of results
    
the query builder will use the default connection, index, and type names setted in configuration file `es.php`. 
 
Index and type names setted in query will override values the configuration file

#### Available methods:


##### Sorting
    
    $documents = ES::orderBy("created_at", "desc")->get();
    $documents = ES::orderBy("_score")->get();
    
##### Limit and offset
    
    $documents = ES::take(10)->skip(5)->get();
    
##### Select only specific fields
    
    $documents = ES::select("title", "content")->take(10)->skip(5)->get();
    
##### Where clause
    
    ES::where("id", 150)->get(); or ES::where("id", "=", 150)->get();
    ES::where("id", ">", 150)->get();
    ES::where("id", ">=", 150)->get();
    ES::where("id", "<", 150)->get();
    ES::where("id", "<=", 150)->get();
    ES::where("title", "like", "foo")->get();
    ES::where("hobbies", "exists", true)->get(); or ES::whereExists("hobbies", true)->get();
    
##### Where in clause
    
    ES::whereIn("id", [100, 150])->get();
    
##### Where between clause 
    
    ES::whereBetween("id", 100, 150)->get();
   
  >
    
##### Where not clause
    
    ES::whereNot("id", 150)->get(); or ES::where("id", "=", 150)->get();
    ES::whereNot("id", ">", 150)->get();
    ES::whereNot("id", ">=", 150)->get();
    ES::whereNot("id", "<", 150)->get();
    ES::whereNot("id", "<=", 150)->get();
    ES::whereNot("title", "like", "foo")->get();
    ES::whereNot("hobbies", "exists", true)->get(); or ES::whereExists("hobbies", true)->get();
    
##### Where not in clause
    
    ES::whereNotIn("id", [100, 150])->get();
    
##### Where not between clause 
    
    ES::whereNotBetween("id", 100, 150)->get();
    
    
  >
  
##### Search the entire document
    
    ES::search("bar")->get();
    
    
  >
  
##### Return only first record
    
    ES::search("bar")->first();
    
  >
  
##### Return only count
    
    ES::search("bar")->count();
    
  >
    
##### Paginate results with per_page = 5
      
    $documents = ES::search("bar")->paginate(5);
    
    // getting pagination links
    
    $documents->links();
    
    
  >
  
##### Executing elasticsearch raw queries
    
    ES::raw()->search([
        "index" => "my_index",
        "type"  => "my_type",
        "body"  => [
             "query": [
               "bool": [
                 "must": [
                   [ "match": [ "address": "mill" ] ],
                   [ "match": [ "address": "lane" ] ] 
                 ]
               ]
             ]
           ]
        ]);
  
  
   >
   
##### Insert a new document
    
    ES::insert([
        "title" => "Test document",
        "content" => "sample content"
    ], 3);
    
    
    A new document will be inserted with _id = 3.
  
    [id is optional] if not specified, a unique hash key will be generated 

  
  >
    
##### Bulk insert a multiple of documents at once using multidimensional array of [id => data] pairs
     
     ES::bulk(
         10 => [
            "title" => "Test document 1",
            "content" => "sample content 1"
         ],
         11 => [
            "title" => "Test document 2",
            "content" => "sample content 2"
         ],
     );
     
     The two given documents will be inserted with its associated ids
  
   >
   
##### Update an existing document
       
    ES::update([
       "title" => "Test document",
       "content" => "sample content"
    ], 3);
        
        
    Document has _id = 3 will be updated.
    
    [id is required]
    
   >
   
##### Delete a document
       
    ES::delete(3);
        
    Document has _id = 3 will be deleted.
    
    [id is required]
    

`Good luck`

`Dont forget to send a feedback..`