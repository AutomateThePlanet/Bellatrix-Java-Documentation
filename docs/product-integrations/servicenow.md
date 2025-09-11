---
layout: default
title:  "ServiceNow"
excerpt: "Learn how to use BELLATRIX ServiceNow for ServiceNow applications."
date:   2025-09-10 06:50:17 +0200
parent: product-integrations
permalink: /product-integrations/servicenow/
anchors:
  what-is-servicenow: What Is ServiceNow?
  usage: Usage
  config: Configuration
---

# What is ServiceNow?

**ServiceNow** is a cloud-based platform used by many organizations across different industries to manage workflows and services.
BELLATRIX ServiceNow module offers ready for testing serviceNow based applications.
The page presents the examples of what this module offers.

# Usage

The following section presents comprehensive test scenarios for common ServiceNow application use cases.

## Login to ServiceNow instance

### Example

#### Login with Credentials Input

```java
@Test
public void loadInitialServiceNowPageWithCredentialsInput() {
    ServiceNowPage serviceNowPage = new ServiceNowPage();
    serviceNowPage.loginSection().login("user", "pass");

    serviceNowPage.browser().assertLandedOnPage(BaseInstancesUrlGeneration.getSnInitialLoadUrl());
}
```

#### Login with Configuration Credentials

```java
@Test
public void loadInitialServiceNowPageWithConfigCredentials() {
    ServiceNowPage serviceNowPage = new ServiceNowPage();
    serviceNowPage.loginSection().login();

    serviceNowPage.browser().assertLandedOnPage(BaseInstancesUrlGeneration.getSnInitialLoadUrl());
}
```

### Explanations
***Login with Credentials Input***

The test verifies that the application correctly loads the initial ServiceNow page after logging in with explicitly provided credentials.
This approach is useful when you need to test with different user credentials or when credentials need to be passed directly during test execution.

***Login with Configuration Credentials***

The test verifies that the application correctly loads the initial ServiceNow page after logging in with credentials configured in the system.
The test implementation uses credentials that are pre-configured in the system settings

Using configuration-based credentials is recommended for:
- Keeping sensitive data out of test code
- Easier credential management across different environments
- Better security practices in version control systems

## User Impersonation in ServiceNow

This guide demonstrates how to implement automated user impersonation functionality for ServiceNow instances using the BELLATRIX framework. User impersonation allows administrators and authorized users to temporarily assume the identity of another user for testing, troubleshooting, or administrative purposes.

### Example

#### Impersonate User with Role Verification

```java
@Test
public void impersonateAsUserWithRole() {
    var serviceNowPage = new ServiceNowPage();
    serviceNowPage.loginSection().login();
    serviceNowPage.impersonateUser("User Impersonate");
    
    serviceNowPage.assertUserImpersonate("User Impersonate");
}
```

### Explanations

#### Impersonate User with Role Verification

This test verifies that the application correctly performs user impersonation functionality within ServiceNow and validates that the impersonation was successful.

**Step-by-step breakdown:**

```java
var serviceNowPage = new ServiceNowPage();
```
Creates a new instance of the ServiceNowPage object, which provides access to ServiceNow-specific functionality and page elements

```java
serviceNowPage.loginSection().login();
```
Performs the initial login to ServiceNow using the configured credentials. This establishes the authenticated session required for impersonation operations
        
```java
serviceNowPage.impersonateUser("User Impersonate");
 ```
Initiates the impersonation process for the specified user. This method:
   - Navigates to the user impersonation interface in ServiceNow
   - Searches for the target user "User Impersonate"
   - Activates the impersonation session

```java
serviceNowPage.assertUserImpersonate("User Impersonate");
```
Validates that the impersonation was successful by:
   - Verifying that the current session is running under the impersonated user's context
   - Checking that the user interface reflects the impersonated user's identity

#### Usage Notes

**Important Considerations:**
- Ensure the test user has sufficient privileges to perform impersonation
- The target user "User Impersonate" must exist in the ServiceNow instance
- Impersonation sessions should be properly terminated after testing
- Consider the security implications of automated impersonation in production environments

The test validates both the technical functionality of impersonation and serves as a foundation for more complex user permission and role-based testing scenarios.


## ServiceNow Record Form Operations

### Example

#### Open Record from Table

```java
@Test
public void correctPageOpen_when_selectFirstRecord() {
    serviceNowPage.loginSection().login();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    serviceNowTableViewPage.openRecordInfoFromMainTable(1);

    var recordPage = new IncidentRecordPage();
    recordPage.asserts().assertRecordMainHeader("Incident");
}
```

#### Open Record by sys_id

```java
@Test
public void openRecordFormBySysId () {
    serviceNowPage.loginSection().login();
    var recordUrl =BaseInstancesUrlGeneration.getSnRecordBaseUrl(ProjectTables.INCIDENT_TABLE,"record_sys_id");
    serviceNowPage.navigateToPage(recordUrl);

    var recordPage = new IncidentRecordPage();
    serviceNowPage.switchToInnerFrame();
    recordPage.asserts().assertRecordMainHeader("Incident");
    serviceNowPage.browser().assertLandedOnPage(BaseInstancesUrlGeneration.getSnRecordBaseUrl(ProjectTables.INCIDENT_TABLE, "record_sys_id"));
}
```

#### Open New Record Form

```java
@Test
public void newRecordFormOpen_when_clickNewButton () {
    serviceNowPage.loginSection().login();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    serviceNowTableViewPage.clickOnNewButton();

    var newRecordPage = new IncidentRecordPage();
    var mainFormModel = NewIncidentForm.builder().build();
    newRecordPage.asserts().validateFormState(mainFormModel, newRecordPage.map().mainForm(), false);
}
```

### Explanations

***Open Record from Table***

This test verifies that clicking on a record from the incident table correctly opens the record form page. The test navigates to the incident table, selects the first record, and validates that the correct incident record page is displayed. This approach is useful when you need to interact with records through the standard ServiceNow table interface.

***Open Record by sys_id***

This test demonstrates how to directly navigate to a specific record using its unique system ID (sys_id). The test constructs a URL using the record's sys_id and navigates directly to that record. This method is particularly useful when you know the exact record you want to work with, bypassing the need to search through tables. Note that this approach requires switching to the inner frame context before interacting with record elements.

***Open New Record Form***

This test verifies the functionality of creating a new incident record through the "New" button. After navigating to the incident table and clicking the "New" button, the test validates that a blank incident form is properly displayed with the correct initial state. This approach is essential for testing record creation workflows and ensuring that new record forms are properly initialized.

Using these record form operations enables:
- Direct navigation to specific records when the sys_id is known
- Standard table-based record selection workflows
- New record creation testing
- Proper form state validation for both existing and new records

## ServiceNow Record Form Sub-tabs Operations

### Example

#### Verify Sub-tabs Presence

```java
@Test
public void correctTabsRecordExist_when_openRecord() {
    serviceNowPage.loginSection().login();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    serviceNowTableViewPage.openRecordInfoFromMainTable(1);

    var recordPage = new IncidentRecordPage();

    List<String> expectedTabs = new ArrayList<>(List.of(
        "Task SLAs",
        "Affected CIs",
        "Impacted Services/CIs",
        "Child Incidents"));

    recordPage.asserts().assertBottomTablesTabs(expectedTabs);
}
```

```java
@Test
public void subTabDetailsOpen_when_clickOnTab() {
    serviceNowPage.loginSection().login();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    serviceNowTableViewPage.openRecordInfoFromMainTable(1);

    var recordPage = new IncidentRecordPage();
    recordPage.map().tabByLabel("Child Incidents").click();

    List<String> expectedTabColumns = new ArrayList<>(List.of(
        "Number",
        "Opened",
        "Short description",
        "Caller",
        "Priority",
        "State",
        "Category",
        "Assignment group",
        "Assigned to",
        "Updated",
        "Updated by"));

    recordPage.asserts().assertInnerTableColumns("Child Incidents", expectedTabColumns);
}
```

```java
@Test
public void newRecordFormOpen_when_clickNewButtonForTheSelectedBottomTableTab() {
    serviceNowPage.loginSection().login();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    serviceNowTableViewPage.openRecordInfoFromMainTable(1);

    var recordPage = new IncidentRecordPage();
    recordPage.map().tabByLabel("Child Incidents").click();
    recordPage.map().newButtonByHeading("Child Incidents").click();

    recordPage.asserts().assertRecordMainHeader("Incident");
    recordPage.asserts().assertRecordHeader("New record");
}
```

### Explanations

#### Verify Sub-tabs Presence

This test verifies that all expected sub-tabs are present when opening an incident record. This ensures the record form displays all necessary related information sections.

**Benefits:**
- Validates complete UI structure of incident records
- Ensures all related data sections are accessible
- Provides confidence that the record form renders correctly
- Helps catch missing tabs that could impact user workflows

#### Verify Sub-Tab details

This test validates the functionality of opening sub-tabs and verifies that the correct columns are displayed in the sub-tab's data table. It specifically tests the "Child Incidents" tab and confirms all expected columns are present, ensuring users can access and view related incident data properly.

**Benefits:**
- Confirms sub-tab navigation works correctly
- Validates data table structure within sub-tabs
- Ensures all necessary information columns are available
- Helps identify missing or incorrectly configured table columns

#### Verify new Record Form

This test verifies the ability to create new records directly from sub-tabs. It tests clicking the "New" button within the "Child Incidents" tab and confirms that the new record form opens correctly with appropriate headers. This functionality is essential for creating related records without leaving the current context.

**Benefits:**
- Validates record creation workflow from sub-tabs
- Ensures proper form navigation and context preservation
- Confirms new record forms open with correct headers
- Tests critical functionality for incident management processes

## Working with ServiceNow Table View

### Example

#### Table Navigation and Header Verification

```java
@Test
public void correctRecordsDisplayedInTableView_when_navigateToTableViewByTableName() {
    serviceNowPage.loginSection().login();
    var serviceNowTableViewPage = new ServiceNowTableViewPage();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);

    serviceNowTableViewPage.asserts().assertPartialUrl(ProjectTables.INCIDENT_TABLE);
}
```
```java
@Test
public void correctMainHeaderDisplayedInTableView_when_navigateToTableViewByTableName() {
    serviceNowPage.loginSection().login();
    var serviceNowTableViewPage = new ServiceNowTableViewPage();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);

    serviceNowTableViewPage.asserts().assertMainHeader("Incidents");
}
```

#### Column Visibility Validation

```java
@Test
public void correctColumnsAreVisibleInTableView_when_navigateToTableViewByTableName() {
    serviceNowPage.loginSection().login();
    var serviceNowTableViewPage = new ServiceNowTableViewPage();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    List<String> expectedColumns = new ArrayList<>(List.of(
            "Number",
            "Opened",
            "Short description",
            "Caller",
            "Priority",
            "State",
            "Category",
            "Assignment group",
            "Assigned to",
            "Updated",
            "Updated by"));

    serviceNowTableViewPage.asserts().assertTableLabels(expectedColumns);
}
```

#### Record Search and Validation

```java
@Test
public void confirmRecordExistByValueAndColumnName() {
    serviceNowPage.loginSection().login();

    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);

    serviceNowTableViewPage.assertTableColumnContains("Number", "target_record_value");
}
```
```java
@Test
public void correctRecordsSelected_when_searchByValueInColumn() {
    serviceNowPage.loginSection().login();

    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    serviceNowTableViewPage.searchBy("Number", "target_record_value");

    serviceNowTableViewPage.asserts().assertTableRowsAre(1);
}
```

### Explanations

#### Table Navigation and Header Verification
This test verifies that the application correctly navigates to the ServiceNow Incident table and displays the proper header text.

#### Column Visibility Validation
This test ensures that all essential columns are visible in the Incident table view. The test retrieves the list of visible column names and validates the presence of key columns.

#### Record Search and Validation
The tests demonstrate how to search for specific records within the ServiceNow table view. 

This functionality is essential for quickly locating specific incidents in large datasets.

## Left Navigation in ServiceNow

### Example

#### Option Not Visible When Non-Authorized User Search In Left Navigation

```java
@Test
public void optionNotVisible_when_nonAuthorizedUserSearchInLeftNavigation() {
    serviceNowPage.loginSection().login();
    serviceNowPage.impersonateUser("User Impersonate");
    serviceNowPage.clickMenuItem(ServiceNowMenuItems.All);

    serviceNowPage.filterDataInLeftNavigationPane(ProjectLeftNavigationItem.INCIDENTS);

    var leftNavigationSection = new LeftNavigationSection();
    leftNavigationSection.asserts().assertLeftNavigationItemNotPresentByPath(ProjectLeftNavigationItem.SERVICE_DESK, ProjectLeftNavigationItem.INCIDENTS);
}
```

```java
@Test
public void optionVisible_when_authorizedUserSearchInLeftNavigation() {
    serviceNowPage.loginSection().login();
    serviceNowPage.clickMenuItem(ServiceNowMenuItems.All);

    serviceNowPage.filterDataInLeftNavigationPane(ProjectLeftNavigationItem.INCIDENTS);

    serviceNowPage.asserts().assertLeftNavigationItemPresentByPath(ProjectLeftNavigationItem.SERVICE_DESK, ProjectLeftNavigationItem.INCIDENTS);
}
```

```java
@Test
public void correctPageOpen_when_selectOptionInLeftNavigation() {
    serviceNowPage.loginSection().login();
    serviceNowPage.clickMenuItem(ServiceNowMenuItems.All);

    serviceNowPage.openFromLeftNavigation(ProjectLeftNavigationItem.SELF_SERVICE, ProjectLeftNavigationItem.INCIDENTS);
    serviceNowPage.switchToLastTab();

    serviceNowTableViewPage.asserts().assertPartialUrl(ProjectTables.INCIDENT_TABLE);
}
```

### Explanations

***Option Not Visible When Non-Authorized User Search In Left Navigation***

The test verifies that navigation options are properly hidden from users who lack the necessary permissions. It uses user impersonation to simulate a non-authorized user and confirms that restricted menu items are not visible in the left navigation pane. This test is essential for validating role-based access control and ensuring security compliance in ServiceNow applications.

***Option Visible When Authorized User Search In Left Navigation***

The test verifies that authorized users can see and access navigation options they have permissions for. It confirms that the left navigation properly displays menu items based on user roles and permissions. This test ensures that legitimate users have access to the functionality they need while maintaining proper access controls.

***Correct Page Open When Select Option In Left Navigation***

The test verifies that selecting navigation options leads to the correct destination pages. It validates the navigation flow by clicking on menu items and confirming that the browser navigates to the expected URL. This test ensures that the left navigation menu functions correctly and provides users with reliable access to different sections of the ServiceNow application.

Using left navigation tests is recommended for:

- Validating role-based access control and security permissions
- Ensuring navigation functionality works correctly across different user roles
- Verifying that menu items lead to the correct pages and functionality
- Testing user experience consistency in ServiceNow applications

## ServiceNow Data Creation Tests

### Example

#### Create Entity
```java
@Test
public void createEntity() {
    IncidentRepository incidentRepository = new IncidentRepository();

    Incident incident = incidentRepository.create(Incident.builder()
        .caller(currentUser.getSysId())
        .shortDescription(String.format("Incident Report %s", TimestampBuilder.buildUniqueTextByPrefix("au")))
        .build());

    var entityCreated = incidentRepository.getById(incident);

    Assertions.assertEquals(incident.getCaller(), entityCreated.getCaller());
}
```

#### Update Entity
```java
@Test
public void updateEntity() {
    IncidentRepository incidentRepository = new IncidentRepository();

    Incident incident = incidentRepository.create(Incident.builder()
            .caller(currentUser.getSysId())
            .shortDescription(String.format("Incident Report %s", TimestampBuilder.buildUniqueTextByPrefix("au")))
            .build());


    var updatedIncident = Incident.builder().sysId(incident.getSysId()).shortDescription("Description updated").build();
    incidentRepository.update(updatedIncident);

    var entityFromAPI = incidentRepository.getById(incident);

    Assertions.assertEquals(updatedIncident.getShortDescription(), entityFromAPI.getShortDescription());
}
```

#### Delete Entity
```java
@Test
public void deleteEntity() {
    IncidentRepository incidentRepository = new IncidentRepository();

    Incident incident = incidentRepository.create(Incident.builder()
        .caller(currentUser.getSysId())
        .shortDescription(String.format("Incident Report %s", TimestampBuilder.buildUniqueTextByPrefix("au")))
        .build());

    incidentRepository.delete(incident);

    incidentRepository.validateEntityDoesNotExist(incident);
}
```

