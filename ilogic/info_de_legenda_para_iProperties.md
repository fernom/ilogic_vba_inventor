```
' Este script lê informações salvas em campos de textos de legendas de desenhos (ou blocos)
' e as insere em iProperties do modelo (pode ser alterado para inserir no desenho também)
' Baseado em https://forums.autodesk.com/t5/inventor-ilogic-and-vb-net-forum/ilogic-rule-to-get-a-propmted-text-value-and-populate/td-p/6396213
Dim oDoc As Document
oDoc = ThisApplication.ActiveDocument
Dim oSheet As Sheet
oSheet = oDoc.ActiveSheet
' o trecho abaixo referencia a legenda do desenho. Alterar se for para outro tipo de bloco
Dim oTB1 As TitleBlock
oTB1 = oSheet.TitleBlock
Dim titleDef As TitleBlockDefinition
titleDef = oTB1.Definition
' para cada campo de texto da legenda, definir um oPrompt
Dim oPrompt1 As TextBox
Dim oPrompt2 As TextBox

' o laço abaixo passa por todos os campos de texto,
' verifica se o prompt é o buscado, e lê o valor salvo.
For Each defText As TextBox In titleDef.Sketch.TextBoxes
	On Error Resume Next	
	If defText.Text = "NUM. DESENHO" Then
		oPrompt1 = defText
	ElseIf defText.Text = "TITULO 3" Then
		oPrompt2 = defText
	End If
Next

' Define as variáveis para receber o texto lido
Dim numero, descricao As String
numero = oTB1.GetResultText(oPrompt1)
descricao = oTB1.GetResultText(oPrompt2)

' Abaixo seleciona o modelo (que aparece na primeira vista do desenho)
' e busca o nome de arquivo para acessar as iProperties
Dim modelDoc = ThisDrawing.ModelDocument.FullFileName
Dim filename = IO.Path.GetFileName(modelDoc)

' Grava o valor lido nas iProperties do arquivo de modelo.
iProperties.Value(filename, "Project", "Stock Number") = numero
iProperties.Value(filename, "Project", "Description") = descricao
```
