==============
Release Notes
==============

v1.24.4
~~~~~~~

FIXES
^^^^^
-  Defect 15183   Charts fail to email in integrated instances. The following method needs to be added in the IzendaConfig.cs class

.. code-block:: csharp

        public static void RegisterLoginLogic()
        {
            UserIntegrationConfig.GetAccessToken = (args) =>
            {
                return IzendaBoundary.IzendaTokenAuthorization.GetToken(new Models.UserInfo()
                {
                    UserName = args.UserName,
                    TenantUniqueName = args.TenantId
                });
            }
        }
        
-  Defect 15245   Error Thrown in PostgreSQL when attempting to create Izenda config database
-  Defect 15261   Data from Query is incorrect when using Left join

v1.24.3
~~~~~~~

FIXES
^^^^^
-  Defect 15130   Multiple joins in model between two tables not creating and relationship between both relationships
-  Defect 15140   Dashboard performance improvements
-  Defect 15142   Updated assembly references in the Izenda.BI.Framework

v1.24.2
~~~~~~~

FIXES
^^^^^

-  Defect 15061    After making a field not visible in the data model the field is still shown in existing reports
-  Defect 15124    Hidden Filters are showing as actual filters in subreport when filter inheritance is turned on
-  Defect 15126    Filter aliases not shown under the report filter descriptions
-  Defect 15123    System is adding joins from the tenant model to report after copy
-  Defect 15074    User can still access and design a report they are given No Access to report if it resides in a Visible Category for their role, and there is a higher scope access set (ie Everyone - Full Access)
-  Defect 15177    Hidden Filter fails if the user enters join alias for item in report designer. Documentation Updated (See IAdhocExtension, Hidden report filters)

v1.24.1
~~~~~~~

FIXES
^^^^^

-  Defect 15001 Report Render is taking a long time in the Report Viewer
-  Defect 15023 AVG function on field is truncating all decimals
-  Defect 15032 API POST request to trigger export with filter values
   not working properly. This resolves the initial issue but please note
   all values are case sensitive and GUID values for filter key must be
   lower case. Example request body below for route /api/export/pdf::

    {
     "reportID":"ff1b105c-fffc-407e-98c4-2fc17c3d79b1",
     "filters":[{
      "key" : "0d01fe9f-10ff-4b42-a8f3-b7e4f8983817",
      "value":"800"
     },
     {
      "key":"dea8ee0e-08bf-4a8f-9158-240837b26e2f",
      "value":"10250;#10248"
     }]}
     

-  Defect 15046 Updated insert process for new datasources. This is now
   batched into multiple insert statements to avoid timeout errors. A
   new setting has been added to IzendaSystemSetting table with this
   release to allow control over the number of items in each batch.
   Setting value is InsertBatchSize and default is 10000. Added setting
   to configure Command Timeout in IzendaSystemSetting table, this
   timeout is for the insert and update statements to the Configuration
   Database.
-  Defect 15024 Custom Functions defined JSON are not working, they
   require use of [] around function name which are not added in the
   expression builder. These should be auto added when selected.

v1.24.0
~~~~~~~

FEATURES
^^^^^^^^

-  Added the ability for subreports to inherit filters and their values
   from parent reports

   -  The datasources for the parent/subreport must be exactly the same
   -  The inherit filter checkbox must be checked when setting up
      subreports
   -  These filters will not have to be present on the subreport ahead
      of time

-  Added ability to create Custom In Time Period values for filters
-  Updated support for mapping fields to subreports when values are
   datetime and numeric fields
-  Extended ability for customer to add custom formats for field
   properties
-  Added setting at tenant level to add logo by tenant for header image.
   Setting is located in System Configuration > Report
-  Moved Filter Operator just under Source in Filter Property Panel for
   ease of use and visibility in the property panel
-  Added Default Filter Operators for each Datatype

   -  Date: Equivalence Equals Calendar
   -  Text: Equivalence Manual Entry
   -  Number: Equivalence Manual Entry
   -  Money: Equivalence Manual Entry
   -  Subtotal Auto Add name for subtotal so user is not required to configure a name

-  Change Filter Descriptions default should be set to off
-  Removed extra white space on back of Dashboard Tiles
-  Enhanced search feature for Reports for dashboard and subreport so
   more report results are shown on independent screen
-  Add button on Repoirt List to Navigate to Quick Edit Mode
-  In Report Viewer Hide the View mode button until the user is in quick
   edit mode
-  When navigating to edit a report in report designer user is brought
   to Fields tab not Datasource tab
-  In Role Permissions added option to select all items in each section
-  In Tenant Permissions added option to select all items in each
   section
-  Change Update Results Behavior in report designer, user is not
   required to update results for saving and when navigating to fields
   tab with proper configuration
-  Data Setup > Advanced Settings > Others: Added settings to define Common Filters for Dashboard

   -  Same field of the same data object from the same Database Schema
   -  Same field name regardless of the Database Schema or connection string
   -  Same alias name in Data Model regardless of Database Schema or connection
      string

-  Added support for Export API to accept filter and filter values
-  Remove Copy icon from the backside of report part tile and dashboard
   tile to reduce accidental copy of report part when attempting to flip
   tile. It is now only available on the front side.
-  For Charts and Gauges the items per row and pagination items can now
   be used independently
-  Changed the default size for the filter panel in all areas to default
   2 rows high instead of 3
-  Reports broken from data model changes can now be edited to remove
   fields no longer available in report designer
-  Access limits for sharing will now maintain the parent node so any
   new users to a role will be added to that sharing group by default
   when entire role is selected
-  Increased width of Tenant dropdown in the setting level to ease
   viewing the tenant being selected

FIXES
^^^^^

-  Defect 13990 Label height is inconsistent for filter control boxes in
   the report viewer based in filter control type
-  Defect 14006 When using $/100 format in the sub/grand total the
   preview of the sub/grand total is not displaying properly even when
   actual total is formatted
-  Defect 14020 System missing validated indicator on Connection String
   level when user does not create mapping for these connection strings
-  Defect 14024 Grand Total value for a separator is calculated
   differently in the preview section compared to the preview result in
   the popup for the Grand Total Field
-  Defect 14029 Roles with no access to Functions (not moved to visible
   for this role) can use them in the report designer field function
   dropdown
-  Defect 14031 If report or dashboard was saved with sharing access for
   a role or user will not save change to share with everyone
-  Defect 14035 Missing background color for fields added into Visual
   tab of form designer
-  Defect 14042 Some date time formats are not displaying correctly for
   Grand totals
-  Defect 14124 Subscribe option should not be shown to users with Save
   As access to dashboard, as user has permissions to schedule
-  Defect 14125 View Mode button in the report viewer is showing
   progress bar when clicked and still disabled
-  Defect 14176 Settings Level should be disabled when user is in my
   profile area of application
-  Defect 14177 Source and Destination trees are hidden after clicking
   Validate in Data Advanced Options screen
-  Defect 14186 When using alternating row colors, PDF export is
   different than what is on the screen
-  Defect 14203 Need space between radio button and labels 'Linear' /
   'Value'
-  Defect 14207 Intervals are not presented when user switches back old
   X-axis Type
-  Defect 13501 Currently the system is missing Help indicator in
   following places in Copy Management Mapping areas (In All Mappings,
   in Merge Duplicated Mappings, and in Object Label of To area)
-  Defect 13504 Mapping area in Main page: System variable TenantName
   does not work
-  Defect 13505 The system does not have the checkbox "Merge Duplicate
   Mappings" in Advanced Copy Options page of Copy Management
-  Defect 13523 In Role Setup Tenant Setup anchor link still displays in
   Permissions page for setting level = tenant
-  Defect 13599 In Dashboard list the subcategory does not remain
   expanded when user opens report from list
-  Defect 13655 "There are no records returned" error raised when
   configuring subtotal for a field of a table having data
-  Defect 13775 Link and icons should be removed from report if
   subreport is not copied with report in destination.
-  Defect 13859 Suggested data type is not changed when user changes the
   field in the calculated field expression text box
-  Defect 13868 Fields of newly added stored procedures are not selected
   by default while the Advanced Settings> Set Additive Field Auto
   Visible/Filterable are checked
-  Defect 13876 Subcategory is not displaying when added again after
   deleting
-  Defect 13908 Tool tip error message for Query Limit, Field Limit and
   Pivot Column Limit still show reference to Data Source Limit when set
   to an unsupported number like -1
-  Defect 14216 Missing horizontal scrollbar on popup of subreport when
   needed
-  Defect 14224 X-Axis updates incorrectly when user changes value of
   Interval in XY-Plane settings
-  Defect 14233 After building a report with one report part and saving,
   if deleted without save and moving to the viewer will cause error
-  Defect 14234 General error message shows when copying a
   report/dashboard with deleted report part.
-  Defect 14306 Null value on chart X axis takes name of total label
-  Defect 14761 Using Oracle error message is shown when user selects
   Function = Group Days Old for Date field in Report Container
-  Defect 14774 General error message shows when changing a UserID
-  Defect 14802 Sub report data fails to load when using popup and form
-  Defect 14807 Close button does not work when user clicks on Report
   Name in Report List then clicks on Open button to open the Report
   Viewer
-  Defect 14808 The "Link/this icon was configured to show in other
   settings (Sub-report/Custom URL/ Embedded Javascript). Please select
   the other ones" warning is displayed when user sets both Custom URL
   and Embedded Javascript
-  Defect 14812 Page continues to load if ENTER is clicked to close the
   generate password successful popup.
-  Defect 14815 Sharing record temporarily dismisses when saving then
   updating result.
-  Defect 14867 The "There is no relationship(s) among the following
   data objects. Please manually unselect them or creat relationship for
   them...." message is displayed when user clicks Data Source icon from
   Field tab page
-  Defect 14890 Responsive - Change mobile mode from 1280 to 1024
-  Defect 14933 Unable to go to fields page when selecting another data
   objects from Datasource page
-  Defect 14938 Function for applying Format on DateTime Field does not
   works with Group or without Group function
-  Defect 14940 Unable to export pivot grid
-  Defect 14961 System shows error msg when user open Sub Report while
   Master = Data of Week, Sub Report = Group Date & Time
-  Defect 14963 System shows "No record found" when user opens Sub
   Report while Master = M/d/yy or Week Number, Sub Report = Date of
   Week
-  Defect 14967 System returns incorrect "Day of Week" on Sub Report
   while Master and Sub Report is build from the same table in the same
   Connection String
-  Defect 14978 System shows error msg when user updates Report
   Properties/Field Properties and then changes the report from Front
   side to Back side
-  Defect 14242 Page continues to load when deleting a CF then turning
   to front side of Form
-  Defect 14277 In Oracle cannot add SP to Visible Data Sources
-  Defect 14295 Clicking report name expands report info and should not,
   should take the user to the report viewer directly without this step
-  Defect 14894 Format for page numbers in header and footer do not
   change
-  Defect 14659 PDF Exports are scaling smaller even when printed
   columns per page on.
-  Defect 14672 When export types are disabled at the tenant level
   giving user full report and dashboard access is still showing these
   options
-  Defect 14674 Filter Operator In Time Period showing "Undefined" on
   Dashboard when not a common filter
-  Defect 14679 Gauge pagination is showing when turned off after any
   configuration change to the gauge. It can be turned on and off again
   and will be removed but it must be done after each change.
-  Defect 14228 ReactJS loads twice when integrating with another
   ReactJS app
-  Defect 13925 Out of memory errors occurring when validating many
   tenants using copy function for data model or reports.
-  Defect 14215 Pivot grids do not render columns where all values are 0

v1.23.2
~~~~~~~

FIXES
^^^^^

-  Defect 14771 Cross-Database Issues with Izenda configuration Database
-  Defect 14724 When grouping a date field and changing the format some
   dates are appearing out of order
-  Defect 14727 Setting up the custom tree filter when parent node is
   checked all child elements should be selected
-  Defect 14737 In Time period filter causing errors and report & query
   will not export
-  Defect 14751 MySQL errors logged in accessing report & dashboard
   categories
-  Defect 14794 Tree Filter is adding each list multiple times in
   dashboard when common filter
-  Defect 14698 Error is shown when attempting to use a between date
   filter for any date values in Oracle

v1.23.1
~~~~~~~

FIXES
^^^^^

-  Defect 14690 Simple style gauge is not exporting from standalone
   environments.
-  Defect 14682 Oracle 12c giving errors on inconsistent datatypes of
   CLOB.
-  Defect 14671 Filter aliases not being displayed in the report viewer.
-  Defect 14680 Filter query fails in some cases where certain special
   characters are used in the field name. Fields with aliases in the
   data model may fail in expressions when used with an expression and
   field in the same report.
-  Defect 14685 Authorization error preventing exporting in integrated
   environments.

v1.23.0 (GA)
~~~~~~~~~~~~

FEATURES
^^^^^^^^

-  The Copy Console Utility is now available. This utility can copy
   reports, dashboards, etc to separate API instances

FIXES
^^^^^

-  Defect 14297 Tenants and Roles with access to all report part types
   could only see grids in integrated modes.
-  Defect 14296 A report's QuerySourceId as set to 0 after being copied
   via the copy management console application
-  Defect 14240 Javascript API was unable to set a new locale in a
   standalone deployment
-  Defect 14238 Stored Procedure Lookup Key/Value Inputs did not
   Properly Convert Int Input to Text
-  Defect 14229 Using Calculated Fields as a Filter would return no data
-  Defect 14214 Pivot Grids would not allow for the same field to be
   used as a row and value
-  Defect 14210 Fields with an image data type would not render
-  Defect 14209 Drilldown grids would expand shortly after closing when
   subtotals were applied; subtotals would lose their aggregate metric
   when collapsed
-  Defect 14208 Platform crashes when pulling back reports with large
   record sets (10K/30K)
-  Defect 14109 PostgreSQL input arguments do not carry through to the
   Function area of the data model
-  Defect 14319 Revised UI Grammatical and Spelling Errors
-  Defect 14317 Calculated field queries would identify the wrong field
   to be used for grouping
-  Defect 14318 Users with Full Report and Dashboard Access could not
   save reports that contained report part types they weren't explicity
   granted access to.

v0.22.16
~~~~~~~~

FEATURES
^^^^^^^^

-  Dropdown selections now maintain your current position in the
   dropdown list when selecting multiple values
-  Charting option added for XY Plane to use Linear (interpolates data
   for date and number field types) or Value (only displays values found
   in data from source)
-  Sorting ability now enabled on Grid headers for Report Viewer
-  When adding a Function to a field level item in the Grid this will
   automatically group all other fields in the grid, if the desired use
   is an aggregate function it must be used in a calculated fields
-  Changed the name of "Preview" button in the Report Designer to
   "Report Viewer" as this button moves the user to the Report Viewer to
   view this report

FIXES
^^^^^

-  Defect 13149 When the user hovers the mouse over an item in the
   dropdown, the corresponding item must be highlighted to catch the
   user's interest.
-  Defect 13177 Internet Explorer will not load the schema page in data
   model and has slower behavior in flipping report parts
-  Defect 13262 Page fails to load if user clicks back button in browser
   in login page
-  Defect 13305 Form fails to respond after adding calculated field as
   filter and updating results
-  Defect 13540 In sub/grand totals system displays required msg: "Label
   is required" when user selects function "None" for any field
-  Defect 13621 Using some combox controls in IE, system shows X icon in
   the wrong location
-  Defect 13749 System shows concurrency updated msg when user clicks on
   Validate/Run Copy button although systems has just displayed this msg
   when user click on the work space to open
-  Defect 13792 When user selects ALL items from scheduling page of
   System Configuration page and selects delete, only the first page of
   items shown is actually removed
-  Defect 13873 Invalid reports should not be accessible after retrieved
   from historic version
-  Defect 13897 Filter limit setting of 0 shows all results for data
   driven style filters
-  Defect 13910 Concatenating text using the "+" is not working in all
   cases
-  Defect 13911 When setting query limit to 0 and clicking save system
   reverts to default limit of 100000
-  Defect 13913 Spelling error in Report Settings, No. of archive
   version to keeps, should be No. of archive versions to keep
-  Defect 13925 Out of memory errors occurring when validating many
   tenants using copy function for data model or reports
-  Defect 13966 System shows "No record found" when user search report
   name with special characters
-  Defect 13968 In Chart XY-plane settings starting point is not
   properly cleared if user changes function or format of current field
   used
-  Defect 13972 Chart renders incorrectly if user sets x axis starting
   point as a decimal
-  Defect 14021 Copied dashboard with multiple share with values
   (role/everyone) fails to copy share with values to destination
-  Defect 14044 Invalid date is returned when using MAX function while a
   specific date time format is being used for the original field
-  Defect 14063 User cannot select "None" option for editing on Grand
   Total/SubTotal in Report Designer
-  Defect 14083 System does not reset data on the Filter Properties when
   user un-selects stored procedure on data source tab
-  Defect 14089 Text box is still visible behind date/time picker in
   Threshold settings for charts when selecting date/time field
-  Defect 14091 In Chart XY-Values the starting point field is still
   visbile if the field is text type
-  Defect 14097 User is not able to save "Uncategorized" to "Categories
   allowed for saving dashboards"
-  Defect 14099 When changing Threshold settings, unit labels and
   supplementary KPI for gauge they are not saving
-  Defect 14102 Percentage of Group format Does Not Respect Separators
   in grid reports
-  Defect 14122 Design button in report viewer does not open the report
   in the designer when report is shared to user with 'Full Access' or
   'Save as'
-  Defect 14123 After moving fields between different containers in the
   report designer for a pivot grid the header is incorrectly formatted
-  Defect 14126 Stored Procedure shows error after input parameter set
   in the data model
-  Defect 14128 When adding alternating row color for grid report part
   and exporting to PDF the alternating items are columns not rows
-  Defect 14130 Changing connection to an existing Izenda Configuration
   Database using PostgresSQL instance fails with constraint error in
   system
-  Defect 14139 When user creates report in simple mode and exports the
   report, export shows "no record found"
-  Defect 14171 Charts are showing encoded values for characters and
   should simply display as text
-  Defect 14172 Datetime fields with milliseconds causing error when
   adding different date formats
-  Defect 14175 User defined function for Boolean in MySQL not working
   and page continues to load
-  Defect 14182 When changing the reporting connection string the system
   should simply reconnect to database provided, not connected and
   generate new model
-  Defect 14184 Gauges are not rendering properly when showing undefined
   value
-  Defect 14187 Grammer Correction in message when changing from Single
   Tenant to Multi-Tenant Mode
-  Defect 14188 Stored Procedure returns value not key when lookup is
   defined in the datamodel
-  Defect 14189 Subreports using link new window is returning a blank
   page in report viewer
-  Defect 14202 Receiving "No record found" when using gauge as
   subreport in popup style
-  Defect 14206 Embedded subreport is not loading when using grid in
   main report
-  Defect 14220 Search feature in Scheduling page is not working
   properly, showing no results when schedules are present
-  Defect 14222 For Charts when chaning the XY-Plane of Bubble/Scatter
   types the Value (and Intervals of Value) cannot be applied for X-axis

v0.22.15
~~~~~~~~

FEATURES
^^^^^^^^

-  Stored Procedure input parameters can be used as regular filters with
   indexes and can receive values when passed via URL
-  Cascading allowed for stored procedure input parameters when
   implementing in filter overrides
-  Change to category permissions, if report is shared with user as Full
   Access this user now has ability to see and save in this category. If
   shared with Save As and below the category is visible only, not
   saveable
-  Save As permission now allows access to the report designer but no
   access to save, only save as in the designer

FIXES
^^^^^

-  Defect 8313 System is not properly validating some text fields and
   user receives error when attempting to save report
-  Defect 11022 Some displays are not responding properly in responsive
   mode
-  Defect 13272 Report Parts are broken into two lines when selecting
   Landscape print option
-  Defect 13515 Print Preview section is not functional after selecting
   Margin = Normal/Custom
-  Defect 13536 Data is not cleared in sub/grand total if user clicks
   cancel button on popup
-  Defect 13559 Some report parts when randomly placed in the designer
   are not displaying the same way after printed
-  Defect 13637 System shows field name is not unique when using name
   for subtotal field
-  Defect 13639 Dirty form validation is not consistently performed
   across application
-  Defect 13643 Redundant master ReportId parameter in Subreport URL if
   master report is not saved yet
-  Defect 13645 Incorrect error message shown when user no longer has
   access to report part in dashboard
-  Defect 13661 When user creates a report and adds subreport and
   selects to inherit filters, the filters are only passed as saved not
   when changed in the viewer.
-  Defect 13662 Field mappings are sent to subreport using the entire
   database name and must be changed to use a field mapping attribute to
   shorted the URL and not display the database name
-  Defect 13693 Database type is not updated in Middle Panel immediately
   after save in the middle panel of the connection string area
-  Defect 13735 System shows incorrect message when admin user clicks on
   copied Report in Report List but all data sources in Report are in
   Available Tree - Connection String page
-  Defect 13796 If a user provides an incorrect 'To Object' in a global
   mapping, the local mapping shows the correct value
-  Defect 13799 In Report settings system is not requiring time fields
   for scheduled removal of version history
-  Defect 13810 In Model or Quick Edit mode text in field containers
   should change to read only Add a field with the hyperlink as there is
   no way to drag fields
-  Defect 13812 Side totals on Pivots do not respect the function of the
   field and are always sum
-  Defect 13816 Add Hover text showing database name to Source
   Connection string for copy management to distinguish between
   multiples of the same type
-  Defect 13860 Loading indicator for report viewer stops prior to
   report parts actually loading
-  Defect 13895 System shows duplicate error message when saving report
   as "Example Report Name" even if no duplicate report exists
-  Defect 13914 Proper message is not displayed in Last Successful Run
   when the report is no longer valid
-  Defect 13920 After changing the name of a calculated field used in a
   form the system shows error that calculated field is not found
-  Defect 13924 All option is missing for stored procedure input
   parameter filter dropdowns
-  Defect 13926 Token Timeout Is Not Configurable in stand alone
   application mode
-  Defect 13929 When using an equals popup filter, clicking the x at the
   top-right of the popup does not close it or back out of the filter
   selection
-  Defect 13941 When adding a function to the data model when using
   postgresql the designer is showing errors and user cannot use the
   calculated field pop up
-  Defect 13969 After copying a dsahboard contianing a link to a
   subreport without the subreport contained the system still shows the
   link and report is not available
-  Defect 13973 After Copy Dashboard is run system shows error on report
   when copy report successful. Relationship gets blank data on Foreign
   Data Object
-  Defect 13974 On the XY-Plane settings for chart when using DateTime
   and Format = Day of Week, the Starting Point combobox is too narrow
-  Defect 14000 After changing users datetime setting in profile, some
   buttons of reports are disappeared when viewing in Report List
-  Defect 14005 Error message is displayed "The application has
   encountered..." after copying a report contining a stored procedure
   and filtered input parameter
-  Defect 14007 In Sub/Grand totals the user should be able to select
   the "..." format again once another format has been selected.
-  Defect 14016 Preview button is not enabled when viewing an existing
   subtotal / grand total having type = expression
-  Defect 14017 The list of available options of the Functions dropdown
   is missing when changing from a specific function to "None"
-  Defect 14022 When user is not logged in and navigates to report
   designer url the page is displayed as blank and user is not routed to
   login screen
-  Defect 14026 Unable to apply field level function from function
   dropdown
-  Defect 14037 After Inputing a number into Intervals (X-axis) of chart
   XY-Plane settings for Date & Time field, the page crashes
-  Defect 14040 When designing a chart user cannot clear Intervals
   textbox of XY-plane settings
-  Defect 14043 When designing a chart If user changes Y-axis Intervals
   = 0, page crashes
-  Defect 14045 System does not show error message for user's who cannot
   save report into uncategorized attempts to save in this category
-  Defect 14046 Sort icons for values of Pivot grid and Drilldown grid
   report parts should be disabled
-  Defect 14048 Unable to repeat the subtotal in a table of Form if
   there is an already repeated row.
-  Defect 14052 Cannot enter decimal into Starting Point when the field
   returns numeric/money
-  Defect 14053 Relationship of Destination copied Report has blank
   Foreign Data Object when report in Source has Data Source alias
-  Defect 14055 There are tow Close buttons showing in the Report Viewer
-  Defect 14057 After changes to lanier style for dates on X axis last
   metric is always missing on the axis
-  Defect 14060 After correcting issue found in Format tab of report
   designer user is not allowed to navigate to other tabs
-  Defect 14064 General error message displayed when using filter field
   comparison or blank/not blank when using fusion connection
-  Defect 14073 In Chart X/Y axis intervals do not work correctly when
   selecting Format = Short Hour
-  Defect 14075 Page freezes when adding another report after copying a
   report part having embedded subreport.
-  Defect 14076 System does not show information message to provide
   filter value for stored procedure where input parameter has an alias
   in the data model
-  Defect 14077 User is able to save report with Filter Logic which
   contains a stored procedure
-  Defect 14078 Index of stored procedure parameter in Oracle sources
   begins with 2 instaed of 1
-  Defect 14079 Index of Stored Procedures Parameter is always reset to
   the last index if user adds more filters so the filter logic will not
   work properly
-  Defect 14080 System will not allow user to edit reports where the
   stored procedure has been edited
-  Defect 14081 Double vertical scroll bars and horizontal scroll bar
   are present on Dashboard where dashboard should only ever have one
   vertical scroll bar
-  Defect 14088 In charts user cannot apply the Y-axis threshold for
   metric = All
-  Defect 14093 Cannot open XY-Plane Settings popup on chart property
   panel
-  Defect 14094 Could not redirect to sub-report after click the link to
   the sub-report in system level of applciation
-  Defect 14095 Pop up Subreport is not properly receiving filters for
   subreports
-  Defect 14096 Incorrect property panel when clicking header of chart
   report part type
-  Defect 14098 In Copy Management after successful copy is completed
   and user un-selects some items the copy successful notification is
   displayed again
-  Defect 14103 In some cases when changing Tenant setting level from
   within a report the correct report list is not loading
-  Defect 14104 Using p1value in the URL is not changing the filter
   value in the actual filter
-  Defect 14105 When user with advanced data source mode shares report
   as full access with simple data source mode user when editing report
   the design screen is blank
-  Defect 14112 Removed izenda\_config.js and index.html from embedded
   UI package as it is not needed

v0.22.14
~~~~~~~~

BREAKING CHANGES
^^^^^^^^^^^^^^^^

File name change from izenda-vendors.js to izenda\_vendors.js

Added new js files

-  izenda\_common.js
-  izenda\_locales.js

Please follow the following order when linking the js files for embedded
mode:

-  izenda\_common.js
-  izenda\_locales.js
-  izenda\_vendors.js
-  izenda\_ui.js

FEATURES
^^^^^^^^

-  Feature Data Model Copy Dashboard - allows copying of dashboard
   definitions and associated reports from System to Tenant or Tenant to
   Tenant
-  Formatter for Sub and Grand Totals - allows user to set the format for
   the values on sub and grand totals
-  Cancel Button - Cancel button added to loading bar which allows user
   to cancel long running processes
-  Email Notification - When sending a report as email from the report
   viewer or report list a conformation will be displayed to show the user
   the email was sent
-  Exporting - Report and dashboards can now be exported prior to saving

FIXES
^^^^^

-  Defect 13739 Collapsed and Expanded icon are the same image when in
   copy management copy report list tree
-  Defect 13932 Adding a date to x-axis of chart and selecting month
   name or month shows the same month for each value
-  Defect 13778 After copy report some relationships copied are not in
   the same order in the destination
-  Defect 13750 After copying a report with an embedded subreport the
   embedded subreport is not displayed
-  Defect 13466 Changing the data source and field alias's in the model
   after reports are created is causing some reports and joins to error
-  Defect 13972 Chart renders incorrectly if user sets x axis starting
   point as a decimal
-  Defect 13797 Clicking cancel button prior to making any changes in
   the advanced settings for data model will cause error to display if
   changes are made after and user attempts to save
-  Defect 13696 Clicking save twice in saved workspace of copy
   management will show successful save message when it should show no
   changes found
-  Defect 13601 Copied Threshold settings change when changing the
   threshold it was copied from
-  Defect 13751 Copy Management in Data Model Copy redundant data
   sources (which are not selected on Source Tree) are displayed on Data
   Model Comparison Tree
-  Defect 13763 DateTime Picker in the report viewer is rendered in the
   filter control and must be scrolled when searching for date
-  Defect 13263 Embedded reports are not displayed in the print preview
   and physical print
-  Defect 13673 Error occurs when using calculated field with special
   characters in the name when calling this field into another
   calculated field
-  Defect 13271 Export Query Execution fails for specific conditions in
   report designer
-  Defect 13809 Filter order from report is not respected on dashboard
   common filters
-  Defect 13783 Grids are moving and growing in copy management and
   calculated fields when user is above and below 100% zoom in browser
-  Defect 13713 In Edge browser the concurrency message is not properly
   displayed in Copy Management
-  Defect 13248 In integrated mode chart type previews on the property
   panel are not properly displayed on hover
-  Defect 13768 Incorrect Grammar used in notification in copy
   management when review of workspace is needed
-  Defect 13815 Moving a dashboard tile to the bottom of the screen
   requires the user to move it one tile at a time this should be
   infinite scroll
-  Defect 13047 Print Dashboard doesn't scale to printing paper size
-  Defect 13884 Some dropdowns are expanding down even at the bottom of
   the page when they should expand up
-  Defect 13456 Some map legend format settings work incorrectly (font
   size and background colors)
-  Defect 13982 System is not updating the Owner, Created Date and Last
   Edited date of copied reports after copy
-  Defect 13714 System should display an adequate error message for an
   invalid expression in a calculated field and there should be no
   preview result for an invalid case.
-  Defect 13748 System shows blank Report List Tree when user add "Item
   to Copy" = Report to an existing Workspace with Data Model only
-  Defect 13230 System shows error message when drilling down to a
   subreport with no values matching top level report
-  Defect 13927 Unminify locale data files & support localization JS API
-  Defect 13805 Version of report is increasing when navigating from
   viewer to designer with no changes
-  Defect 13737 When metric values contain negative number gauge will
   not render
-  Defect 13938 When user adds a new stored procedure to the data model
   the fields are not set to visible and filterable by default
-  Defect 13806 When user changes permissions for a role without access
   to users tab the users are removed for that role
-  Defect 13813 When user changes subtotal or grand total to NONE this
   should remove the subtotal and remove the flag
-  Defect 14027 Move and copy options for Report Version History do not
   work
-  Defect 9767 Notification should be displayed to user when emailing a
   report that it was sent
-  Defect 11424 Field mapping in subreport set up should not allow
   duplicates
-  Defect 11747 Enhanced chart axis when using date time values to allow
   for proper formatting of Thresholds and Starting Points
-  Defect 12651 When creating a dashboard with only one tile all filters
   should be considered common
-  Defect 12800 Configure Password Options should only be enabled for
   users with Edit functionality
-  Defect 13273 Error message not properly displayed when Foreign Data
   Data Object in releationship is null
-  Defect 13275 In display resolution 1920x1080 the Copy Management page
   is not properly displayed
-  Defect 13496 When changing alias of Data Source Alias some calculated
   fields are broken when used in the report
-  Defect 13609 When changing between User Defined Functions on a
   specific field the system will display an error
-  Defect 13640 User received error when creating calculated field of
   static string value for any table with no values
-  Defect 13653 When user changes Alias of parameter, system does not
   update the new alias on Filter Fields, and data on Preview section is
   blank
-  Defect 13681 Calculated Fields are not properly removed when used as
   filters after deleting
-  Defect 13685 Mapping is not shown after user checks and unchecks
   DataModel or Reports
-  Defect 13690 Search function not working for Reports in Copy
   Management Report Tree
-  Defect 13691 System shows incorrect Status of Copy Management when
   user selects 2 Destination, and 1 Destination has no existing
   Connection String which is selected in Mapping
-  Defect 13707 Embedded subreport is using report name instead of
   report ID which causes issues in copying the report
-  Defect 13712 Large blank space in the copy management page in the
   report copy settings area when selecting existing workspace
-  Defect 13950 Charts with multiple metrics on Y axis are frozen when
   clicking on the XY-Pane settings options in the designer's property
   panel
-  Defect 13951 After deleting a report part and adding a new one with
   sub/grand total report loads searching for deleted item
-  Defect 13955 After adding a new Connection String in Destination,
   system does not shows the new item in "To Database Name" in mapping
-  Defect 13957 When using only one report part the name of common
   filter is missing
-  Defect 13958 When using a date on X axis and user inputs number into
   Intervals in XY-Plane settings, the page crashes
-  Defect 13959 Charts are rendering incorrectly when user clears
   existing Date/Time value in Starting Point of XY-Plane settings
-  Defect 13960 Progress bar in export pop up is displayed at incorrect
   size
-  Defect 13970 In Copy Management if user adds blank Global Mapping the
   system will still copy, this blank mapping should cause error message
-  Defect 13977 Input field of search loses focus after user enters
   first character in Copy Management
-  Defect 13978 Chart will not render when using date/time field on
   x-axis and using formats (Short Hour / Long Hour/Short Date & Long
   Hour/Long Date & Long Hour)
-  Defect 13991 When common filter is removed from report the dashboard
   containing this report part is not properly updated and continues to
   load without displaying report part
-  Defect 13997 System shows Uncategorized multiple times in error when
   altering role permissions for category access
-  Defect 13998 System is printing only one page for dashboard
   containing multiple report parts
-  Defect 13999 Export Query Execution duplicates queries if report
   contains multiple report parts
-  Defect 14001 User can see reports in category which they do not have
   proper permissions to see in search result of Report Part Selection
   pop up for dashboard
-  Defect 14004 In preview of sub/grand total if format is applied it is
   not displaying in the preview of the popup window
-  Defect 14013 Field name alias changes are not updated on field in
   form report part containers
-  Defect 14030 In Copy management user cannot navigate to another page
   or middle panel after deleting destination of copy function
-  Defect 14034 Exported form does not show the embedded subreport in
   export
-  Defect 14047 Close button missing from Dashboard Toolbar
-  Defect 13853 Popup style subreport showing no results in report after
   saving
-  Defect 13857 StartDate field is blank in Schedule/Subscribe popup
   window and should default to current date
-  Defect 13863 Export failed when user create report has maximum width
   in Report eader
-  Defect 13864 Datetime format of header and footer does not work.
-  Defect 13865 Preview Data of Calculated Field is displayed
   incorrectly when Field in data source has value = NULL
-  Defect 13867 Form report part will not display data containing all
   "0"
-  Defect 13870 When export to disk path points to folder which does not
   exist system should attempt to create
-  Defect 13871 Header should be removed from popup style subreport
-  Defect 13874 Query Limit does not work correctly when selecting a
   value of Preview Records
-  Defect 13885 Performance settings appears on tenant users' settings
   page
-  Defect 13890 Query Limit does not work when exporting a report
-  Defect 13891 Edited date in Report history matches Created/Edited
   date in report list
-  Defect 13892 Field limit is not respected on Add field button on Form
   properties panel.
-  Defect 13893 Dashboard does not load and freezes after all reports
   are deleted that dashboard is created from
-  Defect 13896 System always shows "Uncategorized" in drop down list of
   Category on Save/Save As pop up but user is not able to select this
   item
-  Defect 13900 Deleted roles are still displayed in Access Limit
   dropdown
-  Defect 13901 After saving a report with calculated field as filter,
   filter is not displayed in the report viewer
-  Defect 13904 Resize report Body to make design configuration easier
   for end user
-  Defect 13916 When creating a form user can format a field selected
   from field properties and it changes another field name.
-  Defect 13917 Proper error message is not displayed when user reaches
   the filed limit set in settings and tries to add another field
-  Defect 13939 When sharing a report created with advanced data sources
   mode with a user simple data sources the system shows errors
-  Defect 13883 Extended length of all system dropdowns for easier
   selection
-  Defect 13834 Filter panel height in report viewer and dashboard
   should default to show entire filter box
-  Defect 13878 Sparkline Chart contains too much padding and shows as
   Icon on Small Resolution
-  Defect 13879 Changing title or description on copied report part
   changes it on the original report part

v0.22.13
~~~~~~~~

FEATURES
^^^^^^^^

-  New Settings Added:

   -  System Configuration > Report settings added to allow removal of
      archive versions on scheduled intervals
   -  Data Setup > Advanced Settings > Field Limit Allow system
      administrator to set the maximum number of fields allowed in one report
      part
   -  Data Setup > Advanced Settings > Query Limit Allows system
      administrator to set the maximum number of values returned from query
   -  Data Setup > Advanced Settings > Pivot Column Limit Allows system
      administer to set the maximum number of pivot columns returned for pivot
      grid styles
   -  Data Setup > Advanced Settings > Filter Limit Allows system
      administrator to set the maximum number of values for all filter input
      and lookups
-  Date Format Culture - Allows users to set the preferred date
   formats available in the field options. When set by user with one format
   setting, users with different format settings can change but still see
   the dates in reports as their preferred date format.
-  Azure PDF Export Support - Added ability to use service in Azure for
   exporting using EVO. See configuration guide: :download:`V7-Pdf-Exports-in-Azure-Websites.pdf </_static/images/V7-Pdf-Exports-in-Azure-Websites.pdf>`

-  Stored Procedure Parameters accept multiple values - Added ability to
   use stored procedures which accept multiple values for individual
   input parameters. Default configuration is set to use a comma as the
   delimiter when passed to the stored procedure. This can be altered to
   use any character for delimiter by altering the following in the
   IzendaSystemSetting table::
            
      Update IzendaSystemSetting where Name = 'StoredProcParamDelimiter'
      Set Value = '<your delimiter here>'

-  Dashboard & Report Draft Printing - User can now export or print
   unsaved reports and dashboards in viewer
-  Report List - When the last report from a category is deleted the
   category is no longer displayed in the report list, selection
   dropdowns, and permissions
-  License Checker - After validating a license with a start date in the
   future, system will revert to prior valid license

FIXES
^^^^^

-  Defect 616 System should display message when application cannot
   reach license server as needed for online license mode
-  Defect 5729 Calculated Fields function list in popup expanded for
   ease of use
-  Defect 9664 Altered results returned from Subtotal when no results
   are found to display no record returned
-  Defect 13113 Reports with required filters should display no data in
   the report viewer until filter values are added by the user
-  Defect 11893 When deleting the last report in a category the category
   should be deleted
-  Defect 13784 When role is given full access to reports and
   dashboards, the system should automatically make this role's data
   source access advanced but is keeping the default causing users
   access advanced joins in the designer but not allowing edits for the
   same role
-  Defect 13708 Reconnect is no longer needed to obtain stored procedure
   schema
-  Defect 13573 Settings License does not display full Izenda version
   number
-  Defect 13743 User with lower permissions is sometimes able to access
   designer using URL when they should not
-  Defect 13675 User with permission to create new report with no access
   to schedule or access is still seeing these tabs in the report
   designer
-  Defect 13660 Role with no permissions to create dashboards gets error
   when attempting to view a dashboard they have access to
-  Defect 12065 User can work with all functional buttons on reports
   which are no longer valid and should receive an error message when
   attempting to access
-  Defect 7746 Paging of a data grid should be updated with the data on
   UI when number of records changes in the database
-  Defect 13591 Validation of recurrence in scheduled items is not
   enforcing a numeric value
-  Defect 13853 Subreports with style popup shows blank page when using
   forms
-  Defect 13756 Report parts become blank after save with "snap to grid"
   checked
-  Defect 13744 User Defined Functions cannot be added to role data
   model
-  Defect 13855 Error message "The syntax of the expression is
   incorrect." when building grid with user define function
-  Defect 13628 When user is in ALL REPORTS in report list and moves to
   view a report once Close button is selected, user should be returned
   to ALL Reports, not the report's category
-  Defect 13849 Error displays and grid is cleared after selecting Add
   Side Total in Columns area of Pivot Grid
-  Defect 13587 Cancel is saving settings for sub and grand totals
-  Defect 13791 When creating a Form Subtotal and Grandtotal freeze the
   page
-  Defect 13603 In some Charts the X-axis title is removed after
   changing the default title
-  Defect 13689 After changing a range from Average/Range only on charts
   selecting Range = Null displays error message
-  Defect 13605 Metrics in the same separator are not grouped together
   properly in some chart types
-  Defect 13577 System is not exporting query after selecting Query
   Execution
-  Defect 13602 Missing validation when selecting the same view setting
   for Subreport, Custom URL, Embedded Javascript to ensure user does
   not set the same style on each item
-  Defect 13619 System shows error message when user select aggregated
   function for calculated fields
-  Defect 13511 When creating a Calculated Field Preview Data is
   selecting null records and should display the first actual value
-  Defect 13654 Using Oracle error message displays when formatting a
   grouped datetime field
-  Defect 13680 In Oracle and PostgreSQL error message displays when
   selecting BLANK/ NOT BLANK filter operators
-  Defect 13803 System does not show proper error message when license
   cannot connect to license server
-  Defect 13726 Printing some Gauges there is an extra line under Label
   title
-  Defect 13671 Error message should display when user clicks update
   results without relationships set
-  Defect 13479 Dynamic check box stored procedures is not checked by
   default if removed from available datasources and re-added
-  Defect 13732 After clicking update results in dashboard common
   filter's descriptions are not updating for dashboard tiles
-  Defect 12783 Could not print the dashboard tile or report after
   adding a new report part tile prior to saving
-  Defect 13589 Remove Print PDF from back of Dashboard tile as this is
   not available
-  Defect 13688 In Copy Management system freezes when user removes a
   tenant from the destination
-  Defect 13694 In Copy Management New workspace should be keep all
   content after save as workspace
-  Defect 13718 In Copy Management Error message is displayed when
   copying reports created using multiple database sources and added
   joins
-  Defect 13740 In copy Management System shows error message when user
   copies a report created using two connection strings
-  Defect 13684 In Copy Management System shows blank data on "To
   Object" on local mapping when user creates mapping of Type = Database
-  Defect 13664 Changing tenant in settings level dropdown does not take
   user to tenant's report list when user is in quick edit and report
   viewer
-  Defect 13752 Permission restriction error message displays when going
   to Quick Edit of a shared report when user has Full Access
-  Defect 13729 User is unable to move the shared report when they have
   full access role in the same tenant
-  Defect 13717 Page Break After Separator causes gauges not render if
   number of records is greater than 12
-  Defect 13724 Using Page Break after Separator prints redundant blank
   page and duplicate separator title in export files
-  Defect 13538 System always displayed error message: "This Field is
   invalid" when user selects calculated field in Sub Total/Grand Total
-  Defect 13801 Error message for license expiry does not disappear when
   the system can connect the license manager again
-  Defect 13898 Draft saving in dashboard does not include 5th title
-  Defect 13719 Copy Report should be remove all the subreport links and
   icons when user copies main report without the sub-report
-  Defect 13687 New reports should not be automatically included in
   saved workspace for report copy
-  Defect 13758 "All" check box is not checked as default value when
   user selects reports in new workspace.
-  Defect 13725 In Copy Management system does not keep the selected
   Report in Report List when user navigates from main screen to
   Advanced Screen without saving new workspace
-  Defect 13695 In Copy Management drop down list of Database Name is
   blank when the Source report is deleted and workspace is update after
   concurrency updated message is displayed
-  Defect 13686 In Copy Management Drop down list of Database Name is
   blank when user selects Item to Copy = "Reports", creates mapping
   then unchecks it and check on "Data Model" check box
-  Defect 13236 SQL query execution changed to include parameters as
   comments for ease of running query outside of Izenda
-  Defect 13136 After Updating the Field Name of a calculated field, the
   updated text should be displayed on this calculated field on Field
   list Filter, and Field Name on Field Name Properties
-  Defect 1057 After validating a future license, system should revert
   to prior valid license before
-  Defect 13526 In Copy Management using Copy Role after the role is
   copied, the role's permissions are unchecked instead of being
   inherited from the tenant
-  Defect 13500 In Copy Management system is not validating all schemas
   for data model copy
-  Defect 13296 When building a chart the Metric & Point Options overlap
   with long Breadcrumb

v0.22.12
~~~~~~~~

BREAKING CHANGES
^^^^^^^^^^^^^^^^

**Namespace changed from Izenda.Synergy to Izenda.BI** See more
information :doc:`here </dev/ref_interfaces>`.

FEATURES
^^^^^^^^

-  Data Model Copy Report - allows copying of report definitions from
   System to Tenant or Tenant to Tenant
-  Page Break after Separator for
   Gauges and Charts - allows page breaks between separators for Gauges and
   Charts when exporting and printing
