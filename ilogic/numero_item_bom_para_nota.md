## Criar propriedade em peça com número do seu item na BOM
Este código pega um determinado item da montagem e transforma o seu número de item da BOM para uma propriedade, assim pode ser chamado (por exemplo, em uma nota de texto no desenho)

No caso o item chama “GRAMPO” mas pode ser modificado é claro.

```
Dim oBOM As BOM = ThisAssembly.Document.ComponentDefinition.BOM
'Make sure the views are enabled
oBOM.StructuredViewEnabled = True
'oBOM.PartsOnlyViewEnabled = True
'---------------------------------
'NOTAS
MsgBox("O Part Number do item deve estar como GRAMPO")
MsgBox("Verificar se BOM está como Structured ou Parts Only e acertar no código")

For Each oBOMRow As BOMRow In oBOM.BOMViews.Item("Structured").BOMRows
'For Each oBOMRow As BOMRow In oBOM.BOMViews.Item("Parts Only").BOMRows
        If TypeOf oBOMRow.ComponentDefinitions(1) Is VirtualComponentDefinition Then
            'MsgBox(oBOMRow.ComponentDefinitions(1).PropertySets("{32853F0F-3444-11D1-9E93-0060B03C1CA6}")("Part Number").Value & ": " & oBOMRow.ItemNumber)
                       If oBOMRow.ComponentDefinitions(1).PropertySets("{32853F0F-3444-11D1-9E93-0060B03C1CA6}")("Part Number").Value = "GRAMPO"
                               'MsgBox(oBOMRow.ItemNumber)
                               iProperties.Value("Custom", "ITEM_GRAMPO") = oBOMRow.ItemNumber
                       End If
        End If
Next
```
