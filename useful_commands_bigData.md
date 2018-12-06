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

object projunion{
  def main(args: Array[String]): Unit = {
  
    val accessKeyId = ""
    val secretAccessKey = ""
	//Instantiate Spark Session
	val sparkConf = new SparkConf().setAppName("nameofproj").setMaster("local").getOrCreate()
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

    // FileSystem.get(new URI("s3a://pathtofile"), sc.hadoopConfiguration).delete(new Path("s3a://pathtofile"), true)
    FileSystem.get(new URI("s3a://pathtofile"), sc.hadoopConfiguration).delete(new Path("s3a://pathtofile"), true)
      // sc.hadoopConfiguration.set("fs.s3a.awsAccessKeyId", accessKeyId)
      // sc.hadoopConfiguration.set("fs.s3a.awsSecretAccessKey",secretAccessKey)


    val path1 = sc.textFile("s3a://pathtofile1")
    val path2 = sc.textFile("s3a://pathtofile2")

    val rddunion = path1.union(path2)
	
      rddunion.coalesce(1).saveAsTextFile("s3a://pathtofile")

    val verify = sc.textFile("s3a://pathtofile/part*")
      verify.count()
      verify.collect().map(line => println(line))

  }
}


https://github.com/djannot/ecs-p3/blob/master/spark/spark.md


https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/examples-s3-objects.html
https://bitbucket.org/atlassian/aws-scala
https://github.com/EMCECS/ecs-samples/tree/master/aws-java-workshop/src/main/java/com/emc/ecs/s3/sample
https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-install.html

http://bigdatatech.taleia.software/2015/12/07/bash-script-to-upload-files-to-a-amazon-s3-bucket-using-curl/

https://italy.emc.com/collateral/TechnicalDocument/docu59635.pdf


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

```




