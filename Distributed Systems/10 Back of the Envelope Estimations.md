[[Back of the envelope.pdf]]
### 1. What Exactly is a "Back-of-the-Envelope" Calculation?

In simple terms, a back-of-the-envelope calculation is the process of making quick, rough estimates using simple math and common-sense assumptions. The name comes from the idea of jotting down informal figures on any convenient scrap of paper, like the back of an envelope.

The goal isn't to find a perfectly precise answer. Instead, it's about arriving at an approximate, "good enough" number that can guide your initial design decisions. This approach focuses on understanding the magnitude of the problem you're trying to solve. As the source creator emphasizes, the process is intentionally informal and focused on getting a general sense of scale:

"It is all rough calculations...(Everything runs on estimates, rough estimates.)

Remember, these are starting points. As the source material emphasizes, the true skill comes from making estimates, building the system, and learning from the difference between your calculations and reality. Before you can start making these estimates, you need a few key numbers and concepts in your mental toolkit.

### 3. The Essential Toolkit: Numbers Every Designer Should Know

This section covers the foundational knowledge required to perform any estimation quickly and effectively.

#### 3.1. Rule #1: Approximations are Your Best Friend

The first and most important rule is to round off numbers to make your calculations simpler and faster. You are looking for an estimate, not a perfect answer.

A classic example is the number of seconds in a day:

- **Exact Calculation:** `24 hours * 60 minutes/hour * 60 seconds/minute = 86,400 seconds`
- **Approximation for Estimation:** This can be rounded up to **100,000 seconds** for easier mental math.

#### 3.2. Sizing Up Data: From Kilobytes to Petabytes

Understanding data units is fundamental. The following table provides the common units you'll encounter.

#### Common Data Units

|   |   |
|---|---|
|Unit (Short Name)|Approximate Value|
|Kilobyte (KB)|1 Thousand|
|Megabyte (MB)|1 Million|
|Gigabyte (GB)|1 Billion|
|Terabyte (TB)|1 Trillion|
|Petabyte (PB)|1 Quadrillion|

#### 3.3. Understanding Speed: The Latency Hierarchy

Latency is the time it takes for data to travel from one point to another. In system design, you must commit one simple rule to memory: **memory (like RAM) is fast, and the disk is slow.** This single principle drives countless architectural decisions.

The fastest memory in a computer is the CPU cache because it is physically located on the processor itself. The **L1 cache** is the fastest (0.5 nanoseconds), and the **L2 cache** is the next fastest (7 nanoseconds). The table below shows just how vast the speed differences are between accessing data from the cache versus a physical disk.

#### Key Latency Numbers

|   |   |
|---|---|
|Operation|Time|
|**L1 cache reference**|**0.5 ns**|
|**L2 cache reference**|**7 ns**|
|Main memory reference|100 ns|
|Round trip within same datacenter|500,000 ns|
|**Disk seek**|**10,000,000 ns**|

The numbers in this table reveal the cornerstone of high-performance system design. A disk seek takes 10,000,000 nanoseconds, while an L1 cache reference takes just 0.5 nanoseconds. This is a performance difference of roughly 20 million times! This staggering gap is precisely why caching systems (like Redis) are not just "nice-to-have" features; they are an absolute necessity for building fast, responsive applications.

Now that you have these foundational numbers in your toolkit, let's put them to work in a practical, real-world scenario.

### 4. Let's Design Twitter: A Step-by-Step Walkthrough

This practical exercise will walk you through how to apply these concepts to estimate the needs of a large-scale application like Twitter.

#### 4.1. Step 1: State Your Assumptions

Clearly stating your assumptions is the most critical first step. It provides a transparent foundation for all your calculations.

- **Monthly Active Users (MAU):** 300 million
- **Daily Active Users (DAU):** 50% of MAU are active daily
- **User Activity:** Users post 2 tweets per day on average
- **Media Content:** 10% of tweets contain media (photos/videos)
- **Average Media Size:** 1 MB per photo or video
- **Data Retention:** Data needs to be stored for 5 years

#### 4.2. Step 2: Calculate the Daily Load (Queries Per Second)

Next, we estimate how many requests our system needs to handle every second, often called Queries Per Second (QPS).

- **Calculate Daily Active Users (DAU):** `300 million MAU * 50% = 150 million DAU`
- **Calculate Tweets per Second (QPS):** `(150 million users * 2 tweets/day) / 100,000 seconds/day = 3,000 Tweets/Second` Note that we're using our approximation of 100,000 seconds per day. This makes the mental math far simpler (300 million / 100,000 = 3000) and is perfectly acceptable for a back-of-the-envelope estimate.
- What does this number tell you? It means your database must be able to handle at least **3,000 insert operations every second**.
- **Calculate Peak QPS:** Systems rarely have a perfectly steady flow of traffic. You must plan for sudden spikes, like during a global event. A simple way to estimate this is to double the average QPS. `3,000 QPS * 2 = 6,000 Peak QPS` Your system should be able to handle a peak load of 6,000 queries per second to remain available.

#### 4.3. Step 3: Estimate Storage Needs

Finally, let's calculate how much storage space the platform will require over time.

- **Calculate Daily Media Storage:** `150 million DAU * 2 tweets/day * 10% with media * 1 MB/media = 30 Terabytes per day`
- **Calculate 5-Year Storage Needs:** `30 TB/day * 365 days/year * 5 years ≈ 55 Petabytes` What does this number tell you? It means you will need to plan for approximately **55 petabytes of storage** over the next five years. A number this large immediately tells a system designer that storage cannot be handled by a single traditional database. It necessitates a distributed object storage solution like Amazon S3 or a distributed file system like HDFS.

These estimates, while rough, provide a solid starting point for making architectural decisions. The principles learned here can be generalized into a set of best practices.

### 5. Best Practices for Effective Estimation

Keep these key tips in mind to improve your estimation skills.

1. **Embrace Rounding and Approximations** Remember that the goal is speed and simplicity, not perfect precision. Round numbers to make calculations easier.
2. **Write Down Your Assumptions** This clarifies your thought process for your team (a key point from the video) and is critical for communicating your design choices effectively in an interview setting (a key point from the blog post).
3. **Always Label Your Units** A number like "55" is meaningless on its own. Is it 55 Megabytes or 55 Petabytes? Labeling your units prevents confusion and critical errors.
4. **Break Down the Problem** Complex problems are easier to solve when broken into smaller components. Estimate the needs of each component individually and then add them up to get a total estimate.
5. **Perform a Sanity Check** After you have a final number, step back and ask: "Does this seem plausible?" Comparing your estimate to a similar, existing service can help you validate whether your number is in a reasonable range.
