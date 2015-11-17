Joiner
======

A simple utility for SQL-like joins with Json or GeoJson data. Also creates join reports so you can know how successful a given join is.

## Installation

To install as a Node.js module:

````
npm install joiner
````

Or to install as a command-line utility:

````
npm install joiner -g
````

To use as both, run both commands.

## Methods

All methods return an object with the following structure:

````
data: [data object],
report: {
	diff: {
		a: [data in A],
		b: [data in B],
		a_and_b: [data in A and B],
		a_not_in_b: [data in A not in B],
		b_not_in_a: [data in B not in A]
	}:
	prose: {
		summary: [summary description of join result, number of matches in A and B, A not in B, B not in A.]
		full:    [full list of which rows were joined in each of the above categories]
	}
}
````

__.left__ 

_.left(leftData, leftDataKey, rightData, rightDataKey, [nestedKeyName])_

Perform a left join on the two array-of-object json datasets. Optionally, you can pass in a key name in case the left data's attribute dictionary is nested, such as in GeoJson where the attributes are under a `properties` object.


__.geoJson__ 

_.geoJson(leftData, leftDataKey, rightData, rightDataKey, 'properties')_

Does the same thing as __.left__ but passes in `properties` as the nested key name.

## Usage

See the [`examples`](https://github.com/mhkeller/joiner/tree/master/examples) folder.

As you can see, it puts a lot of data in memory, so it's probably best to avoid very large datasets.

## Command line interface

````
Usage: joiner 
		-a FILE_PATH 
		-k DATASET_A_KEY 
		-b FILE_PATH 
		-l DATASET_B_KEY 
		-m (json|geojson) # defaults to `json`
		-n NEST_ID 
		-o OUT_FILE_PATH 
		-d (summary|full) # defaults to `summary`
````

The first four parameters, `-a`, `-k`, `-b` and `-l` are required. 

If you specify an output file, it will write the join report to the same directory. For example, `-o path/to/output.csv` will also write `-o path/to/output-report.json`

`-m` defaults to `json`. `-m geojson` acts the same as the `.geoJson` method above. 

Supported input and output formats: `json`, `csv`, `csv`, `psv`. Format will be inferred from the file ending on both input and output file paths. For example, `-a path/to/input/file.csv` will read in a csv. `-o path/to/output/file.csv` will write a csv.

If you don't specify an output file with `-o`, Joiner will print the join report to the console. By default it will just specify the summary report. To print the full report, specify `-d full`.
