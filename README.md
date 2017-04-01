# XHProf FlameGraphs

A script to convert a directory of xhprof or uprofiler sampling files to a format that can be read by Brendan Gregg's [FlameGraph](https://github.com/brendangregg/FlameGraph) script.

Note: This works with files generated by `(uprofiler|xhprof)_sample_enable`/`(uprofiler|xhprof)_sample_disable`. It will not work with the more common hierarchical xhprof/uprofiler format.

# Usage

First, generate the sampling data, writing the files with a `sample_xhprof` (resp. `sample_uprofiler`) file extension. Then run the script passing the directory containing those files as an argument like so:

## Generating the sample data

One way to generate the sample data is prepend the following fragment to the entry point of the application, often `index.php`:

```php
<?php

tideways_enable(TIDEWAYS_FLAGS_NO_SPANS);

// your application code;

$data = tideways_disable();
file_put_contents(
    sys_get_temp_dir() . '/' . uniqid() . '.yourapp.xhprof',
    serialize($data)
);

```

## Building the flamegraph from the data

```
./xhprof-sample-to-flamegraph-stacks /directory/with/sample/xhprof/files | flamegraph.pl > xhprof-flamegraph.svg
```

