## Vertical Scaling

**Formal Definition:**  
Vertical scaling (also called _scaling up_) is the process of increasing the capacity of a single machine by adding more powerful hardware — more CPU cores, RAM, faster storage, or a better network interface — to handle increased load.

**Concept Explanation:**  
You have one server. It starts struggling under load. Instead of adding more servers, you upgrade the existing one. It's the simplest form of scaling because your architecture doesn't change — the same server just becomes more powerful. No changes to code, no distribution logic, no coordination between machines. But every machine has a physical ceiling, and you'll eventually hit it.

**When it breaks down and why:**

- There's a hard upper limit — you can only put so much RAM or CPU into one machine.
- Downtime during upgrades is often unavoidable.
- It becomes disproportionately expensive at the high end (doubling performance can cost 10x more).
- It's a single point of failure — if the machine goes down, everything goes down.

**Example #1 (Tech):** A database server running PostgreSQL starts slowing down under heavy queries. The team upgrades it from 16GB RAM to 128GB RAM and moves to a higher-core CPU. The database can now handle more concurrent queries without changing any application code.

**Example #2 (Real-World):** A small restaurant gets busier. Instead of opening a second location, the owner expands the kitchen — bigger stoves, more prep counters, a larger refrigerator. Same single location, just more capacity packed in.

------

## Horizontal Scaling

**Formal Definition:**  
Horizontal scaling (also called _scaling out_) is the process of adding more machines to a system so that the load is distributed across multiple nodes, each handling a portion of the total traffic or computation.

**Concept Explanation:**  
Instead of making one machine stronger, you bring in more machines and spread the work. This requires a way to route requests across machines — typically a load balancer sitting in front. The system now has to be designed to work in a distributed fashion: sessions, state, and data need to be handled carefully so any machine can serve any request.

**When it breaks down and why:**

- Distributed systems are inherently more complex — you now have to handle network failures, data consistency, and coordination.
- Not all workloads can be partitioned easily (e.g. tightly coupled stateful processes).
- Introduces latency in inter-node communication.
- Requires a load balancer, which itself can become a bottleneck or single point of failure if not designed carefully.

**Example #1 (Tech):** A web application getting 10x more traffic spins up 9 additional application servers behind a load balancer. Each server handles a slice of incoming HTTP requests, and the system scales linearly with each machine added.

**Example #2 (Real-World):** Instead of expanding one restaurant, the chain opens 9 new locations across the city. Each location serves its own neighborhood. A central dispatch system (like Swiggy/Zomato) routes customers to the nearest outlet — that's your load balancer.

---
## Pre-Processing

**Formal Definition:**  
Pre-processing is the technique of doing expensive or predictable computation _ahead of time_ — before the request arrives — so that when the request comes in, the result can be served instantly from pre-computed data rather than being calculated on the fly.

**Concept Explanation:**  
The core idea is shifting work from _request time_ to _idle time_. If you know certain computations will be needed repeatedly, or you can anticipate what will be asked, you run those computations in advance and store the results. When the actual request arrives, you're just doing a cheap lookup instead of a heavy calculation. This dramatically reduces latency for the end user.

**When it breaks down and why:**

- If the input space is too large or unpredictable, you can't pre-compute everything — you'll waste resources computing things never asked for.
- Pre-computed data can go stale if the underlying data changes frequently (freshness problem).
- Storage cost increases — you're trading compute time for space.
- Doesn't work well for personalized or highly dynamic queries that are unique per user/request.

**Example #1 (Tech):** A news website pre-computes the "Top 10 trending articles" every 5 minutes and caches the result. When a user visits the homepage, the server doesn't query and rank thousands of articles in real time — it just returns the cached list instantly.

**Example #2 (Real-World):** A school cafeteria pre-cooks and portions meals before lunch break starts. When students arrive, food is ready to be served immediately. No one is waiting for their meal to be cooked from scratch during the rush.