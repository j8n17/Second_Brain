# 주간 회고
<%*
/* utils */
const linkTailRegex = /\.md\|[^\]]+/
const removeLinkTail = s => new String(s).replace(linkTailRegex, '')

/* config */
const Inbox = [
  /Inbox\/Real_Inbox/, /Inbox\/Project/,
]
const Clippings = [
  /Clippings/,
]
const In_progress = [
  /Inbox\/ToDo/,
]
const Complete = [
  /Study/, /Obsidian/, /Zotero/,
]

/* main logic */
const dateFormat = 'yyyy-MM-dd'
const dv = this.DataviewAPI
let startDate = await tp.system.prompt('Start date', dv.date('today').minus(dv.duration('7 day')).toFormat(dateFormat))
if (!startDate) {
  return
}
tR += startDate

startDate = dv.date(startDate)

let endDate = await tp.system.prompt('End date (optional)', dv.date('today').toFormat(dateFormat))
console.log('endDate', endDate)
if (endDate) {
  endDate = dv.date(endDate)
} else {
  endDate = startDate.plus(dv.duration('1 day'))
}

console.log('dv', dv, startDate, endDate)
%>

### 특이사항

---
# 주간 작성 노트


## Inbox

<%*
const out = dv.pages()
  .where(p => {
    if (Inbox.filter(v => v.test(p.file.path)).length == 0) {
      return false
    }
    return p.file.mtime >= startDate && p.file.mtime < endDate
  })
  .map(p => {
    return `- ${removeLinkTail(p.file.link)}`
  })
  .join("\n")
tR += out
%>

### Clippings

<%*
const out_2 = dv.pages()
  .where(p => {
    if (Clippings.filter(v => v.test(p.file.path)).length == 0) {
      return false
    }
    return p.file.mtime >= startDate && p.file.mtime < endDate
  })
  .map(p => {
    return `- ${removeLinkTail(p.file.link)}`
  })
  .join("\n")
tR += out_2
%>

## In Progress

<%*
const out_3 = dv.pages()
  .where(p => {
    if (In_progress.filter(v => v.test(p.file.path)).length == 0) {
      return false
    }
    return p.file.mtime >= startDate && p.file.mtime < endDate
  })
  .map(p => {
    return `- ${removeLinkTail(p.file.link)}`
  })
  .join("\n")
tR += out_3
%>

## Complete

<%*
const out_4 = dv.pages()
  .where(p => {
    if (Complete.filter(v => v.test(p.file.path)).length == 0) {
      return false
    }
    return p.file.mtime >= startDate && p.file.mtime < endDate
  })
  .map(p => {
    return `- ${removeLinkTail(p.file.link)}`
  })
  .join("\n")
tR += out_4
%>

