### Automation purpose ###
This automation is the solution of a technical test with the following statements:

1. Login to the ACME System: https://acme-test.uipath.com/
2. Navigate to Work Items
3. Build a blank Excel report that will contain the extracted check data. This report should contain the following columns:
   1. Client Name
   2. Client ID
   3. Check Number
   4. Check Data
   5. Check Amount
4. Extract all Work Items with “Type” = “WI2” and “Status” = “Open”
5. For each Work item of “Type” = “WI2” and “Status” = “Open”:
   1. Download the check request PDF
   2. Extract the following information from the PDF:
      1. Client Name
      2. Client ID
      3. Check Number
      4. Check Date
      5. Check Amount
  3. Add the extracted data to the Excel Report into its applicable column
  4. Go back to the Work Item details page
  5. Click on Update Work Item
  6. Update the Work Item comments with “Extracted check information to report.” and status to “Completed”
6. Save the completed Excel report with the name “Client-Check-Copy-Report-[Current Date].xlsx”
7. Display a Message at the end of the process that shows the file path to the completed excel report and indicates the process completed successfully.

### Development environment ###
#### Project tools ####
- Windows 10
- Uipath Community Licence Latest
- Studio version 2025.0.166 STS
- Google Chrome Dev Browser Version 138.0.7166.3
- Uipath extension in chrome version 24.10.2
- Excel en licencia de Microsoft 365 version 16.0.18730.20142
  
#### Project dependencies ####
- UiPath.Credentials.Activities: [2.1.0]
- UiPath.Excel.Activities: [3.0.1]
- UiPath.PDF.Activities: [3.22.1]
- UiPath.System.Activities: [25.4.2]
- UiPath.Testing.Activities: [25.4.0]
- UiPath.UIAutomation.Activities: [24.10.7]

### Steps to launch the automation correctly ###
1. Create an account in https://acme-test.uipath.com/
2. Go to Data/Config.xlsx and change the following values:
   1. Robot_Path: Put the path where you clone this project on your machine.
3. Create a credential with the name "ACMESystem1_Credential" in Windows Credential Manager to store your ACME website credentials.
4. Go to chrome settings (chrome://settings/). In the Downloads menu activate "Ask where to save each file before downloading" option.

### Documentation is included in the Documentation folder ###

### REFrameWork Template ###
**Robotic Enterprise Framework**

* Built on top of *Transactional Business Process* template
* Uses *State Machine* layout for the phases of automation project
* Offers high level logging, exception handling and recovery
* Keeps external settings in *Config.xlsx* file and Orchestrator assets
* Pulls credentials from Orchestrator assets and *Windows Credential Manager*
* Gets transaction data from Orchestrator queue and updates back status
* Takes screenshots in case of system exceptions


### How It Works ###

1. **INITIALIZE PROCESS**
 + ./Framework/*InitiAllSettings* - Load configuration data from Config.xlsx file and from assets
 + ./Framework/*GetAppCredential* - Retrieve credentials from Orchestrator assets or local Windows Credential Manager
 + ./Framework/*InitiAllApplications* - Open and login to applications used throughout the process

2. **GET TRANSACTION DATA**
 + ./Framework/*GetTransactionData* - Fetches transactions from an Orchestrator queue defined by Config("OrchestratorQueueName") or any other configured data source

3. **PROCESS TRANSACTION**
 + *Process* - Process trasaction and invoke other workflows related to the process being automated 
 + ./Framework/*SetTransactionStatus* - Updates the status of the processed transaction (Orchestrator transactions by default): Success, Business Rule Exception or System Exception

4. **END PROCESS**
 + ./Framework/*CloseAllApplications* - Logs out and closes applications used throughout the process


### For New Project ###

1. Check the Config.xlsx file and add/customize any required fields and values
2. Implement InitiAllApplications.xaml and CloseAllApplicatoins.xaml workflows, linking them in the Config.xlsx fields
3. Implement GetTransactionData.xaml and SetTransactionStatus.xaml according to the transaction type being used (Orchestrator queues by default)
4. Implement Process.xaml workflow and invoke other workflows related to the process being automated
