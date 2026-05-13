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
	public static void main(String[] args) throws IllegalArgumentException, IOException, ClassNotFoundException, InterruptedException
	{
		Configuration c = new Configuration();
		Job job = new Job(c, "WordCount");
		job.setJarByClass(WDriver.class);
		job.setMapperClass(WMapper.class);
		job.setReducerClass(WReducer.class);
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
		FileInputFormat.addInputPath(job, new Path(args[0]));
		FileOutputFormat.setOutputPath(job, new Path(args[1]));
		System.exit(job.waitForCompletion(true)?0:1);
	}
}
----------------------------------------------------------------------------------------------------

Reducer.java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;
public class WReducer extends Reducer<Text, IntWritable, Text, IntWritable>
{
	private int maxcount = 0;
	private Text maxword = new Text();
	
	public void reduce(Text word, Iterable<IntWritable> values, Context con) throws IOException, InterruptedException
	{
		int sum = 0;
		for(IntWritable value:values)
		{
   			sum += value.get();
		}
 
  		if(maxcount < sum)
 		{
   			maxcount = sum;
   			maxword.set(word.toString());
  		}
 	}
 
	protected void cleanup(Context con) throws IOException, InterruptedException
 	{
  		con.write(maxword, new IntWritable(maxcount));
 	}
}
----------------------------------------------------------------------------------------------------

Mapper.java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;
 
public class WMapper extends Mapper<LongWritable, Text, Text, IntWritable>
{
	public void map(LongWritable key, Text value, Context con) throws  IOException, InterruptedException
	{
		String[] words = value.toString().split("\\R");
 
		for(String word:words)
		{
   			String[] temp = value.toString().split(" ");
   			con.write(new Text(temp[0]), new IntWritable(1));
  		}
 	}
}
----------------------------------------------------------------------------------------------------

JAR FILES TO BE ADDED:
1. other location > computer > usr > local > hadoop > share > hadoop > mapreduce > hadoop-mapreduce-client-jobclient-3.3.6.jar
2. other location > computer > usr > local > hadoop > share > hadoop > mapreduce > hadoop-mapreduce-client-core-3.3.6.jar
3. other location > computer > usr > local > hadoop > share > hadoop > mapreduce > hadoop-mapreduce-client-common-3.3.6.jar

4. other location > computer > usr > local > hadoop > share > hadoop > hdfs > hadoop-hdfs-client-3.3.6.jar
5. other location > computer > usr > local > hadoop > share > hadoop > hdfs > hadoop-hdfs-3.3.6.jar

6. other location > computer > usr > local > hadoop > share > hadoop > common > hadoop-common-3.3.6.jar

--------------------------------------------------------------------------------------------------------------------------
CREATE JAR FILE AFTER THIS
--------------------------------------------------------------------------------------------------------------------------

COMMANDS FOR HADOOP:
start-dfs.sh
start-yarn.sh
hdfs dfs -put input.txt /Platypus
hadoop jar jar_file_name.jar driver_file_name /Platypus/input.txt /Platypus/output
hdfs dfs -cat /Platypus/output/part-r-0000
Output:
