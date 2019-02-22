<hr class="processing" style="display:none"/>
<h2>Big Data Analysis</h2>
<!-- textlint-disable terminology -->

Similarly to what has been described in section Publication of context
information as Open Data, Cygnus software allows to store all the selected data
published in the Context Broker in an HDFS based storage. This allows having a
long term historic database of context information that can be used for later
analysis, for instance implementing map & reducing algorithm or performing
queries over big data through Hive.

Similarly to what has been described in the section Publication of context
information as Open Data, the Cygnus component can be configured to gather data
from the Context Broker and store it in HDFS. The configuration in this case
should include the Cosmos Namenode endpoints, the service port, user’s
credential, the Cosmos API used (webhdfs, infinity, httpfs), the type of
attribute  and endpoint of Hive server.

Once the Context Data has been stored, it is possible to use the Big Data GE to
process it either with a map & reduce application or with Hive. It is of course
also possible to process in the Big Data GEs other big datasets, either by
themselves or in combination with context information.

A typical example would be to analyse massive information gathered from sensors
in a city over a long period of type. All the data would have been gathered
through Context Broker and Cygnus and stored in Big Data for a long period of
time. In order to analyse the data, a few steps should be followed (the examples
in this whitepaper are based on a global and shared instance of Cosmos Big Data
GE):

