# Deploy Blitz

<!--Writerside adds this topic when you create a new documentation project.
You can use it as a sandbox to play with Writerside features, and remove it from the TOC when you don't need it anymore.-->

## Create Deploy Logic

```mermaid
sequenceDiagram
    Deployment Manager -->> Webhook Service: set webhook name
    Webhook Service -->> Git Service: Git pull/clone
    Git Service -->> Script Locator: search path for script
    Script Locator -->> Script Verifier: Verify integrity of sh/yaml
    Script Verifier -->> Database: store sh/yaml script
    Database -->> Runtime Engine: script as string
    Runtime Engine -->> Webhook Service: ws://Daemon Run Job Logs
    Webhook Service -->> Deployment Manager: localhost:8080/webhook/{name}
```

- [X] Create Deploy
- [x] Create Webool
- [x] Git with Token git clone `https://<username>:${GITHUB_TOKEN}@github.com/username/repository.git`

```plantuml
@startmindmap
* Deployment Manager
    * set webhook name
        * Webhook Service
            * error
            * Git pull/clone
                * Git Service
                    * error
                    * search path for script
                        * Script Locator
                            * error
                            * Verify integrity of sh/yaml
                                * Script Verifier
                                    * error
                                    * store sh/yaml script
                                        * Database
                                            * error
                                            * script as string
                                                * Runtime Engine
                                                    * error
                                                    * ws://Daemon Run Job Logs
                                                        * Webhook Service
                                                            * error
                                                            * localhost:8080/webhook/{name}
                                                                * Deployment Manager
@endmindmap

```
### Database Structure

```plantuml
@startuml

class deploy {
    id : INTEGER
    ..
    app_name: VARCHAR(255)
    version: INTEGER
    status: BOOLEAN DEFAULT TRUE
    endpoint_path: VARCHAR(255)
    script: TEXT
    creation_date: DATETIME
    updated_date: DATETIME
    
    .. UNIQUE ..
    
    app_name
    
}

class deploy_history {
    id : INTEGER
    ..
    deploy_id : INTEGER
    endpoint_path: VARCHAR(255)
    script: TEXT
    creation_date: DATETIME
    updated_date: DATETIME
    .. UNIQUE ..
    
    deploy_id
    
}

deploy -right-> deploy_history : "deploy_id"
@enduml

```