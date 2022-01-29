## Nota de peso no desenho (somando da lista de peças)
Este surgiu por causa de uma necessidade específica: como eu estava editando arquivos do frame generator para colocar furação, o peso indicado na lista (puxado dos arquivos) estava diferente do peso indicado pelo assembly.

A solução que encontrei foi pegar os pesos na lista de materiais e somar.

```
' Set a reference to the drawing document.
' This assumes a drawing document is active.
Dim oDrawDoc As DrawingDocument
oDrawDoc = ThisApplication.ActiveDocument

' Set a reference to the first parts list on the active sheet.
' This assumes that a parts list is on the active sheet.

Dim oPartList As PartsList
oPartList = oDrawDoc.ActiveSheet.PartsLists.Item(1)
Dim tudo As Double
tudo = 0
' Iterate through the contents of the parts list.
Dim i As Long
For i = 1 To oPartList.PartsListRows.Count
        oCell = oPartList.PartsListRows.Item(i).Item("PESO")
        teste = CDblAny(oCell.Value)
        tudo = tudo + teste
        'MsgBox(tudo)
Next
'MsgBox("totalizado: " + CStr(tudo))
iProperties.Value("Custom", "PESO_UNITARIO") = Ceil(tudo)
```
