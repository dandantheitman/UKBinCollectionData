[![Made with Python](https://img.shields.io/badge/Made%20With%20Python-red?style=for-the-badge&logo=python&logoColor=white&labelColor=red)](https://www.python.org)

[![HACS Badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg?style=for-the-badge)](https://github.com/robbrad/UKBinCollectionData)
[![Current Release](https://img.shields.io/github/v/release/robbrad/UKBinCollectionData?style=for-the-badge&filter=*)](https://github.com/robbrad/UKBinCollectionData/releases)
[![PyPi](https://img.shields.io/pypi/v/uk_bin_collection?label=PyPI&logo=pypi&style=for-the-badge&color=blue)](https://pypi.org/project/uk-bin-collection/)

[![GitHub license](https://img.shields.io/github/license/robbrad/UKBinCollectionData?style=for-the-badge)](https://github.com/robbrad/UKBinCollectionData/blob/master/LICENSE)
[![GitHub issues](https://img.shields.io/github/issues-raw/robbrad/UKBinCollectionData?style=for-the-badge)](https://github.com/robbrad/UKBinCollectionData/issues?q=is%3Aopen+is%3Aissue)
[![GitHub closed issues](https://img.shields.io/github/issues-closed-raw/robbrad/UKBinCollectionData?style=for-the-badge)](https://github.com/robbrad/UKBinCollectionData/issues?q=is%3Aissue+is%3Aclosed)
[![GitHub contributors](https://img.shields.io/github/contributors/robbrad/UKBinCollectionData?style=for-the-badge)](https://github.com/robbrad/UKBinCollectionData/graphs/contributors)

[![Test Councils](https://img.shields.io/github/actions/workflow/status/robbrad/UKBinCollectionData/behave.yml?style=for-the-badge&label=Test+Councils)](https://github.com/robbrad/UKBinCollectionData/actions/workflows/behave.yml)
![Codecov](https://img.shields.io/codecov/c/gh/robbrad/UKBinCollectionData?style=for-the-badge)
[![CodeQL Analysis](https://img.shields.io/github/actions/workflow/status/robbrad/UKBinCollectionData/codeql-analysis.yml?style=for-the-badge&label=CodeQL+Analysis)](https://github.com/robbrad/UKBinCollectionData/actions/workflows/codeql-analysis.yml)
[![Publish Release](https://img.shields.io/github/actions/workflow/status/robbrad/UKBinCollectionData/release.yml?style=for-the-badge&label=Publish+Release)](https://github.com/robbrad/UKBinCollectionData/actions/workflows/release.yml)
[![Test Report Deployment](https://img.shields.io/github/actions/workflow/status/robbrad/UKBinCollectionData/pages%2Fpages-build-deployment?style=for-the-badge&label=Test+Report+Deployment)](https://github.com/robbrad/UKBinCollectionData/actions/workflows/pages/pages-build-deployment)

# UK Bin Collection Data (UKBCD)
This project aims to provide a neat and standard way of providing bin collection data in JSON format from UK councils that have no API to do so.

Why do this?
You might want to use this in a Home Automation - for example say you had an LED bar that lit up on the day of bin collection to the colour of the bin you want to take out, then this repo provides the data for that. 

**PLEASE respect a councils infrastructure / usage policy and only collect data for your own personal use on a suitable frequency to your collection schedule.**

Most scripts make use of [Beautiful Soup 4](https://pypi.org/project/beautifulsoup4/) to scrape data, although others use different approaches, such as emulating web browser behaviour, or reading data from CSV files.

[![](https://img.shields.io/badge/-41BDF5?style=for-the-badge&logo=homeassistant&logoColor=white&label=HomeAssistant+Thread)](https://community.home-assistant.io/t/bin-waste-collection/55451)
[![](https://img.shields.io/badge/Request%20a%20council-gray?style=for-the-badge&logo=github&logoColor=white)](https://github.com/robbrad/UKBinCollectionData/issues/new/choose)

---

## Home Assistant Usage

### Install with HACS (recommended)

1. Ensure you have [HACS](https://hacs.xyz/) installed
1. In the Home Assistant UI go to `HACS` > `Integrations` > `⋮` > `Custom repositories`.
1. Enter `https://github.com/robbrad/UKBinCollectionData` in the `Repository` field.
1. Select `Integration` as the category then click `ADD`.
1. Click `+ Add Integration` and search for and select `UK Bin Collection Data` then click `Download`.
1. Restart your Home Assistant.
1. In the Home Assistant UI go to `Settings` > `Devices & Services` click `+ Add Integration` and search for `UK Bin Collection Data`.
1. If your see a "URL of the remote Selenium web driver to use" field when setting up your council, you'll need to provide the URL to a web driver you've set up seperately such as [standalone-chrome](https://hub.docker.com/r/selenium/standalone-chrome).

### Install manually

1. Open the folder for your Home Assistant configuration (where you find `configuration.yaml`).
1. If you do not have a `custom_components` folder there, you need to create it.
1. [Download](https://github.com/robbrad/UKBinCollectionData/archive/refs/heads/master.zip) this repository then copy the folder `custom_components/uk_bin_collection` into the `custom_components` folder you found/created in the previous step.
1. Restart your Home Assistant.
1. In the Home Assistant UI go to `Settings` > `Devices & Services` click `+ Add Integration` and search for `UK Bin Collection Data`.

---

## Standalone Usage
```commandline
PS G:\Projects\Python\UKBinCollectionData\uk_bin_collection\collect_data.py
usage: collect_data.py [-h] [-p POSTCODE] [-n NUMBER] [-u UPRN] module URL

positional arguments:
  module                Name of council module to use                           (required)
  URL                   URL to parse                                            (required)

options:
  -h, --help                            show this help message                  (optional)
  -p POSTCODE, --postcode POSTCODE      Postcode to parse - should include      (optional)
                                        a space and be wrapped in double
                                        quotes                                  
  -n NUMBER, --number NUMBER            House number to parse                   (optional)
  -u UPRN, --uprn UPRN                  UPRN to parse                           (optional)
```

### Quickstart
The basic command to execute a script is:
```commandline
python collect_data.py <council_name> "<collection_url>"
```
where ```council_name``` is the name of the council's .py script (without the .py) and ```collection_url``` is the URL to scrape.
The help documentation refers to these as "module" and "URL", respectively. Supported council scripts can be found in the `uk_bin_collection/uk_bin_collection/councils` folder.

Some scripts require additional parameters, for example, when a UPRN is not passed in a URL, or when the script is not scraping a web page.
For example, the Leeds City Council script needs two additional parameters - a postcode, and a house number. This is done like so:

```commandline
python collect_data.py LeedsCityCouncil https://www.leeds.gov.uk/residents/bins-and-recycling/check-your-bin-day -p "LS1 2JG" -n 41
```
- A **postcode** can be passed with `-p "postcode"` or `--postcode "postcode"`. The postcode must always include a space in the middle and
be wrapped in double quotes (due to how command line arguments are handled).
- A **house number** can be passed with `-n number` or `--number number`.
- A **UPRN reference** can be passed with `-u uprn` or `--uprn uprn`.

To check the parameters needed for your council's script, please check the [project wiki](https://github.com/robbrad/UKBinCollectionData/wiki) for more information.


### Project dependencies
Some scripts rely on external packages to function. A list of required scripts for both development and execution can be found in the project's [PROJECT_TOML](https://github.com/robbrad/UKBinCollectionData/blob/feature/%2353_integration_tests/pyproject.toml) 
Install can be done via 
`poetry install` from within the root of the repo.

---

## UPRN Finder
Some councils make use of the UPRN (Unique property reference number) to identify your property. You can find yours [here](https://www.findmyaddress.co.uk/search) or [here](https://uprn.uk/).

---

## Requesting your council
To make a request for your council, first check the [Issues](https://github.com/robbrad/UKBinCollectionData/issues) page to make sure it has not already been requested. If not, please fill in a new [Council Request](https://github.com/robbrad/UKBinCollectionData/issues/new/choose) form, including as much information as possible, including:
- Name of the council
- URL to bin collections
- An example postcode and/or UPRN (whichever is relevant)
- Any further information

Please be aware that this project is run by volunteer contributors and completion depends on numerous factors - even with a request, we cannot guarantee if/when your council will get a script.

---

## Reports

- [3.10](https://robbrad.github.io/UKBinCollectionData/3.10/)
- [3.11](https://robbrad.github.io/UKBinCollectionData/3.11/)

---

## FAQ
#### I've got an issue/support question - what do I do?
Please post in the [HomeAssistant thread](https://community.home-assistant.io/t/bin-waste-collection/55451) or raise a new (non council request) [issue](https://github.com/robbrad/UKBinCollectionData/issues/new).

#### I'd like to contribute, where do I start?
Contributions are always welcome! See ```CONTRIBUTING.md``` to get started. Please adhere to the project's [code of conduct](https://github.com/robbrad/UKBinCollectionData/blob/master/CODE_OF_CONDUCT.md).

- If you're new to coding/Python/BeautifulSoup, feel free to check [here](https://github.com/robbrad/UKBinCollectionData/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) for issues that are good for newcomers!
- If you would like to try writing your own scraper, feel free to fork this project and use existing scrapers as a base for your approach (or `councilclasstemplate.py`).

## Contributors
<a href="https://github.com/robbrad/UKBinCollectionData/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=robbrad/UKBinCollectionData"  alt="Image of contributors"/>
</a>