#### Get Entity
```java
@Test
public void getEntity() {
    IncidentRepository incidentRepository = new IncidentRepository();

    Incident incident = incidentRepository.create(Incident.builder()
        .caller(currentUser.getSysId())
        .shortDescription(String.format("Incident Report %s", TimestampBuilder.buildUniqueTextByPrefix("au")))
        .build());

    var incidentRegistered = incidentRepository.getById(incident);

    Assertions.assertEquals(incident.getShortDescription(), incidentRegistered.getShortDescription());
}
```
#### Get All Entities
```java
@Test
public void getAllEntities() {
    IncidentRepository incidentRepository = new IncidentRepository();

    serviceNowPage.loginSection().login();
    serviceNowTableViewPage.open(ProjectTables.INCIDENT_TABLE);
    List<Incident> incidents = incidentRepository.getAll();

    Assertions.assertEquals(serviceNowTableViewPage.map().totalRowsInfo().getText(), String.valueOf(incidents.size()));
}
```

### Explanations

***Create Entity***

The test verifies that the application correctly creates a new incident entity in ServiceNow and validates that the created entity can be retrieved with matching properties.
This approach demonstrates the basic entity creation workflow using the IncidentRepository with builder pattern for constructing incident objects with dynamic timestamps.

***Update Entity***

The test verifies that the application correctly updates an existing incident entity and validates that the changes are persisted in ServiceNow.
The test implementation creates an incident, modifies its short description, and confirms the update was successful by retrieving the entity from the API.

***Delete Entity***

The test verifies that the application correctly deletes an incident entity from ServiceNow and validates that the entity no longer exists.
This approach demonstrates proper cleanup operations and entity validation after deletion using the repository's validation methods.

***Get Entity***

The test verifies that the application correctly retrieves a specific incident entity by its identifier and validates that the retrieved data matches the original entity.
The test implementation demonstrates the basic read operation workflow for individual entities using the repository pattern.

***Get All Entities***

The test verifies that the application correctly retrieves all incident entities and validates the count against the ServiceNow table view interface.
This approach combines API data retrieval with UI validation to ensure consistency between different data access methods.

#### Recommendations

Using the BELLATRIX ServiceNow data creation module is recommended for:

- Automated CRUD operations testing
- Data consistency validation across API and UI
- Entity lifecycle management in test scenarios
- Integration testing with ServiceNow instances

## UIB Dashboard Navigation Examples

### Example

#### Open Workspace Dashboard Test

```java
@Test
public void openWorkspaceDashboardTest(){
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.MANAGER_WORKSPACE);

    workspaceManagerPage.assertSidebarButtons();
    workspaceManagerPage.assertTabButtons();
}
```

#### Dashboard Sidebar Option Test
```java
@Test
public void dashboardSidebarOptionTest() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.assertDashboardMainHeading("expected_dashboard_heading");
}
```

#### Dashboard Component Card Test
```java
@Test
public void dashboardComponentCardTest() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.asserts().assertMainHeading("expected_main_heading");
}
```

### Explanations

***Open Workspace Dashboard Test***

The test verifies the successful opening of the Manager Workspace and validates that all essential UI components are properly loaded. This test ensures that both sidebar buttons and tab buttons are present and functional after workspace initialization.

This approach is useful when you need to:
- Verify workspace initialization
- Validate UI component availability
- Ensure proper workspace loading

***Dashboard Sidebar Option Test***

The test demonstrates navigation within the Manager Workspace by clicking on the Work Order Dashboard menu item from the left sidebar. This test validates that the navigation successfully loads the Work Order Dashboard page and displays the correct main heading.

The test implementation verifies:
- Successful sidebar navigation
- Proper page loading after menu selection
- Correct heading display validation

***Dashboard Component Card Test***

The test shows the complete navigation flow from the Manager Workspace to the All Work Orders page through component card interaction. This test verifies that clicking on the "All Work Orders" card component properly navigates to the dedicated All Work Orders page with the correct heading displayed.

