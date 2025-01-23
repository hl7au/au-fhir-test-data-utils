# au-fhir-test-data-utils
# HL7 AU FHIR Test Data Utilities

This repository also contains source code for command line utilities used to generate the FHIR JSON files and upload them to a FHIR Server. These tools are closely related  to [`au-fhir-test-data`](https://github.com/hl7au/au-fhir-test-data), a repository designed for the maintenance of a small example data set To demonstrate the functionality of the HL 7 AU standards.

There are three tools in this repository:

* `Sparked.Csv2FhirMapping` expands comma separated value files into JSON FHIR resources. 
* `Sparked.TestDataClient` uploads the values to a server 
* `xls-converter` converter adjusts some content of excel fields according to yields

## Did you find an error?
We appreciate your contributions to improving au-fhir-test-data. **If you encounter any bugs or defects, please follow the steps below to report them**:

### 1. Search for Existing Issues
Before submitting a new issue, please search the GitHub [Issues list](https://github.com/hl7au/au-fhir-test-data/issues) to check if your issue has already been reported. If you find an existing issue, you can add your comments or additional information to it.

### 2. Open a New Issue
If you do not find a similar issue, you can open a [new one](https://github.com/hl7au/au-fhir-test-data/issues). Click on the New Issue button and provide the following details:

```
Title: A brief and descriptive title for the issue.
Description: A detailed description of the issue, including:
1. Steps to reproduce the issue.
2. Expected and actual behaviour.
3. Screenshots or another related information (if applicable).
```

### Questions?
If you have a question, the best place to start is Zulip e.g. the https://chat.fhir.org/#narrow/stream/179173-australia/topic/AU.20FHIR.20Test.20Data topic

## How to Contribute to HL7 AU Test Data
We value contributions to **au-fhir-test-data**. Here’s how you can help:

### 1. Communicate Before You Start
- Open a [GitHub issue](https://github.com/hl7au/au-fhir-test-data/issues) to discuss your plans to help avoid duplication of effort, align and prioritise your contributions based on the scope of the project - refer to the [HL7 AU Test Data Project Scope Statement](https://confluence.hl7.org/display/HA/HL7+Australia+Project+Registry?preview=/184927329/248874957/Test%20Data%20Project%201.2.pdf).
- Join the fortnightly HL7 AU Infrastructure and Tooling Community Meetings ([register here](https://confluence.hl7.org/display/HAFWG/Infrastructure+and+Tooling+Contact+List)) where we discuss and triage issues. Feel free to add your issue to the [meeting agenda](https://confluence.hl7.org/pages/viewpage.action?pageId=265492851#CommunityMeetingAgendaandMinutes-MeetingDetails) and we'll aim to discuss your issue/ proposed contribution when you are present at the meeting.
- Use Zulip to connect with the team and community asynchronously: 
  - Specific topic for the HL7 AU Test Data project: [AU FHIR Test Data](https://chat.fhir.org/#narrow/stream/179173-australia/topic/AU.20FHIR.20Test.20Data)
  - General: [Australia Stream](https://chat.fhir.org/#narrow/stream/179173-australia)

### 2. Contribute Code
1. Fork this repository.
2. Create a branch and use the GitHub issue number followed by a meaningful description as the branch name for your test data contribution, sticking to lowercase and hyphens to separate words. For example, the following is a branch for GitHub issue #123 for adding resources with logical references: `123-logical-refs`
3. Make your contributions/ changes.
4. Submit a pull request (PR) for review.
5.  Once the PR has been reviewed and feedback addressed collaboratively, it will be merged into the main branch.

# Test Data Command Line Utilities
There are two command line utilities to generate the FHIR JSON files and upload the generated test data to a FHIR Server. These utilites are developed using DotNet.  
Note: The test data utilities are being transitioned from [`hl7au / au-fhir-test-data`](https://github.com/hl7au/au-fhir-test-data) to [`hl7au / au-fhir-test-data-utils`](https://github.com/hl7au/au-fhir-test-data-utils) to separate the tools for generating and managing FHIR test data from the repository containing the data itself. This transition includes integrating the new utilities into the GitHub Actions workflows of `hl7au / au-fhir-test-data` to streamline automation and test data management.

## Sparked.Csv2FhirMapping
The Sparked.Csv2FhirMapping is a utility that maps the CSV files located in the testdata-csv folder and generates the FHIR JSON files. 
The Sparked.Csv2FhirMapping.sln has a dependency on the `fhir-net-mappinglanguage` submodule, which needs to be initialised as part of cloning this repo, or if you have already clone this repository you can initialise the submodule retrospectively.

### Initialise submodules as part of cloning the repo
```
$ git clone --recurse-submodules https://github.com/hl7au/au-fhir-test-data.git

```

### Retrospectively initialise submodules after cloning the repo
```
$ git submodule update --init

```
After executing the submodule update command, the `fhir-net-mappinglanguage` will no longer be empty.

### Building Sparked.Csv2FhirMapping

0. Install the dotnet framework in the appropriate version.
1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `BuildCsvFhirMapping.bat`. This will execute a batch file that builds.


### Generate data

#### Generate Specific Data

1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `GenerateData.bat ` followed by an output directory and resource type. This will execute a batch file that generates.

Usage:
```
GenerateData.bat output-folder resource-type
```

Example with local directory `generated` and resource type `Patient`: 
```
GenerateData.bat generated Patient
```

#### Generate All Data

1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `GenerateAll.bat `. This will execute a batch file that generates all.

Usage:
```
GenerateAll.bat
```

## Upload Data
The Sparked.TestDataClient is a command line utility that uploads a batch of FHIR JSON files to a FHIR Server.

### Building Sparked.TestDataClient

0. Install the dotnet framework in the appropriate version.
1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `BuildTestDataClient.bat`. This will execute a batch file that builds.

### Uploading

#### Upload specific data

1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `UploadData.bat `. This will execute a batch file that uploads all to the fhir-server of the specified resource-type located in the input-folder. The FHIR server request's Authorization header is generated using the  auth-scheme and auth-parameter arguments.

Usage:
```
UploadData.bat fhir-server auth-scheme auth-parameter input-folder resource-type
```

Example: 
```
UploadData.bat https://fhir.hl7.org.au/aucore/fhir/DEFAULT Basic {{base64-encouded-userid:password}} generated Patient
```

#### Upload all generated data
1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `UploadGenerated.bat `. This will execute a batch file that uploads all to the fhir-server all data located in the folder `generated`. The FHIR server request's Authorization header is generated using the  auth-scheme and auth-parameter arguments.

Usage:
```
UploadGenerated.bat fhir-server auth-scheme auth-parameter
```

Example: 
```
UploadGenerated.bat https://fhir.hl7.org.au/aucore/fhir/DEFAULT Basic {{base64-encouded-userid:password}}
```

#### Upload all generated eRequesting data
1. Open a Command Prompt `cmd` ([advice](https://www.digitalcitizen.life/open-cmd/))
2. Type `UploadERequesting.bat `. This will execute a batch file that uploads all to the fhir-server all eRequesting data located in the folder `generated`. The FHIR server request's Authorization header is generated using the  auth-scheme and auth-parameter arguments.

Usage:
```
UploadERequesting.bat fhir-server auth-scheme auth-parameter
```

Example: 
```
UploadERequesting.bat https://fhir.hl7.org.au/aucore/fhir/DEFAULT Basic {{base64-encouded-userid:password}}
```
