Driver.java

import java.io.IOException;

import org.apache.hadoop.conf.Configuration;

import org.apache.hadoop.io.IntWritable;

import org.apache.hadoop.io.Text;

import org.apache.hadoop.mapreduce.Job;

import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;

import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import org.apache.hadoop.fs.Path;

public class WDriver

{

&#x09;public static void main(String\[] args) throws IllegalArgumentException, IOException, ClassNotFoundException, InterruptedException

&#x09;{

&#x09;	Configuration c = new Configuration();

&#x09;	Job job = new Job(c, "WordCount");

&#x09;	job.setJarByClass(WDriver.class);

&#x09;	job.setMapperClass(WMapper.class);

&#x09;	job.setReducerClass(WReducer.class);

&#x09;	job.setOutputKeyClass(Text.class);

&#x09;	job.setOutputValueClass(IntWritable.class);

&#x09;	FileInputFormat.addInputPath(job, new Path(args\[0]));

&#x09;	FileOutputFormat.setOutputPath(job, new Path(args\[1]));

&#x09;	System.exit(job.waitForCompletion(true)?0:1);

&#x09;}

}

\----------------------------------------------------------------------------------------------------



Reducer.java

import java.io.IOException;

import org.apache.hadoop.io.IntWritable;

import org.apache.hadoop.io.Text;

import org.apache.hadoop.mapreduce.Reducer;

public class WReducer extends Reducer<Text, IntWritable, Text, IntWritable>

{

&#x09;private int maxcount = 0;

&#x09;private Text maxword = new Text();

&#x09;

&#x09;public void reduce(Text word, Iterable<IntWritable> values, Context con) throws IOException, InterruptedException

&#x09;{

&#x09;	int sum = 0;

&#x09;	for(IntWritable value:values)

&#x09;	{

&#x20;  			sum += value.get();

&#x09;	}

&#x20;

&#x20; 		if(maxcount < sum)

&#x20;		{

&#x20;  			maxcount = sum;

&#x20;  			maxword.set(word.toString());

&#x20; 		}

&#x20;	}

&#x20;

&#x09;protected void cleanup(Context con) throws IOException, InterruptedException

&#x20;	{

&#x20; 		con.write(maxword, new IntWritable(maxcount));

&#x20;	}

}

\----------------------------------------------------------------------------------------------------



Mapper.java

import java.io.IOException;

import org.apache.hadoop.io.IntWritable;

import org.apache.hadoop.io.LongWritable;

import org.apache.hadoop.io.Text;

import org.apache.hadoop.mapreduce.Mapper;

&#x20;

public class WMapper extends Mapper<LongWritable, Text, Text, IntWritable>

{

&#x09;public void map(LongWritable key, Text value, Context con) throws  IOException, InterruptedException

&#x09;{

&#x09;	String\[] words = value.toString().split("\\\\R");

&#x20;

&#x09;	for(String word:words)

&#x09;	{

&#x20;  			String\[] temp = value.toString().split(" ");

&#x20;  			con.write(new Text(temp\[0]), new IntWritable(1));

&#x20; 		}

&#x20;	}

}

\----------------------------------------------------------------------------------------------------



JAR FILES TO BE ADDED:

1\. other location > computer > usr > local > hadoop > share > hadoop > mapreduce > hadoop-mapreduce-client-jobclient-3.3.6.jar

2\. other location > computer > usr > local > hadoop > share > hadoop > mapreduce > hadoop-mapreduce-client-core-3.3.6.jar

3\. other location > computer > usr > local > hadoop > share > hadoop > mapreduce > hadoop-mapreduce-client-common-3.3.6.jar



4\. other location > computer > usr > local > hadoop > share > hadoop > hdfs > hadoop-hdfs-client-3.3.6.jar

5\. other location > computer > usr > local > hadoop > share > hadoop > hdfs > hadoop-hdfs-3.3.6.jar



6\. other location > computer > usr > local > hadoop > share > hadoop > common > hadoop-common-3.3.6.jar



\--------------------------------------------------------------------------------------------------------------------------

CREATE JAR FILE AFTER THIS

\--------------------------------------------------------------------------------------------------------------------------



COMMANDS FOR HADOOP:

start-dfs.sh

start-yarn.sh

hdfs dfs -put input.txt /Platypus

hadoop jar jar\_file\_name.jar driver\_file\_name /Platypus/input.txt /Platypus/output

hdfs dfs -cat /Platypus/output/part-r-0000

Output:

