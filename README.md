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
<pre>
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
##### Code Order 
<pre>
&#8594; 1.Reader
&#8594; 2.Processor 
&#8594; 3.Writer
&#8594; 4.Listener
&#8594; 5.BatchConfig
&#8594;   a.Reader Object
&#8594;   b.Processor object
&#8594;  c.Writer object
&#8594;   d.Step configuration using StepBuilderFactory
&#8594;   e.Job Configuration using JobBuilderFactory
&#8594; 6.Runner (Job Luncher code)
&#8594; 7.properties file
</pre>
##### Definitions of Reader,Processor,Writter
<pre>
⮕Reader: It will read data from source location. we need to define
  one class that reads data as item line by line
</pre>
<pre>
⮕Processor: it will do data conversions, calculations,..etc
  we need to define one class that implements ItemProcessor
</pre>

<pre>
⮕Writer: It will write data to destination (ex: database). we need
  to define a class that implements ItemWriter(I) and override write method.
</pre>
<pre>
Class A{}

&#8594; 1 Creating Object
A a=new A();

&#8594; 2 Anonymous Inner class + Object creation
A a=new A(){

};
&#8594; 3 Instance Block + Anonymous Inner class + Object creation
A a=new A(){
{
}
};
</pre>
###### Block -3 types, Instance Block,Static block,local block(for ,if,while)
 
###### What is Instance Block where it is used?
<pre>
 A) Instance Blocks are used to execute any logic before constructor.\
    if constructor is not available then we can use even instance block
	
	&#8594; Incase of anonymous inner class(No CLASS NAME), we can not write
	    constructor then we use Instance Block.
	
	
	&#8594; Anonymous inner class code is good for singletime execution, they are faster in
	     load and unload even.
</pre>
####Example:::
<pre>
public class Test{
   public static void main(String[] args){
    A ob=new A(){
	//we can not write like $1() {} as like constructor
	//so use instance block
	  {
	  System.out.println("Ok-IB-2");
	  }
	
	}
   }
}
 </pre>


#### Note:
<pre>
 A a=new A(){}; is like creating a sub class without any name, that is called
 as anonymous inner class. it is faster compared to normal subclass and ontime
 usage only
 
&#8594; Incase of name-less (Anonymous) class we can not write constructor. so using
   so using instance block (Instance initilizer Block)
   
  A a=new A(){
  {
  }
  }
</pre>
  
 Anonymous =name-less class + Object created at same time
===============================================================================================
Spring Boot Batch : CSV to MySql
==================================================================
CSV: Comma Separated Values(old Excel File Type)
<pre>
&#8594; CSV is a file type, that holds multiple lines data
&#8594; Every line data contains comma symbols between two values
&#8594; This is one of old ExcelFile Format
&#8594; This format is good for strong large data in simple text format
&#8594; This can transformed and read in simpleway.
&#8594; Example CSV file is given as
</pre>

##### Employee.csv
##### ------------
<pre>
10,sam,3.2
11,tin,8.2
12,syed,6.5
</pre>
<pre>
&#8594; convert above csv file one line into one object(Ex: Employee object)
&#8594; This is converted object store in database (ex:emptab in MYSQL)
&#8594; Batch Processing always runs as background process. No need to stop UI operations
&#8594; Batch Processing is like a Demon Thread
</pre>

###### -----Coding And Execution Steps: CSV TO MYSQL----
<pre>
Step#1. create one csv file in class path (or any location)
       ex:products.csv
step#2. Provide data to csv file as multiple line.

step#3. FlatefileItemReader uses methods to read CSV file
        using setResoure(fileName)

step#4. OneLine is read by LineMapper from CSV file

Step#5.Line is devided into values usng Line Tokenizer

Step#6.One Object is created and values are set based on variables
       (fields) names using FieldSetMapper.
	   
Step#7.Processor is taking inout from ItemReader as one Object
     it will do calculations like discount(3%) gst(6%)..etc

step#8 processor returns object that has all fields values
      (even calculated data)

step#9 JDBCBatchItemQrite will read one By one Line from processor
       and collects data as one List Type

step#10 We need to create one database connection between our app to write
     data to table (ie DataSource-Db connection)
	 
Step#11 Write one INSERT SQL query using parameters(id,code...etc)

step#12 set data to SQL using objects(from list) and create as JDBC batch
     JDBC Batch(set of SQL queries)
</pre>

<pre>
&#8594; Here ItemReader(FlatefileItemReader) and writer (JDBCBatchItemWriter) are predefined
        But We need to configure using spring Java Based configuration(Anonymous inner class style)
        only we need to code Processor
</pre>



