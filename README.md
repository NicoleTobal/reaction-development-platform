# Reaction Commerce - Customized version

This is a customized version of Reaction Commerce.

For Reaction commerce documentation go to: https://docs.reactioncommerce.com/docs/intro.html.
For original Reaction commerce code go to: https://github.com/reactioncommerce/reaction-development-platform.

## Hardware prerequisites

According to Reaction's official documentation 2GB is needed for running all containers and 6GB to build all images.

## Mounting all services in localhost

1. Build the images for reaction-admin, reaction-identity and api:

```
docker-compose -f docker-compose.example.yml build
```

2. Run docker containers for hydra and reaction images built:

```
docker-compose -f docker-compose.example.yml up -d
```


## Deploying in production

> Prerequisite: have a .env file in reaction-admin, reaction-identity, reaction, reaction-hydra and root folder with the same environment variables than in the .env.example files but with real values.

1. Run a mongo instance on the server:

```
docker-compose -f docker-compose-mongo.yml up -d
```

2. Connect to docker container:

```
docker exec -it reaction-development-platform_mongo_1 bash
```

3. Connect to mongo instance:

```
mongo --username USERNAME_FROM_.ENV --password PASSWORD_FROM_.ENV
```

4. Initiate replica set and create user for accesing the databases:

```
rs.initiate();
db.createUser(
  {
    user: "user",
    pwd: "pwd",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
);
```

5. Build the images for reaction-admin, reaction-identity and api, according to the changes that need to be uploaded (they can also be built somewhere else and pushed to a dockerhub repository):

```
docker-compose build
```

6. Run docker containers for hydra and reaction images built:

```
docker-compose up -d
```