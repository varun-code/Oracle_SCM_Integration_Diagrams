flowchart LR

    %% =====================================================
    %% EXTERNAL ACTORS
    %% =====================================================

    USERS["Users / Partners"]

    %% =====================================================
    %% T-MOBILE EDGE
    %% =====================================================

    subgraph TMOBILE["T-Mobile Enterprise Platform"]

        WAF["OCI WAF<br/>DDoS / OWASP"]

        APIGW["OCI API Gateway<br/>Authentication / Routing / Rate Limit"]

        subgraph SERVICES["T-Mobile Microservices"]

            ORDER["Order Service"]

            INVENTORY["Inventory Service"]

            SHIPMENT["Shipment Service"]

            PROCUREMENT["Procurement Service"]

        end

        KAFKA[["Kafka<br/>Enterprise Event Streaming"]]

        SCM_INT["SCM Integration Service"]

        DB[("T-Mobile<br/>Order Database")]

    end


    %% =====================================================
    %% ORACLE SCM
    %% =====================================================

    subgraph ORACLE["Oracle SCM Cloud"]

        AUTH["Oracle Authorization Server<br/>OAuth2"]

        SCM_API["Oracle SCM<br/>REST APIs"]

        SCM_DATA[("Oracle SCM<br/>Business Data")]

    end


    %% =====================================================
    %% MAIN FLOW
    %% =====================================================

    USERS --> WAF
    WAF --> APIGW

    APIGW --> ORDER
    APIGW --> INVENTORY
    APIGW --> SHIPMENT
    APIGW --> PROCUREMENT

    ORDER --> DB

    ORDER --> KAFKA
    INVENTORY --> KAFKA
    SHIPMENT --> KAFKA
    PROCUREMENT --> KAFKA

    KAFKA --> SCM_INT

    SCM_INT -->|"OAuth2 Client Credentials"| AUTH

    AUTH -->|"Bearer Access Token"| SCM_INT

    SCM_INT -->|"Authenticated REST APIs"| SCM_API

    SCM_API --> SCM_DATA


    %% =====================================================
    %% COLORS
    %% =====================================================

    classDef external fill:#E3F2FD,stroke:#1565C0,stroke-width:2px,color:#0D47A1;

    classDef edge fill:#FFF3E0,stroke:#EF6C00,stroke-width:2px,color:#E65100;

    classDef service fill:#E8F5E9,stroke:#2E7D32,stroke-width:2px,color:#1B5E20;

    classDef kafka fill:#FFF8E1,stroke:#F9A825,stroke-width:2px,color:#6D4C00;

    classDef database fill:#F3E5F5,stroke:#7B1FA2,stroke-width:2px,color:#4A148C;

    classDef integration fill:#E0F7FA,stroke:#00838F,stroke-width:2px,color:#006064;

    classDef oracle fill:#FCE4EC,stroke:#C2185B,stroke-width:2px,color:#880E4F;

    classDef auth fill:#FFFDE7,stroke:#827717,stroke-width:2px,color:#5F5B00;


    class USERS external;

    class WAF,APIGW edge;

    class ORDER,INVENTORY,SHIPMENT,PROCUREMENT service;

    class KAFKA kafka;

    class DB database;

    class SCM_INT integration;

    class AUTH auth;

    class SCM_API,SCM_DATA oracle;
