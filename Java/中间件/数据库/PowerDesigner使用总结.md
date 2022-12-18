# PowerDesigner使用总结

---

# 一、实用脚本

1、将`comment`转换为`name`

```vbscript
Option   Explicit
ValidationMode   =   True
InteractiveMode   =   im_Batch
Dim tabname
Dim   mdl   '   the   current   model

'如果tabname留空，则对所有表进行操作
tabname = ""
'   get   the   current   active   model
Set   mdl   =   ActiveModel
If   (mdl   Is   Nothing)   Then
      MsgBox   "There   is   no   current   Model "
ElseIf   Not   mdl.IsKindOf(PdPDM.cls_Model)   Then
      MsgBox   "The   current   model   is   not   an   Physical   Data   model. "
Else
      ProcessFolder   mdl
End   If

'   This   routine   copy   name   into   comment   for   each   table,   each   column   and   each   view
'   of   the   current   folder
Private   sub   ProcessFolder(folder)
      Dim   Tab   'running     table
      for   each   Tab   in   folder.tables
            dim cando
            cando = 1
            if tabname<>"" and Instr(tab.name, tabname) = 0 then
               cando = 0
            end if
            'msgBox tab.name
            'msgBox cando
            if   not   tab.isShortcut and cando=1   then
                  'msgBox "即将对如下表操作："&tab.name
                  tab.comment   =  split(tab.name ,"#")(0)  
                  Dim   col   '   running   column
                  for   each   col   in   tab.columns
                        col.comment=   split(col.name,"#")(0)
                        'msgBox col.comment
                  next
            end   if
      next

      Dim   view   'running   view
      for   each   view   in   folder.Views
            if   not   view.isShortcut   then
                  view.comment   =   split(view.name,"#")(0)
            end   if
      next

      '   go   into   the   sub-packages
      Dim   f   '   running   folder
      For   Each   f   In   folder.Packages
            if   not   f.IsShortcut   then
                  ProcessFolder   f
            end   if
      Next
 

end   sub
```



# 二、常用操作

### 字段操作

1、设置非空：`mandatory`

2、自增：`identity`

3、