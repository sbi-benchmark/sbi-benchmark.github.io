---
sidebar: true
---

# Code and Reproducibility

## How to benchmark new algorithms?

We provide [`sbibm`](https://github.com/sbi-benchmark/sbibm), a simulation-based inference benchmarking framework, which is available at:
> [github.com/sbi-benchmark/sbibm](https://github.com/sbi-benchmark/sbibm)

`sbibm` includes tasks, reference posteriors, metrics, plotting, and integrations with exiting algorithm toolboxes. It is designed to be highly extensible and easily used in research projects: Having installed the framework via `pip install sbibm`, using it to benchmark existing or new algorithms is very simple, as we sketch out in the example code below:

```python
import sbibm

task = sbibm.get_task("two_moons")  # See sbibm.get_available_tasks() for all tasks
prior = task.get_prior()
simulator = task.get_simulator()
observation = task.get_observation(num_observation=1)  # 10 per task

# These objects can then be used for custom inference algorithms, e.g.
# we might want to generate simulations by sampling from prior:
thetas = prior(num_samples=10_000)
xs = simulator(thetas)

# Alternatively, we can import existing algorithms, e.g:
from sbibm.algorithms import rej_abc  # See help(rej_abc) for keywords
posterior_samples, _, _ = rej_abc(task=task, num_samples=10_000, num_observation=1, num_simulations=100_000)

# Once we got samples from an approximate posterior, compare them to the reference:
from sbibm.metrics import c2st
reference_samples = task.get_reference_posterior_samples(num_observation=1)
c2st_accuracy = c2st(reference_samples, posterior_samples)

# Visualise both posteriors:
from sbibm.visualisation import fig_posterior
fig = fig_posterior(task_name="two_moons", observation=1, samples=[posterior_samples])  
# Note: Use fig.show() or fig.save() to show or save the figure

# Get results from other algorithms for comparison:
from sbibm.visualisation import fig_metric
results_df = sbibm.get_results(dataset="main_paper.csv")
fig = fig_metric(results_df.query("task == 'two_moons'"), metric="C2ST")
```

More details can be found [in the `sbibm` repository](https://github.com/sbi-benchmark/sbibm).


## Manuscript results

All results of the manuscript, as well as code and instructions to reproduce them, are available at:
> [github.com/sbi-benchmark/results/benchmarking_sbi](https://github.com/sbi-benchmark/results/tree/main/benchmarking_sbi)
