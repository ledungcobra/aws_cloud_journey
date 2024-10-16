---
title: "CronJob"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

#### Problem

- **Job** run immediately when created.
- You need to schedule the job to run periodically or in a specific time.
- Want service to run periodically like `cron` service in Unix/Linux.

#### Solution

- Use **CronJob** to schedule the job to run periodically.

#### Example

- Suppose you want to run the Job every 5 minutes.

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: batch-job-every-five-minutes
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        metadata:
          labels:
            app: periodic-batch-job
        spec:
          restartPolicy: OnFailure
          containers:
            - name: main
              image: luksa/batch-job
```

- `schedule: "*/5 * * * *"` means every 5 minutes.

- First `*` is Minute range 0-59
- Second `*` is Hour range 0-23
- Third `*` is Day of month range 1-31
- Fourth `*` is Month range 1-12
- Fifth `*` is Day of week range 0-7 (0 and 7 both represent Sunday)

![After create CronJob](images/_index.png)
