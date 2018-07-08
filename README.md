# cf-gcp-cloudsql-boot

An example spring boot application which uses `Spring Cloud GCP` to configure on `Cloud Foundry` a `GCP Cloud SQL` provisioned by `GCP Service Broker`.

https://github.com/GoogleCloudPlatform/gcp-service-broker/blob/master/docs/use.md
https://docs.spring.io/spring-cloud-gcp/docs/1.1.0.BUILD-SNAPSHOT/reference/htmlsingle/#_cloud_foundry

## deploy
```
cf create-service google-cloudsql-mysql standard cloudsql -c '{"authorized_networks": "0.0.0.0/0", "region": "europe-west2"}'
./mvnw clean package
cf push --no-start
cf bind-service gcp-cloudsql-boot cloudsql -c '{"role": "editor"}'
cf start gcp-cloudsql-boot
```

## test

```
curl -X POST -H "Content-Type: application/json" -d '{"title":"My first note", "content":"a great content"}' https://gcp-cloudsql-boot.<domain>/api/notes/
curl https://gcp-cloudsql-boot.<domain>/api/notes/
curl https://gcp-cloudsql-boot.<domain>/api/notes/1
curl -X PUT -H "Content-Type: application/json" -d '{"title":"My first note", "content":"a great content update"}' https://gcp-cloudsql-boot.<domain>/api/notes/1
```

## credits

Based https://www.callicoder.com/spring-boot-rest-api-tutorial-with-mysql-jpa-hibernate/