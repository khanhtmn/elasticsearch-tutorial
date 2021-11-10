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
