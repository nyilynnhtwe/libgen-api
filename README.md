# LibGen Search API

Search Library Genesis using this Python library.

Allows you to search by title or author, and to optionally filter results.

![PyPI - Downloads](https://img.shields.io/pypi/dm/libgen-api?style=for-the-badge)
![GitHub](https://img.shields.io/github/license/harrison-broadbent/libgen-api?style=for-the-badge)
![PyPI](https://img.shields.io/pypi/v/libgen-api?style=for-the-badge)
![GitHub Repo stars](https://img.shields.io/github/stars/harrison-broadbent/libgen-api?style=for-the-badge)

## Contents

- [Getting Started](#getting-started)
- [Basic Searching](#basic-searching)
- [Filtered Searching](#filtered-searching)
  - [Filter Fields](#filter-fields)
  - [Filtered Title Searching](#filtered-title-searching)
  - [Filtered Author Searching](#filtered-author-searching)
  - [Non-exact Filtered Searching](#non-exact-filtered-searching)
- [Resolving mirror links](#resolving-mirror-links)
- [More Examples](#more-examples)
- [Further Information](#further-information)
- [Contributors](#contributors)

---

Please ⭐ if you find this useful!

---

## Getting Started

Install the package -

```
pip install libgen-api
```

Perform a basic search -

```python
from libgen_api import LibgenSearch
s = LibgenSearch()
results = s.search_title("Pride and Prejudice")
print(results)
```

Check out the [results layout](#results-layout) to see how the results data is formatted.

## Basic Searching:

Search by title or author:

### Title:

```python
from libgen_api import LibgenSearch
s = LibgenSearch()
results = s.search_title("Pride and Prejudice")
print(results)
```

### Author:

```python
from libgen_api import LibgenSearch
s = LibgenSearch()
results = s.search_author("Jane Austen")
print(results)
```

## Filtered Searching

Skip to the [Examples](#filtered-title-searching)

- You can define a set of filters, and then use them to filter search results for both title and author searches.
- You can filter results by any combination of the fields returned from a search.
- By default, filtering will remove results that do not match the filters exactly -
  - This can be adjusted by setting `exact_match=False` when calling one of the filter methods.
  - Filters are case-sensitive, and for numerical values (ID, Pages etc. ) the given value, of type string, must match exactly.
- Case-insensitive and substring filtering can be specified by passing `exact_match=False` as an argument.

### Filter Fields

You can filter against any of the Library Genesis column names, which are given as -

```python
col_names = [
        "ID",
        "Author",
        "Title",
        "Publisher",
        "Year",
        "Pages",
        "Language",
        "Size",
        "Extension",
        "Mirror_1",
        "Mirror_2",
        "Mirror_3",
        "Mirror_4",
        "Mirror_5",
        "Edit",
    ]
```

### Filtered Title Searching

```python
from libgen_api import LibgenSearch

tf = LibgenSearch()
title_filters = {"Year": "2007", "Extension": "epub"}
titles = tf.search_title_filtered("Pride and Prejudice", title_filters, exact_match=True)
print(titles)
```

### Filtered Author Searching

```python
from libgen_api import LibgenSearch

af = LibgenSearch()
author_filters = {"Language": "German", "Year": "2009"}
titles = af.search_author_filtered("Agatha Christie", author_filters, exact_match=True)
print(titles)
```

### Non-exact Filtered Searching

```python
from libgen_api import LibgenSearch

ne_af = LibgenSearch()
partial_filters = {"Year": "200"}
titles = ne_af.search_author_filtered("Agatha Christie", partial_filters, exact_match=False)
print(titles)
```

## Resolving mirror links

The mirror links returned in the results are not direct download links and do not resolve to a downloadable
URL without further parsing. Generally, the `Mirror_1` link in the results contains the most useful download URLs, so
this is used by the `resolve_download_links` helper method. This method takes a single dictionary from the results and 
returns a dictionary of all the download links for `Mirror_1`:
```python
from libgen_api import LibgenSearch
s = LibgenSearch()
results = s.search_author("Jane Austen")
item_to_download = results[0]
download_links = s.resolve_download_links(item_to_download)
print(download_links)
```

Example output:
```json
{
  "GET": "http://example.com/file.epub",
  "Cloudflare": "http://example.com/file.epub",
  "IPFS.io": "http://example.com/file.epub",
  "Infura": "http://example.com/file.epub",
}
```

### More Examples

See the [testing file](test.py) for more examples.

## Results Layout

Results are returned as a list of dictionaries:

```json
[
  {
    "Author": "John Smith",
    "Edit": "http://example.com",
    "Extension": "epub",
    "ID": "00000",
    "Language": "German",
    "Mirror_1": "http://example.com",
    "Mirror_2": "http://example.com",
    "Mirror_3": "http://example.com",
    "Mirror_4": "http://example.com",
    "Mirror_5": "http://example.com",
    "Pages": "410",
    "Publisher": "Publisher",
    "Size": "1005 Kb",
    "Title": "Title",
    "Year": "2021"
  }
]
```

## Further information

- If there are no results, the library will return an empty array.
- All fields are strings
- If a value is not present, the field will contain an empty string
- Some listings will have page count listed in the form of "count[secondary-count]" as this is how they appear on LibGen.
- Only the first page of results (max. 25) will be returned.

## Contributors

A massive thank you to those that have contributed to this project!

Please don't hesitate to raise an issue, or fork this project and improve on it.

Specific thanks to -

- [calmoo](https://github.com/calmoo)
