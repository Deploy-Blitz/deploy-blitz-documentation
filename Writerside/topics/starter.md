# Deploy Blitz

<!--Writerside adds this topic when you create a new documentation project.
You can use it as a sandbox to play with Writerside features, and remove it from the TOC when you don't need it anymore.-->

## Create Deploy Logic

```mermaid
sequenceDiagram
    Create Deploy -->> Create Webhook: set webhook name
    Create Webhook -->> Upload Script: Verify integrity file sh/yaml
    Upload Script -->> Database: store sh/yaml script
    Database -->> Create Webhook: OK
    Create Webhook -->> Create Deploy: localhost:8080/webhook/{name}
```

```plantuml
@startmindmap
* create deploy
    * set name
        * new name
            * valid script
                * store in database
                * error
        * error
@endmindmap
```
### Database Structure

```plantuml
@startuml

class deploy {
    id : INTEGER
    ..
    app_name: VARCHAR
    version: INTEGER
    status: BOOLEAN DEFAULT TRUE
    endpoint_path: VARCHAR(10)
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
    endpoint_path: VARCHAR(10)
    script: TEXT
    .. UNIQUE ..
    
    deploy_id
    
}

deploy -right->deploy_history
@enduml 
```