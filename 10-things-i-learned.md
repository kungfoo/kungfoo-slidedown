# 10 things I learned working at a SaaS scale-up

---

## ⌚ A bit of history

What did I do while _not_ at ergon?

- Had my half-year parental leave 😉
- Worked at an established company that imploded due to disagreements on strategy
- Proceeded to work a [beekeeper.io](https://beekeeper.io) for 2.5y

---

## 🧮 About beekeeper

A SaaS scale-up providing a work-force communication tool and integration platform for front-line employees.

- ETH Spinoff, located in CH, US, PL and others
- Received $45M of series B funding in 2019
- Over 500 tenants/clients in over 150 countries

---

## 🚀 Techie Stuff

- (Micro)service architecture
- Python, Java, JS, Swift, Kotlin and Scala as first class citizens
- Docker for everything
- Kubernetes for orchestration
- Runs in the cloud

---

## 1: 🔬 (micro)service architectures are distributed systems

Might be stating the obvious, but:

You now have all of these (and more...) problems:

- The network _is_ unreliable
- Computers break, nodes go bad
- Slow consumers
- Things just crash
- Clock drift

---

## 2: 🤖 Automate everything

- Creating a new service including
    - Source files
    - Build system
    - Jenkins pipelines
    - Deployment descriptor files
- Deployments to everywhere
- Rolling back things
- Set up of developer stack including cloning repositories and building _everything_. 

---

## 3: 🌍 Standardization is key

> Cool, so now you have a venerable zoo of applications running on kubernetes...

But every service...

- builds the same way, no matter the implementation language
- ships the same way
- logs to a central place and `stdout`
- has `/health` and `/metrics` endpoints
- can get configured via `ENV` vars

---

### 🇪🇺 GDPR excursion

Most data is kept per `tenant-id` or per `tenant-id` and `user-id`

- Every service owns it's data
- ➡ Every service must have a `/delete/${tenant-id}` endpoint

---

## 4: 🙋‍♀️ 🙋🏾‍♂️ Key infrastructure services need their own OPS team

- Managed DB instances/schemas
- Cloud Nodes
- Kubernetes Clusters
- Message Queues
- Credentials for access to things

---

## 5: 📡 Interservice communication

Best ways to do it in order of preference (descending order of difficulty):

1. Message Passing
2. gRPC
3. REST over http

Consumer driven contract tests would be really nice to have

---

## 6: 💻 Monitor, log  and measure everything

### Logging

- Every request should get a `TRACE_ID` header when entering the system
- Needs to be passed on all the way to every service touching data
- Should be in present in any logging context
- Never log PII

---

### Monitor

- Resource usage
- Queue lengths
- Message rates and dead-letter queuing rates
- Request rates and error rates per service and endpoint
- API Performance per service and endpoint

Aggregate all of that in one place.

---

### Measure

Measure what users _actually_ do to verify your assumptions.

- Usage of features
- Interaction patterns

For example:

- Built a new search engine using drill-down into categories
- Want to know if real life search results are any good

Measure:

- Which item do people click after searching or do they search _again_?
- Do they use the drill-down functions?

---

## 7: ✅ Feature flag/toggle everything

- Should be able to roll back everything or toggle it OFF when it goes south
- All changes must be backwards compatible or include an automatic migration
- Key enabler for continuous delivery to all environemnts

---

## 8: 🪄 Orchestration is fun

- Spin up more pods of one or several type: easy
- Add more nodes: relatively easy
- Swap out a node gone bad: mildly interesting
- Cluster access needs to be limited in time and audited

But not every project needs orchestration.

---

## 9: 🔨 Continuous delivery is cool

... (and can be a bit scary).

- If done right, it deploys in 5 minutes globally to all clusters
- Need to be able to roll back
- Continuously deliver risky changes to your own dogfooding environment, then promote to production
- Deliver everything else straight to production, but _feature flag_ it

--- 

## 10: 📟 Devops means you _own_ the services you build

- You will get paged (during office hours) if it breaks
- Write and share playbooks for other people to fix stuff
- When in doubt: Try turning it off and on again: `kubectl delete pod misbehaving-service-f6731`

---

# ❓ Questions

***