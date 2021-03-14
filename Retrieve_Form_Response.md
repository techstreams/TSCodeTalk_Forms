# TSCodeTalk > Forms > Get Form Response


This script retrieves data from a Google Form response.

---

```javascript
/* 
 * This method retrieves a form item response
 * @param {ItemResponse} ir - form item response
 * @return {Object} response
 */
getFormItemResponse(ir) {
  const item = ir.getItem(),
        title = item.getTitle(),
        itemtype = item.getType();
  let content, date, day, footer = null, hour, minute, month, pattern, year;

  switch (itemtype) {
    case FormApp.ItemType.CHECKBOX:
      content = ir.getResponse().map(i => i).join('\n');
      break;
    case FormApp.ItemType.GRID:
      content = item.asGridItem().getRows()
                    .map((r,i) => {
                       const resp = ir.getResponse()[i];
                       return `${r}: ${resp && resp !== '' ? resp : ' '}`;
                    }).join('\n');
      break;
    case FormApp.ItemType.CHECKBOX_GRID:
      content = item.asCheckboxGridItem().getRows()
                    .map((row,i) => ({name:row,val:ir.getResponse()[i]}))
                    .filter(row => row.val)
                    .map(row => `${row.name}:\n${row.val.filter(v => v !== '').map(v => `  - ${v}`).join('\n')}`)
                    .join('\n');
      break;
    case FormApp.ItemType.DATETIME:
      pattern = /^(\d{4})-(\d{2})-(\d{2})\s(\d{1,2}):(\d{2})$/;
      [, year, month, day, hour, minute] = pattern.exec(ir.getResponse());
      date = new Date(`${year}-${month}-${day}T${('0' + hour).slice(-2)}:${minute}:00`);
      content = Utilities.formatDate(date,Session.getTimeZone(),"yyyy-MM-dd   h:mm aaa");
      break;
    case FormApp.ItemType.TIME:
      pattern = /^(\d{1,2}):(\d{2})$/;
      [, hour, minute] = pattern.exec(ir.getResponse());
      date = new Date();
      date.setHours(parseInt(hour,10), parseInt(minute,10));
      content = Utilities.formatDate(date,Session.getTimeZone(),"h:mm aaa");
      break;
    case FormApp.ItemType.DURATION:
      content = `${ir.getResponse()}`;
      footer = 'Hrs : Min : Sec';
      break;
    case FormApp.ItemType.SCALE:
      const scale = item.asScaleItem();
      content = ir.getResponse();
      footer = `${scale.getLeftLabel()}(${scale.getLowerBound()}) ... ${scale.getRightLabel()}(${scale.getUpperBound()})`;
      break;
    case FormApp.ItemType.MULTIPLE_CHOICE:
    case FormApp.ItemType.LIST:
    case FormApp.ItemType.DATE:
    case FormApp.ItemType.PARAGRAPH_TEXT:
    case FormApp.ItemType.TEXT:
      content = ir.getResponse();
      break;
    default:
      content = "Unsupported form element";
  }
  return {title:title, content:content, footer:footer};
}
```
