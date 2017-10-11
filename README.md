# About this Repo

This is the Git repo of the Docker [official image](https://docs.docker.com/docker-hub/official_repos/) for [kong](https://registry.hub.docker.com/_/kong/). See [the Docker Hub page](https://registry.hub.docker.com/_/kong/) for the full readme on how to use this Docker image and for information regarding contributing and issues.

The full readme is generated over in [docker-library/docs](https://github.com/docker-library/docs), specifically in [docker-library/docs/kong](https://github.com/docker-library/docs/tree/master/kong).

See a change merged here that doesn't show up on the Docker Hub yet? Check [the "library/kong" manifest file in the docker-library/official-images repo](https://github.com/docker-library/official-images/blob/master/library/kong), especially [PRs with the "library/kong" label on that repo](https://github.com/docker-library/official-images/labels/library%2Fkong). For more information about the official images process, see the [docker-library/official-images readme](https://github.com/docker-library/official-images/blob/master/README.md).

<!-- THIS FILE IS GENERATED BY https://github.com/docker-library/docs/blob/master/generate-repo-stub-readme.sh -->


## 注意

第一次安装需要进行数据库初始化

1. 修改docker-compose文件，把POSTGRES_DB随便写一个，或者不写
```
kong-database:
  image: postgres:9.4
  ports:
    - 5432
  environment:
    - POSTGRES_USER=kong
    - POSTGRES_DB=kongxxxxx
```    
2. docker-compose up -d
3. docker run -it kong bash
4. CREATE DATABASE kong OWNER kong;
5. 修改/etc/kong/kong.conf.default中datastore那一列 
6. kong migrations up -c /etc/kong/kong.conf.default
7. 修改docker-compose文件，设置- POSTGRES_DB=kong
```
kong-database:
  image: postgres:9.4
  ports:
    - 5432
  environment:
    - POSTGRES_USER=kong
    - POSTGRES_DB=kong
``` 
完成

8. psql -h 172.29.231.80 -U kong -d kong
9. \l 查看数据库
10. \dt 查看所有表

```
                   List of relations
 Schema |             Name              | Type  | Owner 
--------+-------------------------------+-------+-------
 public | acls                          | table | kong
 public | apis                          | table | kong
 public | basicauth_credentials         | table | kong
 public | cluster_events                | table | kong
 public | consumers                     | table | kong
 public | hmacauth_credentials          | table | kong
 public | jwt_secrets                   | table | kong
 public | keyauth_credentials           | table | kong
 public | oauth2_authorization_codes    | table | kong
 public | oauth2_credentials            | table | kong
 public | oauth2_tokens                 | table | kong
 public | plugins                       | table | kong
 public | ratelimiting_metrics          | table | kong
 public | response_ratelimiting_metrics | table | kong
 public | schema_migrations             | table | kong
 public | ssl_certificates              | table | kong
 public | ssl_servers_names             | table | kong
 public | targets                       | table | kong
 public | ttls                          | table | kong
 public | upstreams                     | table | kong
(20 rows)

```