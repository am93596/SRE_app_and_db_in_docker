# Making our app microservice work with mongodb

> Prerequisites: follow the steps in [this repo](https://github.com/am93596/SRE_Microservices) for setting up an app container.  
- `docker pull mongo`
- `docker run -d -p 27017:27017 mongo`
- Create a new folder called `db` and add the `mongod.conf` file. Then add a `Dockerfile` with the following contents:
```dockerfile
FROM mongo

WORKDIR usr/src/db

COPY mongod.conf /etc/

EXPOSE 27017
```
- Then create a file in the app folder called `docker-compose.yml` with the following content:
```yaml
version: "2"
services:
  db:
    build:
      context: ../db
    ports:
      - "27017:27017"

  app:
    build:
      context: .
    ports:
      - "80:3000"
    links:
      - db
    environment:
      - DB_HOST=mongodb://db:27017/posts
```
- In the app Dockerfile, uncomment the line that says `RUN node seeds/seed.js`
- Run the `docker-compose` from inside the app folder using `docker-compose up -d`
- Use `docker-compose down` to delete the containers created by the file
