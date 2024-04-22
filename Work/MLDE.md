name
searcher : single - no hyperparameter tuning
	name
	metric: validation metric for Mlde 
smaller_is_better
max_length 
entrypoint: 

```
name: core-api-stage-4
entrypoint: python3 4_distributed.py launcher

# NEW: configure multiple slots per trial.
resources:
  slots_per_trial: 8

hyperparameters:
  increment_by:
    type: int
    minval: 1
    maxval: 8

searcher:
   name: single
   metric: x
   max_length: 100

max_restarts: 0

resources:
  resource_pool: h100
  slots_per_trial: 1
workspace: "St Jude"
project: kwong
labels:
  - checkpoint
```


det  e create const.yaml

```
resource_pool: h100
slots_per_trial: 1
```

.detignore

startup-hook.sh
- used for smaller tasks

should create own image, something tied to github actions

