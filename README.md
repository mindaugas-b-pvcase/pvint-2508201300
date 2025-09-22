### 🏢 Booking.com Payments System (60-Minute Live Exercise)

This task is a time-boxed, \~1-hour live coding exercise. The goal is not to finish everything, but to demonstrate your ability to reason, prioritize, communicate trade-offs, and write clean code under pressure.

-----

### 📝 Context

You'll be building a backend service to process payments for a global booking platform, similar to Booking.com.

#### What We're Evaluating:
  * **Coding Fundamentals (Tier 1-2):** Can you quickly ship something correct and functional?
  * **Code Quality:** Do you structure your code cleanly, even when rushing?
  * **Concurrency (Tier 3):** Do you know how to introduce concurrency safely?
  * **Creative Thinking (Tier 4):** Can you think creatively about anomalies and performance, even if you only stub or explain the solution?

#### Deliverables (within the hour):
  * **Working Code:** A solution for as many tiers as you can reasonably complete.

-----

### 🚀 Tiered Requirements

Progress through these tiers in order. It's perfectly fine to jump ahead and leave `TODOs` if you clearly explain your plan.


-----

#### Tier 1 — Process & Update (Baseline)
Read data from [data/example_input.txt](./data/example_input.txt), count occurrences of each log level (e.g., INFO, WARN, ERROR) and write summary JSON to output/summary.json. Handle basic errors (don’t crash on bad data).

Each line has the shape: [timestamp] [log_level] [service_name] [user_id] [message], Example: `2025-07-22T12:34:56Z INFO AuthenticationService user123 User login successful`.

**Input Format (`data/example_payments.json`):**

```csv
2025-07-22T12:34:56Z SUCCESS PaymentGateway tx-123 Payment processed successfully.
2025-07-22T12:34:57Z FAILURE FraudDetection tx-124 High-risk transaction detected.
```

**Example output (`output/summary.json`):**

```json
{
  "SUCCESS": 1000,
  "FAILURE": 120,
  "ERROR": 30
}
```


> Note: You may use a GPT tool to generate the initial solution for this tier. The goal is to get a working baseline quickly and then refine it yourself.

Suggested GPT Prompt for Initial Tier 1 Solution:

```txt
Write the simplest possible program in <YOUR_LANGUAGE> that:
1. Reads a text file line by line from data/input.txt.
2. Counts how many times each log level (INFO, WARN, ERROR) appears.
3. Writes a JSON file to output/summary.json with counts per log level.
4. Ignores malformed lines without stopping the program.

Keep it minimal — no over*engineering, no abstractions, no separation of concerns.

Each line has the shape: [timestamp] [log_level] [service_name] [user_id] [message]
Example: 2025-07-22T12:34:56Z INFO Payments user123 User login successful

Example summary:
{
  "INFO": 1000,
  "WARN": 120,
  "ERROR": 30
}
```
-----

#### Tier 2 — Deeper Analytics

In addition to Tier 1, compute the following analytics:

  * Identify the **top 10 users** and **top 10 booking types** (e.g., based on a regex on `booking_id`) by transaction volume.
  * Compute **peak payment hours** (group by hour of the day).
  * Compute **total revenue** by currency.

Extend the `output/summary.json` to include these analytics.

**Example extended summary:**

```json
{
  "transaction_statuses": {
    "SUCCESS": 1000,
    "FAILURE": 120
  },
  "analytics": {
    "success_failure_ratio": 8.33,
    "top_services": ["PaymentGateway", "FraudDetection", "..."],
    "top_transactions": ["tx-123", "tx-456", "..."],
    "peak_hours": ["12:00-13:00", "16:00-17:00"]
  }
}
```

-----

#### Tier 3 — Concurrency & Asynchronous Processing

In addition to Tier 2:

  * **Request Handler:** Implement a simple API endpoint that accepts a payment request and adds it to a queue for processing.
  * **Worker Pool:** Use threads, async, or worker processes to handle multiple payment requests from the queue concurrently.
  * **Safety:** Show you've considered safety (race conditions, back-pressure) even if your implementation is minimal (e.g., a simple queue + worker pool).

-----

#### Tier 4 — Anomalies & Performance (Stretch)

Design, pseudo-code, or partially implement the following:

  * **Anomaly Detection:** Define and detect at least one anomaly type, such as:
      * Duplicate `transaction_id`
      * Unusually high transaction amount
      * A single user attempting many failed payments in a short period
        Output anomalies to `output/anomalies.json`.
  * **Performance/Memory:** Log or print the execution time and peak memory usage of your process.
  * **Scalability Thought Experiment:** Briefly outline how you'd handle billions of transactions under a 500MB RAM cap with a `<1s` latency for a single payment request.

-----

### 💻 Tech Instructions

  * Use any backend language you prefer: Python, .NET, Node.js, Rust, Go, etc.
  * You may use the internet to look up syntax.
  * **Except for the initial Tier 1 baseline, do not use AI to generate a working solution.** We want to see your problem-solving ability, not someone else's.

### 💡 Tips

  * Prefer small, composable functions over giant scripts.
  * Leave `TODO:` comments where you skip a part, and briefly explain your plan.
  * For concurrency, a simple worker pool is a great starting point.
  * Good luck, have fun, and narrate your thinking as you go\!
