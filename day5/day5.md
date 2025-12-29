```graph TD
    subgraph Internet["インターネット (外部ネットワーク)"]
        User["ユーザー (ブラウザ)"]
    end

    User <--> IGW["Internet Gateway<br/>(玄関口)"]

    subgraph VPC["VPC (10.0.0.0/16)"]
        IGW <--> PublicRT["Route Table (Public)"]

        subgraph PublicSubnet["Public Subnet (10.0.1.0/24)"]
            direction TB
            WebSG["Security Group (Web)"]
            WebServer["EC2: Web Server<br/>(Frontend)"]
            WebServer --- WebSG
        end

        subgraph PrivateSubnet["Private Subnet (10.0.2.0/24)"]
            direction TB
            APISG["Security Group (API)"]
            APIServer["EC2: API Server<br/>(Backend/Go)"]
            APIServer --- APISG
        end

        %% 通信の案内
        PublicRT --- PublicSubnet
        
        %% 内部通信
        WebServer -- "VPC内部通信 (L3/L4)" --> APIServer
    end

    %% 注釈
    style VPC fill:#f9f,stroke:#333,stroke-width:2px
    style PublicSubnet fill:#e1f5fe,stroke:#01579b
    style PrivateSubnet fill:#fff9c4,stroke:#fbc02d
```