# New app check List

This is a check list for the spring applications on this project

- [ ] Create spring-boot application in Kotlin
- [ ] Set up a module for Functional tests
- [ ] Set up actuator with `/info`, `/health` and `/metrics`
     - [ ] endpoints should be exposed under `/private`
     - [ ] create unit tests to ensure endpoints exist
     - [ ] create FT also to ensure endpoints exist
     - Info:
        - [ ] The `info` endpoint should include `build` details (configured on gradle config file in the module)
        - [ ] The `info` endpoint should include `git` details with the plugin `com.gorylenko.gradle-git-properties`
     - Metrics:
        - [ ] The metrics should be exposed using Micrometer with the dependency: `io.micrometer:micrometer-registry-prometheus` 
- [ ] Set up Open API and Swagger UI (https://springdoc.org/)
     - [ ] Set up unit tests and Functional tests to ensure the endpoints exist
     - [ ] Set up the app version on the OpenApi/Swagger from versioning plugin
- [ ] Set up axion versioning plugin (https://github.com/allegro/axion-release-plugin)
- [ ] Update Readme
    - [ ] include instructions to run and test the project
    - [ ] include instructions about the project dependencies and what is necessary to run it locally
- [ ] Update Insomnia with requests and environment variable (https://github.com/groot-mg/insomnia-requests)
- [ ] (Optional) Set up Eureka client
- [ ] (Optional) Set up security (Oauth2 integration with Keycloak)
- [ ] Set up Github Actions
   - [ ] Build and tests
   - [ ] Sonar Analyses
- [ ] Update prometheus to scrape from the new app (https://github.com/groot-mg/observability-tools)
- [ ] Add application on the local setup (https://github.com/groot-mg/docker-local-setup)
- [ ] Include the new app in the Grafana health dashboards
- [ ] Set up the app on API Gateway to forward requests (https://github.com/groot-mg/gateway)