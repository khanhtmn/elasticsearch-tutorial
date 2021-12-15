# Elasticsearch Tutorial

This is a repository for the Elasticsearch tutorial. In the commands below you'll find a step-by-step instruction about:

1. How to set up a simple Elasticsearch instance up and running

2. Some common failures that Elasticsearch may encounter and how to fix them

## Set up guide

Let’s try running an instance of Elasticsearch. In order to do so, you need to have the following on your computer:

- Git: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
- Docker: https://docs.docker.com/get-docker/
- Shakespeare data: You can download it directly from here or read about its origin here

### Disclaimer:

- All of the following commands are compatible with `Linux/MacOS`. If you’re using `Windows`, please use Google to get their equivalence.
- Because Elasticsearch is quite memory intensive, you should turn off any unnecessary tabs or applications on your computer before running Elasticsearch.
- You may need to run the Docker commands with `sudo` if there’s any error about permission. If you don’t want to use `sudo` every time you run the Docker commands, considering following this advice: https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo


### Step-by-step guide

- Step 1: Pulling the Elastic image from Elasticsearch Docker page (not from the Docker Hub). This may take a few minutes because the image is approximately 700 MB.

```$ docker pull docker.elastic.co/elasticsearch/elasticsearch:7.15.1```

- Step 2: Check if we have the Elasticsearch image

```$ docker images```

- Step 3: Spin up a single Elasticsearch instance with one node (you can do more than one too)

```$ docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.15.1```

- Step 4: Check if we have run step 3 successfully

```$ docker ps```

- Step 5: Make a call to our Elasticsearch instance

```$ curl http://localhost:9200```

- Step 6: Check if we have any indices yet

```$ curl -X GET "localhost:9200/_cat/indices?v"```

- Step 7: Create a new index called “newindex”

```$ curl -X PUT http://localhost:9200/newindex```

- Step 8: Check if the index “newindex” was created

```$ curl -X GET http://localhost:9200/newindex?pretty```

- Step 9: We can check all the indices we have so far

```$ curl -X GET "localhost:9200/_cat/indices?v"```

Note: The parameters `?pretty` in step 8 is optional. We just use it to make the JSON format result more readable. You can omit it from the call and it still works completely fine. 

## Short experiments

### Set up the system for the experiment

- Step 1: Clone the tutorial repository

```$ git clone https://github.com/khanhtmn/elasticsearch-tutorial.git```

- Step 2: Change the directory to elasticsearch-tutorial

```$ cd elasticsearch-tutorial```

- Step 3: Set vm.max_map_count

```$ sysctl -w vm.max_map_count=262144```

If you have permission error, then you can run the command with sudo:

```$ sudo sysctl -w vm.max_map_count=262144```

- Step 4: Start the Elasticsearch cluster with 3 nodes

```$ docker-compose up```

It will take a while for all the nodes to be up and running. To test whether the nodes are there yet, you can make a curl call:

```$ curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"```

### Experiment

In this experiment, we will shut down a node in the 3-node cluster above to see how Elasticsearch handles power outage. We will shut down 2 nodes in total: the first node is the non-master node and the second node is the master node.

To shut down the first non-master node, do the following:

- Step 1: Check the master node

```$ curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"```

- Step 2: Check the container id of the nodes

```$ docker ps```

- Step 3: Stop the non-master node

```$ docker stop <container ID>```

I stop the es03 node in my case:

```$ docker stop 34b17f47a340```

- Step 4: Check if the cluster is still up with docker and curl

```
$ docker ps
$ curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"
```

- Step 5: Let’s re-start the node that we shut down and make all the checks in step 4 again

```
$ docker start <container ID> (from step 3)
$ docker ps
$ curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"
```
- Step 6: Stop the master node

Use the container id of the master node from step 3 to shut it down: 

```$ docker stop <container ID>```

I stop the es01 node in my case:

```$ docker stop 85ff9c5fa7f4```

- Step 7: Check the cluster status with docker and curl

```
$ docker ps
$ curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"
```

- Step 8: Re-start the master node

```$ docker start <container ID> (from step 6)```

I start the es01 node in my case:

```$ docker start 85ff9c5fa7f4```

- Step 9: Check the cluster status with docker and curl again

```
$ docker ps
$ curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"
```

- Step 10: Shut down the cluster

```$ docker-compose down```







