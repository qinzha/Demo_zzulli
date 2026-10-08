# Demo_zzulli
demonstration for Zzulli

Markdown
```mermaid
graph TD
    %% Users
    Student([Student])
    Teacher([Teacher])
    Admin([Admin])

    subgraph EduPlatform ["Online Education Platform System Boundary"]
        %% Frontend Containers
        WebSPA["Web Frontend (SPA)<br/>[React / Next.js]<br/>Provides web interface for all user roles"]
        MobileApp["Mobile App<br/>[React Native / Flutter]<br/>Provides mobile learning experience for Students"]

        %% Backend Containers
        API["REST & WebSocket API Gateway<br/>[Node.js / FastAPI / Spring Boot]<br/>Handles auth, business logic, course workflows, and streaming metadata"]
        Worker["Background Job Worker<br/>[Python / Node.js Queue Worker]<br/>Processes async jobs (video encoding, email batches, report generation)"]

        %% Data Persistence
        DB[("Primary Database<br/>[PostgreSQL]<br/>Stores users, courses, enrollments, grades, and payment logs")]
        Cache[("In-Memory Cache<br/>[Redis]<br/>Stores user sessions, rate limits, and course catalog cache")]
        Queue[("Message Queue<br/>[RabbitMQ / Redis Streams]<br/>Buffers async background tasks")]
    end

    %% External Systems
    #Stripe["Stripe Payment Gateway"]
    #SendGrid["SendGrid Email Service"]
    #CDN["AWS S3 + CloudFront CDN"]

    %% Connections
    Student -->|HTTPS| WebSPA
    Student -->|HTTPS| MobileApp
    Teacher -->|HTTPS| WebSPA
    Admin -->|HTTPS| WebSPA

    WebSPA -->|HTTPS / REST / WS| API
    MobileApp -->|HTTPS / REST| API

    API -->|SQL Read/Write| DB
    API -->|Read/Write Cache| Cache
    API -->|Publish Tasks| Queue
    
    Queue -->|Consume Jobs| Worker
    Worker -->|SQL Updates| DB
    Worker -->|Trigger Emails| SendGrid
    Worker -->|Upload Video Chunks| CDN

    API -->|Payments| Stripe
    API -->|Generate Signed URLs| CDN

```
