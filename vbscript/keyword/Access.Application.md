# Access.Application
## 概要
Access.Application は Access アプリケーション オブジェクトを表す ProgID です。

Access アプリケーション オブジェクトは、通常、Access をオートメーション操作する場合に使用します。

### サンプル
```vbscript
' File : Make9x9TableByAccess.vbs
' Usage : CScript //Nologo Make9x9TableByAccess.vbs
' Description : VBScript から Access を使用して「九九表」を作成するサンプル
Option Explicit

Call Main()

Sub Main()
    Dim acc
    Set acc = CreateObject("Access.Application")
    acc.Visible = True
    
    Dim strFileName
    strFileName = "C:\9x9Table.mdb"
    Call CreateDatabase( acc, strFileName )
    
    Dim strTableName
    Dim strFieldNames
    strTableName = "9x9Table"
    strFieldNames = "F1,F2,F3,F4,F5,F6,F7,F8,F9"
    Dim db
    Set db = acc.CurrentDb
    Call CreateTableWithFields( db, strTableName, strFieldNames )
    
    Dim rs
    Set rs = db.OpenRecordset( strTableName )
    Call Make9x9Table( rs )
End Sub

Sub CreateDatabase( acc, strFileName )
    acc.NewCurrentDatabase strFileName
End Sub

Sub CreateTableWithFields( db, strTableName, strFieldNames )
    Dim tdf
    Set tdf = db.CreateTableDef(strTableName)
    Dim strArray
    strArray = Split( strFieldNames, "," )
    Dim strFieldName
    For Each strFieldName In strArray
        Dim fld
        Set fld = tdf.CreateField( strFieldName, 4 ) ' Const dbLong = 4
        tdf.Fields.Append fld
    Next
    db.TableDefs.Append tdf
End Sub

Sub Make9x9Table( rs )
    Dim x
    Dim y
    For y = 1 To 9
        rs.AddNew
        For x = 1 To 9
            rs(x-1) = x * y  ' rs(i) : Field Index
        Next
        rs.Update
    Next
End Sub
```

### 実行結果

|F1 |F2 |F3 |F4 |F5 |F6 |F7 |F8 |F9 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|1|2|3|4|5|6|7|8|9|
|2|4|6|8|10|12|14|16|18|
|3|6|9|12|15|18|21|24|27|
|4|8|12|16|20|24|28|32|36|
|5|10|15|20|25|30|35|40|45|
|6|12|18|24|30|36|42|48|54|
|7|14|21|28|35|42|49|56|63|
|8|16|24|32|40|48|56|64|72|
|9|18|27|36|45|54|63|72|81|

### 参考情報
- Microsoft Access オブジェクト モデル - MSDN Library
- Microsoft Access データベース破損のトラブルシューティング方法 (2005/09/14 Rev:4.0) - Microsoft KB
- Access 2000 で Jet 4.0 データベースの動作環境を最適に保つ方法 (2007/05/21 Rev:5.0) - Microsoft KB
- 複数のバージョンの Office を Office 2003 と併用することに関する情報 - Microsoft KB
- 複数のバージョンの Office がインストールされている場合の Office オートメーションについて - Microsoft KB
- Office アプリケーションのパスを調べる方法 - Microsoft KB
