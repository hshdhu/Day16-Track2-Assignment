# CP3 — LightGBM Benchmark

## 1. Environment

- Cloud: AWS
- Region: us-east-1
- Instance: t3.small
- Architecture: x86_64
- Python: 3.10.12
- LightGBM: 4.7.0
- scikit-learn: 1.7.2
- pandas: 2.3.3
- NumPy: 2.2.6

The t3.small instance was used because t3.medium was not available under the applicable free-tier conditions.

## 2. Dataset

- Dataset: Credit Card Fraud Detection
- Total rows: 284,807
- Fraud rows: 492
- Normal rows: 284,315
- Missing values: 0
- Target: `Class`

The dataset is highly imbalanced, so Accuracy alone is not sufficient to evaluate the classifier.

## 3. Data Split

A fixed random seed of 16 was used.

- Train: 170,883 rows (60%)
- Validation: 56,962 rows (20%)
- Test: 56,962 rows (20%)

The split was stratified by the target class.

The validation set was used for early stopping. The test set was used only for final evaluation.

## 4. Training

LightGBM configuration:

- `n_estimators`: 300
- `learning_rate`: 0.05
- `random_state`: 16
- `n_jobs`: 2
- Early stopping: 20 rounds

Results:
- Data loading time: 2.289 seconds
- Training time: 3.470 seconds
- Best iteration: 68

## 5. Evaluation

Final test-set metrics:

- AUC-ROC: 0.976848
- Accuracy: 0.999508
- F1: 0.847826
- Precision: 0.906977
- Recall: 0.795918

AUC-ROC was calculated using predicted fraud probabilities. Accuracy, F1, Precision, and Recall were calculated using a decision threshold of 0.5.

## 6. Inference Benchmark

The inference benchmark used a warm-up phase, which was excluded from the reported measurements.

### Single-row latency

- Repeats: 50
- Median latency: 1.179 ms

### Batch throughput

- Batch size: 1,000 rows
- Repeats: 10
- Median batch time: 0.003065 seconds
- Throughput: 326,278.91 rows/second

The inference measurements include model prediction only and exclude VM creation, dataset download, and model training time.

## 7. Conclusion

The CP3 benchmark successfully completed on the AWS x86_64 VM. The benchmark produced valid evaluation metrics and inference performance measurements, which were saved in `benchmark_result.json`.


## CP4 — Resource and Cost Observation

### CPU and Memory

The CPU and memory statistics were collected after the CP3 benchmark completed. Therefore, they represent the post-benchmark system state rather than peak resource utilization during training.

![CPU and process status](screenshots/01-top.png)

![Memory status](screenshots/02-free-memory.png)

### Network

Network statistics were collected using `ip -s link`. These are cumulative RX/TX counters and do not represent instantaneous network throughput.

![Network statistics](screenshots/03-network.png)

### AWS Billing (2:04 AM)

AWS Billing and Cost Management was checked for the lab account.

At the time of observation, AWS showed that the free-plan credits cover eligible costs, while the month-to-date cost, forecasted cost, and cost breakdown were unavailable.

Billing data had not yet been updated in Cost Explorer/Billing at the time of observation. Therefore, no recorded AWS cost is claimed from this screenshot.

![AWS Billing status](screenshots/04-aws-billing.png)
