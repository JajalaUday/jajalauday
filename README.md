# Hi, I'm Uday Jajala 👋

I'm a Full Stack Java Developer with around 7 years of experience building scalable, production-grade web applications. I've worked across industries — banking, healthcare, automotive, and consumer electronics — and I genuinely enjoy the challenge of designing systems that are both reliable and maintainable.

Right now I'm working at Citi Bank in the US, where I'm part of a team building a loan evaluation platform from the ground up. Before that I was at CVS Pharmacy, KIA Motors, and Samsung. Each role pushed me in different directions, and looking back, I think that variety is what made me a more well-rounded engineer.

---

## 🔧 What I Work With

**Languages & Frameworks**
- Java, Spring Boot, Spring MVC, Spring Security (JWT / OAuth2), Spring JPA, Hibernate
- Angular (14, 16) — TypeScript, RxJS, reactive forms, custom directives
- RESTful APIs, Microservices Architecture, Apache Kafka

**Cloud & DevOps**
- AWS — S3, EC2, EKS, Lambda, DynamoDB, CloudWatch
- Docker, Kubernetes, Jenkins CI/CD, Maven, Gradle
- SonarQube, Splunk, Snyk, Artifactory

**Databases**
- Oracle DB, SQL Server, MongoDB, Neo4j

**Testing**
- JUnit, Mockito, JaCoCo, Postman, RestAssured

**Methodologies**
- Agile / Scrum, TDD, CI/CD, Code Reviews

---

## 💼 Work Experience

### Full Stack Java Developer — Citi Bank, USA
**June 2025 – Present**

This is easily the most complex project I've worked on. We built a loan evaluation platform using a microservices architecture — customer service, loan calculation service, document management, audit & compliance, and payments all running as separate services communicating through REST APIs and Kafka for async events.

A few things I'm particularly proud of here:

- Built the core loan product logic for Home, Auto, and Personal loans — including integrating with external interest rate APIs to pull live market data into our calculations
- Designed and implemented multipart file upload APIs that support PDF, JPEG, PNG, and DOC formats, with files going straight to AWS S3 with server-side encryption
- Built the customer-facing Angular dashboard with real-time loan status updates over WebSocket and a notification center for application events
- Created a document upload portal with drag-and-drop, inline file preview, and status tracking (Pending → Verified → Approved / Rejected)
- Set up Lambda functions specifically for virus scanning incoming documents before they ever touch storage
- Implemented DynamoDB Streams to trigger downstream notifications whenever data changes
- Built out CloudWatch dashboards so the team has real-time visibility into what the system is doing
- Deployed everything on EKS with Docker, auto-scaling groups on EC2, and an Application Load Balancer handling traffic distribution
- Maintained a fully automated Jenkins pipeline (more on that below) covering everything from code commit to blue/green production deploys

---

### Java Developer — CVS Pharmacy, USA
**January 2024 – May 2025**

At CVS I worked heavily on the backend, specifically around event-driven architecture using Kafka. I designed producers and consumers for high-volume event streaming and set up topics, partitions, and consumer groups for fault-tolerant data processing.

One thing that stuck with me from this role was using Splunk to track down Kafka message failures — digging into offset IDs to figure out exactly where messages died and then building logic to reprocess them cleanly. It's unglamorous work but the kind of thing that keeps a system trustworthy.

I also got to work with Neo4j here, modeling complex relationships using Cypher queries inside Spring Boot microservices. It's a completely different way of thinking about data compared to relational databases, and I found it really interesting.

- Built and containerized microservices with Docker, deployed on Kubernetes with rolling updates through Jenkins
- Developed Angular UI components with two-way data binding, reactive forms, and custom directives
- Implemented security using Spring Security with JWT tokens and stateless authentication

---

### Full Stack Java Developer — KIA Motors, India
**August 2021 – July 2023**

KIA was my first experience building at scale in a product company setting. I worked on web applications serving real customers, which meant performance and reliability actually mattered in a tangible way.

- Helped drive a 20% increase in user engagement through UI optimizations in Angular — collaborated closely with the design team on that one
- Built AWS Lambda functions for serverless computing, which reduced operational overhead noticeably
- Developed and maintained database objects (tables, views, stored procedures, triggers) in SQL Server
- Wrote unit and integration tests with JUnit that improved overall test coverage across the team

---

### Java Developer — Samsung, India
**December 2018 – August 2021**

