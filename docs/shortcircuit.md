# From Crisis to Resilience: How a Circuit Breaker Saved Our Platform Post-Thanksgiving

## Black Friday Afterglow… Until It Wasn’t

It was the Monday after Thanksgiving. Our engineering team was riding high on the success of a flawless Black Friday weekend. Dashboards were green, sales numbers were up, and we were just beginning to breathe.

Then came the first Slack ping.

> “Anyone else seeing failed checkouts?”

What started as a trickle became a flood: missing order confirmations, duplicated charges, and abandoned carts. PagerDuty screamed. Latency spiked. CPU metrics glowed red. Chaos had arrived.

## The Real Culprit: A Downstream Domino

Our Payments Aggregator—the critical service connecting us to third-party payment processors—was choking. But the problem wasn’t in our codebase. One of the external processors was overwhelmed by post-holiday traffic and timing out.

Unfortunately, our Aggregator didn’t know when to stop trying.

With no timeouts or circuit breaker logic in place, it kept retrying and waiting for responses that would never come. Requests backed up. Thread pools maxed out. A single slow downstream service was pulling down our entire backend.

It was my second month on the job as a platform engineer—and I was deep in the fire.

## The Fix: Implementing the Circuit Breaker Pattern

Once we manually stabilized the platform and restored operations, we knew we had to fix the architecture. Our answer was clear: implement the Circuit Breaker pattern.

The logic was simple:

> “Don’t keep knocking on a door that won’t open.”

Here's how we built our circuit breaker module :

* **Failure Threshold**: 50% failures over a rolling 10-second window
* **Timeouts**: Hard 2-second max on all external calls
* **Cool-down Period**: 30 seconds before retrying
* **Half-Open Testing**: Let in a few requests to see if recovery is possible
* **Fallback messaging **: Show users a graceful "We’re processing your payment" message, while queueing the transaction for retry

We paired this with:

* **Kafka queues** to ensure no customer transaction was lost
* **Grafana dashboards** to visualize breaker state and metrics
* **Chaos drills** simulating real-world outages

The improvement was instant and dramatic.

## Tools That Made a Difference

* **New inhouse compoenent that we built ** – Lightweight, modular, and based on failfast
* **Grafana + Prometheus** – Built clarity into failure patterns
* **Kafka** – Let us decouple request handling from payment fulfillment
* **PagerDuty** – Alerted us just early enough
* **Incident Review Templates** – Helped us write a playbook others could use

## What I Learned (and Would Tell My Day-1 Self)

This incident transformed how I think about platform resilience. Here are the hard-earned lessons I’d share with any engineer stepping into a new DevOps or Platform Engineering role:

* **Design for failure** — always. No dependency is too reliable to take down your system.
* **Fail fast, not slow**. A clear error is better than a timeout that cripples everything.
* **Observability is oxygen**. You need to see the problem before you can solve it.
* **Get wins early**. My first trust-building moment was demoing the breaker logic in action — it spoke louder than any documentation.
* **Standardize resilience**. Timeouts, retries, and circuit breakers shouldn’t be exceptions — they should be defaults.

## The Aftermath: A New Normal

Since that turning point, we’ve rolled out circuit breakers across all critical outbound calls. We’ve extended the logic into our service mesh. We’ve even begun applying it to internal service boundaries where latency and load can snowball.

What could have been a platform-breaking failure turned into our resilience blueprint.

More than just fixing a bug, we changed how we build — and how we think.

Platform engineering isn’t just about keeping the lights on. It’s about designing systems that fail gracefully, recover automatically, and earn trust continuously.

If you’re just stepping into this space: Welcome. The chaos will find you — but with the right patterns, you’ll be ready.


Here are some more patterns that I have been using: <br>[Patterns at scale](docs/PatternsAtScale.md)
