# Reset Ambari Admin Password

```
sudo ambari-admin-password-reset
```

# Reset MySQL root password

```
sudo systemctl stop mysqld
sudo systemctl set-environment MYSQLD_OPTS="--skip-grant-tables --skip-networking"
sudo systemctl start mysqld
mysql -uroot
```

> Once in Mysql is up and running

```
FLUSH PRIVILEGES;
SET PASSWORD FOR 'root'@'localhost'=PASSWORD('hadoop');
FLUSH PRIVILEGES;
quit;
sudo systemctl unset-environment MYSQLD_OPTS
sudo systemctl restart mysqld
mysql -uroot -phadoop
```

### unzip and untar

```
sudo unzip file.zip
sudo tar xvf file.tar.gz
```

### delete files in AWS S3 Bitbucket
```
import java.net.URI
import org.apache.hadoop.fs.FileSystem
import org.apache.hadoop.fs.Path

FileSystem.get(new URI("s3n://bucket"), sc.hadoopConfiguration).delete(new Path("s3n://bucket/path_to_delete"), true)
```
```
import java.io._
import java.net.URI
import org.apache.hadoop.fs.FileSystem
import org.apache.hadoop.fs.Path
import org.apache.commons.io.FileUtils
import org.apache.spark.SparkSession

object escunion{
  def main(args: Array[String]): Unit = {

    //check to delete the existing files or folder
    
    val accessKeyId = "ebia-vol-test-user"
    val secretAccessKey = "qsUJepZHhevNxhyqfXeM97vJ188hkdvajjmga5/b"
	//Instantiate Spark Session
	val sparkConf = new SparkConf().setAppName("SparkECS").setMaster("local").getOrCreate()
    val sc: SparkContext = new SparkContext(sparkConf)
        val spark = SparkSession
            .builder
            .appName("my spark application name")
            .config("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
            .config("spark.hadoop.fs.s3a.access.key", "my access key")
            .config("spark.hadoop.fs.s3a.secret.key", "my secret key")
            .config("spark.hadoop.fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
            .config("spark.hadoop.fs.s3a.multiobjectdelete.enable","false")
            .config("spark.hadoop.fs.s3a.fast.upload","true")
            .config("spark.sql.parquet.filterPushdown", "true")
            .config("spark.sql.parquet.mergeSchema", "false")
            .config("spark.hadoop.mapreduce.fileoutputcommitter.algorithm.version", "2")
            .config("spark.speculation", "false")
            .getOrCreate

    // FileSystem.get(new URI("s3a://ebia-vol-test/SparkECSCoalesce"), sc.hadoopConfiguration).delete(new Path("s3a://ebia-vol-test/SparkECSCoalesce"), true)
    FileSystem.get(new URI("s3a://ebia-vol-test/SparkECSCoalesce"), sc.hadoopConfiguration).delete(new Path("s3a://ebia-vol-test/SparkECSCoalesce"), true)
      // sc.hadoopConfiguration.set("fs.s3a.awsAccessKeyId", accessKeyId)
      // sc.hadoopConfiguration.set("fs.s3a.awsSecretAccessKey",secretAccessKey)


    val gd_path = sc.textFile("s3a://ebia-vol-test/gd_lyrics.txt")
    val jack_path = sc.textFile("s3a://ebia-vol-test/jack_straw.txt")

    val rddunion = gd_path.union(jack_path)
	
      rddunion.coalesce(1).saveAsTextFile("s3a://ebia-vol-test/SparkECSCoalesce")

    val verify = sc.textFile("s3a://ebia-vol-test/SparkECSCoalesce/part*")
      verify.count()
      verify.collect().map(line => println(line))

  }
}































https://github.com/djannot/ecs-p3/blob/master/spark/spark.md

val gd_path = sc.textFile("s3a://ebia-vol-test/gd_lyrics.txt")
val row_gd = gd_path.map(line => Row.fromSeq(line.split(",", -1)))
var df_gd = sqlContext.createDataFrame(row_gd, schema)

val jack_path = sc.textFile("s3a://ebia-vol-test/jack_straw.txt")
val row_jack = jack_path.map(line => Row.fromSeq(line.split(",", -1)))
var df_jack = sqlContext.createDataFrame(row_jack, schema)

val df3 = df_gd.join(df_jack)

val df1 = sqlContext.createDataFrame(rowRdd1, new StructType(schema.tail.toArray))


https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/examples-s3-objects.html
https://bitbucket.org/atlassian/aws-scala
https://github.com/EMCECS/ecs-samples/tree/master/aws-java-workshop/src/main/java/com/emc/ecs/s3/sample
https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-install.html

http://bigdatatech.taleia.software/2015/12/07/bash-script-to-upload-files-to-a-amazon-s3-bucket-using-curl/

https://italy.emc.com/collateral/TechnicalDocument/docu59635.pdf


Camel Router with Scala DSL Project
===================================

To build this project use

    mvn install

To run this project

    mvn exec:java
    
For more help see the Apache Camel documentation

    http://camel.apache.org/



****=============For me to remember==========****
name := "rest"

version := "1.0"

scalaVersion := "2.10.2"

libraryDependencies ++= Seq(
    "io.spray" % "spray-can" % "1.1-M8",
    "io.spray" % "spray-http" % "1.1-M8",
    "io.spray" % "spray-routing" % "1.1-M8",
    "com.typesafe.akka" %% "akka-actor" % "2.1.4",
    "com.typesafe.akka" %% "akka-slf4j" % "2.1.4",
    "com.typesafe.slick" %% "slick" % "1.0.1",
    "mysql" % "mysql-connector-java" % "5.1.25",
    "net.liftweb" %% "lift-json" % "2.5.1",
    "ch.qos.logback" % "logback-classic" % "1.0.13"
)

resolvers ++= Seq(
    "Spray repository" at "http://repo.spray.io",
    "Typesafe repository" at "http://repo.typesafe.com/typesafe/releases/"
)



======================
public class Test {
    public static String uid = "root";
    public static String secret = "KHBkaH0Xd7YKF43ZPFbWMBT9OP0vIcFAMkD/9dwj";
    public static String viprDataNode = "http://ecs.yourco.com:9020";

    public static String bucketName = "myBucket";
    public static File objectFile = new File("/photos/cat1.jpg");

    public static void main(String[] args) throws Exception {

        AmazonS3Client client = new AmazonS3Client(new BasicAWSCredentials(uid, secret));

        S3ClientOptions options = new S3ClientOptions();
        options.setPathStyleAccess(true);

        AmazonS3Client client = new AmazonS3Client(credentials);
        client.setEndpoint(viprDataNode);
        client.setS3ClientOptions(options);

        client.createBucket(bucketName, "Standard");
        listObjects(client);

        client.putObject(bucketName, objectFile.getName(), objectFile);
        listObjects(client);

        client.copyObject(bucketName,objectFile.getName(),bucketName, "copy-" + objectFile.getName());
        listObjects(client);
    }

    public static void listObjects(AmazonS3Client client) {
        ObjectListing objects = client.listObjects(bucketName);
        for (S3ObjectSummary summary : objects.getObjectSummaries()) {
            System.out.println(summary.getKey()+ "   "+summary.getOwner());
        }
    }
}
==============
https://mvnrepository.com/artifact/com.emc.ecs/spark-ecs-s3?fbclid=IwAR2l8tgXt-McagEPIxDpwZFHUMD9A-e1b5yvEokJG8yVEE2dfjsOXqz3KEs

==============

```