This is where I started. Samsung gave me a strong foundation in the Spring ecosystem — Spring MVC, Dependency Injection, Spring JDBC — and I built several internal web applications here using JSP, JavaScript, jQuery, and Bootstrap on the frontend.

- Integrated MongoDB for schema-less, high-volume data workloads
- Used Hibernate and HQL for data access layer operations
- Set up Swagger documentation for APIs and maintained YAML config files
- Configured AWS CloudWatch monitoring for EC2 instances
- Helped migrate legacy applications to modern frameworks, improving performance by around 20%
- Collaborated with UI/UX to redesign interfaces that increased user retention by 15%

---

## 🏗️ Featured Project: Loan Evaluation Platform (Citi Bank)

This is my most recent and most complex project, so it's worth going into more depth.

**The Problem:** Evaluate loan applications at scale across multiple loan types, with proper document management, compliance tracking, and real-time status visibility for customers.

**The Architecture:**

```
Customer → API Gateway → [Customer Service]
                       → [Loan Calculation Service] ← External Rate APIs
                       → [Document Service] → AWS S3
                       → [Audit & Compliance Service]
                       → [Payment Service]
                            ↕ (Kafka for async events)
                       → [Notification Service] ← DynamoDB Streams
```

**Key Technical Decisions:**

- Used Kafka for async communication between services so that things like document uploads and audit logging don't block the main loan processing flow
- Chose AWS S3 with server-side encryption for document storage — bucket policies lock down access so only the right services can read/write
- WebSocket connections on the frontend mean customers see their loan status update in real time without polling
- Lambda functions handle virus scanning as a separate concern — files get scanned before anything else touches them
- Blue/green deployments mean we can push to production with zero downtime and roll back instantly if something goes wrong

---

## ⚙️ CI/CD Pipeline

One thing I've put real effort into is our Jenkins pipeline. Here's how it flows:

1. **Code Commit** — Developer pushes to a feature branch (e.g., `feature/credit-score-integration`), which triggers a webhook to Jenkins
2. **Maven Build** — Jenkins compiles the Java code and packages it into a JAR. If it doesn't compile, we stop right there
3. **Unit Tests** — JUnit and Mockito tests run, with JaCoCo tracking coverage. We maintain over 85% coverage. Any failure stops the pipeline
4. **SonarQube** — Code quality gate requires an A rating with zero critical issues. Catches code smells, security vulnerabilities, duplication, and bugs
5. **Docker Build** — Multi-stage Dockerfiles produce lightweight images (under 200MB), tagged with version and build number
6. **Artifactory** — Verified images get pushed to our private Docker registry, separated by environment (dev/QA/prod)
7. **Deploy to Dev** — Automatic rollout to the dev EKS cluster using `kubectl set image`, pods replaced one by one
8. **Integration Tests** — End-to-end API tests using RestAssured against the dev environment (loan application flows, document uploads, status checks)
9. **Deploy to QA** — QA team does manual testing here — UI scenarios, edge cases, regression
10. **Production (Blue/Green)** — After QA approval:
    - New version deploys to "green" environment, "blue" stays live
    - Smoke tests run on green
    - Load balancer flips traffic to green instantly
    - Blue stays up as an instant rollback option
    - Once green is confirmed stable, blue scales down

Slack notifications go out at each stage. If anything fails, the team gets an alert with logs. On a successful prod deploy, we post what version went out and who approved it.

---

## 🎓 Education

**M.S. in Computer Science**
Rivier University, New Hampshire, USA — *Sept 2023 – May 2025*

**B.E. in Electronics and Communication Engineering**
Geethanjali College of Engineering and Technology, Telangana, India — *June 2015 – May 2019*

---

## 🤝 How I Work

I'm a big believer in code reviews — not just for catching bugs, but for keeping the team aligned on how we're solving problems. I try to be the kind of person who asks questions during reviews rather than just leaving comments, because the conversation is usually more valuable than the feedback alone.

I've worked in Agile teams for most of my career, so sprint ceremonies feel natural to me — standups, planning, retros. I actually like retros when they're done well, because there's always something the team could do better, and it's good to have a structured space to name it.

When I'm onboarding newer developers or explaining architectural decisions, I try to lead with "why" before "what." The implementation details change. The reasoning behind them usually doesn't.

---

## 📬 Get in Touch

- 📧 udaychari428@gmail.com
- 📱 +1 (603) 825-8995
- 📍 New Hampshire, USA

---

*Thanks for stopping by. Feel free to explore the repos — I try to keep things reasonably documented.*
