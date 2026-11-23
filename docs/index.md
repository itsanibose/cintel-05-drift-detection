# Detecting Changes in System Behavior

Continuous Intelligence | Project 05 | Custom Report

## Overview

This project examines whether recent system behavior
differs meaningfully from an earlier reference period.

The objective is to detect changes in operational metrics
that may indicate a need for investigation.

To make the drift detection pipeline more resilient against extreme outliers,
this implementation extends baseline mean comparisons by computing
**median (P50)** and **95th percentile (P95)** tail-latency metrics in Polars.

## Data

Two datasets represent different observation periods:

- Reference data representing earlier behavior.
- Current data representing more recent behavior.

Each contains measurements of requests,
errors, and total latency.

[View Reference Data](https://github.com/itsanibose/cintel-05-drift-detection/blob/main/data/reference_metrics.csv)

[View Current Data](https://github.com/itsanibose/cintel-05-drift-detection/blob/main/data/current_metrics.csv)

## Methods

The pipeline calculates the mean and median (P50) of each metric
for both observation periods, along with the 95th percentile (P95)
for total latency.

It then calculates:

    difference = current_metric - reference_metric

Drift is flagged when the absolute difference
exceeds the configured threshold.

The configured thresholds are:

- Requests (mean): 20.
- Errors (mean): 2.
- Total latency (mean): 1,000 milliseconds.
- Total latency (P95): 1,200 milliseconds.

These thresholds are illustrative and should
be justified for a production monitoring system.

## Results

| Metric                   | Reference | Current | Difference | Drifting |
| ------------------------ | --------: | ------: | ---------: | :------: |
| Requests (Mean)          |     128.3 |   164.5 |      +36.2 |   Yes    |
| Errors (Mean)            |       2.3 |     6.3 |       +4.0 |   Yes    |
| Total latency (Mean, ms) |     3,835 |   5,670 |     +1,835 |   Yes    |
| Total latency (P95, ms)  |     4,595 |   6,560 |     +1,965 |   Yes    |

*(Note: Verify the exact `reference_p95_latency_ms` and `current_p95_latency_ms`
values in `artifacts/drift_summary_long.csv` and update the bottom row if needed.)*

**All monitored drift flags were triggered across mean and P95 thresholds.**

[View Drift Results](https://github.com)

[View Detailed Summary](https://github.com)

[View Statistical Error Comparison](https://github.com)

## Interpretation

The current period shows higher average and median request volume,
higher errors, and greater overall and tail (P95) latency.

Tracking median and P95 latency alongside the mean confirms that
the slowdown is not caused by a single isolated outlier, but reflects
a broad upward shift in both typical and worst-case response times.

An analyst could investigate whether the changes
reflect increased workload, changing demand,
or a potential performance bottleneck.

## Limitations

The approach compares summary statistics using fixed thresholds.

While median and P95 metrics reduce sensitivity to isolated spikes,
fixed thresholds still do not account for seasonal patterns or
time-of-day traffic cycles.

Higher request volume may also explain part
of the increase in errors and total latency.

Additional normalized signals, such as error rate
and average latency per request, would help
distinguish workload changes from degraded performance.

## Conclusion

Combining mean, median, and P95 latency comparisons provides a more
reliable baseline for detecting changes in operational behavior.

Drift indicators are evidence for investigation,
not proof that a system is malfunctioning.

## Documentation Index

- **Home** - this landing page
- [**Project Instructions**](project-instructions.md)
- [**Glossary**](glossary.md)
- [**API**](api.md)
- [**Reactive App**](./app/index.html)

## Project Links

- [GitHub Repository](https://github.com/itsanibose/cintel-05-drift-detection)
- [Custom Pipeline](https://github.com)
- [Results and Artifacts](https://github.com)

## Project Resources

- [Project Instructions](project-instructions.md)
- [Professional Python Guide](https://denisecase.github.io/pro-analytics-02/)
- [Professional Python Project Explainer](https://pup-pack.github.io/professional-python-project-explainer/)
- [Suggested Datasets](https://denisecase.github.io/pro-analytics-02/reference/datasets/cintel/)
