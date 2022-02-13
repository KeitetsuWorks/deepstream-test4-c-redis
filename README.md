# deepstream-test4-app + Redis + RedisInsight

## deepstream-test4-app

* [nvcr.io/nvidia/deepstream:6.0-devel](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/deepstream)
  * `/opt/nvidia/deepstream/deepstream-6.0/sources/apps/sample_apps/deepstream-test4`

## RedisInsight: Redis GUI

* [RedisInsight | The Best Redis GUI](https://redis.com/redis-enterprise/redis-insight/)
  * [Installing RedisInsight on Docker | Redis Documentation Center](https://docs.redis.com/latest/ri/installing/install-docker/)

## My environment

* Ubuntu 20.04.3 LTS (amd64)
* GeForce GTX 1070 Ti
  * Driver Version: 510.47.03
  * CUDA Version: 11.6
* Docker version 20.10.12, build e91ed57
* docker-compose version 1.29.2, build 5becea4c

## Running under Docker

1. Download this project

    ```console
    $ git clone https://github.com/KeitetsuWorks/deepstream-test4-c-redis.git
    $ cd deepstream-test4-c-redis/
    ```

1. Start the docker containers

    ```console
    $ docker-compose up -d
    Creating network "deepstream-test4-c-redis_frontend" with the default driver
    Creating volume "deepstream-test4-c-redis_redisinsight" with default driver
    Creating redis ... done
    Creating nvds         ... done
    Creating redisinsight ... done
    ```

1. Check if Redis is working (Optional)

    ```console
    $ docker exec -it redis /bin/bash
    root@redis:/data# redis-cli
    127.0.0.1:6379> set testKey testValue
    OK
    127.0.0.1:6379> keys *
    1) "testKey"
    127.0.0.1:6379> get testKey
    "testValue"
    127.0.0.1:6379> exit
    root@redis:/data# exit
    ```

1. Run `deepstream-test4-app`

    ```console
    $ xhost +
    $ docker exec -it nvds /bin/bash
    # cd sources/apps/sample_apps/deepstream-test4/
    # deepstream-test4-app -i ../../../../samples/streams/sample_720p.h264 -p /opt/nvidia/deepstream/deepstream-6.0/lib/libnvds_redis_proto.so --conn-str="redis;6379" -t ds-meta -s 0
    ```
1. Check Redis database

    * CUI

        ```console
        $ docker exec -it redis /bin/bash
        root@redis:/data# redis-cli
        127.0.0.1:6379> keys *
        1) "ds-meta"
        2) "testKey"
        ```

    * RedisInsight

        Visit `http://localhost:8001` in your web browser.

## Related projects

* [deepstream-test5-app + Apache Kafka + Node-RED](https://gist.github.com/KeitetsuWorks/8713a6ddbb3ab220110338f26f3e35ef)
