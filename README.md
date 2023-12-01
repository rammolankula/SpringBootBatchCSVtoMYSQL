# SpringBootBatchCSVtoMYSQL

###### BatchProcessing : Multiple Operations are executed as one task. Executing one Big/Large Task Excuting one Big/Large Task executed step By step.
###### *) Batch processing is used  on data(Large set of data) processing
###### Ex: Database <-> Json,XML,Excel...etc

###### Every task is called as JOB, it subtask is called as STEP
###### One Job contains multiple (One or many) steps
###### Steps contains again
<pre>
   a.Reader    |  ItemReader
   b.Processor |  ItemProcessor 
   c.Writer    |  ItemWriter
  &#8594; Job Execution /Start done by JobLuncher
  &#8594; Job,Steps,execution details...etc are stored in JobRepository(Memory)
</pre>
#### Note
<pre>
 &#8594; a.One Job contains atleast one step, at most n-steps
 &#8594; b.one Step Contains 1-Reader 1-Processor 1-Writer
 &#8594; c. Reader -Reads data from source (Taking input)
 &#8594; d.Processor -Do Operations/Calculation..etc given by reader
 &#8594; e.Writer -writes data to destination(Give output)
 &#8594; f. Source/Destination can be a database, file system (XML,excel..etc) 
</pre>
#### Step & Job 
<pre>
 &#8594; *)StepBuilderFactory is a class this is used to create one step object using configuration
 &#8594; *)JobBuilderFactory is a class this is used to create one job object using  configurations
</pre>
#### Step Work Flow
#### ===============
<pre>
&#8594; a. Item Reader reads data from source as one GenericType ex: String (T).
&#8594; b. same data is gives to Item Processor Input Type ie String (I)
&#8594; c. Item processor will do operation any return different type like Product(O)
&#8594; d. Item Processor will send to ItemWrier.Here writer Generic Type(T)
&#8594; e. Must match with Item Processor Output(Product) Type
</pre>
   
#### Batch Processing API Details
#### ============================
<pre>
&#8594; package: org,springframework.batch.item
&#8594; Interface: ItemReader<T>
&#8594; method :read():Taking

&#8594; Interface : ItemProcessor<I,o>
&#8594; method    :process(I item):o

&#8594; Interface :ItemWriter<T>
&#8594; method    :write(List<? extends T> items):void

&#8594; Interface :Job
&#8594; class     : JobBuilderFactory

&#8594; Interface :JobLuncher
&#8594;     :run(job,jobParameters)

&#8594; Interface :JobExecutionListener
&#8594; method    :beforeJob(je):void
&#8594; afterJob(je):void
</pre>
#### Q)What is the difference between List<T> and List<? extends T>?
<pre>
A) List T works only for same type of Object
   List ? extends T works for same type and its subType objects
</pre>
   
#### Step can be created using below details
&#8594; StepName
&#8594; chunk size with Input type and OutPut Type
&#8594; Item Reader class Object
&#8594; Item Processor class Object
&#8594; Item Writer class object
&#8594; Finally build

Job can be created using below details
&#8594; Job Name
&#8594; job execution listener
&#8594; incrementer (calls steps in order)
&#8594; staring step
&#8594; next step
&#8594; next step
&#8594; Finally build
</pre>




