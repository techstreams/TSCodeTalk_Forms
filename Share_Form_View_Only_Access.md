# TSCodeTalk > Forms > Share Form "View" Only Access


This script shares a form with "View" only access.  *(Forms do not automatically provide a way to share forms as "View" Only.)*

---

```javascript
function share() {
 
    var formId = "<FORM FILE ID>";
    var file = DriveApp.getFileById(formId);

    file.setSharing(
      DriveApp.Access.ANYONE_WITH_LINK, 
      DriveApp.Permission.VIEW
    );
  
}
```
