# TSCodeTalk > Forms > Share Form "View" Only Access


This script shares a form with "View" only access.

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
