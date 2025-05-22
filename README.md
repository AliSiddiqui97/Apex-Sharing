# Apex-Sharing
The ApexSharing class is an Apex utility that allows dynamic sharing of records with public groups using the native Salesforce sharing model. It is designed to be called from Flows or other automated processes using the @InvocableMethod.

Features
Dynamically determines the SObject type from the record ID.

Constructs and inserts share records (e.g., CustomObject__Share) for any custom or standard object that supports sharing.

Supports sharing with Public Groups via UserOrGroupId.

Sets sharing AccessLevel to 'Read' and RowCause to 'Manual'.

Can be invoked from Flows using a wrapper class.

Use Case
This class is useful in scenarios where:

You need to dynamically share a wide range of objects (custom or standard) with public groups.

You are using Flows to automate record sharing logic.

You want to avoid hardcoding specific object names or manually creating sharing logic for each object.

How It Works
Input: A list of wrapper objects, each containing a recordId and a groupId.

SObject Type Detection: Uses the ID prefix to determine the SObject type of the record.

Dynamic Sharing: Constructs a corresponding <Object>Share SObject using Type.forName.

Sharing Record Creation: Populates required fields and inserts the share records.

Error Handling: Logs failures but does not throw unhandled exceptions.

Apex Signature
apex
Copy
Edit
@InvocableMethod(label='Dynamically Share Records with Public Groups')
public static void shareRecords(List<shareRecordsWrapper> context)
Wrapper Class: shareRecordsWrapper
Field	Type	Required	Description
recordId	String	Yes	The ID of the record to be shared
groupId	String	Yes	The ID of the Public Group to share with

Example Flow Configuration
To use this in a Flow:

Add an Action element.

Choose the Apex Class named ApexSharing.shareRecords.

Provide a list of recordId and groupId pairs as inputs.

Considerations
This method only works for objects that support manual sharing (e.g., custom objects with OWD set to Private or Controlled by Parent).

The invoking user must have the appropriate permissions to share the records.

If the object doesn't support sharing or the share type cannot be resolved, that record will be skipped.

Limitations
Only supports Read access level as of now.

Sharing records are not created for objects that don’t have a corresponding <Object>Share object.

No support for removing/revoking shares; only adds them.