-  Charts Embedded within a grid as an
   embedded subreport now scale to the size of the grid column. This aspect
   ratio is the same as the chart report part that is embedded.
-  Enhanced Ability to turn on or off some modules per system or tenant

FIXES
^^^^^

-  Defect 8218 - When changing the Field Alias in the model some fields
   are not properly updated in the join on the report
-  Defect 13497 - If DataSource Category and DataSource Alias names are
   the same the report will error
-  Defect 13397 - Subreport popup displays within the subreport popup
-  Defect 13572 - Query export fails when using a calculated field until
   report is saved
-  Defect 13545 - Word/Excel export fails to work with invalid custom
   URL field
-  Defect 13571 - Hover labels do not match the chart when using dates
-  Defect 13594 - Using a dynamic stored procedures the input parameter
   is not saving in the designer page when changed.
-  Defect 13663 - When in Quick Edit mode Values list fails to load for
   stored procedures where the input parameters were defined in the
   datamodel.
-  Defect 13561 - Export fails when using Comparison type filter and
   between datetime using Chart, Gauge and Map
-  Defect 13459 - Content of search drop down fails to show all values
   in Report, Dashboard, and Template list
-  Defect 13253 - Prevent any actions on reports other than delete where
   the connection string is hidden
-  Defect 13588 - In Field Property Grand or Sub totals preview button
   is not enabled after selecting a Function
-  Defect 13631 - Report name becomes blank after updating result while
   using a dynamic stored procedure
-  Defect 13647 - Subtotal still displays even though Function = None
-  Defect 13583 - Report Fails to create subtotal with expressions using
   two calculated fields
-  Defect 13642 - Subreports built using a dynamic stored procedures
   with different parameters failed to render
-  Defect 13650 - When using a stored procedure 'No record found'
   displayed if no user define filter value are found in the Data Model
   and user enters valid filter value in Report Designer
-  Defect 13636 - System shows error when user select Filter Value =
   BLANK instead of no records found when there are no blank values
-  Defect 13607 - Chart Drilldown fails when building a chart and using
   the input paramater of stored procedure as labels container
-  Defect 13617 - Chart Fails to generate SQL properly when using
   drilldown and value is null
-  Defect 13548 - Range formatting for area charts did not persist for
   new metric
-  Defect 13627 - System does not allow user to create a SubTotal/Grand
   Total which has the same name as a calculated field
-  Defect 13535 - Using Quick Edit when adding or removing a field the
   report is not actually moved to the destination category
-  Defect 13482 - System fails to print embedded subreport
-  Defect 13608 - Using Oracle Database User receives error 'There is an
   error when querying data. Please update the configuration.' when
   report is built from stored procedure
-  Defect 13606 - When using input parameter dates are causing Error
   'The query syntax is incorrect.' when build report with stored
   procedure in MySQL
-  Defect 13473 - Using MS Edge & IE all rows on Grid in the DataModel
   are blank or too large to view
-  Defect 13625 - When sorting on some aggregated fields with altered
   format user receives error from query
-  Defect 13579 - Field selection dropdown generates wrong list for
   smart tag dropdown list in forms
-  Defect 13615 - Repeater html structure is broken if adding another
   table to a repeated table
-  Defect 13595 - User receives no error tool-tip when attempting to
   save a report with a duplicate relationship
-  Defect 13576 - In the data model a duplicate message is displayed
   when user edits an existing calculated field without save
-  Defect 13261 - Validated Indicator on Data Model Tree of Advanced
   Copy screen does not disappear when the mapping is edited/deleted
-  Defect 13624 - Search feature on Report and Dashboard list is not
   updating data as input when searching
-  Defect 13596 - Validation is not happening correctly on Dashboard
   Common Filters prior to save
-  Defect 13514 - When selecting a new dashboard without preset layout
   new report part tiles are not properly formatted
-  Defect 13490 - In Data Model Copy Management validation is
   case-sensitive when user input "To Object" on global mapping
-  Defect 13716 - Copy Management Data Model Mapping Drop down list of
   Database Name is blank when user selects Item to Copy = "Data Model",
   creates mapping without unselect items on data sources in Source box
-  Defect 13254 - Copy Management Advanced Copy Validate System missing
   validated indicator on functions and stored procedures which have no
   fields
-  Defect 13480 - Field is not recognized when changing database name in
   the connection string
-  Defect 13461 - Tooltips are not displayed on almost pages of the
   system when user opens an existing report
-  Defect 13651 - Dashboard not showing proper permission error with
   user who has no access to report.
-  Defect 11156 - Schema fails to load when there are 1000's of Data
   Sources
-  Defect 13656 - With a Report created from Dynamic stored procedure it
   is no longer valid if user update filter value in Data Model
-  Defect 10164 - Conditional Formatting using Range Percentage not
   displaying properly when used with multiple fields
-  Defect 13635 - Drop down list of Filter type Equivalence is not
   repopulated correctly when there are 2 parameters in Filter section
-  Defect 13623 - System shows error message that schema is not correct
   for stored procedure which is not dynamic but still allows user to
   navigate to fields tab
-  Defect 13584 - System shows the actual field name on backend when
   user creates calculated field with IF THEN ELSE END system
-  Defect 13539 - Calculated Fields with special characters error when
   added to Report part container
-  Defect 13638 - Duplicate check does not work for duplicate filter
   alias when using stored procedure
-  Defect 13574 - In Data Setup, Advanced Settings an error message is
   displayed "Another user recently modified this data..." when saving
   with Sort Column Name
-  Defect 13629 - System shows duplicate schema when user re-executes a
   static stored procedure after changing it to Dynamic then back to
   Static
-  Defect 13657 - In the Data Model Relationships after deleting a
   physical relationship in the database after reconnect it remains in
   the model
-  Defect 13682 - In Copy Management validate button is always displayed
   although user has not selected anything
-  Defect 13652 - Exporting PDF/Word with Form having encoded special
   characters in field alias fails
-  Defect 13192 - The current report list page is not updating after
   clicking on a specific category and moving or copying a report
-  Defect 13147 - User cannot update value on Report Title & Description
   after changing any configuration options on Format Properties
-  Defect 13237 - Exported files should have hyperlink when applying
   customURL, embeddedJavascript and subreport
-  Defect 13279 - Stored Procedure Input Parameter configured in
   Datamodel with lookup value not displaying results in filter dropdown
-  Defect 13611 - Query validation showing errors when user changes
   language
-  Defect 13612 - Using French "Inner" item is not selected by default
   for join type
-  Defect 13233 - In Integrated environment Sub-report using popup style
   doesn't work
-  Defect 13169 - In Integrated environment Cell spacing and cell
   padding do not apply on front side
-  Defect 13119 - In Integrated environment Active dropdown button color
   is not consistent
-  Defect 13303 - In Integrated environment table in preview mode
   differs from standalone environment
-  Defect 13436 - Export progress bar is still sometimes displayed after
   logging into a different user account
-  Defect 13476 - Calculated Fields Functions and Operators should only
   list out those functions and operators that can be used in a specific
   DB server type
-  Defect 13297 - Title and Description of dashboard tile does not
   display when printing and exporting
-  Defect 13770 - Link and Popup style subreports not working from Forms

v0.22.11-hotfix
~~~~~~~~~~~~~~~

-  Defect 13679 - When using connect or reconnect button on connection
   sting system is executing stored procedures prior to moving them to
   visible. With this hot fix the stored procedures will not be executed
   until items are moved to visible and selecting reconnect. This will
   be further altered to execute once items are moved to visible, but
   for now reconnect will execute the stored procedure to obtain schema
   (column names).

v0.22.11
~~~~~~~~

FEATURES
^^^^^^^^

-  Stored Procedure input parameters can now be used as fields in the
   report or for joining to other items
-  Report & Dashboard ID should be
   displayed in URL after saving while still in designer
-  After Reconnecting
   to the Connection String, system is not reloading the removed or updated
   physical relationship of Visible Datasources
-  Delete icon access to field
   mapping rows in subreports for easy removal of the field mappings

FIXES
^^^^^

-  Defect 13118 - When in Embedded mode Add New User and Configure
   Password Options are hidden in the user tab UI
-  Defect 13161 - Dirty Form read is not working in embedded mode
   properly
-  Defect 13200 - Exporting issues (Header & Footer Missing, content of
   the report body section is not fully exported)
-  Defect 13558 - When columns names have some special characters PDF &
   Word exports can fail
-  Defect 13467 - Unable change data type of Expression subtotal
-  Defect 13544 - Custom URL link with field reference not working on
   export - Export contains field name not value
-  Defect 13556 - Logging - User ID is not logged with actions in INFO
   level logging
-  Defect 13393 - System user with permissions to create Dashboard not
   showing New option
-  Defect 2053 - No error message shown when the API is not accessible
-  Defect 12795 - Removed auto scroll from Permissions page leaving
   section titles as links
-  Defect 12827 - When user with advanced datasource mode in report
   designer shares report with simple datasource user, the datasources
   should not be available for edit
-  Defect 13448 - When user clears cookies and refreshes the page while
   logged in, no errors should be displayed and user should be directed
   to login page
