CPHL
====

<b>Disclaimer: this is a brain-storming document, and is not in any way final nor concrete.</b>

CPHL, or the Cross-Platform Hypertext Language is a hypertext specification for RESTful APIs.  CPHL is based on HAL, but is designed to allow for greater flexibility, transmission of additional information, and empowerment of resource specific code on demand.

It is also designed to be action driven, rather than resource driven.  This means that a resource may be included multiple times if necessary (ie edit, delete), but described by its title, description, and the methods it utilizes - making it more explicit than other formats.

CPHL requires specific key value pairs that can then be distributed across different platforms, such as JSON, XML, and YAML (see examples below).

<h3>General Rules</h3>
CPHL is designed to be as flexible as possible, but does recommend the use of both the _definition and _links collections, although at minimum the _links collection is required.

Based on HAL, CPHL is nestable meaning that you can utilize it for actions specific to the entire collection or singular result, or nest it within each item of the collection for that item's specific actions, or both.


<h3>The Specification</h3>
<h4>_definition</h4>
_definition contains the collection of API definitions, as described by WADL, RAML, Swagger, or API Blueprint.

The _definition collection breaks down into key value pairs where the name of the spec (camelCase, no spaces) represents the key, and the URL to the file containing the API defintion is the value.

```
"_definition" : {
  "raml" : "http://api.domain.com/docs/api/raml",
  "swagger" : "http://api.domain.com/docs/api/swagger"
}
```

<h4>_links</h4>
_links contains the hypertext links collection utilizing the action as the key for easy deserialization and quick access.

Each individual link contains the following structure (optional constraints italic):
- *title*: the title of the action (eg: Edit User)
- *description*: a brief description of the action
- <b>href</b>: the URI for the client to achieve the action
- *expiration*: timestamp of when the link expires/ is no longer valid
- *methods*: an array of available methods to perform this specific action (eg edit: put, patch; view: get)
- *formats*: an array of formats that this action supports (ie JSON, XML, etc)
  - *{formatName}*
    - *mimeType*: the mime-type of the format (eg: application/json)
    - *schema*: the URL to the schema for this format
- *docHref*: the URL to online documentation for this action
- *code*: an array of available code libraries for this action
  - *{language}*: the URL of the code library accessible to the client
  
<h5>Reserved Names</h5>
Within the _links collections certain key names are reserved for specific actions.  These are based on the most commonly used hypermedia links, as well as CRUD for that specific collection/ item.  They include:
- create: Create a new record via the POST method
- read: retrieve an item or collection via GET
- update: utilization of the put/ patch methods to update an item, or ALL items in a collection*
- delete: deletes the item or the collection*
- search: the resource to perform a search on a collection
- first: links to the first record in a collection
- beginning: links to the first set of records in a paginated result
- prev: links to the previous set of records in a paginated result
- next: links to the next set of records in a paginated result
- last: links to the last record of a paginated result
- end: links to the last set of records in a paginated result
- base: links back to the starting point of a hypermedia API
  
<h3>Content-type Headers</h3>
CPHL supports multiple Content-type headers to express the information that should be included in the response.  Every CPHL header starts with "cphl" followed by the format (ie "json"), and then by an optional "code" to express whether or not the code links should be included in the response.

For example:
<b>application/cphl+json</b> will return the _definition and _links collections, but omit the "code" property for the different _links actions as well as the other content-types available under "formats."

<b>application/cphl+json+code</b> will return the _definition and _links collections, but omit the formats property (other than than the json format).

<b>application/cphl+json+code+formats</b> will include both the code and formats properties.

<b>application/cphl+json+formats</b> will include the formats property, but continue to exclude the code properties.

<h3>Examples</h3>
<h4>application/cphl+json+code+formats</h4>
```
"_definition" : {
  "raml" : "http://api.domain.com/docs/api/raml",
  "swagger" : "http://api.domain.com/docs/api/swagger"
}
 
"_links" : {
  "update" : {
    "title" : "Edit User",
    "description" : "edit the user",
    "href" : "/api/resource",
    "methods" : ["put", "patch"],
    "formats" : {
        "json" : {
          "mimeType" : "application/json",
          "schema" : "http://api.domain.com/docs/api/editSchema.json"
        },
        "xml" : {
          "mimeType" : "text/xml",
          "schema" : "http://api.domain.com/docs/api/editSchema.xml"
        },
    },
    "docHref" : "http://api.domain.com/docs/edit",
    "code" : {
        "php" : "http://code.domain.com/phplib/edit.tgz",
        "java" : "http://code.domain.com/javalib/edit.tgz",
        "ruby" : "http://code.domain.com/rubylib/edit.tgz",
    }
  }
}
```

<h4>application/cphl+xml+code+formats</h4>
```
<_definition>
  <raml>http://api.domain.com/docs/api/raml</raml>
  <swagger>http://api.domain.com/docs/api/swagger</swagger>
</_definition>
 
<_links>
  <update>
    <title>Edit User</title>
    <description>edit the user</description>
    <href>/api/resource</href>
    <methods>put</methods>
    <methods>patch</methods>
    <formats>
      <json>
        <mimeType>application/json</mimeType>
        <schema>http://api.domain.com/docs/api/editSchema.json</schema>
      </json>
      <xml>
        <mimeType>text/xml</mimeType>
        <schema>http://api.domain.com/docs/api/editSchema.xml</schema>
      </xml>
    </formats>
    <formats>xml</formats>
    <docHref>http://api.domain.com/docs/edit</docHref>
    <code>
      <php>http://code.domain.com/phplib/edit.tgz</php>
      <java>http://code.domain.com/javalib/edit.tgz</java>
      <ruby>http://code.domain.com/rubylib/edit.tgz</ruby>
    </code>
  </update>
</_links>
```

<h4>application/cphl+yaml+code+formats</h4>
```
_definition:
  raml: http://api.domain.com/docs/api/raml
  swagger: http://api.domain.com/docs/api/swagger
    
_links:
  update:
    title: Edit User
    description: edit the user
    href: /api/resource
    methods:
      - put
      - patch
    formats:
      json:
        mimeType: application/json
        schema: http://api.domain.com/docs/api/editSchema.json
      xml:
        mimeType: text/json
        schema: http://api.domain.com/docs/api/editSchema.xml
    docHref: http://api.domain.com/docs/edit
    code:
      php: http://code.domain.com/phplib/edit.tgz
      java: http://code.domain.com/javalib/edit.tgz
      ruby: http://code.domain.com/rubylib/edit.tgz
```

<h3>Clients and Cross-Compatiblity</h3>
CPHL is compatible with any JSON HAL client as it is simliar in structure and shares the "title" and "href" properties.  However, the description, methods, formats, docHref, and code properties are not supported by HAL and will be ignored.  Likewise, CPHL does not support curies or URI relations at this time.

Otherwise CPHL can be deserialized and used across any language with the appropriate library (whether it be JSON, XML, YAML, etc).
