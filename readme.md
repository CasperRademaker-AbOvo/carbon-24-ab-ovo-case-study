# Carbon24 hackathon submission for Ab Ovo

This repository accompanies our Carbon24 submission, more information on our submission can be found at https://github.com/Green-Software-Foundation/hack/issues/125

## Usage instructions: Impact Framework Manifest

1. Clone this repo
1. Install Node.js v20 for your platform, if not installed yet, along with npm
1. Add a terminal session with the cloned repository as your working directory. 
1. Execute `npm install` to install dependencies
1. If you'd like to use our example input dataset, unzip `./example-data/if-manifest-data.zip` into your working directory. You can also paste in custom data in our Impact Framework manifest at `./if-manifest-no-data.yaml`
1. Execute `npx ie --manifest ./if-manifest-data.yaml --output ./if-manifest-output` to process the example input manifest file. Results are written to `if-manifest-output.yaml` in your active directory.

## Demo of Power BI report

[Check out this public demo of the Power BI report](https://app.powerbi.com/view?r=eyJrIjoiODBmMWFiYWEtNGI5Ny00OTYwLWFhNTAtYTNkYTBkZTJhZTFjIiwidCI6ImIwZGM1ZWE3LTExOTctNDUxMC05MjhhLTkyZDJjZjRiNzdlZSIsImMiOjh9)

## Usage instruction: Power BI semantic model / report

1. Install Power BI Desktop (note that as of the moment of writing (April 2024), this is only available for Windows)
1. Create a dedicated directory on your hard drive where you will store input files for the Power BI model.
1. Convert the output manifest file to JSON (Power BI currently can't import YAML). For example you can use https://github.com/bronze1man/yaml2json. Store the JSON in the created directory from previous step. On Windows we can use `Get-Content .\if-manifest-output.yaml | {yaml2json binary} > .\if-manifest-output.json` from Powershell. 
1. Create a subdirectory for grid emission data from https://www.electricitymaps.com/data-portal. The Power BI model only works with hourly data. Download only the timeframes that overlap with your input dataset, and only regions of particular interest, as all input data will currently be used to join with your immpact framwork manifest. More data means longer processing times (or possibly, out of memory issues).
1. In the created directory (from step 2) you will also need to store a CSV file with log lines from your application. The requirements for this file are mentioned in section `Requirements for application logs`.
1. Open `Carbon24 Ab Ovo submission.pbit` in Power BI Desktop.
1. You will be requested to enter four parameters. The first parameter should be kept as per default value. For the other parameters, provide the folder and file locations for the relevant input data files. Examples: `D:\impact-framework\electricity-maps-hourly-emissions`, `D:\impact-framework\output-manifest\if-manifest-output.json` and `D:\impact-framework\application-logs\application-logs.csv`
1. Click OK and wait for data load to finish. If this takes too long or you run into memory issues, firstly limit the amount of data in the ElectricityMaps folder, and secondly limit the amount of data in your application logs and manifest. 
1. Report can be edited as per your requirements, and published to a Power BI tenant.

## Requirements for application logs

- Logs should be stored in a single CSV file, with semicolon as delimiter.  
- Each row in the CSV file should contain a single function/process call or transaction in your application.  
- All observations are assumed to take one second. If your observations take longer than one second, please split them over multiple rows.  

The CSV file should contain the following columns. All columns are mandatory to exist currently.  

1. `META_UNIQUEID`: Unique identifier of server environment that the observation took place on. Needs to match with values from manifest.
1. `timestamp`: Starting timestamp of observation, on second level, in ISO 8601 notation. Please note that the timezone postfix should NOT be included, all timestamps are assumed to be in UTC time.
1. `function`: Free-form descriptive name for the function/process/transaction being called.
1. `memoryusage_bytes`: Memory usage in bytes by function call.
1. `cpu_cycles_used`: CPU cycles used by function call.
1. `system_component`: Free-form name of component area of function call. This is used in Power BI to group function calls, for example, you could split between scheduled background processes and user-initiated function calls. 

For an example file, see `application-logs.csv` in `./example-data/if-manifest-data.zip`

## Support

Need support, or do you have questions? Please create an issue in this repository or [contact us at Ab Ovo](https://ab-ovo.com/contact/) 