-  Defect 13301 - Title and Description for Dashboard tiles are not
   picked up as changes when resaving a dashboard
-  Defect 13223 - Filter descriptions are removed in dashboard after
   exporting or printing a dashboard
-  Defect 13483 - In the Data Model the second grid (slave grid) doesn't
   fill the remaining section of the page
-  Defect 13184 - In Charts some configured colors do not match colors
   in preview mode
-  Defect 13299 - Pagination fails to work on form report
-  Defect 13168 - Page format is broken when embedding a form with table
   to another form with table.
-  Defect 13291 - When using Forms embedded subreport shows blank when
   there is pair of repeaters in form
-  Defect 13179 - In Report List user cannot change report name and is
   slow when entering category on Move or Copy Report
-  Defect 13495 - When using embedded mode print report does not work on
   Izenda Report View tab
-  Defect 13142 - When in Embedded mode using Design and Subscribe
   button do not work on Report List and Report Viewer
-  Defect 13172 - Using Oracle data source and the AVG function number
   must be rounded to maximum 28 digit number
-  Defect 13156 - Both Printing and Preview functions, in the format
   footer report, the {pageNumber} and {currentDateTime} are not
   populated values
-  Defect 13284 - Map is incorrectly redrawn when having point option
   State Province
-  Defect 13292 - Removed calculated fields from the data model in join
   dropdowns. These fields cannot be used for joining.
-  Defect 13527 - System errors when using a calculated field in compare
   field filters
-  Defect 13282 - Missing check Filter rules when navigating from Data
   Source to Field tab while using stored procedures. Filter data must
   be populated
-  Defect 13181 - Custom headers were inconsistent between
   printing/exported pages
-  Defect 13285 - Aggregate fields used as filters were underlined by
   default
-  Defect 13174 - Calculated fields were not recognized as having an
   aggregation when used alongside aggregated fields in a form
-  Defect 13173 - Calculated fields were lost when using 'Save As' on a
   report
-  Defect 13283 - All stored procedure parameters were flagged with
   errors when only one was incorrect.
-  Defect 13287 - Calculated fields built off of calculated fields were
   showing inaccurate results
-  Defect 13280 - Ntext type fields in SQL causing errors when used in
   report
-  Defect 13189 - Image files render within form instead of showing
   encoded values
-  Defect 13532 - Snap to Grid' checkbox label was not being translated
   when new languages were selected
-  Defect 13235 - Changes were lost when switching from View Mode to
   Quick Edit
-  Defect 13347 - Word/Excel exports lost filter information; Word
   export did not carry through table repeaters;
-  Defect 13178 - Sparklines showing only 6 items by default, regardless
   of preview record selection
-  Defect 13298 - User could see roles on the Access page they were
   denies permission to share with
-  Defect 13521 - Users would not be visible in the system if all of
   their associated roles were deleted
-  Defect 5729 - Interactive/Clickable space on the function in the
   expression builder was too small
-  Defect 13077 - Page breaks on grids with embedded charts would split
   the chart across multiple pages
-  Defect 13512 - User could see roles on the Access page they were
   denies permission to share with
-  Defect 13506 - Calculated Fields built in the data model could be
   deleted in the report designer
-  Defect 13517 - User was unable to delete Calculated Fields if they
   were at any point used in Sub Total/Grand Total calculations
-  Defect 13502 - User could navigate to the left-panel tabs without
   having chosen a data source in the report designer
-  Defect 13568 - Reports would not save if a stored procedure used in
   the report contained a boolean parameters
-  Defect 13477 - When copying stored procedures from system level to a
   tenant, parameters with any field properties checked would not copy
-  Defect 13475 - When copying tables from a system level to a tenant
   level, table categories are not copied
-  Defect 13251 - Dashboards would print unused tiles as the basic
   report part tile instead of blank space
-  Defect 13492 - Tool-tips on Dashboard titles/descriptions would not
   render if the text was too long
-  Defect 13597 - Forms with nested repeaters could not be exported
-  Defect 13485 - After copying a form tile, the original form would be
   blank until interacted with if the copy is deleted
-  Defect 13513 - Forms would freeze if a sub-report was added from the
   front of the tile
-  Defect 13130 - Report part dropdown would render out of place in the
   designer if used while a report part was loading
-  Defect 13494 - Equivalence applied to Calculated Fields would produce
   inconsistent data
-  Defect 13581 - System would error if a Calculated Field is used in a
   report with any field that has the same name as the calculated field
-  Defect 13489 - System would error if an aggregation was applied to a
   Calculated Field set up in the data model
-  Defect 13446 - Functions would throw an error if the resulting
   calculation exceeded the capacity for the data type
-  Defect 13560 - Adding separators to a Sparkline chart misaligns the
   columns from their headers
-  Defect 13259 - Blank pages were inserted between report parts when
   exporting to PDF
-  Defect 13274 - MySQL/Oracle fusion joins would error out if a null
   value was found
-  Defect 13547 - Charts that contained DateTime values as drilldown
   labels would fail to render
-  Defect 13249 - Subreports placed in a form with an existing repeater
   would fail
-  Defect 13592 - Scheduling tab would render as blank
-  Defect 13519 - Categories with sub-categories would not be shown with
   an expansion icon
-  Defect 13191 - Searching in the dashboard/report list performed
   slowly
-  Defect 13531 - Exporting a report with a logo in the header would
   cause the logo to overlap data if the image size was too large
-  Defect 13486 - Several report parts would showed duplicate metric
   selections dropdown and drilldown breadcrumbs
-  Defect 13454 - Report parts would still indicate a sub-report was
   present after the sub-report was deleted
-  Defect 8218 - Alias changes in the data model would invalidate joins
   created in existing reports

v0.22.10
~~~~~~~~

FIXES
^^^^^

-  Defect 13542 - Metric dropdown is duplicated in pie, donut, tree, and
   other chart types
-  Defect 13468 - Charts are not exporting in BETA build 0.22.7
-  Defect 13543 - DateTime conditional formatting is inconsisent in the
   Report Designer

v0.22.9
~~~~~~~

BREAKING CHANGES
^^^^^^^^^^^^^^^^

-  All BETA customers upgrading from 0.22.7 or prior to 0.22.9 will need
   a new License Key and Token. Please contact
   customersupport@izenda.com for your Token.

-  Adjusted spelling error in the Web.config file::

    <!--Izenda-->
        <add key="izendaapiprefix" value="api" />
        <add key="izendapassphrase" value="vqL7SF+9c9FIQEKUOhSZapacQgUQh" />
    <!--Izenda End-->

-  Refactored some JavaScript API for ease of integration within client
   application. This will require a small change to the integration code
   as shown below for customers who are already integrating Izenda
   within their application.

   Please see code samples of use before (izenda.integrate.js)::

       IzendaSynergy.setCurrentUserContext(currentUserContext).then(function () {
           IzendaSynergy.render(document.getElementById('izenda-root'));
       });

   And now after::

       IzendaSynergy.setCurrentUserContext(currentUserContext);
       IzendaSynergy.render(document.getElementById('izenda-root'));

FEATURES
^^^^^^^^

-  12489 Feature - Report Designer Calculated Fields added ability to
   delete a calculated field

FIXES
^^^^^

-  Defect 13309 - Tenant Setup all tenant management still shows when
   system mode is "1"
-  Defect 13081 - Report Viewer Export PDF with grid and two charts
   below the second chart gets moved and cut off
-  Defect 13114 - Report Designer Chart Sorting by Y Axis is not working
-  Defect 12644 - Report Designer Maps remove mismatch Point Errors for
   Points
-  Defect 9927 - Report Designer Form Adding RSS Feed or other Widget to
   Form causes Form to stop working
-  Defect 13276 - Web Config Spelling Error in Web.config
-  Defect 12753 - Report Designer Map remove Icon option in Embedded
   Javascript
-  Defect 13018 - Report Designer Calculated Fields should not allow the
   use of "[" "]" in the field name but system was not checking so
   errors occur when using these characters
-  Defect 13086 - Tenant Setup when user deletes a Tenant, the content
   panel still displays that Tenant's information on the Details tab
-  Defect 13244 - Tenant Setup when selecting an existing Tenant,
   content panel is not displaying the corresponding data of this tenant
-  Defect 13430 - Tenant Setup Delete an existing Tenant, Permission Tab
   of the next Tenant is displayed incorrectly data (displayed data of
   the deleted previous Tenant)
-  Defect 13462 - Settings Report List Drop down list of Setting Level
   for Tenant Level is displayed incorrectly after tenant user logs in
   and then system user logs in
-  Defect 13268 - Report Viewer Printed Report parts are not same as
   report designer when checking Snap to Grid in the report designer
-  Defect 13449 - Report Designer Grid "No record found" displays after
   building Pivot/Drill Down grid
-  Defect 13199 - Report Designer chart does not render when multiple
   fields are added in the labels container
-  Defect 13201 - Report Designer Chart User cannot use Threshold and
   Range option together
-  Defect 13256 - Report Designer error message shows when delete
   existing filter with operator before filter with no operator
