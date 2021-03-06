

============================
Report Designer Details APIs
============================

The **Report Designer** page allows user to

-  create a report
-  select report data sources
-  create report filters
-  design report layout
-  set up report schedule

This page documents the APIs related to report details.

List of APIs
------------

.. list-table::
   :class: apitable
   :widths: 35 65
   :header-rows: 1

   * - API
     - Purpose
   * - `GET report/category/{type}/(tenant\_id)`_
     - Returns an array of allowed saving categories for Report (type=0), Template (type=1) or Dashboard (type=2).
   * - `GET report/subcategory/{parent\_category\_id}`_
     - Returns an array of allowed saving sub-categories under the Report category specified by parent_category_id.
   * - `GET report/allCategories/{type}/(tenant\_id)`_
     - Returns the list of categories by type (Reports/Templates/Dashboards) in parent-child hierarchy.
   * - `POST report/category`_
     - Renames a report category.
   * - `DELETE report/category/{report\_category\_id}`_
     - Removes the report category specified by report_category_id.
   * - `POST report/validateCategoryName`_
     - Validates a report category name.
   * - `GET report/reportByProperty/{report\_id}/{property}`_
     - Returns a report section.
   * - `POST report`_
     - Saves a report.
   * - `POST report/draft`_
     - Saves a report as draft.
   * - `POST report/cancel`_
     - Deletes temporary data of a report.
   * - `POST report/loadReport`_
     - Returns a report's details.

       .. warning::

          This API is obsolete, use `POST report/loadForEdit`_ instead.

   * - `POST report/loadForEdit`_
     - Returns an object with report key and report's  data.
   * - `GET report/(tenant\_id)`_
     - Returns all report categories together with the reports inside.
   * - `POST report/validateReportName`_
     - Validates that a report name is unique and not empty.
   * - `POST report/validate`_
     - Validates that a report name is unique and its filter and relationships are valid.
   * - `POST report/search`_
     - Searches for reports.
   * - `POST report/advancedSearch`_
     - Searches for reports with advanced options.
   * - `GET report/reportMode`_
     - Returns the report mode.
   * - `POST report/reportMode/{value}`_
     - Sets the report mode to Simple or Advanced.
   * - `POST report/detectReportChange`_
     - Verifies that all report details are up to date, without physical changes, and valid.
   * - `GET report/function/{function\_mode}/{data\_type}/(tenant\_id)`_
     - Returns a list of report functions filtered by mode (field, sub-total, grand-total), by data type and by tenant_id if provided.
   * - `GET report/allReports/(tenant\_id)`_
     - Returns a list of all reports filtered by tenant_id if provided.
   * - `POST report/detectSchemaChange`_
     - Verifies that all report filter fields are without changes.
   * - `POST report/updateResults`_
     - Updates the report data model.
   * - `POST report/loadAccesses`_
     - Returns a list of all accesses.
   * - `POST report/loadSchedules`_
     - Returns a list of all scheduled deliveries.
   * - `GET report/reportPart/{report\_part\_id}/(report\_id)`_
     - Returns the report part definition specified by report_part_id from draft (using report_id) or from database (without report_id).
   * - `POST report/validateExistingField`_
     - Validates that some fields exist in a report.
   * - `GET report/reportId/(tenant\_id)?reportName=value&categoryName=value`_
     - Returns report id from report name and category name.
   * - `POST report/validateExistingReport`_
     - Validates that a report exists in a category.
   * - `GET report/reportsByTenant/(tenant\_id)`_
     - Returns list of reports by tenant, with each report containing a list of report parts.
   * - `POST report/updateRenderingTime`_
     - Updates the rendering time of a report.
   * - `GET report/isReportValid/(report\_id)`_
     - Returns true if there is a report definition for the specified report_id.
   * - `POST report/printDraft`_
     - Saves report as draft for printing.
   * - `GET report/printDraft/{report\_id}`_
     - Gets report as draft for printing.
   * - `GET report/accessPriority/(report\_dashboard\_id)`_
     - Returns access priority for report/dashboard.

GET report/category/{type}/(tenant\_id)
---------------------------------------

Returns an array of allowed saving categories by type (Reports/Templates/Dashboards), filtered by tenant_id if given.

   type
      - 0 = Reports
      - 1 = Templates
      - 2 = Dashboards

**Request**

    No payload

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      GET /api/report/category/1 HTTP/1.1

   Sample response::

      [{
         "name": "Category 1",
         "type": "Templates",
         "parentId": null,
         "tenantId": null,
         "status": 2,
         "id": "17c176e1-500b-4378-8c59-1f69e84e425b",
         "state": 0,
         "modified": null"
     }, {
         "name": "Sub Category 1",
         "type": "Templates",
         "parentId": "17c176e1-500b-4378-8c59-1f69e84e425b",
         "tenantId": null,
         "status": 2,
         "id": "14b3f8c7-c4e8-4730-a57e-3b28ad75b097",
         "state": 0,
         "modified": null"
     }, {
         "name": "Sub Category 2",
         "type": "Templates",
         "parentId": "17c176e1-500b-4378-8c59-1f69e84e425b",
         "tenantId": null,
         "status": 2,
         "id": "72d44e10-a707-455e-99dc-054088b6b2f3",
         "state": 0,
         "modified": null"
     }]


GET report/subcategory/{parent\_category\_id}
---------------------------------------------

Returns an array of allowed saving sub-categories under the Report category specified by parent\_category\_id.

**Request**

    No payload

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      GET /api/report/subcategory/17c176e1-500b-4378-8c59-1f69e84e425b HTTP/1.1

   Sample response::

      [{
         "name": "Sub Category 1",
         "type": null,
         "parentId": "17c176e1-500b-4378-8c59-1f69e84e425b",
         "tenantId": null,
         "status": 2,
         "id": "72d44e10-a707-455e-99dc-054088b6b2f3",
         "state": 0,
         "modified": null
     }, {
         "name": "Sub Category 2",
         "type": null,
         "parentId": "17c176e1-500b-4378-8c59-1f69e84e425b",
         "tenantId": null,
         "status": 2,
         "id": "14b3f8c7-c4e8-4730-a57e-3b28ad75b097",
         "state": 0,
         "modified": null
     }]


GET report/allCategories/{type}/(tenant\_id)
--------------------------------------------

Returns the list of categories by type (Reports/Templates/Dashboards) in parent-child hierarchy.

   type
      - 0 = Reports
      - 1 = Templates
      - 2 = Dashboards

**Request**

    No payload

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      GET /api/report/allCategories/0 HTTP/1.1

   Sample response::

      [{
         "name": "Category 1",
         "type": 0,
         "parentId": null,
         "tenantId": null,
         "subReportCategories": null,
         "canDelete": false,
         "status": 2,
         "id": "f2d79ff5-3aa8-4ae6-b0d0-e47687a77380",
         "state": 0,
         "inserted": true,
         "version": null,
         "created": null,
         "createdBy": null,
         "modified": null,
         "modifiedBy": null
     }, {
         "name": "Category 2",
         "type": 0,
         "parentId": null,
         "tenantId": null,
         "subReportCategories": [{
             "name": "Sub-category 1",
             "type": 0,
             "parentId": "f514e26f-501c-4369-8ea9-de4eba208bdf",
             "tenantId": null,
             "subReportCategories": null,
             "canDelete": false,
             "status": 2,
             "id": "81517214-273b-42e9-91b5-8ef766cc5761",
             "state": 0,
             "inserted": true,
             "version": null,
             "created": null,
             "createdBy": null,
             "modified": null,
             "modifiedBy": null
         }],
         "canDelete": false,
         "status": 2,
         "id": "f514e26f-501c-4369-8ea9-de4eba208bdf",
         "state": 0,
         "inserted": true,
         "version": null,
         "created": null,
         "createdBy": null,
         "modified": null,
         "modifiedBy": null
     }]


POST report/category
---------------------------------------

Renames a report category.

**Request**

    A :doc:`models/Category` object

**Response**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  | **success**
             | boolean
          -  Is the rename successful
          -
       *  -  | **messages**
             | array of strings
          -  The error messages
          -

**Samples**

   .. code-block:: http

      POST /api/report/category HTTP/1.1

   Request Payload::

      {
        	"id" : "f2d79ff5-3aa8-4ae6-b0d0-e47687a77380",
        	"type" : 1,
        	"name" : "Category 1 renamed",
        	"parentId" : null,
        	"tenantId" : null,
        	"status" : 2,
        	"state" : 0,
        	"modified" : null,
        	"canDelete" : false,
        	"subCategories" : [],
        	"subReportCategories" : null,
        	"reports" : []
      }

   Successful response::

      {
         "success": true,
         "messages": null
     }


DELETE report/category/{report\_category\_id}
----------------------------------------------

Removes the report category specified by report_category_id.

**Request**

    No payload

**Response**

   An :doc:`models/OperationResult` object with the **success** field populated:

   .. list-table::
      :header-rows: 1

      *  -  Field
         -  Description
         -  Note
      *  -  | **success**
            | boolean
         -  Is the rename successful
         -

**Samples**

   .. code-block:: http

      DELETE /api/report/category/f285a869-25fb-428e-8cef-856241ba4249 HTTP/1.1

   Sample response in case of error::

      {
     	"success" : false,
        	"messages" : [{
        			"key" : "",
        			"messages" : ["This category (or its sub-category) containing report(s)."]
        		}
        	]
      }


POST report/validateCategoryName
---------------------------------------

Validates a report category name.

**Request**

    Payload: a :doc:`models/Category` object

**Response**

    - true if the category name is valid and not duplicated
    - false if not

**Samples**

   .. code-block:: http

      POST /api/report/validateCategoryName HTTP/1.1

   With payload::

      {
        "name": "InternetSales",
        "type": 0,
        "parentId": "5d034fc7-0cc8-46b7-beb3-35b22c57827c",
        "id": "45f17b8a-3708-4f36-80ef-9178b7124841"
      }

   Sample response::

      true

GET report/reportByProperty/{report\_id}/{property}
------------------------------------------------------------

Returns a report section.

**Request**

    property
      * 0 = All
      * 1 = DataSource
      * 2 = Relationship
      * 3 = Filter
      * 4 = ReportPart
      * 5 = CalculatedField
      * 6 = DynamicQuerySourceField
      * 7 = Scheduling
      * 8 = Access
      * 9 = Report

**Response**

    A :doc:`models/ReportDefinition` object

**Samples**

   .. code-block:: http

      GET /api/report/reportByProperty/e09f9d45-b721-4012-b8e7-c31c58d52af3/3 HTTP/1.1

   .. container:: toggle

      .. container:: header

         Sample response:

      .. code-block:: json

         {
           "inaccessible": false,
           "category": {
             "name": "Sales",
             "type": 0,
             "parentId": null,
             "tenantId": null,
             "canDelete": false,
             "editable": false,
             "savable": false,
             "subCategories": [],
             "checked": false,
             "reports": null,
             "dashboards": null,
             "id": "93de93b9-d5d1-48f1-800d-1db1ffc02614",
             "state": 0,
             "deleted": false,
             "inserted": true,
             "version": null,
             "created": null,
             "createdBy": "John Doe",
             "modified": null,
             "modifiedBy": null
           },
           "subCategory": null,
           "reportRelationship": [],
           "reportPart": [],
           "reportFilter": {
             "filterFields": [],
             "logic": "",
             "visible": true,
             "reportId": "e09f9d45-b721-4012-b8e7-c31c58d52af3",
             "id": "93f2af72-1309-46fe-a779-ff426574619f",
             "state": 0,
             "deleted": false,
             "inserted": true,
             "version": null,
             "created": null,
             "createdBy": "John Doe",
             "modified": null,
             "modifiedBy": null
           },
           "calculatedFields": [],
           "accesses": [],
           "schedules": [],
           "dynamicQuerySourceFields": [],
           "name": "FactInternetSales Date",
           "reportDataSource": [],
           "type": 0,
           "previewRecord": 10,
           "advancedMode": true,
           "allowNulls": false,
           "isDistinct": false,
           "categoryId": "93de93b9-d5d1-48f1-800d-1db1ffc02614",
           "categoryName": "Sales",
           "subCategoryId": null,
           "subCategoryName": null,
           "tenantId": null,
           "tenantName": null,
           "description": "",
           "title": "",
           "lastViewed": "2017-01-05T07:25:53.557",
           "owner": "John Doe",
           "ownerId": "9fc0f5c2-decf-4d65-9344-c59a1704ea0c",
           "excludedRelationships": "",
           "numberOfView": 7,
           "renderingTime": 1359.8571428571429,
           "createdById": "9fc0f5c2-decf-4d65-9344-c59a1704ea0c",
           "modifiedById": "9fc0f5c2-decf-4d65-9344-c59a1704ea0c",
           "snapToGrid": false,
           "usingFields": "78c99b13-af5d-47b9-9d2a-9fae8bc2b51c,80d98874-67fd-49f7-8755-497c0393736b",
           "hasDeletedObjects": false,
           "header": { "removed": "for brevity" },
           "footer": { "removed": "for brevity" },
           "titleDescription": { "removed": "for brevity" },
           "sourceId": null,
           "checked": false,
           "copyDashboard": false,
           "exportFormatSetting": { "removed": "for brevity" },
           "deletable": true,
           "editable": true,
           "movable": true,
           "copyable": true,
           "accessPriority": 1,
           "active": true,
           "id": "e09f9d45-b721-4012-b8e7-c31c58d52af3",
           "state": 0,
           "deleted": false,
           "inserted": true,
           "version": 6,
           "created": "2016-11-21T07:22:01",
           "createdBy": "John Doe",
           "modified": "2016-11-21T08:42:07.763",
           "modifiedBy": "John Doe"
         }


POST report
---------------------------------------

Saves a report.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object

**Response**

    An :doc:`models/OperationResult` object

