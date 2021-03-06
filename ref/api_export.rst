

============================
Export APIs
============================


List of APIs
------------

.. list-table::
   :class: apitable
   :widths: 35 65
   :header-rows: 1

   * - API
     - Purpose
   * - `POST export/{format:exportformat}`_
     - Exports the dashboard, report or report part specified in request parameters.
   * - `POST export/email`_
     - Emails or exports report/dashboard based on the input subscription parameter.
   * - `POST export/performanceStatisticsTrend`_
     - Exports the performance statistics trend to csv.


POST export/{format:exportformat}
--------------------------------------------------------------

Exports the dashboard, report or report part specified in request parameters.

**Request**

    **ContentType** = "application/x-www-form-urlencoded"

    **Keys**:

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  NULL
          -  Description
          -  Note
       *  -  **dashboardID** |br|
             string
          -  Y
          -  The id of the dashboard if available
          -
       *  -  **dashboardPartID** |br|
             string
          -  Y
          -  The id of the dashboard part if available
          -
       *  -  **reportID** |br|
             string
          -  Y
          -  The id of the report if available
          -
       *  -  **draftID** |br|
             string
          -  Y
          -  The id of the draft if available
          -
       *  -  **reportPartID** |br|
             string
          -  Y
          -  The id of the report part if available
          -
       *  -  **filters** |br|
             array of objects
          -  Y
          -  An array of :doc:`models/FilterItem` objects
          -
       *  -  **fileSessionKey** |br|
             string
          -  Y
          -  The key of this specific session, it will be set to cookie <fileSessionKey>=true on success
          -
       *  -  **reportDefinition** |br|
             string
          -  Y
          -  The report definition if available
          -
       *  -  **dashboardDefinition** |br|
             string
          -  Y
          -  The dashboard definition if available
          -
       *  -  **dashboardPartIndex** |br|
             string
          -  Y
          -  The index of the dashboard part if available
          -
       *  -  **access_token** |br|
             string
          -  Y
          -  The authentication token
          -
       *  -  **current_tenant** |br|
             string
          -  Y
          -  The id of the current tenant if available
          -



**Response**

    The content of the export

**Samples**

   .. code-block:: http

      POST /api/export/pdf HTTP/1.1

   Request form:

   .. code-block:: text

      url:http://localhost:8888/report/view/e09f9d45-b721-4012-b8e7-c31c58d52af3
      reportId:e09f9d45-b721-4012-b8e7-c31c58d52af3
      filters:[]
      draftId:f862931f-f897-4f98-ab13-687b79b91806
      tenantId:
      fileSessionKey:e09f9d45-b721-4012-b8e7-c31c58d52af3294
      access_token:123Abc..=
      current_tenant:

   Response::

      the file

   The cookie will be set:

   .. code-block:: text

      e09f9d45-b721-4012-b8e7-c31c58d52af3294=true

POST export/email
--------------------------------------------------------------

Emails or exports report/dashboard based on the input subscription parameter.

**Request**

    Payload: a :doc:`models/Subscription` object

**Response**

    * true if successful
    * false if not

**Samples**

   .. code-block:: http

      POST /api/export/email HTTP/1.1

   Request payload::

      {
        "reportId": "e09f9d45-b721-4012-b8e7-c31c58d52af3",
        "deliveryType": "Email",
        "deliveryMethod": "link",
        "exportAttachmentType": "Pdf",
        "emailSubject": "{reportName}",
        "emailBody": "Dear,<br><br>Please open report by clicking on the following link.<br><br>{reportLink} <br><br>Regards,<br>{currentUserName}",
        "recipients": "jdoe@acme.com",
        "additionalRecipients": ""
      }

   Sample response::

      true


POST export/performanceStatisticsTrend
--------------------------------------------------------------

Exports the performance statistics trend to csv.

**Request**

    **ContentType** = "application/x-www-form-urlencoded"

    **Keys**:

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  NULL
          -  Description
          -  Note
       *  -  **access_token** |br|
             string
          -
          -  The access token
          -
       *  -  **current_tenant** |br|
             string (GUID)
          -
          -  The id of the tenant
          -

**Response**

    The file 

**Samples**

   .. code-block:: http

      POST /api/export/performanceStatisticsTrend HTTP/1.1

   Request form:

   .. code-block:: text

      access_token:123Abc..=
      current_tenant:

   Response::

      the file