Browse the Cosmos portal (http://cosmos.lab.fiware.org/cosmos-gui/). Use an
already registered user in FI-LAB to create a Cosmos account. The details of
your account will be given once registered, typically:

-   Cosmos username: if your FI-LAB username is`<my_user>@mailprovider.com`,
    your cosmos username will be `<my_user>`. This will give you a Unix-like
    account in the head node of the global instance, being your user space
    `/home/<my_user>/`.
-   Cosmos HDFS space: Apart from your Unix-like user space in the Head Node,
    you will have a HDFS space located at the entire cluster, it will be
    `/user/<my_user>/`

Now you should be ready to login into the head node of the global instance of
Cosmos in FI-LAB, simply using your FI-LAB credentials:

    [remote-vm]$ export COSMOS_USER= // this is not strictly necessary, junt in order the example commands can be copied&pasted
     [remote-vm]$ ssh $COSMOS_USER@cosmos.lab.fiware.org

Once logged, you can have access to your HDFS space by using the Hadoop file
system commands:

    [head-node]$ export COSMOS_USER= // this is not strictly necessary, junt in order the example commands can be copied&pasted
     [head-node]$ hadoop fs -ls /user/$COSMOS_USER // lists your HDFS space
     [head-node]$ hadoop fs -mkdir /user/$COSMOS_USER/new_folder // creates a new directory called "new_folder" under your HDFS space

...

Apart from using the context data stored, you can upload your own data to your
HDFS space using the Hadoop file system commands. This can be only done after
logging into the Head Node, and allows uploading Unix-like local files placed in
the Head node:

    [head-node]$ echo "long time ago, in a galaxy far far away…" > unstructured_data.txt
     [head-node]$ hadoop fs -mkdir /user/$COSMOS_USER/input/unstructured/
     [head-node]$ hadoop fs -put unstructured_data.txt /user/$COSMOS_USER/input/unstructured/

However, using the WebHDFS/HttpFS RESTful API will allow you to upload files
existing outside the global instance of Cosmos in FI-LAB. The following example
uses HttpFS instead of WebHDFS (uses the TCP/14000 port instead of TCP/50070),
and curl is used as HTTP client (but your applications should implement your own
HTTP client):

    [remote-vm]$ curl -i -X PUT "http://cosmos.lab.fiware.org:14000/webhdfs/v1/user/$COSMOS_USER/input_data?op=MKDIRS&user.name=$COSMOS_USER"
     [remote-vm]$ curl -i -X PUT "http://cosmos.lab.fiware.org:14000/webhdfs/v1/user/$COSMOS_USER/input_data/unstructured_data.txt?op=CREATE&user.name=$COSMOS_USER"
     [remote-vm]$ curl -i -X PUT -T unstructured_data.txt –header "content-type: application/octet-stream" http://cosmos.lab.fiware.org:14000/webhdfs/v1/user/$COSMOS_USER/input_data/unstructured_data.txt?op=CREATE&user.name=$COSMOS_USER&data=true

As you can see, the data uploading is a two-step operation, as stated in the
WebHDFS specification: the first invocation of the API talks directly with the
Head Node, specifying the new file creation and its name; then the Head Node
sends a temporary redirection response, specifying the data node among all the
existing ones in the cluster where the data has to be stored, which is the
endpoint of the second step. Nevertheless, the HttpFS gateway implements the
same API but its internal behaviour changes, making the redirection to point to
the head node itself.

If the data you have uploaded to your HDFS space is a CSV-like file, i.e. a
structured file containing lines of data fields separated by a common character,
then you can use Hive to query the data:

    [head-node]$ echo "luke,tatooine,jedi,25" >> structured_data.txt
     [head-node]$ echo "leia,alderaan,politician,25" >> structured_data.txt
     [head-node]$ echo "solo,corellia,pilot,32" >> structured_data.txt
     [head-node]$ echo "yoda,dagobah,jedi,275" >> structured_data.txt
     [head-node]$ echo "vader,tatooine,sith,50" >> structured_data.txt
     [head-node]$ hadoop fs -mkdir /user/$COSMOS_USER/input/structured/
     [head-node]$ hadoop fs -put structured_data.txt /user/$COSMOS_USER/input/structured/

A Hive table can be created, which is like a SQL table. Log into the Head Node,
invoke the Hive CLI and type the following in order to create a Hive table:

    [head-node]$ hive
     hive> create external table <my_user>_star_wars (name string, planet string, profession string, age int) row format delimited fields terminated by ',' location '/user/<my_user>/input/structured/';

These Hive tables can be queried locally, by using the Hive CLI as well:

    [head-node]$ hive
     hive> select * from <my_user>_star_wars; // or any other SQL-like sentence, properly called HiveQL

Or remotely, by developing a Hive client (typically, using JDBC, but there are
some other options for other non-Java programming languages) connecting
tocosmos.lab.fi-ware.org:10000. Several pre-loaded MapReduce examples can be
found in every Hadoop distribution. You can list them by ssh'ing the head node
and commanding Hadoop:

    [head-node]$ hadoop jar /usr/lib/hadoop-0.20/hadoop-examples.jar

For instance, you can run the word count example (this is also known as the
"hello world" of Hadoop) by typing:

    [head-node]$ hadoop jar /usr/lib/hadoop-0.20/hadoop-examples.jar wordcount /user/$COSMOS_USER/input/unstructured/unstructured_data.txt /user/$COSMOS_USER/output/

Please observe the output HDFS folder is automatically created. The MapReduce
results are stored in HDFS. You can download them to your Unix user space within
the head node by doing:

    [head-node]$ hadoop fs -getmerge /user/$COSMOS_USER/output /home/$COSMOS_USER/count_result.txt

You can also download any HDFS file to you home user in the head node by doing:

    [head-node]$ hadoop fs -get /user/$COSMOS_USER/structured/structured_data.txt /home/$COSMOS_USER/

If you want to download the HDFS file directly to a remote machine, you must use
the WebHDFS/HttpFS RESTful API:

    [remote-vm]$ curl -i -L "http://cosmos.lab.fi-ware.org:14000/webhdfs/v1/user/$COSMOS_USER/structured/structured_data.txt?op=OPEN&user.name=$COSMOS_USER"

If you want to start experimenting and doing hands-on work, have a look at:

-   [Cygnus](https://github.com/ging/fiware-cygnus/)
-   [Cosmos](https://github.com/ging/fiware-cosmos-orion-flink-connector/)