-  Defect 13111 - Report Designer Relationship of an existing report is
   changed to blank on Data Object/Foreign Data Object when user update
   Data Source Alias
-  Defect 13453 - Report Designer Grid Column's width is expanded beyond
   standard 150 px width automatically after building a grid with
   separator
-  Defect 13478 - Page loads indefinitely when logging out at report
   designer and quick edit page
-  Defect 13250 - Page freezes when adding a subreport to a Form field
-  Defect 13432 - When adding a repeater to a Form field the system
   fails
-  Defect 13246 - When using Firefox the Font setting is not working in
   Map's Legend
-  Defect 13265 - Data Setup Advanced Settings Category New scroll bar
   applied to page
-  Defect 13269 - Copy Management when using Oracle or MySQL System
   shows error message when user runs copy Data Model. Issue is with
   Timeout and Transaction blocking
-  Defect 13463 - Copy Management System shows blank page on Copy
   management when a Tenant user logs in and logs out before admin user
   logs in
-  Defect 13429 - Common All dropdowns fail to work on the first time
   user enters the application
-  Defect 13434 - Admin User is Unable to logout while in a tenant level
-  Defect 12228 - Copy Management GUI of Grid on main page and Advanced
   Copy Management page extends beyond table
-  Defect 13224 - Setting Level Section is not shown after changing from
   single-tenant to multi-tenant in integrated mode
-  Defect 13185 - Export fails when user saves report name with special
   characters
-  Defect 13225 - Report Designer when creating Functions and Operators
   tab is displayed incorrectly when navigating calculated field popup
-  Defect 13176 - Report Viewer Printed report with embedded subreport
   is not shown correctly
-  Defect 13198 - When using Quick Edit to add or edit a filter for any
   existing report, displayed Filter values are not updated in report
   description
-  Defect 13219 - Report Designer Map Unknown Error when drilling down
   in Country Map when using a fusion connection to multiple databases
-  Defect 13222 - SubReport Link is not displayed when map is too small
   for label to show
-  Defect 13227 - Form exporting is not properly spacing the data as
   shown on screen
-  Defect 13129 - Report Designer Gauge No error displays when comparing
   data with a text field
-  Defect 13286 - When crating a report with two joined datasource and
   one is a stored procedure parameters show up in filter panel as blank
   field not the @parameter
-  Defect 13278 - Report Designer when creating a drilldown grid the
   value is reset to 0 after clicking collapse icon next to All
-  Defect 13137 - Exporting in Word, Excel, PDF missing report elements
   (Description, Font Size and Color, Filter Description, Subtotal &
   Grand Total)
-  Defect 13145 - Report Designer when user adds one Calculated Field
   which contains aggregated function into Filter section, system shows
   error message
-  Defect 13196 - Report Designer & Report Viewer System shows error
   message when user adds one Filter of Aggregated Function on Donut
   chart
-  Defect 13194 - Added Hover text to show report part of aggregated
   fields in filter field list
-  Defect 13121 - Report Designer & Data Model Calculated Field is
   Missing progress bar when user clicks on Preview button or OK button
-  Defect 13138 - When in Integrated Mode User Setup should be disabled
-  Defect 13146 - When in Integrated Mode Print Report from Report List
   or Viewer does not work
-  Defect 13190 - When using Firefox Chart labels do not update properly
   when changing label text direction
-  Defect 13221 - Record counts for exporting and printing dashboards is
   limited by items per page not record limit per tile
-  Defect 13210 - When printing chart from dashboard they are split
   between two pages
-  Defect 13231 - Exporting a specific report from the back of a
   dashboard tile is only exporting the current report part, not the
   entire report
-  Defect 13215 - In Dashboard the Email Search Function does not work
   with criteria "ALL"
-  Defect 13214 - Dashboard Text Tile with large string of data is
   showing outside of the tile
-  Defect 13158 - No error message shown when Combination Chart fails to
   render data value
-  Defect 13143 - When in Integrated Mode when user copies a gauge
   report part and tries to reposition it in the report designer the
   report fails to save
-  Defects 10674 - Save As popup still refers to report instead of
   template when saving template

v0.22.7
~~~~~~~

-  13200 - When exporting forms with repeaters, the repeated sections
   are not properly formatted.
-  13395 - When exporting forms with repeaters, extra <br> tags are
   added, creating additional line breaks.
-  13396 - When using a field alias and adding a subtotal/grandtotal
   expression on the aliased field, an error is shown stating that the
   given that field is not found.

v0.22.5
~~~~~~~

-  12906 - Report List & Dashboard List Slow Loading
-  13030 - Forms Repeaters do not work for repeating rows
-  13115 - Separator Format is broken when using some Separator Styles
-  13116 - MySQL Export Query no longer working from Report Designer
-  13083 - Printing to HTML causes all charts to print vertically
   positioned rather than horizontal
-  12987 - Report Designer removed grid lines from the grand total
-  12765 - Report Designer DateTime Fields with Color Formatting Using
   Value Highlighting Multiple Dates instead of the one selected
-  13094 - All "Data Source" check boxes are not selected on Role Setup
   when user has already setting it on Tenant Setup
-  13089 - Tenant Setup section is not respected on Role Setup
   Permission page of 1 Tenant User
-  13109 - Layout displayed inconsistently when clicking icon for next
   when using paging
-  13087 - Form only displays records based on Item Per Page, instead of
   Preview Record
-  12880 - Metric Drop-down is cutoff when viewing in Preview Mode for
   Map
-  12818 - Color formatting does not work for DateTime fields
-  13104 - Pagination checkbox is checked by default but the section
   does not show at page bottom until saved
-  13102 - Page freezes when copying a form in full screen mode
-  13107 - Cannot delete field in forms or open field properties when
   clicking on field pill
-  13085 - Incorrect default label of Y-axis when Metrics = 'All'
-  13099 - Chart option Spline does not apply on Area chart type
-  13090 - Charts do not render when using Range option
-  13075 - Cannot change item per page of copied report part
-  13092 - When building a Chart user cannot sort separator when Labels
   contain multiple fields
-  13106 - Pivot Grid Number of Columns per exported page should be
   consistent with Preview section
-  13103 - When user with Advanced Datasource Mode opens a Report in the
   Designer created by Simple Datasource user, system shows error
   message for missing relationship
-  13095 - Report Designer System does not prevent a user form saving a
   report in which relationships use incorrect join aliases
-  13074 - Using Advanced Datasource Mode in Report Designer the Drop
   down list of Foreign Data Object is displayed incorrect with Join
   Alias
-  13202 - Report Designer in Data Source Join Field & Field is not
   using Enhanced Dropdown list
-  13073 - Calculated Field Grand Total & Sub Total when using Oracle
   Preview shows no data when user creates 1 Calculated Field for Oracle
   datasource
-  12828 - Report Viewer is missing some report parts when going to a
   saved report with multiple report parts
-  13148 - Form Repeaters are cloning fields when highlighting multiple
   fields and adding repeater
-  13108 - Form when using parallel repeaters Data values are not
   displayed in new line
-  11375 - System not showing Validated Indicator on Data Model Tree
   after the Destination has already been validated in Advanced Copy
-  13091 - Dashboard Text type tiles showing text beyond the tile
   boarder with longer text values
-  13088 - Could not save the dashboard after edited the Text type tile
-  13100 - In Dashboard & Report Designer Schedule Email Body template
   is blank
-  11013 - Common Application Footer is not fixed with the bottom of the
   Middle Panel and Left navigation
-  12822 - When sing IE or Edge Cannot login to system
-  13027 - In Data Setup, Copy Management Concurrency updated message is
   always displayed when user clicks on Save the first time
-  13124 - Responsive issue with 1366 x 768 in the datasource tab of
   Report Designer the Relationship section is not scrollable
-  13122 - Report Designer page freezes after selecting the report part
   drop-down in the Report Part Property panel when in full screen mode
-  13120 - Tool Tip Should be displayed when users hovers over column
   header
-  13126 - When changing the grid style in the report part the Preview
   section is not displayed
-  13131 - When using Drill Down Grid Icon for collapse & explain is not
   displaying consistently
-  13123 - Field Properties for Alternative Text Settings lost when
   editing Field
-  13141 - Page freezes after using Text Color & Cell Color or
   Alternative Text then applying Grand Total
-  13132 - When using Report Part Gauge Preview is blank when deleting
   Metric 1 even though Metric 2 is available to build Gauge
-  13140 - Report Part Form the Formatting does not apply on datetime
   field
-  13134 - Form Repeater when used with Embedded Report Setting does not
   render subreport outside of the Repeater
-  13135 - Removed Non-editable option from Form Property Panel
-  13159 - Data returned from Calculated Field is displayed incorrectly
   when user combines data between Azure and MSSQL
-  13150 - Report Designer user is unable to save after resizing report
   part, the report shows no changes were found
-  13128 - Error received when switching to a Tenant Level to create a
   report using a Map report part

v0.22.4
~~~~~~~

-  13117 - Distinct Checkbox not working with MSSQL
-  13146 - Print from Report List and Report Viewer not working in
   Integrated Mode
-  13166 - Removed Foreign System References in Log File

v0.22.3
~~~~~~~
