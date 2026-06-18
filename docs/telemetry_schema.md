# Telemetry Schema

## Purpose

This schema defines the standard format used throughout the Hybrid Network Congestion Framework.

All data sources (Mininet simulations, public datasets, and processed datasets) must be converted into this format before entering the machine learning pipeline.

---

## Features

| Feature              | Type     | Unit        | Description                            |
| -------------------- | -------- | ----------- | -------------------------------------- |
| timestamp            | datetime | UTC         | Sample timestamp                       |
| throughput_mbps      | float    | Mbps        | Measured throughput                    |
| bandwidth_usage_pct  | float    | %           | Percentage of available bandwidth used |
| link_utilization_pct | float    | %           | Link utilization                       |
| packet_rate_pps      | float    | packets/sec | Packet transmission rate               |
| queue_length         | float    | packets     | Queue occupancy                        |
| latency_ms           | float    | ms          | End-to-end latency                     |
| jitter_ms            | float    | ms          | Delay variation                        |
| packet_loss_pct      | float    | %           | Packet loss percentage                 |

---

## Derived Features

| Feature          | Description                        |
| ---------------- | ---------------------------------- |
| congestion_score | Continuous congestion metric (0–1) |
| severity         | Low / Medium / High / Critical     |

---

## Congestion Score

Normalized weighted score:

congestion_score =
0.35 × link_utilization +
0.25 × queue_length +
0.20 × latency +
0.10 × packet_loss +
0.10 × jitter

All components must be normalized to [0,1].

---

## Severity Levels

Low: 0.00 – 0.30

Medium: 0.30 – 0.60

High: 0.60 – 0.80

Critical: 0.80 – 1.00

---

## Prediction Target

Future congestion score:

Target = congestion_score(t+1)

Future severity:

Target = severity(t+1)
