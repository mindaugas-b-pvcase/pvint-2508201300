Booking.com Payments System (60-Minute Live Exercise)
This task is time-boxed to ~1 hour. You won’t finish everything, and that's expected. The goal is to see how you reason, prioritize, communicate trade-offs, and write code under constraints.

Context
You'll build a backend service to process payments for a booking platform similar to Booking.com.

What We're Evaluating:
Can you design a scalable, resilient system (Tier 0)?

Can you quickly ship something correct (Tier 1–2)?

Do you structure code cleanly, even when rushing?

Do you know how to introduce concurrency safely (Tier 3)?

Can you think creatively about anomalies/performance (Tier 4), even if you only stub or explain?

Deliverables (within the hour):
Design diagram for Tier 0.

Working code for as many tiers as you can reasonably complete.

Tiered Requirements
Progress through tiers in order. It’s OK to jump ahead and leave TODOs if you explain your plan.

Tier 0 — System Design Exercise (Before Coding)
Scenario: Design a payment processing system for a global booking platform.
Premise:

Payment requests come in via a REST API endpoint.

Load is highly variable: sometimes a few per second, sometimes spikes of 10,000+ per second during flash sales.

Payment types vary: from simple credit card payments to complex e-wallets and bank transfers.

All payments must be processed, and their status updated in a transactional manner.

The system should auto-scale to handle peaks efficiently while being cost-effective when idle.

It must be resilient (handle failures gracefully) and provide idempotency for requests.

Task:
Use any diagramming tool (e.g., Excalidraw, Lucidchart, Miro, Draw.io) to propose a high-level architecture.
Show components for:

Ingestion pipeline (API endpoint)

Payment processing & orchestration

Database for transaction status

Auto-scaling mechanism

Resilience / fault-tolerance

Idempotency handling

You do not need to code this part, but be ready to explain trade-offs.

Tier 1 — Process & Update (Baseline)
Read transaction data from data/example_payments.json, process each transaction, and write a summary of final statuses to output/summary.json. Handle basic errors (don't crash on bad data).

Input Format (each line is a JSON object):

JSON

{"transaction_id": "tx-123", "user_id": "user456", "booking_id": "book789", "amount": 100.50, "currency": "USD", "payment_method": "credit_card", "timestamp": "2025-07-22T12:34:56Z"}
Processing Logic:

Simulate a payment gateway call:

payment_id is a hash of transaction_id.

Simulate success/failure: odd transaction_id numbers fail ("status": "failed"), even ones succeed ("status": "succeeded").

Add a gateway_response field to the output.

Example summary (output/summary.json):

JSON

[
  {
    "transaction_id": "tx-123",
    "status": "failed",
    "gateway_response": {"message": "Invalid transaction ID"}
  },
  {
    "transaction_id": "tx-124",
    "status": "succeeded",
    "gateway_response": {"gateway_tx_id": "gtx-9988"}
  }
]
Note: You may use a GPT tool to generate the initial solution for this tier. The goal is to get a working baseline quickly and then refine it yourself.

Suggested GPT Prompt for Initial Tier 1 Solution:
Write the simplest possible program in <YOUR_LANGUAGE> that:

Reads a JSON file line by line from data/example_payments.json.

For each JSON object, simulate a payment process:

If transaction_id ends in an even number, set the status to "succeeded".

If transaction_id ends in an odd number, set the status to "failed".

Write the final transaction objects to a new JSON file at output/summary.json.

Ignores malformed lines without stopping the program.

Keep it minimal — no over-engineering, no abstractions, no separation of concerns.

Tier 2 — Deeper Analytics
In addition to Tier 1:

Identify the top 10 users and top 10 booking types (e.g., based on a regex on booking_id) by transaction volume.

Compute peak payment hours (group by hour of the day).

Compute total revenue by currency.

Extend output/summary.json to include these analytics.

Example extended summary:

JSON

{
  "transaction_statuses": [
    {"transaction_id": "tx-123", "status": "failed", ...},
    ...
  ],
  "analytics": {
    "total_revenue_by_currency": {"USD": 10000.50, "EUR": 5000.25},
    "top_users": ["user123", "user456", ...],
    "top_booking_types": ["Hotel", "Flight", ...],
    "peak_hours": ["12:00-13:00", "16:00-17:00"]
  }
}
Tier 3 — Concurrency & Asynchronous Processing
In addition to Tier 2:

Request Handler: Implement an API endpoint (even a simple one) that accepts a payment request and adds it to a queue for processing.

Worker Pool: Use threads/async/workers to process multiple payment requests concurrently from the queue.

Safety: Show you considered safety (race conditions, back-pressure) even if it's minimal (e.g., a queue + worker pool).

Tier 4 — Anomalies & Performance Constraints (Stretch)
In addition to Tier 3 (design, pseudo-code or partial implementation is acceptable):

Anomaly detection: Define and detect at least one anomaly type (e.g., duplicate transaction_id, unusually high transaction amount, a single user attempting many failed payments in a short time). Output to output/anomalies.json.

Performance/Memory visibility: Log or print execution time and peak memory usage.

Scalability thought experiment: Briefly outline (code or notes) how you'd handle larger inputs (billions of transactions) under a 500MB RAM cap and <1s latency for a single payment request.

Tech Instructions
Use any backend language: Python, .NET, Node.js, Rust, Go, etc.

Use the internet to look up syntax if you need.

Except for the initial Tier 1 baseline, do not use AI to generate a working solution—we are interested to see your thought process and problem-solving ability, not someone else’s.

Tips
Small, composable functions win over giant scripts.

Leave TODO: comments where you skip (explain briefly).

For concurrency, a simple worker pool beats over-engineering.

Good luck—have fun and narrate your thinking as you go!









