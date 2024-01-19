
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
 g3t utilities meta to_tabular --help
Usage: g3t utilities meta to_tabular [OPTIONS] META_DATA_PATH
                                     TABULAR_DATA_PATH

  Convert FHIR to tabular format (experimental)

  meta_data_path: meta_data FHIR directory
  tabular_data_path: tabular data directory

Options:
  --file_type [tsv|xlsx]  Output file format: tsv or excel

```

Convert the tabular data to FHIR.

```sh
g3t utilities meta from_tabular --help
Usage: g3t utilities meta from_tabular [OPTIONS] META_DATA_PATH
                                       TABULAR_DATA_PATH

  Convert tabular to FHIR format (experimental)

  meta_data_path: meta_data FHIR directory
  tabular_data_path: tabular data directory

```