Using component card navigation is recommended for:
- Multi-step navigation flows
- Component-based UI interaction
- Comprehensive navigation testing

These tests provide comprehensive coverage of the Manager Workspace navigation patterns, ensuring reliable access to work order management functionality within ServiceNow's UIB interface. The tests validate both the workspace initialization and the multi-step navigation flows that users commonly perform when managing work orders.

## UIB Record View Examples

### Example

#### Open and Verify Record Form

```java
@Test
public void openRecordForm() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.openRecordByColumnValue("column_name", "column_value");

    var entityPage = new EntityPage();
    var actualFormData = entityPage.readTemplateForm();

    var expectedFormData = TemplateModel.builder()
            .number("actual_number")
            .assetLocation("asset_location_choice")
            .asset("asset_id")
            .workType("work_type_id")
            .assignedTo("assigned_user_id")
            .priority("priority_choice")
            .assignmentGroup("assignment_group_id")
            .build();

        entityPage.assertTemplateForm(expectedFormData);
}
```

#### Fill New Record Form
```java

@Test
public void fillRecordForm() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    var entityPage = new EntityPage();

    entityPage.mainContent.getButtonByText("New").click();

    var newFormData = TemplateModel.builder()
            .assetLocation("asset_location_choice")
            .asset("asset")
            .workType("work_type_id")
            .assignedTo("assigned_user_id")
            .priority("priority_choice")
            .assignmentGroup("assignment_group_id")
            .build();

    entityPage.fillTemplateForm(newFormData);
}
```

### Explanations

***Open and Verify Record Form***

This test demonstrates how to navigate to an existing work order record and verify its form data. The test logs into ServiceNow, opens the Manager Workspace, navigates to the Work Order Dashboard, accesses all work orders, opens a specific record by its number, reads the form data, and asserts it against expected values. 

This approach is useful for:
- Validating that existing records contain the correct information
- Ensuring that the UIB record view displays data accurately
- Verifying data integrity across different ServiceNow environments
- Testing record retrieval and display functionality

***Fill New Record Form***

This test shows how to create and populate a new work order record form. The test follows the same navigation pattern to reach the work orders section, then clicks the "New" button to create a fresh record and fills it with predefined data using the TemplateModel builder pattern. 

This method is ideal for:
- Testing record creation workflows
- Ensuring that form fields can be properly populated with new data in the UIB interface
- Validating form validation and data entry processes
- Testing the complete record creation lifecycle from navigation to data input

Using the TemplateModel builder pattern provides:
- Type-safe data construction
- Reusable test data objects
- Clear separation between test logic and test data
- Improved maintainability when form structures change---

## UIB Table View Examples

### Example

#### Table Refresh Test
```java
@Test
public void workSpaceTableViewSettings_refreshIconTest(){
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.map().refreshIcon().click();

    allWorkPage.asserts().assertMainHeading("expected_main_heading");
}
```

#### Edit Columns Popup Open Test
```java
@Test
public void workSpaceTableViewSettings_editColumnsOpenPopupTest() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.map().listActionsIcon().click();
    allWorkPage.map().editColumnsButton().click();

    allWorkPage.asserts().assertPopupHeading();
}
```

#### Edit Columns Close Popup Test
```java
@Test
public void workSpaceTableViewSettings_editColumnsClosePopupTest() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.map().listActionsIcon().click();
    allWorkPage.map().editColumnsButton().click();
    allWorkPage.map().closeDialogueOnEditList().click();

    allWorkPage.asserts().assertMainHeading("expected_main_heading");
}
```

#### Edit Columns Select Test
```java
@Test
public void workSpaceTableViewSettings_editColumnsPopupSelectColumnTest() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.addAnyColumnByListAction("column_name","column_locator" );

    allWorkPage.asserts().assertEditColumnIsChecked("column_locator");
}
```

#### Edit Columns Deselect Test
```java
@Test
public void workSpaceTableViewSettings_editColumnsPopupDeselectColumnTest() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.removeAnyColumnByListAction("column_name","column_locator" );

    allWorkPage.asserts().assertEditColumnNotChecked("column_locator");
}
```

