# DO180

Alright, let’s unpack this carefully — this kind of timeout pattern with Camunda (Zeebe) under moderate load (200 TPS, 10 concurrent users) is a clear signal of a bottleneck or configuration mismatch between the gateway and the Zeebe broker. The key clues are:
	•	504 Gateway Timeout
	•	Request command-api-1 to camunda-qat-zeebe-0... timed out in PT15S
	•	and occasionally 401 (which might be a side effect, not the root cause).

Let’s go through likely causes in order of probability:

⸻

1. Gateway-to-Broker Communication Bottleneck

The message Request command-api-1 to camunda-qat-zeebe-0... timed out indicates that the gateway sent a request to the broker (port 26501) and didn’t get a response within 15 seconds.
This typically means the broker is overloaded or slow to process commands.

Root causes here might be:
	•	Broker CPU saturation (too many workflow instances being created or updated).
	•	Disk I/O bottleneck — Zeebe persists state using RocksDB. High write load without SSD-backed disks or low IOPS storage leads to lag.
	•	Insufficient Zeebe partitions or replicas. A single-partition setup under 200 TPS will choke.
	•	Backpressure kicking in — Zeebe’s internal flow control pauses new requests when it can’t keep up.

How to verify:
	•	Check broker logs for Backpressure applied or Actor is blocked messages.
	•	Check kubectl top pods for CPU/memory spikes.
	•	Use Camunda Operate or metrics (if enabled) to see processing lag.

⸻

2. Timeout Configuration Too Low

By default, Zeebe gateway waits 15 seconds (PT15S) for a broker response. At high TPS, this can be too aggressive.

Fix:
Increase requestTimeout in gateway configuration, for example:zeebe:
  gateway:
    network:
      requestTimeout: In short:

Here’s what’s most likely happening:

The Zeebe broker is under heavy load (CPU or disk I/O bound), causing gateway requests to time out after 15 seconds. This triggers 504 errors and secondary 401s on retry.

⸻

Next steps to confirm and fix:
	1.	Check broker logs for backpressure or timeout messages.
	2.	Monitor pod resources (kubectl top pods) during the load test.
	3.	Scale out:
	•	Increase partitions (zeebe.broker.partitionsCount)
	•	Add broker replicas
	•	Move to SSD-backed disks.
	4.	Increase request timeout on gateway to 30s temporarily.
	5.	Re-run the load test and observe if 504s reduce.











