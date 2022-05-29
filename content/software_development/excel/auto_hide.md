## Excel: auto row & column hiding

**Automatically hide rows and columns throughout a workbook in an instant...**

---

When managing a large number of reports, the process of hiding and un-hiding
rows and columns can become laborious. So writing some code to do this
automatically is the obvious step.

The first step is to create a row and a column within each report that denotes
what to hide and what to show. In row `1`, fill each cell with `TRUE` (show this
cell's column) or `FALSE` (hide this cell's column). Do this until you have
covered all the columns in use on this worksheet. Now assign a
worksheet-specific name to this range of booleans - `ColumnHide`. Repeat the
same process in column `A` to deal with all the used rows in the worksheet -
give this the worksheet-specific name `RowHide`.

When automatically showing and hiding, there are two important elements that
reduce the time it takes...

Firstly, to minimise the number of show/hide actions, these should only be
performed once rather than incrementally through each row or column. We achieve
this by using the VBA Union method to create a single range on which to act. For
example:

```vb
For Each c In ws.Range("ColumnHide")
  If c.Value Then Set Colshowrange = Union(Colshowrange, c)
Next c 
Colshowrange.EntireColumn.Hidden = False
```

Secondly, there can be some slowdown caused by Excel updating the pagebreaks as
this process takes place. This could happen up to 4 times for a single
worksheet (show columns, hide columns, show rows, hide rows), so we use the
`DisplayPageBreaks` property to avoid this delay:

```vb
ws.DisplayPageBreaks = False
```

The real utility of the method we are designing is that by using the names
`RowHide` and `ColumnHide` we are able to easily feed new ranges into the macro
by having it cycle through all named ranges and then add any appropriate
worksheets to a collection. Later on we can refer to the named ranges via the
worksheet indices stored in this collection:

```vb
For Each Nm In ThisWorkbook.Names
  If Nm.Name Like "*!ColumnHide" Or Nm.Name Like "*!RowHide" Then
    On Error Resume Next
    collShowHide.Add Nm.Parent.Index, CStr(Nm.Parent.Index)
    On Error GoTo 0
  End If
Next Nm
```

As you may have spotted, by originally using cell values of `TRUE` or `FALSE`,
we can also formulaically determine whether a row/column is shown/hidden.
Perhaps some rows are only appropriate when viewing certain months, for
instance. In this situation we would use a worksheet-change macro to call row &
column hiding whenever a new month is selected.

[Click here](_static/Row_Column_Hide.xlsm) to download a working example where certain rows and 
columns are only visible if the Genus _Compsognathus_ is selected on the 
worksheet. Macros must be enabled for this example to function. Below is the 
code in full:

```vb
Sub RowColHide()

  Dim c As Range
  Dim i As Integer
  Dim ws As Worksheet
  Dim Nm As Name
  Dim collShowHide As New Collection
  
  Dim Colhiderange As Range
  Dim Colshowrange As Range
  Dim Rowhiderange As Range
  Dim Rowshowrange As Range
  
'identify worksheets that need the row or column hiding treatment
  For Each Nm In ThisWorkbook.Names
    If Nm.Name Like "*!ColumnHide" Or Nm.Name Like "*!RowHide" Then
      On Error Resume Next
      collShowHide.Add Nm.Parent.Index, CStr(Nm.Parent.Index)
      On Error GoTo 0
    End If
  Next Nm
  
  Application.Calculation = xlCalculationManual
  Application.ScreenUpdating = False
  
'cycle through worksheets that need the row or column hiding treatment
    For i = 1 To collShowHide.Count
      Set ws = ThisWorkbook.Sheets(collShowHide(i))
      Application.StatusBar = "Auto-hiding... " && i

'preventing pagebreak updates while hiding takes place
      ws.DisplayPageBreaks = False

'clear the range variables before continuing
      Set Colhiderange = Nothing
      Set Colshowrange = Nothing 
      Set Rowhiderange = Nothing
      Set Rowshowrange = Nothing

'create ranges of columns to hide and columns to show
      On Error GoTo Continue1
      For Each c In ws.Range("ColumnHide")
        On Error GoTo 0
        If c.Value Then
          If Colshowrange Is Nothing Then
            Set Colshowrange = c
          Else
            Set Colshowrange = Union(Colshowrange, c)
          End If
        Else
          If Colhiderange Is Nothing Then
            Set Colhiderange = c
          Else
            Set Colhiderange = Union(Colhiderange, c)
          End If
        End If
      Next c
    
'hide and show the appropriate columns - all at once via the created ranges
      Colshowrange.EntireColumn.Hidden = False
      Colhiderange.EntireColumn.Hidden = True
      
      GoTo Continue3

Continue1:
      Resume Continue3

Continue3:
'create ranges of rows to hide and rows to show
      On Error GoTo Continue2
      For Each c In ws.Range("RowHide")
        On Error GoTo 0
        If c.Value Then
          If Rowshowrange Is Nothing Then
            Set Rowshowrange = c
          Else
            Set Rowshowrange = Union(Rowshowrange, c)
          End If
        Else
          If Rowhiderange Is Nothing Then
            Set Rowhiderange = c
          Else
            Set Rowhiderange = Union(Rowhiderange, c)
          End If
        End If
      Next c

'hide and show the appropriate rows - all at once via the created ranges
      Rowshowrange.EntireRow.Hidden = False
      Rowhiderange.EntireRow.Hidden = True
      
      GoTo Continue4

Continue2:
      Resume Continue4

Continue4:
      On Error GoTo 0
      ws.DisplayPageBreaks = True
    Next i

  Application.ScreenUpdating = True
  Application.Calculation = xlCalculationAutomatic
  Application.StatusBar = "Auto-hiding..."
  DoEvents
  Application.StatusBar = False

End Sub
```
