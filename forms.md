Forms in CPHL
====

<b>Disclaimer: this is a brain-storming document, and is not in any way final nor concrete.</b>

CPHL lets you provide form field information (along with logic, in a separate property) within the hypermedia's "code" property.

Forms should be an array of fields with their HTML attributes provided in the content-type of the call they made (for example, if using JSON, the form should be represented as JSON).

All fields *MUST* have a name, a type, and whether or not the field is required.

For example, with JSON, form fields would be represented as:

```
"form" : {
  "fields" : [
    {
      "name" : "firstName",
      "type" : "text",
      "required" : true,
      "minLength" : 3,
      "maxLength": 20
    },
    {
      "name" : "lastName",
      "type" : "text",
      "required" : true,
      "minLength" : 3,
      "maxLength": 20
    },
    {
      "name" : "age",
      "type" : "text",
      "required" : true,
      "pattern" : "[0-9]{1,3}"
    },
    {
      "name" : "favoriteColor",
      "type" : "text",
      "required" : true,
      "value" : "red"
    },
  ]
}
```

With XML, form fields would be represented as:

```
<form>
  <fields>
    <field>
      <name>firstName</name>
      <type>text</type>
      <required>true</required>
      <minLength>3</minLength>
      <maxLength>20</maxLength>
    </field>
    <field>
      <name>lastName</name>
      <type>text</type>
      <required>true</required>
      <minLength>3</minLength>
      <maxLength>20</maxLength>
    </field>
    <field>
      <name>age</name>
      <type>text</type>
      <required>true</required>
      <pattern>[0-9]{1,3}</pattern>
    </field>
    <field>
      <name>favoriteColor</name>
      <type>text</type>
      <required>true</required>
      <value>red</value>
    </field>
  </fields>
</form>
```

Last but not least, with YAML:

```
form:
  fields:
    -
      name: "firstName",
      type: "text",
      required: true,
      minLength: 3,
      maxLength: 20
    -
      name: "lastName",
      type: "text",
      required: true,
      minLength: 3,
      maxLength: 20
    },
    -
      name: "age",
      type: "text",
      required: true,
      pattern: "[0-9]{1,3}"
    -
      name: "favoriteColor",
      type: "text",
      required: true,
      value: "red"
  ```
