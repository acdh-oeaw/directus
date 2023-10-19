# Directus

This repo is based on the [Docker Compose based Self-Hosting Quickstart Directus documentation](hhttps://docs.directus.io/self-hosted/docker-guide.html "Docker Compose based Self-Hosting Quickstart Directus documentation") and contains configuration files needed for creating deployment pipeline based on GitHub actions that deploys it together with official [Directus](https://hub.docker.com/r/directus/directus "Directus") and [Redis](https://hub.docker.com/_/redis/ "Redis") Docker images as Directus stack to ACDH-CH Kubernetes environment. 

Due to specific ACDH-CH environment that is using centralized PostgresDB, dedicated PostgresDB Docker image is not used with this setup.

Environment variables needed for the Directus stack:

|Name|Required|Type|Level|Description|
|----|:------:|----|:---:|-----------|
|KUBE_CONFIG|:white_check_mark:|Secret|Org|base64 encoded K8s config file. Usually set at the Org level and shared by all (public) repositories. |
|C2_KUBE_CONFIG|:white_check_mark:|Secret|Org|If you deploy using the workflow for the second cluster the C2_ variant is used. |
|KUBE_NAMESPACE|:white_check_mark:|Variable|Repo/Env|The K8s namespace the deployment should be installed to. |
|DIRECTUS_PUBLIC_URL|:white_check_mark:|Variable|Env|The URI that should be configured for access to the service. |
|DIRECTUS_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|CACHE_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|K8S_SECRET_KEY|:white_check_mark:|Secret|Repo/Env|Secret key |
|K8S_SECRET_SECRET|:white_check_mark:|Secret|Repo/Env|Secret |
|K8S_SECRET_DB_CLIENT|:white_check_mark:|Secret|Repo/Env|Type of an external DB service. |
|K8S_SECRET_DB_HOST|:white_check_mark:|Secret|Repo/Env|Hostname of an externa PostgresDB service. |
|K8S_SECRET_DB_PORT|:white_check_mark:|Secret|Repo/Env|Port of an external PostgresDB service. |
|K8S_SECRET_DB_USER|:white_check_mark:|Secret|Env|Username for the PostgresDB database. |  
|K8S_SECRET_DB_PASSWORD|:white_check_mark:|Secret|Env|Password for the PostgresDB database. |
|K8S_SECRET_DB_DATABASE|:white_check_mark:|Secret|Env|Name of the PostgresDB database to use. |  
|K8S_SECRET_CACHE_ENABLED|:white_check_mark:|Secret|Repo/Env|Set to true to enable caching. |
|K8S_SECRET_CACHE_STORE|:white_check_mark:|Secret|Repo/Env|Service used for caching. Should be set to redis. |
|K8S_SECRET_REDIS|:white_check_mark:|Secret|Env|URL of Redis service. Should be set to redis://cache:6379 |  
|K8S_SECRET_ADMIN_EMAIL|:white_check_mark:|Secret|Env|E-mail address of admin user. |
|K8S_SECRET_ADMIN_PASSWORD|:white_check_mark:|Secret|Env|The password for the admin user. |  

### How to deploy new Directus instance

1. Create Kubernetes namespace.
2. Create PostgresDB database.
3. Create domain for the service and point it to the cluster.
4. Create new GitHub environment for the service that should have the same name as new GitHub branch that will be used for the new Directus instance.
5. Add GitHub environment variables and secrets described in the table above.
6. Create a new branch in this repo that should have tha same name as the Kubernetes namespace created in the first step.
8. The newly created GitHub branch will trigger the Github pipeline that will deploy new Directus stack to the Kubernetes Cluster.
