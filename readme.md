# study-hdfs
A small study project on HDFS

## lets first start HDFS
The following command will start a HDFS (namenode/2nd-namenode/datanode), yarn,
resource manager and nodemanager.

```
$ docker run -it -p 50070:50070 -p 9000:9000 sequenceiq/hadoop-docker:2.7.0 /etc/bootstrap.sh -bash
```

We can check the java processes:

```
bash-4.1# jps
671 NodeManager
972 Jps
574 ResourceManager
416 SecondaryNameNode
130 NameNode
226 DataNode
```

## accessing the Hadoop web services
- The hadoop cluster web interface is available on [localhost:50070].
- Hadoop is available on [localhost:9000]

## Hadoop utilities
In the docker container, `$HADOOP_PREFIX` is pointing to the hadoop homedir,
which is:

```
bash-4.1# echo $HADOOP_PREFIX
/usr/local/hadoop
```

To put the hadoop 'bin' files on the path, type:

```
export PATH=$HADOOP_PREFIX/bin:$PATH
```

## Accessing HDFS
After you've exported the PATH (see above), the `hdfs` utility should be
available, eg:

```
bash-4.1# hdfs dfs -ls /
Found 1 items
drwxr-xr-x   - root supergroup          0 2015-05-16 05:42 /user
```

We can get help with:

```
bash-4.1# hdfs dfs -help
```

Let's put a file on HDFS:

```
bash-4.1# echo "foobar" > test.txt
bash-4.1# hdfs dfs -put test.txt /test.txt
bash-4.1# hdfs dfs -ls /
Found 2 items
-rw-r--r--   1 root supergroup          7 2020-04-08 16:50 /test.txt
drwxr-xr-x   - root supergroup          0 2015-05-16 05:42 /user
```

Lets get the file from hdfs to our local file system:

```
# first remove the file from local
bash-4.1# rm /test.txt
# we can get the file in multiple ways
# copy it to '/'
bash-4.1# hdfs dfs -get /test.txt /
# be explicit about the name
bash-4.1# hdfs dfs -get /test.txt /test.txt
# rename the file
bash-4.1# hdfs dfs -get /test.txt /foo.txt
```

Now remove the file from HDFS:

```
bash-4.1# hdfs dfs -rm /test.txt
Deleted /test.txt
``` 

We can also delete a file, skipping the trash:

```
# create a zero bytes file
bash-4.1# hdfs dfs -touchz /test.txt
# delete the file, skip the trash
bash-4.1# hdfs dfs -rm -skipTrash /test.txt
Deleted /test.txt
```

To delete the trash

```
bash-4.1# hdfs dfs -expunge
```

# Resources
- [Single Node Hadoop Cluster](https://www.alibabacloud.com/blog/setup-a-single-node-hadoop-cluster-using-docker_595278)
- [hadoop](https://hub.docker.com/r/sequenceiq/hadoop-docker)
- [HDFS tutorial](http://hadooptutorial.info/hdfs-file-system-commands/)
