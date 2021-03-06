# Xpress_parse

Help a user or a group of users to semi-automatically parse and select (and 
label) papers searched on Web Of Science based on their abstracts.

## Description

Keeping up to data with the scientific literature can be overwhelming and 
this is also tedious today to go through many articles and find those 
relevant to one's research, for example to write a review.

This tools simply takes as input the article records downloaded for given 
search(es) on the Web Of Science (woscc) website and interactively queries 
the user by showing the abstract and asking for input whether this article 
record should be skipped, or kept and if kept, whether it should be labeled.   

It can help a team of humans to go through all studies retrieved using a 
search on the Web Of Science website, and selecting some based on their 
abstracts. I can be useful to find studies that could have been missed by 
searching on SRA and EBI if one is looking for papers with sequences.

## Install
```
pip install Xpress-parse
```
or
```
git clone https://github.com/FranckLejzerowicz/Xpress_parse.git
cd Xpress_parse
pip install -e .
```

## Input

One or more than one input file can be passed to `-i` or `--i-script` but 
each file must be in the format as downloaded from
[WoS basic search](https://www.webofscience.com/wos/woscc/basic-search):
* "Export" must be in **tab delimited** format
![](Xpress_parse/resources/images/export_tab.png)

* "Record Content" must be **Full Record**
![](Xpress_parse/resources/images/record_content.png)

By default the record file downloaded are named `savedrecs.txt` and it is 
advised to either rename this file, or to place each in a folder documenting 
the search it originates from, e.g. a folder structure:

```
.
└── woscc_searches
    ├── plastic_microbes
    │   ├── readme.txt     # a file explining this search
    │   └── savedrecs.txt  # the results of this search
    └── biofilm_dna
        ├── readme.txt     # a file explining this other search
        └── savedrecs.txt  # the results of this other search
```

Which would be:
```
Xpress \
   -i woscc_searches/plastic_microbes/savedrecs.txt \
   -i woscc_searches/biofilm_dna/savedrecs.txt
```

## Outputs

An output file path can be provided (otherwise the output is placed on 
the working directory and named `xpress_records_YYYY-MM-DD.tsv`)

```
Xpress \
   -i woscc_searches/plastic_microbes/savedrecs.txt \
   -i woscc_searches/biofilm_dna/savedrecs.txt \
   -o my_output.tsv 
```

This output is a non redundant concatenation of all the input tables, with 
extra columns:
* `file`: input file or (comma-separated) input files where the record exists. 
* `PERSON`: user who (empty if no use of option `-u`).
* `labels`: double underscore-separated ("__") labels assigned to a record.  

## Team work

One might ask help from colleagues to go through a large list of papers.
For example, if there are 2000 papers to check and 10 people in the team, a 
work load of 200 papers per person is reasonable. For a given command line, 
the team members can use the option `-u` (or `--p-users`) multiple times, 
with each time for each user, **but with your name first**, e.g.:

```
Xpress \
   -i woscc_searches/plastic_microbes/savedrecs.txt \
   -i woscc_searches/biofilm_dna/savedrecs.txt \
   -o my_output.tsv \
   -u Franck \  # always put YOUR name first
   -u Kenia \
   -u Pau \
   -u Tam \
   -u Alex \
   -u Maria
``` 
This will split the records list into even-sized chunks at a rate of one 
chunk per user. When the chnuk selection is completed, you username will be 
added to the file name, e.g. `xpress_records_YYYY-MM-DD_<YOURNAME>.tsv`

Hence, user `Kenia` would need to run this command to work on another chunk 
of the same search(es):
```
Xpress \
   -i woscc_searches/plastic_microbes/savedrecs.txt \
   -i woscc_searches/biofilm_dna/savedrecs.txt \
   -o my_output.tsv \
   -u Kenia \  # that will parse over Kenia's share
   -u Franck \ # the order of the others is not important 
   -u Pau \
   -u Tam \
   -u Alex \
   -u Maria
``` 

## Stop/Resume

In case you want to make a break while you go through the abstracts, just 
type either of `exit` or `stop`, or use the escape keyboards command 
CTRL-C.

You will be asked if you want to remove a study (that would have 
been added by mistake) and then to add a study (that you failed to add).

Then, it will write a file with a timestamp in its name, so that when the 
command is re-run, these studies collected before the break are re-read and 
the full collection updated so that you only start where you left. Note that 
the file written with the timestamp will be located in the same folder as 
the first file given to option `-i` (remember that you can use `-i` multiple 
times to process multiple searches).

So, **if you stop and restart later, 
be sure to not only use the same file(s) and in the same command line, but also 
make sure that the first file in you command is located in the same folder as 
it was before you stopped!** Otherwise, the parsing will restart from scratch. 

## Usage
```
Xpress -i </path/to/savedrecs.txt> [ -i </path/to/another/savedrecs_file.txt>]
```


## Options
```
  -i, --i-records TEXT  Path(s) to WoS search result(s)
  -o, --o-output TEXT   Selected records (default to
                        xpress_records_YYYY-MM-DD.tsv)

  -u, --p-user TEXT     User name(s)
  -l, --p-labels TEXT   Path to file with 2 tab-separated
                        columns: keywords and labels

  --version             Show the version and exit.
  --help                Show this message and exit.
```

### Bug Reports

contact `franck.lejzerowicz@gmail.com`
