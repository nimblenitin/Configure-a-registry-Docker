# Configure-a-registry-Docker

Following are the steps to configure a Docker registry-

```

1. Create an environment variable named REGISTRY_variable where variable is the name of the configuration option and _ represents indentation level. Use the following command to override the root directory value by specifying a configuration variable from the environment using the -e argument:
$ sudo docker run -d -p 5000:5000 --restart=always --name registry -e \
> REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/somewhere registry:2

2. List the running containers to check the newly created registry with new Storage filesystem root directory
$ sudo docker ps

3. Override the entire configuration file by creating a new file named config.yml
$ cd /etc/docker
$ sudo vi config.yml

version: 0.1
log:
  fields:
    service: registry
storage:
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
auth:
  htpasswd:
    realm: basic-realm
    path: /etc/registry
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
    
4. Use the following command to run a registry container with a new config.yml file specified in it:
$ sudo docker run -d -p 8000:80 --restart=always --name registry \
> -v `pwd`/config.yml:/etc/docker/config.yml \
> registry:2

5. You can use the below command to check the running containers.
$ sudo docker ps

```
