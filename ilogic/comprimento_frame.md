## Cria propriedade com comprimento de peça do **frame generator**
Este código pega a propriedade G_L de um arquivo de frame e transforma em uma propriedade com nome, para ser exposta (como na BOM ou como texto em um desenho)

Note que ela converte unidades imperiais e arredonda. Se for métrico precisa ser adaptado.

```
Dim oAsm As AssemblyDocument = ThisDoc.Document
Dim oTransaction As Transaction = ThisApplication.TransactionManager.StartTransaction(oAsm, "FG base unit to each") 'Make this a single transaction
For Each oDoc As Document In oAsm.AllReferencedDocuments 'Traverse all referenced documents
        If oDoc.DocumentInterests.HasInterest("{AC211AE0-A7A5-4589-916D-81C529DA6D17}") _'Frame generator component
               AndAlso oDoc.DocumentType = DocumentTypeEnum.kPartDocumentObject _'Part
               AndAlso oDoc.IsModifiable _    'Modifiable (not reference skeleton)
               AndAlso oAsm.ComponentDefinition.Occurrences.AllReferencedOccurrences(oDoc).Count > 0 'Exists in assembly (not derived base component)
                       Dim oPartDoc As PartDocument = oDoc
                       Dim model As String = oPartDoc.FullFileName
                       Dim filename = IO.Path.GetFileName(model)
                       teste = iProperties.Value(filename, "Custom", "G_L")
                       medida = Len(teste) - 3
                       teste2 = Mid(teste, 1, medida)
                       'MsgBox(teste2)
                       valor = Math.Round(CDblAny(teste2) * 25.4)
                       'MsgBox(valor)
                       iProperties.Value(filename, "Custom", "Comprimento") = valor
        End If
Next
oTransaction.End 'End the trasaction
```
