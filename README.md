# Hi, I'm Uday Jajala 👋

I'm a Full Stack Java Developer having experience in building scalable, production-grade web applications. I've worked across industries — banking, automotive, and consumer electronics — and I genuinely enjoy the challenge of designing systems that are both reliable and maintainable.

Right now I'm in the US, where I'm part of a team building a loan evaluation platform from the ground up. 

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

---

## 🏗️ Featured Project: Loan Evaluation Platform 

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


---

## 🤝 How I Work

I'm a big believer in code reviews — not just for catching bugs, but for keeping the team aligned on how we're solving problems. I try to be the kind of person who asks questions during reviews rather than just leaving comments, because the conversation is usually more valuable than the feedback alone.

I've worked in Agile teams for most of my career, so sprint ceremonies feel natural to me — standups, planning, retros. I actually like retros when they're done well, because there's always something the team could do better, and it's good to have a structured space to name it.

When I'm onboarding newer developers or explaining architectural decisions, I try to lead with "why" before "what." The implementation details change. The reasoning behind them usually doesn't.

---

## 📬 Get in Touch

- 📧 udaychari428@gmail.com
- 📱 +1 (603) 825-8995
- 📍 New Jersey, USA

---

*Thanks for stopping by. Feel free to explore the repos — I try to keep things reasonably documented.*
