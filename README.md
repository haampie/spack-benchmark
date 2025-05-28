# How to use this extension

This extension adds a command to Spack to benchmark concretization. To set it up, clone the repository somewhere
and modify your `config.yaml` by adding the repository directory to the extensions:
```yaml
config:
  extensions:
  - /some/path/spack-benchmark
```
The extension requires `pandas`, `matplotlib` and `tqdm` - so install them in some way. For instance:
```console
$ pip install pandas matplotlib tqdm
```
At this point you should be able to print the following help:
```console
$ spack solve-benchmark -h
usage: spack solve-benchmark [-h] SUBCOMMAND ...

benchmark concretization speed

positional arguments:
  SUBCOMMAND
    run       run benchmarks and produce a CSV file of timing results
    plot      plot results recorded in a CSV file

optional arguments:
  -h, --help  show this help message and exit
```

## Benchmark concretization speed

To benchmark concretization speed, and record results in a CSV file:
```console
$ spack solve-benchmark run [options] <specfile>
```
Where `<specfile>` can be:
* A path to a text file with one spec per line.
* The name of a predefined benchmark (e.g., `radiuss`, `propagation`). The tool will look for a corresponding file (e.g., `radiuss.txt`) in its `benchmark/cmd/data/` directory.

The command goes over all the inputs and records concretization time for each of them.

Here are the available options:
* `-o, --output <filename>`: Specify the CSV file to store the timing results.
* `-r, --repetitions <N>`: Number of times each spec is benchmarked (default: 1).
* `--reuse`: Enable maximum reuse of Spack's buildcaches and installations to speed up tests for already concretized/installed specs.
* `--configs <config_list>`: Comma-separated list of Clingo configurations to use for solving (default: "tweety"). Valid options: "tweety", "handy", "trendy", "many".
* `-n, --nprocess <N>`: Number of parallel processes to use for running the benchmarks (default: number of CPU cores).
* `-s, --shuffle`: Shuffle the order of specs to be concretized. This can be useful for averaging out system load effects over a long benchmark run.

**Example Usage:**

Create a spec file (e.g., using `spack list`):
```console
$ spack list -t radiuss > my_specs.txt
```
Run the benchmark with specific options:
```console
$ spack solve-benchmark run --repetitions 3 --configs tweety,handy --nprocess 4 --shuffle -o my_results.csv my_specs.txt
```
Or run with a predefined benchmark name:
```console
$ spack solve-benchmark run --repetitions 3 --configs tweety,handy --nprocess 4 --shuffle -o my_results.csv radiuss
```