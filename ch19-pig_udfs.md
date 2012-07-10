# Chapter 19a: The Hadoop Java API

## When to use the Hadoop Java API

Don't.

## How to use the Hadoop Java API

Decompose your problem into small isolable transformations. Implement each as a Pig Load/StoreFunc or UDF (User-Defined Function) making calls to the Hadoop API.[^1]

## The Skeleton of a Hadoop Java API program

I'll trust that for very good reasons -- to interface with an outside system, performance, a powerful library with a Java-only API -- Java is the best choice to implement

      [ Your breathtakingly elegant, stunningly performant solution to a novel problem ]

When this happens around the office, we sing this little dirge[^2]:

      Life, sometimes, is Russian Novel. Is having unhappy marriage and much snow and little vodka.
      But when Russian Novel it is short, then quickly we finish and again is Sweet Valley High.

What we *don't* do is write a pure Hadoop-API Java program. In practice, those look like this:

      HORRIBLE BOILERPLATE TO DO A CRAPPY BUT SERVICABLE JOB AT PARSING PARAMS
      HORRIBLE BOILERPLATE TO DO A CRAPPY BUT SERVICABLE JOB AT READING FILES
      COBBLED-TOGETHER CODE TO DESERIALIZE THE FILE, HANDLE SPLITS, ETC
      
      [ Your breathtakingly elegant, stunningly performant solution to a novel problem ]

      COBBLED-TOGETHER CODE THAT KINDA DOES WHAT PIG'S FLATTEN COMMAND DOES
      COBBLED-TOGETHER CODE THAT KINDA DOES WHAT PIG'S CROSS COMMAND DOES
      A SIMPLE COMBINER COPIED FROM TOM WHITE'S BOOK
      1000 LINES OF CODE TO DO WHAT RUBY COULD IN THREE LINES OF CODE
      HORRIBLE BOILERPLATE TO DO A CRAPPY BUT SERVICABLE JOB AT WRITING FILES      
      UGLY BUT NECESSARY CODE TO GLUE THIS TO THE REST OF THE ECOSYSTEM

The surrounding code is ugly and boring; it will take more time, produce more bugs, and carry a higher maintenance burden than the important stuff. More importantly, the high-level framework provides an implementation far better than it's worth your time to recreate.[^3]

Instead, we write

      A SKELETON FOR A PIG UDF DEFINITION
      [ Your breathtakingly elegant, stunningly performant solution to a novel problem ]

      A PIG SCRIPT

# Chapter 19: Pig UDFs

### Pig UDFs



### LoadFunc / StoreFunc



### Algebraic UDFs let Pig go fast

One of the great things


### Wonderdog -- an ElasticSearch UDF



__________________________________________________________________________
__________________________________________________________________________
__________________________________________________________________________

[^1] Doesn't have to be Pig -- Hive, Cascading, and Crunch and other high-level frameworks abstract out the boring stuff while still making it easy to write custom components.

[^2] If the novel lasts all week, someone will tell this joke and then we will walk carefully to the bar.

    The church, it is close by -- but the way is cold and icy.
    The bar, it is far away -- but we shall walk carefully.

[^3] ... and when the harsh reality of a production dataset reveals that your data has an unforseen and crippling "stuck reducer" problem, you're facing a fundamental re-think of your program's design rather than a one-line change from `JOIN` to `SKEW JOIN`. See the chapter on Advanced Pig.