#### Open Record by Column Value Test
```java
@Test
public void openRecordByColumnValue() {
    serviceNowPage.loginSection().login();
    serviceNowPage.openWorkspace(ServiceNowWorkspaces.CUSTOM_WORKSPACE_NAME);

    workspaceGeneralPage.sidebar.getMenuItem(LeftSidebarSection.MenuItems.DASHBOARD).click();

    managerDashboardPage.mainContent.getNowCardButton("card_button_name").click();

    allWorkPage.filterByColumnValue("column_name", "column_value");
    allWorkPage.openRecordFromDataGrid("column_name", "column_value");
}
```

### Explanations

#### Table Refresh Test Explanation
This test verifies that the table refresh functionality works correctly within the ServiceNow workspace manager. This is essential for validating that data updates are properly reflected in the UI.

#### Edit Columns Popup Open Tests Explanation
This test tests the ability to open the column editing popup from the table view. The test accesses the list actions menu and clicks the edit columns button, then verifies that the popup opens correctly by checking for the proper heading. This functionality allows users to customize which columns are visible in their table view.

#### Edit Columns Popup Close Test
This test validates that the column editing popup can be properly closed and returns the user to the main table view. After opening the edit columns popup, the test clicks the close button and verifies that the user is returned to the All Work Orders page with the correct heading displayed.

#### Column Selection Tests Explanation

This test demonstrates how to add columns to the table view using the edit columns functionality. 


This test shows how to remove columns from the table view. This test uses the `removeAnyColumnByListAction` method to deselect the "Active" column and validates that the column checkbox is unchecked, confirming the column will be hidden from the table view.

#### Record Navigation Test Explanation

This test tests the ability to filter and open specific records from the table view. The test filters the table by a specific work order number and then opens that record from the data grid. This functionality is crucial for users who need to quickly locate and access specific records within large datasets.

#### Best Practices

Using these UIB table view testing patterns provides:
- Comprehensive coverage of table interaction capabilities
- Validation of user interface responsiveness
- Verification of data filtering and navigation functionality
- Ensuring proper popup behavior and state management

# Config

Before proceeding with Bellatrix ServiceNow module, verify that the following prerequisites are satisfied.

## Prerequisites

### 1. Client Instance of ServiceNow Available

**Instance Options:**
- **Option 1:** Company live ServiceNow instance
- **Option 2:** Testing instance provided by ServiceNow

#### Personal Developer Instance (Free)
1. Go to [https://developer.servicenow.com/](https://developer.servicenow.com/)
2. Click **"Get Started"** or **"Request Instance"**
3. Create a ServiceNow Developer account (if needed)
4. Request a **Personal Developer Instance (PDI)**
5. You'll receive an email with your instance URL and credentials
6. The instance remains active for 10 days of inactivity before hibernating

#### Create a User with Admin Rights
1. Log in with your Developer account
2. Open the **Navigation menu** (click **All** if not visible)
3. Filter for: `sys_user.list` and press Enter
4. Click **New** and fill out the **New Record** form
5. Open the newly created user record
6. Scroll to the **Role** tab at the bottom
7. Click **Edit**
   * Type `admin` in the **Collection**
   * Add `admin` to the **Role List** and click **Save**

#### Optional: Create one more user for Impersonate tests

---

### 2. Update the Config File

**Path:** `src/main/resources/testFrameworkSettings.dev.json`

#### Modify the following in `serviceNowProjectSettings`:

- **Instance:**
    ```jsonlines
  {
    "instance": "instance"
  }
    ```

- **Platform Release:**
    ```jsonlines
    "platformRelease": "release"
    ```

- **Polaris Enabled Flag:**
    ```jsonlines
    "isPolarisEnabled": "true"
    ```

- **Credentials Options:**

  **Option 4.1: Hardcoded Credentials**
    ```jjsonlines
    "userName": "user",
    "password": "pass"
    ```

  **Option 4.2: Use Environment Variables**
    ```jsonlines
    "userName": "{env_servicenow-username-instance}",
    "password": "{env_servicenow-password-instance}"
    ```

  Create the following environment variables:
    - `servicenow-username-instance`
    - `servicenow-password-instance`

---
### 3. Set Environment

In the file `src/main/resources/application.properties set:
```properties
environment=dev
```