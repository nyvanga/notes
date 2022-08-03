# Json and especially the jq command

## Converting a list of maps to a CSV table

From [How to transform JSON to CSV using jq in the command line](https://www.freecodecamp.org/news/how-to-transform-json-to-csv-using-jq-in-the-command-line-4fa7939558bf/)

`(map(keys) | add | unique) as $cols` iterates (map) through the keys in your object and adds unique ones to a variable called `$cols`. In other words, this is how your column headers are made.

`map(. as $row | $cols | map($row[.])) as $rows` takes all objects in the outer array, and iterates through all the object keys (title, slug, publishedAt). It appends the values to an array, which gives you an array of arrays with the values, which is what you want when you're transforming JSON into CSV.

`$cols, $rows[] | @csv` puts the column headers first in the array, and then each of the arrays that are transformed to lines by piping them to `@csv`, which formats the output as… csv.

JSON data:
```
JSON='[
  { "key1": "key1value1", "key2": "key2value1" },
  { "key1": "key1value2", "key2": "key2value2" },
  { "key1": "key1value3", "key2": "key2value3" },
  { "key1": "key1value4", "key2": "key2value4" },
  { "key1": "key1value5", "key2": "key2value5" }
]'
```

Putting it all together:

```
echo "${JSON}" | jq --raw-output '(map(keys) | add | unique) as $cols | map(. as $row | $cols | map($row[.])) as $rows | $cols, $rows[] | @csv'
```

## Printing a list of maps using the `column` command

This function uses the above methods to rearrange the JSON data.
The `|` character is used as separator and the `column` command to bring it all together.
```
# header: json array of column names
# json: json array of maps with the above column names
function json_to_columns() {
    local header=$1; shift
    local json=$1; shift
    local rows_json=$(echo "${json}" | jq --argjson cols "${header}" '. | map(. as $row | $cols | map($row[.] | tostring))')
    (
        echo "${header}" | jq --raw-output '. | join("|")'
        echo "${header}" | sed -r 's/"[^"]+"/"----------"/g' | jq --raw-output '. | join("|")'
        echo "${rows_json}" | jq --raw-output '.[] | join(" |")'
    ) | column -s "|" -t
}
```

And then (using same data as above):
```
HEADERS='[ "key1", "key2" ]'

json_to_columns "${HEADERS}" "${JSON}"
```

Headers can also be rearranged:
```
HEADERS='[ "key2", "key1" ]'

json_to_columns "${HEADERS}" "${JSON}"
```

Repeated:
```
HEADERS='[ "key2", "key1", "key1", "key1", "key1" ]'

json_to_columns "${HEADERS}" "${JSON}"
```

Or left out:
```
HEADERS='[ "key2" ]'

json_to_columns "${HEADERS}" "${JSON}"
```