**Samples**

   .. code-block:: http

      POST /api/report HTTP/1.1

   .. container:: toggle

      .. container:: header

         Sample payload:

      .. code-block:: json

         {
           	"reportKey" : {
           		"key" : "b95d2611-10c5-4808-aa68-9db2ccc719ff",
           		"modified" : null
           	},
           	"section" : 2,
           	"saveAs" : false,
           	"ignoreCheckChange" : false,
           	"report" : {
           		"name" : "Report01",
           		"type" : "Templates",
           		"previewRecord" : 10,
           		"advancedMode" : false,
           		"allowNulls" : false,
           		"isDistinct" : false,
           		"category" : {
           			"id" : null,
           			"name" : "",
           			"type" : "Templates"
           		},
           		"subCategory" : {
           			"id" : null,
           			"name" : "",
           			"type" : "Templates"
           		},
           		"reportDataSource" : [{
           				"aliasId" : "1a67e4e1-7b76-4aac-b905-027bb4302845_Categories",
           				"querySourceId" : "1a67e4e1-7b76-4aac-b905-027bb4302845",
           				"querySourceName" : "Categories",
           				"selected" : true,
           				"categoryId" : "00000000-0000-0000-0000-000000000000",
           				"primaryFields" : [{
           						"name" : "CategoryID",
           						"alias" : "",
           						"dataType" : "int",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "CategoryID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"querySource" : null,
           						"id" : "9fd3b009-4809-47ad-845b-96a9dc4cf71e",
           						"state" : 0,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"dateTimeNow" : "2016-06-10T07:29:35.9754058Z"
           					}
           				]
           			}
           		],
           		"reportRelationship" : [{
           				"id" : "1a67e4e1-7b76-4aac-b905-027bb4302845",
           				"category" : null,
           				"databaseName" : "Northwind",
           				"schemaName" : "dbo",
           				"dataObject" : "Categories",
           				"dataObjectType" : "Table",
           				"relationshipKeyJoins" : [],
           				"relationshipPosition" : 0,
           				"level" : 1
           			}
           		],
           		"reportFilter" : {
           			"status" : 0,
           			"logic" : "",
           			"visible" : false,
           			"filterFields" : [],
           			"id" : "19578f3d-ce47-4e94-a46b-2f7216e059b7",
           			"reportId" : "b95d2611-10c5-4808-aa68-9db2ccc719ff"
           		},
           		"reportPart" : [{
           				"isDirty" : true,
           				"reportPartContent" : {
           					"isDirty" : false,
           					"type" : 3,
           					"columns" : {
           						"elements" : [{
           								"isDirty" : false,
           								"name" : "CategoryName",
           								"properties" : {
           									"isDirty" : false,
           									"dataFormattings" : {
           										"function" : "",
           										"functionInfo" : {},
           										"format" : "Format",
           										"font" : {
           											"family" : "Georgia",
           											"size" : 8,
           											"bold" : true,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {},
           											"cellColor" : {}

           										},
           										"alternativeText" : {},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"width" : {
           											"value" : 0,
           											"unit" : "pixels"
           										},
           										"height" : 0,
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									}
           								},
           								"position" : 1,
           								"field" : {
           									"fieldId" : "0c140c5a-fa48-46f8-91ae-656a394c48ce",
           									"fieldName" : "CategoryName",
           									"fieldNameAlias" : "CategoryName",
           									"dataFieldType" : "Text",
           									"querySourceId" : "1a67e4e1-7b76-4aac-b905-027bb4302845",
           									"querySourceType" : "Table",
           									"sourceAlias" : "Categories",
           									"relationshipId" : null,
           									"visible" : true,
           									"calculatedTree" : null
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}
           						]
           					},
           					"rows" : {
           						"elements" : []
           					},
           					"values" : {
           						"elements" : []
           					},
           					"separators" : {
           						"elements" : []
           					},
           					"groups" : {
           						"elements" : []
           					},
           					"properties" : {
           						"isDirty" : false,
           						"generalInfo" : {
           							"gridStyle" : "Vertical",
           							"separatorStyle" : "Comma"
           						},
           						"table" : {
           							"border" : {
           								"top" : {},
           								"right" : {},
           								"bottom" : {},
           								"midVer" : {},
           								"left" : {},
           								"midHor" : {}

           							},
           							"backgroundColor" : "#efefef"
           						},
           						"columns" : {
           							"width" : {
           								"value" : 60,
           								"unit" : "Pixels"
           							},
           							"alterBackgroundColor" : false
           						},
           						"rows" : {
           							"height" : 15,
           							"alterBackgroundColor" : false
           						},
           						"headers" : {
           							"font" : {
           								"family" : "Georgia",
           								"size" : 12,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"backgroundColor" : "#dbf2ff"
           							},
           							"alignment" : "left",
           							"wordWrap" : true,
           							"removeHeaderForExport" : false
           						},
           						"grouping" : {
           							"useSeparator" : false
           						},
           						"view" : {
           							"dataRefreshInterval" : {
           								"enable" : false,
           								"updateInterval" : 0,
           								"isAll" : true,
           								"latestRecord" : 0
           							}
           						}
           					},
           					"settings" : {},
           					"title" : {
           						"text" : "title",
           						"properties" : {},
           						"settings" : {
           							"font" : {
           								"family" : "",
           								"size" : 0,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"color" : "",
           								"highlightColor" : ""
           							},
           							"alignment" : {
           								"alignment" : ""
           							}
           						},
           						"elements" : []
           					},
           					"description" : {
           						"text" : "desc",
           						"properties" : {},
           						"settings" : {
           							"font" : {
           								"family" : "",
           								"size" : 0,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"color" : "",
           								"highlightColor" : ""
           							},
           							"alignment" : {
           								"alignment" : ""
           							}
           						},
           						"elements" : []
           					}
           				},
           				"positionX" : 0,
           				"positionY" : 0,
           				"width" : 12,
           				"height" : 4,
           				"state" : 1,
           				"modified" : null,
           				"isBackSide" : true,
           				"title" : "Grid"
           			}
           		]
           	}
         }

   Successful response::

      {
         "success": true,
         "messages": null
     }


POST report/draft
---------------------------------------

Saves a report as draft.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object

**Response**

    An :doc:`models/OperationResult` object

**Samples**

   .. code-block:: http

      POST /api/report/draft HTTP/1.1

   .. container:: toggle

      .. container:: header

         Sample payload:

      .. code-block:: json

         {
           	"reportKey" : {
           		"key" : null,
           		"modified" : null
           	},
           	"saveAs" : false,
           	"report" : {
           		"name" : "TestReport",
           		"type" : "Reports",
           		"previewRecord" : 100,
           		"advancedMode" : true,
           		"allowNulls" : false,
           		"distinct" : false,
           		"category" : null,
           		"subCategory" : null,
           		"reportDataSource" : [{
           				"querySourceId" : "aff154e4-af1f-4b57-8e80-72400ca6deac",
           				"querySourceName" : "CustOrdersDetail",
           				"selected" : true,
           				"categoryId" : "00000000-0000-0000-0000-000000000000",
           				"primaryFields" : []
           			}
           		],
           		"reportRelationship" : [],
           		"reportFilter" : null
           	}
         }

   Sample response::

      {
         "success": true,
         "messages": null
      }


POST report/cancel
---------------------------------------

Deletes temporary data of a report.

**Request**

    Payload: a :doc:`models/ReportParameter` object

**Response**
    A :doc:`models/ReportSavingResult` object

**Samples**

   .. code-block:: http

      POST /api/report/cancel HTTP/1.1

   Request Payload::

      {
        	"reportKey" : {
        		"key" : "4fd37956-4b97-4efb-9d71-c750b0c36474"
        	}
      }

   Successful response::

      {
         "reportKey": {
             "key": null,
             "tenantId": null
         },
         "report": null
      }


POST report/loadReport
---------------------------------------

Returns a report's details.

.. warning::

   This API is obsolete, use `POST report/loadForEdit`_ instead.

POST report/loadForEdit
---------------------------------------

Returns an object with report key and report's details.

**Request**

    Payload: a :doc:`models/ReportParameter` object

**Response**

    A :doc:`models/ReportSavingParameter` object, with the **report** field fully populated

**Samples**

   .. code-block:: http

      POST /api/report/loadForEdit HTTP/1.1

   Payload::

      {"reportKey":{"key":"9d34d5d2-447f-465e-8223-d7f66378b5f9"}}

   .. container:: toggle

      .. container:: header

         Sample response:

      .. code-block:: json

         {
           	"saveAs" : false,
           	"report" : {
           		"category" : {
           			"name" : "",
           			"type" : 1,
           			"parentId" : null,
           			"tenantId" : null,
           			"subReportCategories" : null,
           			"id" : "00000000-0000-0000-0000-000000000000",
           			"state" : 0,
           			"modified" : null
           		},
           		"subCategory" : {
           			"name" : "",
           			"type" : 1,
           			"parentId" : null,
           			"tenantId" : null,
           			"subReportCategories" : null,
           			"id" : "00000000-0000-0000-0000-000000000000",
           			"state" : 0,
           			"modified" : null
           		},
           		"reportDataSource" : [{
           				"reportId" : "00000000-0000-0000-0000-000000000000",
           				"querySourceId" : "e1bc2021-3874-4e5a-b51e-d799cef5e29a",
           				"id" : "bc4cabe6-6f64-473d-b5c8-d3faf314e1fb",
           				"state" : 0,
           				"modified" : null
           			}
           		],
           		"reportRelationship" : [],
           		"reportPart" : [],
           		"reportFilter" : {
           			"filterFields" : [],
           			"logic" : "",
           			"visible" : false,
           			"reportId" : "00000000-0000-0000-0000-000000000000",
           			"id" : "e610c0a9-c074-47ec-a633-1195a589549c",
           			"state" : 0,
           			"modified" : null
           		},
           		"calculatedFields" : [],
           		"name" : "",
           		"type" : 1,
           		"previewRecord" : 10,
           		"advancedMode" : true,
           		"allowNulls" : false,
           		"isDistinct" : false,
           		"categoryId" : null,
           		"categoryName" : null,
           		"subCategoryId" : null,
           		"subCategoryName" : null,
           		"tenantId" : null,
           		"description" : null,
           		"createdBy" : null,
           		"createdDate" : "0001-01-01T00:00:00",
           		"modifiedBy" : null,
           		"version" : null,
           		"numberOfViews" : 0,
           		"averageRenderingTime" : 0.0,
           		"id" : "00000000-0000-0000-0000-000000000000",
           		"state" : 1,
           		"modified" : null
           	},
           	"section" : 0,
           	"tenantId" : null,
           	"ignoreCheckChange" : false,
           	"reportKey" : {
           		"key" : "9d34d5d2-447f-465e-8223-d7f66378b5f9",
           		"tenantId" : null
           	}
         }

GET report/(tenant\_id)
---------------------------------------

Returns all report categories together with the reports inside.

**Request**

    No payload

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      GET /api/report HTTP/1.1

   .. container:: toggle

      .. container:: header

         Sample response:

      .. code-block:: json

         [
           {
             "reports": [],
             "name": null,
             "type": 0,
             "parentId": null,
             "tenantId": null,
             "canDelete": false,
             "editable": false,
             "savable": false,
             "subCategories": [
               {
                 "reports": [
                   {
                     "name": "Example Report Name",
                     "type": 0,
                     "previewRecord": 0,
                     "advancedMode": false,
                     "allowNulls": false,
                     "isDistinct": false,
                     "categoryId": null,
                     "categoryName": null,
                     "subCategoryId": null,
                     "subCategoryName": null,
                     "tenantId": "00000000-0000-0000-0000-000000000000",
                     "tenantName": null,
                     "description": null,
                     "title": null,
                     "lastViewed": null,
                     "owner": null,
                     "ownerId": null,
                     "excludedRelationships": null,
                     "numberOfView": 0,
                     "renderingTime": 0,
                     "createdById": null,
                     "modifiedById": null,
                     "snapToGrid": false,
                     "usingFields": null,
                     "hasDeletedObjects": false,
                     "header": null,
                     "footer": null,
                     "titleDescription": null,
                     "exportFormatSetting": null,
                     "deletable": false,
                     "editable": false,
                     "movable": false,
                     "copyable": false,
                     "accessPriority": 0,
                     "active": false,
                     "id": "b166877f-bf1f-4adc-9dac-7575dd5e5183",
                     "state": 0,
                     "deleted": false,
                     "inserted": true,
                     "version": 0,
                     "created": null,
                     "createdBy": "9d2f1d51-0e3d-44db-bfc7-da94a7581bfe",
                     "modified": null,
                     "modifiedBy": null
                   }
                 ],
                 "name": null,
                 "type": 0,
                 "parentId": null,
                 "tenantId": null,
                 "canDelete": false,
                 "editable": false,
                 "savable": false,
                 "subCategories": [],
                 "id": null,
                 "state": 0,
                 "deleted": false,
                 "inserted": true,
                 "version": null,
                 "created": null,
                 "createdBy": "9d2f1d51-0e3d-44db-bfc7-da94a7581bfe",
                 "modified": null,
                 "modifiedBy": null
               }
             ],
             "id": null,
             "state": 0,
             "deleted": false,
             "inserted": true,
             "version": null,
             "created": null,
             "createdBy": "9d2f1d51-0e3d-44db-bfc7-da94a7581bfe",
             "modified": null,
             "modifiedBy": null
           }
         ]

POST report/validateReportName
---------------------------------------

Validates that a report name is unique and not empty.

**Request**

    Payload: a :doc:`models/ReportDefinition` object

**Response**

    A :doc:`models/OperationResult` object, with **success** field means whether the report name is unique (in the specified category and sub-category)

**Samples**

   .. code-block:: http

      POST /api/report/validateReportName HTTP/1.1

   Request payload::

      {
        	"name" : "AnExistingName",
        	"type" : "Templates",
        	"category" : {
        		"id" : "0adae39c-1db0-466d-820b-9f3f59c8e199"
        	},
        	"subCategory" : {
        		"id" : null
        	}
      }

   Response when that report name already exists in Uncategorized category::

      {
         "success": false,
         "messages": [{
             "key": "",
             "messages": ["This report name already exists in \"Uncategorized\" category."]
         }]
      }


POST report/validate
---------------------------------------

Validates that a report name is unique and its filter and relationships are valid.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object

**Response**

    An :doc:`models/OperationResult` object, with **success** field means whether the validation is successful

**Samples**

   .. code-block:: http

      POST /api/report/validate HTTP/1.1

   Request payload::

      {
        	"reportKey" : {
        		"key" : "940529fd-f1fb-4d98-8def-c8dcfa7eba84",
        		"tenantId" : null
        	},
        	"report" : {
        		"id" : "940529fd-f1fb-4d98-8def-c8dcfa7eba84",
        		"type" : "Templates",
        		"category" : {
        			"id" : "0adae39c-1db0-466d-820b-9f3f59c8e199"
        		},
        		"subCategory" : {
        			"id" : null
        		}
        	}
      }

   Sample response::

      {
         "success": true,
         "message": null,
         "errors": []
     }


POST report/search
---------------------------------------

Searches for reports.

**Request**

    Payload: a :doc:`models/ReportDashboardSearchCriteria` object

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      POST api/report/search HTTP/1.1

   Request payload::

      {
        	"criterias" : [{
        			"key" : "All",
        			"value" : "fil"
        		}
        	],
        	"isUncategorized" : false,
        	"sortCriteria" : {
        		"key" : "ReportName",
        		"descending" : false
        	},
        	"tenantId" : null,
        	"type" : "0"
     }

   .. container:: toggle

      .. container:: header

         Sample response:

      .. code-block:: json

         [{
        		"reports" : [],
        		"name" : "0",
        		"type" : 0,
        		"parentId" : null,
        		"tenantId" : null,
        		"canDelete" : false,
        		"savable" : false,
        		"subCategories" : [{
        				"reports" : [{
        						"name" : "filter",
        						"type" : 0,
        						"previewRecord" : 0,
        						"advancedMode" : false,
        						"allowNulls" : false,
        						"isDistinct" : false,
        						"categoryId" : "8da86160-ab16-4f4b-a439-729c8b82b1c6",
        						"categoryName" : null,
        						"subCategoryId" : "6dee7a46-cfab-477a-a952-be4471eab1a0",
        						"subCategoryName" : null,
        						"tenantId" : "00000000-0000-0000-0000-000000000000",
        						"tenantName" : null,
        						"description" : "",
        						"title" : null,
        						"lastViewed" : null,
        						"owner" : null,
        						"ownerId" : null,
        						"headerContent" : null,
        						"footerContent" : null,
        						"excludedRelationships" : null,
        						"numberOfView" : 0,
        						"renderingTime" : 0,
        						"createdById" : null,
        						"modifiedById" : null,
        						"excludedRelationshipIds" : [],
        						"header" : null,
        						"footer" : null,
        						"titleDescriptionContent" : null,
        						"titleDescription" : null,
        						"id" : "df3c8552-1505-4905-9d1d-9574ac1b92de",
        						"state" : 0,
        						"inserted" : true,
        						"version" : 2,
        						"created" : "2016-09-16T08:07:48.1630000-07:00",
        						"createdBy" : null,
        						"modified" : "2016-09-16T08:08:36.2430000-07:00",
        						"modifiedBy" : null
        					}, {
        						"name" : "filter1",
        						"type" : 0,
        						"previewRecord" : 0,
        						"advancedMode" : false,
        						"allowNulls" : false,
        						"isDistinct" : false,
        						"categoryId" : "8da86160-ab16-4f4b-a439-729c8b82b1c6",
        						"categoryName" : null,
        						"subCategoryId" : "6dee7a46-cfab-477a-a952-be4471eab1a0",
        						"subCategoryName" : null,
        						"tenantId" : "00000000-0000-0000-0000-000000000000",
        						"tenantName" : null,
        						"description" : "",
        						"title" : null,
        						"lastViewed" : null,
        						"owner" : null,
        						"ownerId" : null,
        						"headerContent" : null,
        						"footerContent" : null,
        						"excludedRelationships" : null,
        						"numberOfView" : 0,
        						"renderingTime" : 0,
        						"createdById" : null,
        						"modifiedById" : null,
        						"excludedRelationshipIds" : [],
        						"header" : null,
        						"footer" : null,
        						"titleDescriptionContent" : null,
        						"titleDescription" : null,
        						"id" : "c7b52014-ca40-4aad-9a8c-07887743aec4",
        						"state" : 0,
        						"inserted" : true,
        						"version" : 1,
        						"created" : "2016-09-16T08:09:48.3830000-07:00",
        						"createdBy" : null,
        						"modified" : "2016-09-16T08:09:48.3830000-07:00",
        						"modifiedBy" : null
        					}
        				],
        				"name" : "0",
        				"type" : 0,
        				"parentId" : null,
        				"tenantId" : null,
        				"canDelete" : false,
        				"savable" : false,
        				"subCategories" : [],
        				"status" : 2,
        				"id" : "6dee7a46-cfab-477a-a952-be4471eab1a0",
        				"state" : 0,
        				"inserted" : true,
        				"version" : null,
        				"created" : null,
        				"createdBy" : null,
        				"modified" : null,
        				"modifiedBy" : null
        			}
        		],
        		"status" : 2,
        		"id" : "8da86160-ab16-4f4b-a439-729c8b82b1c6",
        		"state" : 0,
        		"inserted" : true,
        		"version" : null,
        		"created" : null,
        		"createdBy" : null,
        		"modified" : null,
        		"modifiedBy" : null
         }]

POST report/advancedSearch
---------------------------------------

Searches for reports with advanced options.

**Request**

    Payload: a :doc:`models/ReportPagedRequest` object

**Response**

    A :doc:`models/PagedResult` object, with **result** field containing an array of :doc:`models/ReportDefinition` objects

**Samples**

   .. code-block:: http

      POST /api/report/advancedSearch HTTP/1.1

   Request payload::

      {
        	"subcategoryid" : null,
        	"categoryId" : null,
        	"tenantId" : null,
        	"pageSize" : 10,
        	"pageIndex" : 1,
        	"sortOrders" : [{
        			"key" : "reportname",
        			"descending" : true
        		}
        	],
        	"criteria" : [{
        			"key" : "reportName",
        			"value" : "test",
        			"operation" : 1
        		}
        	]
      }

   Sample response:

   .. code-block:: json
      :emphasize-lines: 2

      {
         "result" : [],
         "pageIndex" : 1,
         "pageSize" : 10,
         "total" : 0
      }

.. _GET_report/reportMode:

GET report/reportMode
---------------------------------------

Returns the report mode.

**Request**

    No payload

**Response**

    The value of the report mode

    - 0 = Simple
    - 1 = Advanced

**Samples**

   .. code-block:: http

      GET /api/report/reportMode HTTP/1.1

   Sample response::

      1

.. _POST_report/reportMode/{value}:

POST report/reportMode/{value}
---------------------------------------

Sets the report mode to Simple or Advanced.

**Request**

    * call report/reportMode/0 to set Simple mode
    * call report/reportMode/1 to set Advanced mode

**Response**

    * true if succeeded
    * false if there was an error

**Samples**

   .. code-block:: http

      POST /api/report/reportMode/0 HTTP/1.1

   Successful response::

      true


POST report/detectReportChange
---------------------------------------

Verifies that all report details are up to date, without physical changes, and valid.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object, with **section** field specifies where to detect changes

     * 0 = All
     * 1 = DataSource
     * 2 = Fields
     * 3 = CalculatedField

**Response**

    An :doc:`models/OperationResult` object, with **success** field means whether the report is up to date, without physical changes, and valid

**Samples**

   .. code-block:: http

      POST /api/report/detectReportChange HTTP/1.1

   Request Payload to check if a new report with only one data source has physical changes:

   .. code-block:: json
      :emphasize-lines: 6

      {
        	"reportKey" : {
        		"key" : null,
        		"modified" : null
        	},
        	"section" : 1,
        	"report" : {
        		"reportDataSource" : [{
        				"querySourceId" : "1a67e4e1-7b76-4aac-b905-027bb4302845"
        			}
        		]
        	}
      }

   Successful response::

      {
         "success": true,
         "messages": null
      }

   .. container:: toggle

      .. container:: header

         Request Payload for an existing report with two data sources, filter and report part:

      .. code-block:: json

         {
           	"reportKey" : {
           		"key" : "37e99389-fa8a-4f9f-9d03-f6362240c931",
           		"modified" : null
           	},
           	"section" : 2,
           	"report" : {
           		"reportDataSource" : [{
           				"aliasId" : "84340ae7-275e-4bd5-bd77-89916341f20e_Order Details",
           				"querySourceId" : "84340ae7-275e-4bd5-bd77-89916341f20e",
           				"querySourceName" : "Order Details",
           				"selected" : true,
           				"categoryId" : "00000000-0000-0000-0000-000000000000",
           				"primaryFields" : [{
           						"name" : "OrderID",
           						"alias" : "",
           						"dataType" : "int",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "OrderID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"querySource" : null,
           						"id" : "9a4c52a4-f931-40d0-88b9-7f914d49581b",
           						"state" : 0,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"dateTimeNow" : "2016-06-13T07:22:35.8918127Z"
           					}, {
           						"name" : "ProductID",
           						"alias" : "",
           						"dataType" : "int",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "ProductID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"querySource" : null,
           						"id" : "1d379f29-02ae-4f51-ac3a-a627694c3539",
           						"state" : 0,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"dateTimeNow" : "2016-06-13T07:22:35.8918127Z"
           					}
           				]
           			}, {
           				"aliasId" : "8fda0166-5f38-4ca1-ae20-9b6cab288f9d_Orders",
           				"querySourceId" : "8fda0166-5f38-4ca1-ae20-9b6cab288f9d",
           				"querySourceName" : "Orders",
           				"selected" : true,
           				"categoryId" : "00000000-0000-0000-0000-000000000000",
           				"primaryFields" : [{
           						"name" : "orderid_alias",
           						"alias" : "",
           						"dataType" : "int",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "OrderID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"querySource" : null,
           						"id" : "93157476-d4e6-49bb-8900-2fda43e46f87",
           						"state" : 0,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"dateTimeNow" : "2016-06-13T07:22:35.8918127Z"
           					}
           				]
           			}
           		],
           		"reportRelationship" : [{
           				"tempId" : "6da998ae-5451-4a45-ab86-69894e1b3a13",
           				"joinConnectionId" : "a0028b41-f820-4640-927c-68f6ef730b0f",
           				"foreignConnectionId" : "a0028b41-f820-4640-927c-68f6ef730b0f",
           				"joinQuerySourceId" : "84340ae7-275e-4bd5-bd77-89916341f20e",
           				"joinQuerySourceName" : "Order Details",
           				"joinDataSourceCategoryName" : "",
           				"joinDataSourceCategoryId" : "00000000-0000-0000-0000-000000000000",
           				"foreignDataSourceCategoryName" : "",
           				"foreignDataSourceCategoryId" : "00000000-0000-0000-0000-000000000000",
           				"foreignQuerySourceId" : "8fda0166-5f38-4ca1-ae20-9b6cab288f9d",
           				"foreignQuerySourceName" : "Orders",
           				"joinFieldId" : "9a4c52a4-f931-40d0-88b9-7f914d49581b",
           				"joinFieldName" : "OrderID",
           				"foreignFieldId" : "93157476-d4e6-49bb-8900-2fda43e46f87",
           				"foreignFieldName" : "orderid_alias",
           				"alias" : "",
           				"systemRelationship" : true,
           				"joinType" : "Inner",
           				"parentRelationshipId" : "5147885d-0bac-4252-8d33-f9fd96bd3b8e",
           				"position" : null,
           				"relationshipKeyJoins" : [],
           				"reportId" : "00000000-0000-0000-0000-000000000000",
           				"selectedForeignAlias" : "8fda0166-5f38-4ca1-ae20-9b6cab288f9d_Orders",
           				"id" : "6da998ae-5451-4a45-ab86-69894e1b3a13",
           				"state" : 1,
           				"validationKey" : "5147885d-0bac-4252-8d33-f9fd96bd3b8e",
           				"relationshipPosition" : 0,
           				"invalidAlias" : null,
           				"hidden" : false,
           				"level" : 1
           			}
           		],
           		"reportFilter" : {
           			"status" : 0,
           			"logic" : null,
           			"visible" : false,
           			"filterFields" : [{
           					"connectionName" : "Northwind",
           					"querySourceCategoryName" : "dbo",
           					"sourceFieldName" : "ShipCountry",
           					"sourceFieldVisible" : true,
           					"sourceFieldFilterable" : true,
           					"sourceDataObjectName" : "Orders",
           					"dataType" : "Text",
           					"filterId" : "00000000-0000-0000-0000-000000000000",
           					"querySourceFieldId" : "d0f88020-8d3f-4f80-a1ac-0c187f87dfd3",
           					"querySourceType" : "Table",
           					"querySourceId" : "8fda0166-5f38-4ca1-ae20-9b6cab288f9d",
           					"relationshipId" : null,
           					"alias" : "ShipCountry",
           					"position" : 1,
           					"visible" : false,
           					"required" : false,
           					"cascading" : true,
           					"operatorId" : "737307d1-1e5f-407f-889f-1b3c9a66dd6f",
           					"operatorSetting" : null,
           					"value" : "'US'",
           					"sortType" : "Unsorted",
           					"fontFamily" : null,
           					"fontSize" : 0,
           					"textColor" : null,
           					"backgroundColor" : null,
           					"fontBold" : false,
           					"fontItalic" : false,
           					"fontUnderline" : false,
           					"id" : "00000000-0000-0000-0000-000000000000",
           					"status" : 0,
           					"modified" : null,
           					"dateTimeNow" : "2016-06-13T07:23:25.9138114Z",
           					"isParameter" : false,
           					"sourceDataObjectFullName" : "Northwind.dbo.Orders",
           					"selected" : false
           				}
           			],
           			"id" : "00000000-0000-0000-0000-000000000000",
           			"reportId" : "37e99389-fa8a-4f9f-9d03-f6362240c931"
           		},
           		"reportPart" : [{
           				"isDirty" : true,
           				"reportPartContent" : {
           					"isDirty" : false,
           					"type" : 3,
           					"columns" : {
           						"elements" : [{
           								"isDirty" : false,
           								"name" : "ShipCity",
           								"properties" : {
           									"isDirty" : false,
           									"dataFormattings" : {
           										"function" : "",
           										"functionInfo" : {},
           										"format" : "Format",
           										"font" : {
           											"family" : "Georgia",
           											"size" : 8,
           											"bold" : true,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {},
           											"cellColor" : {}

           										},
           										"alternativeText" : {},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"width" : {
           											"value" : 0,
           											"unit" : "pixels"
           										},
           										"height" : 0,
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}

           								},
           								"position" : 1,
           								"field" : {
           									"fieldId" : "f5b9bac6-aa76-402c-8ade-6b8f619e9ced",
           									"fieldName" : "ShipCity",
           									"fieldNameAlias" : "ShipCity",
           									"dataFieldType" : "Text",
           									"querySourceId" : "8fda0166-5f38-4ca1-ae20-9b6cab288f9d",
           									"querySourceType" : "Table",
           									"sourceAlias" : "Orders",
           									"relationshipId" : null,
           									"visible" : true,
           									"calculatedTree" : null
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}, {
           								"isDirty" : false,
           								"name" : "ProductID",
           								"properties" : {
           									"isDirty" : false,
           									"dataFormattings" : {
           										"function" : "",
           										"functionInfo" : {},
           										"format" : "Format",
           										"font" : {
           											"family" : "Georgia",
           											"size" : 8,
           											"bold" : true,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {},
           											"cellColor" : {}

           										},
           										"alternativeText" : {},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"width" : {
           											"value" : 0,
           											"unit" : "pixels"
           										},
           										"height" : 0,
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}

           								},
           								"position" : 2,
           								"field" : {
           									"fieldId" : "1d379f29-02ae-4f51-ac3a-a627694c3539",
           									"fieldName" : "ProductID",
           									"fieldNameAlias" : "ProductID",
           									"dataFieldType" : "Numeric",
           									"querySourceId" : "84340ae7-275e-4bd5-bd77-89916341f20e",
           									"querySourceType" : "Table",
           									"sourceAlias" : "Order Details",
           									"relationshipId" : null,
           									"visible" : true,
           									"calculatedTree" : null
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}, {
           								"isDirty" : false,
           								"name" : "Quantity",
           								"properties" : {
           									"isDirty" : false,
           									"dataFormattings" : {
           										"function" : "",
           										"functionInfo" : {},
           										"format" : "Format",
           										"font" : {
           											"family" : "Georgia",
           											"size" : 8,
           											"bold" : true,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {},
           											"cellColor" : {}

           										},
           										"alternativeText" : {},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"width" : {
           											"value" : 0,
           											"unit" : "pixels"
           										},
           										"height" : 0,
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}

           								},
           								"position" : 3,
           								"field" : {
           									"fieldId" : "1eaa3d97-da56-45ca-b61a-8bf3bb253fea",
           									"fieldName" : "Quantity",
           									"fieldNameAlias" : "Quantity",
           									"dataFieldType" : "Numeric",
           									"querySourceId" : "84340ae7-275e-4bd5-bd77-89916341f20e",
           									"querySourceType" : "Table",
           									"sourceAlias" : "Order Details",
           									"relationshipId" : null,
           									"visible" : true,
           									"calculatedTree" : null
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}
           						]
           					},
           					"rows" : {
           						"elements" : []
           					},
           					"values" : {
           						"elements" : []
           					},
           					"separators" : {
           						"elements" : []
           					},
           					"properties" : {
           						"isDirty" : false,
           						"generalInfo" : {
           							"gridStyle" : "Vertical",
           							"separatorStyle" : "Comma"
           						},
           						"table" : {
           							"border" : {
           								"top" : {},
           								"right" : {},
           								"bottom" : {},
           								"midVer" : {},
           								"left" : {},
           								"midHor" : {}

           							},
           							"backgroundColor" : "#efefef"
           						},
           						"columns" : {
           							"width" : {
           								"value" : 100,
           								"unit" : "Pixels"
           							},
           							"alterBackgroundColor" : false
           						},
           						"rows" : {
           							"height" : 40,
           							"alterBackgroundColor" : false
           						},
           						"headers" : {
           							"font" : {
           								"family" : "Georgia",
           								"size" : 12,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"backgroundColor" : "#dbf2ff"
           							},
           							"alignment" : "left",
           							"wordWrap" : true,
           							"removeHeaderForExport" : false
           						},
           						"grouping" : {
           							"useSeparator" : false
           						},
           						"view" : {
           							"dataRefreshInterval" : {
           								"enable" : false,
           								"updateInterval" : 0,
           								"isAll" : true,
           								"latestRecord" : 0
           							}
           						}
           					},
           					"settings" : {},
           					"title" : {
           						"text" : "Title",
           						"properties" : {},
           						"settings" : {
           							"font" : {
           								"family" : "",
           								"size" : 0,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"color" : "",
           								"highlightColor" : ""
           							},
           							"alignment" : {
           								"alignment" : ""
           							}
           						},
           						"elements" : []
           					},
           					"description" : {
           						"text" : "Description line",
           						"properties" : {},
           						"settings" : {
           							"font" : {
           								"family" : "",
           								"size" : 0,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"color" : "",
           								"highlightColor" : ""
           							},
           							"alignment" : {
           								"alignment" : ""
           							}
           						},
           						"elements" : []
           					}
           				},
           				"positionX" : 0,
           				"positionY" : 0,
           				"width" : 12,
           				"height" : 4,
           				"state" : 1,
           				"modified" : null,
           				"isBackSide" : true,
           				"expandedLevel" : 0,
           				"points" : [{
           						"key" : "All",
           						"filter" : [{
           								"key" : "",
           								"value" : ""
           							}
           						],
           						"expandedLevel" : 0,
           						"isViewSeparator" : false
           					}
           				],
           				"title" : "Grid"
           			}
           		]
           	}
         }


GET report/function/{function\_mode}/{data\_type}/(tenant\_id)
---------------------------------------------------------------

Returns a list of report functions filtered by mode (field, sub-total, grand-total), by data type and by tenant_id if provided.

**Request**

    No payload

**Response**

    A :doc:`models/ReportFunction` object

**Samples**

   To be updated

GET report/allReports/(tenant\_id)
---------------------------------------

Returns a list of all reports filtered by tenant_id if provided.

**Request**

    No payload

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   To be updated

POST report/detectSchemaChange
---------------------------------------

Verifies that all report filter fields are without changes.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object

**Response**

   .. list-table::
      :header-rows: 1


      *  -  Field
         -  Required
         -  Description
         -  Note
      *  -  | **hasChanged**
            | boolean
         -  R
         -  * true if there is no change
            * false if there is any change
         -
      *  -  | **filterFields**
            | array of objects
         -	R
         -  An array of :doc:`models/ReportFilterField` objects
         -

**Samples**

   .. code-block:: http

      POST /api/report/detectSchemaChange HTTP/1.1

   .. container:: toggle

      .. container:: header

         Sample payload:

      .. code-block:: json

         {
           	"reportKey" : {
           		"key" : "d797d877-6ae1-443a-b5f4-e9fbaeb884a8",
           		"modified" : null
           	},
           	"section" : 2,
           	"saveAs" : false,
           	"ignoreCheckChange" : true,
           	"report" : {
           		"name" : "don't know 2",
           		"type" : 0,
           		"previewRecord" : 10,
           		"advancedMode" : true,
           		"allowNulls" : false,
           		"isDistinct" : false,
           		"category" : {
           			"id" : "97bbeef3-0a80-4a0e-9640-962af1f3f1dc",
           			"name" : "",
           			"type" : 0
           		},
           		"subCategory" : {
           			"id" : null,
           			"name" : "",
           			"type" : 0
           		},
           		"reportDataSource" : [{
           				"aliasId" : "d4ed8e8e-3cc1-4815-a5c9-30602847345b_order_details",
           				"querySourceId" : "d4ed8e8e-3cc1-4815-a5c9-30602847345b",
           				"querySourceName" : "order_details",
           				"selected" : true,
           				"categoryId" : "3c88fe79-4284-4abe-8b25-5cff7b132474",
           				"primaryFields" : [{
           						"name" : "OrderID",
           						"alias" : "",
           						"dataType" : "smallint",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "OrderID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"hasAggregatedFunction" : false,
           						"querySource" : null,
           						"fullPath" : null,
           						"id" : "b5b95958-24f9-40a1-b95b-5e22c2d658d0",
           						"state" : 0,
           						"inserted" : true,
           						"version" : null,
           						"created" : null,
           						"createdBy" : null,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"modifiedBy" : null
           					}, {
           						"name" : "ProductID",
           						"alias" : "",
           						"dataType" : "smallint",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "ProductID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"hasAggregatedFunction" : false,
           						"querySource" : null,
           						"fullPath" : null,
           						"id" : "9c9f3f50-e223-41f4-adfe-0ff407e3bd4c",
           						"state" : 0,
           						"inserted" : true,
           						"version" : null,
           						"created" : null,
           						"createdBy" : null,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"modifiedBy" : null
           					}
           				]
           			}, {
           				"aliasId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc_orders",
           				"querySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           				"querySourceName" : "orders",
           				"selected" : true,
           				"categoryId" : "3c88fe79-4284-4abe-8b25-5cff7b132474",
           				"primaryFields" : [{
           						"name" : "OrderID",
           						"alias" : "",
           						"dataType" : "smallint",
           						"izendaDataType" : "Numeric",
           						"allowDistinct" : false,
           						"visible" : true,
           						"filterable" : true,
           						"deleted" : false,
           						"querySourceId" : "00000000-0000-0000-0000-000000000000",
           						"parentId" : null,
           						"expressionFields" : [],
           						"filteredValue" : "",
           						"type" : 0,
           						"groupPosition" : 0,
           						"position" : 0,
           						"extendedProperties" : "{\"PrimaryKey\":true}",
           						"physicalChange" : 0,
           						"approval" : 0,
           						"existed" : false,
           						"matchedTenant" : false,
           						"functionName" : null,
           						"expression" : null,
           						"fullName" : null,
           						"calculatedTree" : null,
           						"reportId" : null,
           						"originalName" : "OrderID",
           						"isParameter" : false,
           						"isCalculated" : false,
           						"hasAggregatedFunction" : false,
           						"querySource" : null,
           						"fullPath" : null,
           						"id" : "0f19cd74-e4e3-4ada-acea-03f9c98a8e3b",
           						"state" : 0,
           						"inserted" : true,
           						"version" : null,
           						"created" : null,
           						"createdBy" : null,
           						"modified" : "0001-01-01T00:00:00.0000000-08:00",
           						"modifiedBy" : null
           					}
           				]
           			}
           		],
           		"reportRelationship" : [{
           				"tempId" : "73880663-9fe1-4e70-a02b-ea4471978a73",
           				"joinConnectionId" : "b513ddd4-ef23-4dbb-901d-2be802896616",
           				"foreignConnectionId" : "b513ddd4-ef23-4dbb-901d-2be802896616",
           				"joinQuerySourceId" : "d4ed8e8e-3cc1-4815-a5c9-30602847345b",
           				"joinQuerySourceName" : "order_details",
           				"joinDataSourceCategoryName" : "postgres",
           				"joinDataSourceCategoryId" : "3c88fe79-4284-4abe-8b25-5cff7b132474",
           				"foreignDataSourceCategoryName" : "postgres",
           				"foreignDataSourceCategoryId" : "3c88fe79-4284-4abe-8b25-5cff7b132474",
           				"foreignQuerySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           				"foreignQuerySourceName" : "orders",
           				"joinFieldId" : "b5b95958-24f9-40a1-b95b-5e22c2d658d0",
           				"joinFieldName" : "OrderID",
           				"foreignFieldId" : "0f19cd74-e4e3-4ada-acea-03f9c98a8e3b",
           				"foreignFieldName" : "OrderID",
           				"alias" : "",
           				"aliasTempId" : "alias_709",
           				"systemRelationship" : false,
           				"joinType" : "Inner",
           				"parentRelationshipId" : "aa176ab4-15cb-451f-8f6a-a46272cf0e15",
           				"position" : null,
           				"relationshipKeyJoins" : [],
           				"reportId" : "d797d877-6ae1-443a-b5f4-e9fbaeb884a8",
           				"selectedForeignAlias" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc_orders",
           				"id" : "73880663-9fe1-4e70-a02b-ea4471978a73",
           				"state" : 0,
           				"validationKey" : "73880663-9fe1-4e70-a02b-ea4471978a73",
           				"relationshipPosition" : 0,
           				"needAlias" : false,
           				"previousAlias" : "",
           				"invalidAlias" : null,
           				"hidden" : false,
           				"level" : 1
           			}
           		],
           		"reportFilter" : {
           			"logic" : "",
           			"visible" : true,
           			"filterFields" : [{
           					"connectionName" : "postgres",
           					"querySourceCategoryName" : "public",
           					"sourceFieldName" : "Freight",
           					"sourceFieldVisible" : true,
           					"sourceFieldFilterable" : true,
           					"sourceDataObjectName" : "orders",
           					"dataType" : "Numeric",
           					"filterId" : "8c572300-61ad-47ef-8496-94686f7f1301",
           					"querySourceFieldId" : "46875d94-d79e-4b39-a863-aaede78e176b",
           					"querySourceType" : "Table",
           					"querySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           					"relationshipId" : null,
           					"alias" : "Freight",
           					"position" : 1,
           					"visible" : false,
           					"required" : false,
           					"cascading" : true,
           					"operatorId" : null,
           					"operatorSetting" : null,
           					"value" : null,
           					"sortType" : "Unsorted",
           					"fontFamily" : "Roboto",
           					"fontSize" : 8,
           					"textColor" : null,
           					"backgroundColor" : null,
           					"fontBold" : false,
           					"fontItalic" : false,
           					"fontUnderline" : false,
           					"id" : "6f31e0cb-1cdf-416a-a5c5-8a89236903e3",
           					"state" : 0,
           					"modified" : null,
           					"dateTimeNow" : "",
           					"isParameter" : false,
           					"sourceDataObjectFullName" : "postgres.public.orders",
           					"selected" : false,
           					"dataFormatId" : null
           				}
           			],
           			"id" : "8c572300-61ad-47ef-8496-94686f7f1301",
           			"reportId" : "d797d877-6ae1-443a-b5f4-e9fbaeb884a8"
           		},
           		"reportPart" : [{
           				"isDirty" : false,
           				"reportPartContent" : {
           					"isDirty" : false,
           					"type" : 3,
           					"columns" : {
           						"text" : null,
           						"properties" : {
           							"addSideTotal" : false,
           							"useExpanders" : false
           						},
           						"settings" : {},
           						"elements" : [{
           								"reportPartContent" : null,
           								"isDirty" : false,
           								"name" : "Group (ShipCountry)",
           								"properties" : {
           									"isDirty" : true,
           									"fieldItemVisible" : true,
           									"dataFormattings" : {
           										"function" : "7f942ac7-08d8-41fa-9e89-bad96f07f102",
           										"functionInfo" : {
           											"id" : "7f942ac7-08d8-41fa-9e89-bad96f07f102",
           											"name" : "Group",
           											"expression" : null,
           											"dataType" : "Text",
           											"formatDataType" : "Text",
           											"syntax" : null,
           											"expressionSyntax" : null,
           											"isOperator" : false,
           											"extendedProperties" : {}
           										},
           										"format" : {},
           										"font" : {
           											"family" : "Roboto",
           											"size" : 14,
           											"bold" : false,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											},
           											"cellColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											}
           										},
           										"alternativeText" : {
           											"rangePercent" : null,
           											"rangeValue" : null,
           											"value" : null
           										},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}
           								},
           								"position" : 1,
           								"field" : {
           									"fieldId" : "8e65a222-66f1-470b-9f53-7f6481110d5e",
           									"fieldName" : "ShipCountry",
           									"fieldNameAlias" : "Group (ShipCountry)",
           									"dataFieldType" : "Text",
           									"querySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           									"querySourceType" : "Table",
           									"sourceAlias" : "orders",
           									"relationshipId" : "00000000-0000-0000-0000-000000000000",
           									"visible" : true,
           									"calculatedTree" : null,
           									"isCalculated" : false
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}
           						],
           						"name" : "columns"
           					},
           					"rows" : {
           						"text" : null,
           						"properties" : {
           							"useExpanders" : false
           						},
           						"settings" : {},
           						"elements" : [{
           								"reportPartContent" : null,
           								"isDirty" : false,
           								"name" : "ShipCity",
           								"properties" : {
           									"isDirty" : true,
           									"fieldItemVisible" : true,
           									"dataFormattings" : {
           										"function" : "",
           										"functionInfo" : {},
           										"format" : {},
           										"font" : {
           											"family" : "Roboto",
           											"size" : 14,
           											"bold" : false,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											},
           											"cellColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											}
           										},
           										"alternativeText" : {
           											"rangePercent" : null,
           											"rangeValue" : null,
           											"value" : null
           										},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}
           								},
           								"position" : 1,
           								"field" : {
           									"fieldId" : "63cb0d69-1451-4c51-b825-6946975e58c5",
           									"fieldName" : "ShipCity",
           									"fieldNameAlias" : "ShipCity",
           									"dataFieldType" : "Text",
           									"querySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           									"querySourceType" : "Table",
           									"sourceAlias" : "orders",
           									"relationshipId" : "00000000-0000-0000-0000-000000000000",
           									"visible" : true,
           									"calculatedTree" : null,
           									"isCalculated" : false
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}, {
           								"reportPartContent" : null,
           								"isDirty" : false,
           								"name" : "ShipName",
           								"properties" : {
           									"isDirty" : true,
           									"fieldItemVisible" : true,
           									"dataFormattings" : {
           										"function" : "",
           										"functionInfo" : {},
           										"format" : {},
           										"font" : {
           											"family" : "Roboto",
           											"size" : 14,
           											"bold" : false,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											},
           											"cellColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											}
           										},
           										"alternativeText" : {
           											"rangePercent" : null,
           											"rangeValue" : null,
           											"value" : null
           										},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}
           								},
           								"position" : 2,
           								"field" : {
           									"fieldId" : "a28f4649-bf0a-4d2f-ac4b-3ed9e2a4c2a5",
           									"fieldName" : "ShipName",
           									"fieldNameAlias" : "ShipName",
           									"dataFieldType" : "Text",
           									"querySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           									"querySourceType" : "Table",
           									"sourceAlias" : "orders",
           									"relationshipId" : "00000000-0000-0000-0000-000000000000",
           									"visible" : true,
           									"calculatedTree" : null,
           									"isCalculated" : false
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}
           						],
           						"name" : "rows"
           					},
           					"values" : {
           						"text" : null,
           						"properties" : {},
           						"settings" : {},
           						"elements" : [{
           								"reportPartContent" : null,
           								"isDirty" : false,
           								"name" : "Sum (OrderID)",
           								"properties" : {
           									"isDirty" : true,
           									"fieldItemVisible" : true,
           									"dataFormattings" : {
           										"function" : "902a9168-fc01-4a35-92fb-ea67942d099d",
           										"functionInfo" : {
           											"id" : "902a9168-fc01-4a35-92fb-ea67942d099d",
           											"name" : "Sum",
           											"expression" : null,
           											"dataType" : "Numeric",
           											"formatDataType" : "Numeric",
           											"syntax" : null,
           											"expressionSyntax" : null,
           											"isOperator" : false,
           											"extendedProperties" : {}
           										},
           										"format" : {},
           										"font" : {
           											"family" : "Roboto",
           											"size" : 14,
           											"bold" : false,
           											"italic" : false,
           											"underline" : false,
           											"color" : "",
           											"backgroundColor" : ""
           										},
           										"alignment" : "alignLeft",
           										"sort" : "",
           										"color" : {
           											"textColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											},
           											"cellColor" : {
           												"rangePercent" : null,
           												"rangeValue" : null,
           												"value" : null
           											}
           										},
           										"alternativeText" : {
           											"rangePercent" : null,
           											"rangeValue" : null,
           											"value" : null
           										},
           										"customURL" : {
           											"url" : "",
           											"option" : "Open link in New Window"
           										},
           										"embeddedJavascript" : {
           											"script" : ""
           										},
           										"subTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										},
           										"grandTotal" : {
           											"label" : "",
           											"function" : "",
           											"expression" : "",
           											"dataType" : "",
           											"previewResult" : ""
           										}
           									},
           									"headerFormating" : {
           										"font" : {
           											"family" : null,
           											"size" : null,
           											"bold" : null,
           											"italic" : null,
           											"underline" : null,
           											"color" : null,
           											"backgroundColor" : null
           										},
           										"alignment" : null,
           										"wordWrap" : null,
           										"columnGroup" : ""
           									},
           									"drillDown" : {
           										"subReport" : {
           											"selectedReport" : null,
           											"style" : null,
           											"reportPartUsed" : null,
           											"reportFilter" : true,
           											"mappingFields" : []
           										}
           									},
           									"otherProps" : {}
           								},
           								"position" : 1,
           								"field" : {
           									"fieldId" : "0f19cd74-e4e3-4ada-acea-03f9c98a8e3b",
           									"fieldName" : "OrderID",
           									"fieldNameAlias" : "Sum (OrderID)",
           									"dataFieldType" : "Numeric",
           									"querySourceId" : "b5f20d85-1a96-493a-8b1e-15dc9b1f26bc",
           									"querySourceType" : "Table",
           									"sourceAlias" : "orders",
           									"relationshipId" : "00000000-0000-0000-0000-000000000000",
           									"visible" : true,
           									"calculatedTree" : null,
           									"isCalculated" : false
           								},
           								"isDeleted" : false,
           								"isSelected" : false
           							}
           						],
           						"name" : "values"
           					},
           					"separators" : {
           						"text" : null,
           						"properties" : {},
           						"settings" : {},
           						"elements" : [],
           						"name" : "separators"
           					},
           					"properties" : {
           						"isDirty" : true,
           						"generalInfo" : {
           							"gridStyle" : "Pivot",
           							"separatorStyle" : "Comma"
           						},
           						"table" : {
           							"backgroundColor" : "#efefef",
           							"border" : {
           								"top" : {},
           								"right" : {},
           								"bottom" : {},
           								"midVer" : {},
           								"left" : {},
           								"midHor" : {}
           							}
           						},
           						"columns" : {
           							"width" : {
           								"value" : null,
           								"unit" : "Pixels"
           							},
           							"alterBackgroundColor" : false
           						},
           						"rows" : {
           							"height" : 40,
           							"alterBackgroundColor" : false
           						},
           						"headers" : {
           							"font" : {
           								"family" : "Roboto",
           								"size" : 14,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"backgroundColor" : "#dbf2ff"
           							},
           							"alignment" : "left",
           							"wordWrap" : false,
           							"removeHeaderForExport" : false
           						},
           						"grouping" : {
           							"useSeparator" : true
           						},
           						"view" : {
           							"dataRefreshInterval" : {
           								"enable" : false,
           								"updateInterval" : 0,
           								"isAll" : true,
           								"latestRecord" : 0
           							},
           							"usePagination" : false
           						},
           						"printing" : {
           							"usePageBreakAfterSeparator" : false
           						}
           					},
           					"settings" : {},
           					"dataSource" : {},
           					"title" : {
           						"text" : "",
           						"properties" : {},
           						"settings" : {
           							"font" : {
           								"family" : "",
           								"size" : 14,
           								"bold" : true,
           								"italic" : false,
           								"underline" : false,
           								"color" : "",
           								"highlightColor" : ""
           							},
           							"alignment" : {
           								"alignment" : ""
           							}
           						},
           						"elements" : []
           					},
           					"description" : {
           						"text" : "",
           						"properties" : {},
           						"settings" : {
           							"font" : {
           								"family" : "",
           								"size" : 14,
           								"bold" : false,
           								"italic" : false,
           								"underline" : false,
           								"color" : "",
           								"highlightColor" : ""
           							},
           							"alignment" : {
           								"alignment" : ""
           							}
           						},
           						"elements" : []
           					}
           				},
           				"reportId" : "d797d877-6ae1-443a-b5f4-e9fbaeb884a8",
           				"positionX" : 0,
           				"positionY" : 0,
           				"width" : 12,
           				"height" : 4,
           				"state" : 3,
           				"modified" : null,
           				"numberOfRecord" : 0,
           				"points" : [{
           						"key" : "All",
           						"filter" : [{
           								"key" : "",
           								"value" : ""
           							}
           						],
           						"expandedLevel" : 0,
           						"isViewSeparator" : false
           					}
           				],
           				"configField" : {},
           				"expandedLevel" : 0,
           				"title" : "Grid",
           				"id" : "9e632295-41c6-41ba-a235-29c525d6ef59"
           			}
           		],
           		"header" : {
           			"visible" : false,
           			"items" : [{
           					"isDirty" : false,
           					"type" : "image",
           					"label" : "Image",
           					"id" : "formatDetails_17",
           					"positionX" : 0,
           					"positionY" : 0,
           					"width" : 6,
           					"height" : 6,
           					"name" : "Logo Image",
           					"value" : "",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"imageUrl" : "http://",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_18",
           					"positionX" : 20,
           					"positionY" : 0,
           					"width" : 12,
           					"height" : 2,
           					"name" : "Report Name",
           					"value" : "{reportName}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "thinHorizontalRule",
           					"label" : "Horizontal Rule",
           					"id" : "formatDetails_19",
           					"positionX" : 20,
           					"positionY" : 4,
           					"width" : 12,
           					"height" : 1,
           					"name" : "Upper Separator Line",
           					"value" : "{horizontalRule}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 2
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_20",
           					"positionX" : 20,
           					"positionY" : 5,
           					"width" : 6,
           					"height" : 2,
           					"name" : "Report Generated",
           					"value" : "Report Generated:",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_21",
           					"positionX" : 20,
           					"positionY" : 7,
           					"width" : 6,
           					"height" : 2,
           					"name" : "User",
           					"value" : "User:",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_22",
           					"positionX" : 20,
           					"positionY" : 9,
           					"width" : 6,
           					"height" : 2,
           					"name" : "Tenant",
           					"value" : "Tenant:",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "dateTime",
           					"label" : "Date Time",
           					"id" : "formatDetails_23",
           					"positionX" : 26,
           					"positionY" : 5,
           					"width" : 6,
           					"height" : 2,
           					"name" : "Current Date Time",
           					"value" : "{currentDateTime}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_24",
           					"positionX" : 26,
           					"positionY" : 7,
           					"width" : 6,
           					"height" : 2,
           					"name" : "Current User Name",
           					"value" : "{currentUserName}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_25",
           					"positionX" : 26,
           					"positionY" : 9,
           					"width" : 6,
           					"height" : 2,
           					"name" : "Tenant Name",
           					"value" : "{tenantName}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "horizontalRule",
           					"label" : "Horizontal Rule",
           					"id" : "formatDetails_26",
           					"positionX" : 0,
           					"positionY" : 11,
           					"width" : 32,
           					"height" : 1,
           					"name" : "Lower Separator Line",
           					"value" : "{horizontalRule}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 4
           				}
           			]
           		},
           		"footer" : {
           			"visible" : false,
           			"items" : [{
           					"isDirty" : false,
           					"type" : "horizontalRule",
           					"label" : "Horizontal Rule",
           					"id" : "formatDetails_27",
           					"positionX" : 0,
           					"positionY" : 0,
           					"width" : 32,
           					"height" : 1,
           					"name" : "Separator Line",
           					"value" : "{horizontalRule}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 4
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_28",
           					"positionX" : 0,
           					"positionY" : 1,
           					"width" : 10,
           					"height" : 2,
           					"name" : "Footer Text",
           					"value" : "Footer Text",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "text",
           					"label" : "Text",
           					"id" : "formatDetails_29",
           					"positionX" : 20,
           					"positionY" : 1,
           					"width" : 4,
           					"height" : 2,
           					"name" : "Page",
           					"value" : "Page",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "pageNumber",
           					"label" : "Page Number",
           					"id" : "formatDetails_30",
           					"positionX" : 24,
           					"positionY" : 1,
           					"width" : 8,
           					"height" : 2,
           					"name" : "Page Number",
           					"value" : "{pageNumber}",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}
           			]
           		},
           		"titleDescription" : {
           			"visible" : false,
           			"items" : [{
           					"isDirty" : false,
           					"type" : "title",
           					"label" : "Title",
           					"id" : "formatDetails_31",
           					"name" : "Title",
           					"value" : "",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}, {
           					"isDirty" : false,
           					"type" : "description",
           					"label" : "Description",
           					"id" : "formatDetails_32",
           					"name" : "Description",
           					"value" : "",
           					"font" : {
           						"family" : "Roboto",
           						"size" : 14,
           						"bold" : false,
           						"italic" : false,
           						"underline" : false,
           						"color" : "#000",
           						"backgroundColor" : "#fff"
           					},
           					"color" : "#000",
           					"dashStyle" : "solid",
           					"thickness" : 1
           				}
           			]
           		},
           		"version" : 0,
           		"schedules" : []
           	},
           	"expandedLevel" : 0
         }

   Sample response::

      {
         "hasChanged" : false,
         "filterFields" : []
      }

POST report/updateResults
---------------------------------------

Updates the report data model.

**Request**

      Payload: a :doc:`models/ReportSavingParameter` object

**Response**

      A :doc:`models/ReportSavingResult` object

**Samples**

      To be updated

POST report/loadAccesses
---------------------------------------

Returns a list of all accesses.

**Request**

      Payload: an :doc:`models/AccessPagedRequest` object

**Response**

      A :doc:`models/PagedResult` object, with **result** field containing an array of :doc:`models/UserPermission` objects

**Samples**

   .. code-block:: http

      POST /api/report/loadAccesses HTTP/1.1

   Request Payload:

      .. code-block:: json

         {
           "reportId": "840ba6a8-dfe3-4d92-8341-63e56b95a038",
           "accesses": [],
           "criteria": [
             {
               "key": "All",
               "value": "",
               "operation": 1
             }
           ],
           "pageIndex": 1,
           "pageSize": 10,
           "sortOrders": [
             {
               "key": "shareWith",
               "descending": true
             }
           ]
         }

   Successful response::

      {
        "result": [],
        "pageIndex": 1,
        "pageSize": 10,
        "total": 0
      }

POST report/loadSchedules
---------------------------------------

Returns a list of all scheduled deliveries.

**Request**

      Payload: an :doc:`models/SubscriptionPagedRequest` object

**Response**

      A :doc:`models/PagedResult` object, with **result** field containing an array of :doc:`models/Subscription` objects

**Samples**

   .. code-block:: http

      POST /api/report/loadSchedules HTTP/1.1

   Request Payload::

      {
         "reportId" : "17bbaf02-0b59-4224-93b3-41bb14da516f",
         "subscriptions" : [],
         "criteria" : [{
               "key" : "All",
               "value" : "",
               "operation" : 1
            }
         ],
         "pageIndex" : 1,
         "pageSize" : 10,
         "sortOrders" : [{
               "key" : "name",
               "descending" : true
            }
         ]
      }

   Successful response::

      {
         "result" : [],
         "pageIndex" : 1,
         "pageSize" : 10,
         "total" : 0
      }

GET report/reportPart/{report_part_id}/(report_id)
---------------------------------------------------

Returns the report part definition specified by report_part_id from draft (using report_id) or from database (without report_id).

**Request**

      No payload

**Response**

   A :doc:`models/ReportPartDefinition` object.

**Samples**

   .. code-block:: http

      GET /api/report/reportPart/7391FEC3-53A1-411D-B92E-02C007FB4DD6 HTTP/1.1

   .. container:: toggle

      .. container:: header

         Sample response:

      .. code-block:: json

         {
        	"reportPartContent" : {
        		"showDataInOneGroupNextTogether" : false,
        		"columns" : {
        			"text" : null,
        			"properties" : {},
        			"settings" : {},
        			"elements" : [{
        					"name" : "ShipCountry",
        					"properties" : {
        						"isDirty" : false,
        						"fieldItemVisible" : true,
        						"dataFormattings" : {
        							"function" : "",
        							"functionInfo" : {},
        							"format" : {},
        							"font" : {
        								"family" : "Roboto",
        								"size" : 14,
        								"bold" : false,
        								"italic" : false,
        								"underline" : false,
        								"color" : "",
        								"backgroundColor" : ""
        							},
        							"alignment" : "alignLeft",
        							"sort" : "",
        							"color" : {
        								"textColor" : {
        									"rangePercent" : null,
        									"rangeValue" : null,
        									"value" : null
        								},
        								"cellColor" : {
        									"rangePercent" : null,
        									"rangeValue" : null,
        									"value" : null
        								}
        							},
        							"alternativeText" : {
        								"rangePercent" : null,
        								"rangeValue" : null,
        								"value" : null
        							},
        							"customURL" : {
        								"url" : "",
        								"option" : "Open link in New Window"
        							},
        							"embeddedJavascript" : {
        								"script" : ""
        							},
        							"subTotal" : {
        								"label" : "",
        								"function" : "",
        								"expression" : "",
        								"dataType" : "",
        								"previewResult" : ""
        							},
        							"grandTotal" : {
        								"label" : "",
        								"function" : "",
        								"expression" : "",
        								"dataType" : "",
        								"previewResult" : ""
        							}
        						},
        						"headerFormating" : {
        							"font" : {
        								"family" : null,
        								"size" : null,
        								"bold" : null,
        								"italic" : null,
        								"underline" : null,
        								"color" : null,
        								"backgroundColor" : null
        							},
        							"alignment" : null,
        							"wordWrap" : null,
        							"columnGroup" : ""
        						},
        						"drillDown" : {
        							"subReport" : {
        								"selectedReport" : null,
        								"style" : null,
        								"reportPartUsed" : null,
        								"reportFilter" : true,
        								"mappingFields" : []
        							}
        						},
        						"otherProps" : {}
        					},
        					"settings" : {},
        					"chartType" : null,
        					"showTotal" : false,
        					"position" : 1,
        					"field" : {
        						"fieldId" : "d300a6bd-f218-46c8-a262-3b9fa5ee0382",
        						"originalName" : null,
        						"fieldName" : "ShipCountry",
        						"fieldNameAlias" : "ShipCountry",
        						"dataFieldType" : "Text",
        						"querySourceId" : "d609ecdc-2afc-43ce-a0a4-0583ed667c8f",
        						"querySourceType" : "Table",
        						"sourceAlias" : "Orders",
        						"relationshipId" : "00000000-0000-0000-0000-000000000000",
        						"visible" : true,
        						"filterable" : false,
        						"reportId" : null,
        						"fieldFunctionExpression" : "",
        						"expression" : null,
        						"grandTotalExpression" : "",
        						"subTotalExpression" : "",
        						"sort" : "Unsorted",
        						"function" : null,
        						"calculatedTree" : null,
        						"grandTotalTree" : null,
        						"isCalculated" : false
        					}
        				}
        			]
        		},
        		"rows" : {
        			"text" : null,
        			"properties" : {},
        			"settings" : {},
        			"elements" : []
        		},
        		"values" : {
        			"text" : null,
        			"properties" : {},
        			"settings" : {},
        			"elements" : []
        		},
        		"separators" : {
        			"text" : null,
        			"properties" : {},
        			"settings" : {},
        			"elements" : [{
        					"name" : "Group (Freight)",
        					"properties" : {
        						"isDirty" : false,
        						"fieldItemVisible" : true,
        						"dataFormattings" : {
        							"function" : "7f942ac7-08d8-41fa-9e89-bad96f07f102",
        							"functionInfo" : {
        								"id" : "7f942ac7-08d8-41fa-9e89-bad96f07f102",
        								"name" : "Group",
        								"expression" : null,
        								"dataType" : "Money",
        								"formatDataType" : "Money",
        								"syntax" : null,
        								"expressionSyntax" : null,
        								"isOperator" : false,
        								"extendedProperties" : {}
        							},
        							"format" : {},
        							"font" : {
        								"family" : "Roboto",
        								"size" : 14,
        								"bold" : false,
        								"italic" : false,
        								"underline" : false,
        								"color" : "",
        								"backgroundColor" : ""
        							},
        							"alignment" : "alignLeft",
        							"sort" : "",
        							"color" : {
        								"textColor" : {
        									"rangePercent" : null,
        									"rangeValue" : null,
        									"value" : null
        								},
        								"cellColor" : {
        									"rangePercent" : null,
        									"rangeValue" : null,
        									"value" : null
        								}
        							},
        							"alternativeText" : {
        								"rangePercent" : null,
        								"rangeValue" : null,
        								"value" : null
        							},
        							"customURL" : {
        								"url" : "",
        								"option" : "Open link in New Window"
        							},
        							"embeddedJavascript" : {
        								"script" : ""
        							},
        							"subTotal" : {
        								"label" : "",
        								"function" : "",
        								"expression" : "",
        								"dataType" : "",
        								"previewResult" : ""
        							},
        							"grandTotal" : {
        								"label" : "",
        								"function" : "",
        								"expression" : "",
        								"dataType" : "",
        								"previewResult" : ""
        							}
        						},
        						"headerFormating" : {
        							"font" : {
        								"family" : null,
        								"size" : null,
        								"bold" : null,
        								"italic" : null,
        								"underline" : null,
        								"color" : null,
        								"backgroundColor" : null
        							},
        							"alignment" : null,
        							"wordWrap" : null,
        							"columnGroup" : ""
        						},
        						"drillDown" : {
        							"subReport" : {
        								"selectedReport" : null,
        								"style" : null,
        								"reportPartUsed" : null,
        								"reportFilter" : true,
        								"mappingFields" : []
        							}
        						},
        						"otherProps" : {}
        					},
        					"settings" : {},
        					"chartType" : null,
        					"showTotal" : false,
        					"position" : 1,
        					"field" : {
        						"fieldId" : "fa47d01d-e055-43ad-b1ec-891b1685b9fe",
        						"originalName" : null,
        						"fieldName" : "Freight",
        						"fieldNameAlias" : "Group (Freight)",
        						"dataFieldType" : "Money",
        						"querySourceId" : "d609ecdc-2afc-43ce-a0a4-0583ed667c8f",
        						"querySourceType" : "Table",
        						"sourceAlias" : "Orders",
        						"relationshipId" : "00000000-0000-0000-0000-000000000000",
        						"visible" : true,
        						"filterable" : false,
        						"reportId" : null,
        						"fieldFunctionExpression" : "",
        						"expression" : null,
        						"grandTotalExpression" : "",
        						"subTotalExpression" : "",
        						"sort" : "Unsorted",
        						"function" : "Group",
        						"calculatedTree" : null,
        						"grandTotalTree" : null,
        						"isCalculated" : false
        					}
        				}
        			]
        		},
        		"type" : 3,
        		"properties" : {
        			"isDirty" : false,
        			"generalInfo" : {
        				"gridStyle" : "Vertical",
        				"separatorStyle" : "Comma"
        			},
        			"table" : {
        				"border" : {
        					"top" : {},
        					"right" : {},
        					"bottom" : {},
        					"midVer" : {},
        					"left" : {},
        					"midHor" : {}
        				},
        				"backgroundColor" : "#efefef"
        			},
        			"columns" : {
        				"width" : {
        					"value" : null,
        					"unit" : "Pixels"
        				},
        				"alterBackgroundColor" : false
        			},
        			"rows" : {
        				"height" : 40,
        				"alterBackgroundColor" : false
        			},
        			"headers" : {
        				"font" : {
        					"family" : "Roboto",
        					"size" : 14,
        					"bold" : true,
        					"italic" : false,
        					"underline" : false,
        					"backgroundColor" : "#dbf2ff"
        				},
        				"alignment" : "left",
        				"wordWrap" : false,
        				"removeHeaderForExport" : false
        			},
        			"grouping" : {
        				"useSeparator" : true
        			},
        			"view" : {
        				"dataRefreshInterval" : {
        					"enable" : false,
        					"updateInterval" : 0,
        					"isAll" : true,
        					"latestRecord" : 0
        				},
        				"usePagination" : false
        			},
        			"printing" : {
        				"usePageBreakAfterSeparator" : false
        			}
        		},
        		"settings" : {},
        		"title" : {
        			"text" : "",
        			"properties" : {},
        			"settings" : {
        				"font" : {
        					"family" : "",
        					"size" : 14,
        					"bold" : true,
        					"italic" : false,
        					"underline" : false,
        					"color" : "",
        					"highlightColor" : ""
        				},
        				"alignment" : {
        					"alignment" : ""
        				}
        			},
        			"elements" : []
        		},
        		"description" : {
        			"text" : "",
        			"properties" : {},
        			"settings" : {
        				"font" : {
        					"family" : "",
        					"size" : 14,
        					"bold" : false,
        					"italic" : false,
        					"underline" : false,
        					"color" : "",
        					"highlightColor" : ""
        				},
        				"alignment" : {
        					"alignment" : ""
        				}
        			},
        			"elements" : []
        		},
        		"expandedLevel" : -1
        	},
        	"title" : "Grid",
        	"positionX" : 0,
        	"positionY" : 0,
        	"width" : 12,
        	"height" : 4,
        	"reportId" : "055899a3-8886-4099-b360-fabf4656f5ce",
        	"numberOfRecord" : 0,
        	"id" : "7391fec3-53a1-411d-b92e-02c007fb4dd6",
        	"state" : 0,
        	"inserted" : true,
        	"version" : 1,
        	"created" : "2016-08-29T09:17:05.13",
        	"createdBy" : null,
        	"modified" : "2016-08-29T09:17:05.13",
        	"modifiedBy" : null
        }

.. _POST_report/validateExistingField:

POST report/validateExistingField
---------------------------------------

Validates that some fields exist in a report.

**Request**

      Payload: a :doc:`models/ReportValidation` object.

**Response**

      An :doc:`models/OperationResult` object with the **data** field populated:

      * true: the fields exist

**Samples**

   .. code-block:: http

      POST /api/report/validateExistingField HTTP/1.1

   Request payload::

      To be updated

   Successful response::

      {
         "success": true,
         "data": true
      }


GET report/reportId/(tenant_id)?reportName=value&categoryName=value
--------------------------------------------------------------------

Returns report id from report name and category name.

**Request**

      No payload

**Response**

      An :doc:`models/OperationResult` object with **data** field populated with the id of the report if found.

**Samples**

   .. code-block:: http

      GET /api/report/reportId?reportName=Test%20Report&categoryName=Category%201 HTTP/1.1

   No payload

   Sample response::

      {
         "success": true,
         "data": "4e2d54c8-20e7-437f-a5ce-43e963c763a2"
      }

.. _POST_report/validateExistingReport:

POST report/validateExistingReport
---------------------------------------

Validates that a report exists in a category.

**Request**

      Payload: a :doc:`models/ReportValidation` object.

**Response**

      An :doc:`models/OperationResult` object with **data** field populated

      * true if the report exists
      * false if not

**Samples**

   .. code-block:: http

      POST /api/report/validateExistingReport HTTP/1.1

   Request Payload::

      To be updated

   Successful response::

      {
         "success": true,
         "data": true
      }

GET report/reportsByTenant/(tenant_id)
---------------------------------------

Returns list of reports by tenant, with each report containing a list of report parts.

**Request**

   No payload

**Response**

   An array of :doc:`models/ReportPartDefinition` objects.

**Samples**

   .. code-block:: http

      GET /api/report/reportsByTenant HTTP/1.1

   Request payload::

      To be updated

   Sample response::

      To be updated


POST report/updateRenderingTime
---------------------------------------

Updates the rendering time of a report.

**Request**

   Payload: a :doc:`models/ReportDefinition` object.

**Response**

   An :doc:`models/OperationResult` object with the **success** field populated.

**Samples**

   .. code-block:: http

      POST /api/report/updateRenderingTime HTTP/1.1

   Request payload::

      To be updated

   Sample response::

      To be updated


GET report/isReportValid/(report_id)
---------------------------------------

Returns true if there is a report definition for the specified report_id.

**Request**

   No payload

**Response**

   * true if there is a report definition for the specified report_id
   * false if report_id is null or there is no report definition

**Samples**

   .. code-block:: http

      GET /api/report/isReportValid/3501e30d-6ded-4f26-9922-011633ace5ab HTTP/1.1

   Sample response::

      true

POST report/printDraft
---------------------------------------

Saves report as draft for printing.

**Request**

   Payload: a :doc:`models/ReportSavingParameter` object.

**Response**

   The id of the report in draft.

**Samples**

   To be updated

GET report/printDraft/{report_id}
---------------------------------------

Gets report as draft for printing.

**Request**

   No payload

**Response**

   A :doc:`models/ReportDefinition` object.

**Samples**

   To be updated

GET report/accessPriority/(report_dashboard_id)
------------------------------------------------

Returns access priority for report/dashboard.

**Request**

   No payload

**Response**

   The access priority

**Samples**

   .. code-block:: http

      GET /api/report/accessPriority/e09f9d45-b721-4012-b8e7-c31c58d52af3 HTTP/1.1

   Response::

      1
