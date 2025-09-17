 # Application consists of a Web Server written in Flask. A UI front end written in React. A database layer consisting of Postgres.
<br>
<br>

## ON PREM

```mermaid
flowchart LR
    subgraph Clients
        C1[Client]
        C2[Client]
    end

    CF[Cloudflare CDN]
    CFWAF[Cloudflare WAF]
    CFLB[Cloudflare LB]

  subgraph FrontEnd
    FE1[Front End]
    FE2[Front End]
  end 

subgraph MiddleTier
    MT[Middle Tier]
end

subgraph Database
    DB[Postgres DB]
end

    C1 --> CF
    C2 --> CF

    CF --> CFWAF
    CFWAF --> CFLB

    CFLB --> FE1
    CFLB --> FE2

    FE1 --> MT
    FE2 --> MT

    MT --> DB
```
<br>

1) **Clients**: user's accessing the app. Can be multiple users for anywhere in the world
2) **Content Delivery**
   - [Cloudflare CDN](https://www.cloudflare.com/application-services/products/cdn/): handles global content delivery, 
3) **Firewall**
   - [Cloudflare WAF](https://www.cloudflare.com/application-services/products/waf/) is a Web Application firewall. 
4) **Load Balancer**
    - [Cloudflare Load Balancing](https://www.cloudflare.com/application-services/products/load-balancing/) handles load balancing 
5) **Front End and Middle Tier**
    - Built on on prem servers.
6) **Database**
    - Deploy PostgreSQL on an on prem server. 

<br>
<br>


## Azure and CloudFront IAAS Architecture

```mermaid
flowchart LR
    subgraph Clients
        C1[Client]
        C2[Client]
    end

    CF[Cloudflare CDN]
    CFWAF[Cloudflare WAF]
    CFLB[Cloudflare LB]

    subgraph Virtual Machine Scale Set 
      subgraph FrontEnd
          FE1[Azure VM]
          FE2[Azure VM]
      end

      subgraph MiddleTier
          MT1[Azure VM - Scale Set]
      end
    end

    subgraph Database
        DB[(Postgres on Azure VM)]
    end

    C1 --> CF
    C2 --> CF
    
    CF --> CFWAF

    CFWAF --> CFLB

    CFLB --> FE1
    CFLB --> FE2

    FE1 --> MT1
    FE2 --> MT1

    MT1 --> DB
```

<br>

1) **Clients**: user's accessing the app. Can be multiple users for anywhere in the world
2) **Content Delivery**
   - [Cloudflare CDN](https://www.cloudflare.com/application-services/products/cdn/): handles global content delivery, 
3) **Firewall**
   - [Cloudflare WAF](https://www.cloudflare.com/application-services/products/waf/) is a Web Application firewall. 
4) **Load Balancer**
    - [Cloudflare Load Balancing](https://www.cloudflare.com/application-services/products/load-balancing/) handles load balancing 
5) **Front End and Middle Tier**
    - [Azure Virtual Machine](https://azure.microsoft.com/en-us/products/virtual-machines): for building, deploying and running the web apps. Built in [Virtual Machine Scale Set](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview) for scaling.
6) **Database**
    - Deploy PostgreSQL on a VM. 

**NOTE**: tried using none Azure tools to avoind vendor lockin. 
<br>
<br>

## Azure PAAS Architecture

```mermaid
flowchart LR
  subgraph Clients
    C1["Client"]
    C2["Client"]
  end

  AFD["Azure Front Door 
          (CDN, WAF, LB)"]

  subgraph FrontEnd
    FE1["Azure App Service - Web App"]
    FE2["Azure App Service - Web App"]
  end

  subgraph MiddleTier
    MT["Azure App Service - API App / Azure Functions"]
  end

  subgraph Database
    DB["Azure Database for PostgreSQL (PaaS)"]
  end

  C1 --> AFD
  C2 --> AFD

  AFD --> FE1
  AFD --> FE2

  FE1 --> MT
  FE2 --> MT
  MT --> DB

```
<br>

1) **Clients**: user's accessing the app. Can be multiple users for anywhere in the world
2) **Content Delivery, Firewall and Load Balancing** 
   - [Azure Front Door](https://learn.microsoft.com/en-us/azure/frontdoor/front-door-overview): is a **fully managed SaaS** that handles global content delivery, [firewall](https://learn.microsoft.com/en-us/azure/web-application-firewall/afds/afds-overview) (integrated with Azure Web Application firewall) and [load balancing](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)
3) **Front End**
    - [Azure App Service](https://azure.microsoft.com/en-us/products/app-service): is a **fully managed SaaS** for building, deploying, and scaling web apps. 
4) **Back Middle Tier**
    - [Azure App Service](https://azure.microsoft.com/en-us/products/app-service): is a **fully managed SaaS** for building, deploying, and scaling web apps. 
5) **Database**
    - [Azure Database for PostgreSQL](https://azure.microsoft.com/en-us/products/postgresql) is a **fully managed** PostgreSQL db. 


