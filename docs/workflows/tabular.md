
## Integrate your data


### Tabular


> You can now seamlessly export and import data from TSV and Excel spreadsheets.
> 
> These are new experimental features in this release. We invite users to explore and provide feedback on these additions, which are still in the testing phase.
> Your input will be invaluable in refining and improving these features for future releases.
> 
> `Thank you` for being part of our commitment to delivering cutting-edge solutions.

Convert the FHIR data to tabular form.

```sh
 g3t meta dataframe --help
Usage: g3t meta dataframe [OPTIONS] [DIRECTORY_PATH] [OUTPUT_PATH]

  Render a metadata dataframe.

  directory_path: The directory path to the metadata.
  output_path: The output path for the dataframe. default [meta.csv]

Options:
  --dtale                         Open the graph in a browser using the dtale
                                  package for interactive data exploration.
  --data_type [Specimen|DocumentReference|ResearchSubject]
                                  Create a data frame for a specific data
                                  type.  [required]

```
