---
title: TSCLIB.DLL函数库使用说明
date: 2019-12-12 14:23:41
tags: CSHARP
---
2018年09月29日 14:24:19 小虎是小蜗牛 阅读数：1159
TSCLIB.DLL函式库使用说明方面的问题。注意：使用动态库TSCLIB.DLL前，安装TSC条码印表机驱动。

### 1. openport(a) 
说明:指定电脑端的输出端
参数: 
a:单机列印时，请指定印表机驱动程式名称，例如: TSC CLEVER TTP-243     这里我是用的是GP-9025T
   若连接印表机伺服器，请指定伺服器路径及共用印表机名称，例如: \\SERVER\TTP243 

### 2. closeport() 
说明:关闭指定的电脑端输出埠 
参数:无 

### 3. setup(a,b,c,d,e,f,g) 
说明:设定标签的宽度、高度、列印速度、列印浓度、感应器类别、gap/black mark垂直间距、gap/black mark偏移距离) 
参数: 
a:字串型别，设定标签宽度，单位mm 
b:字串型别，设定标签高度，单位mm 
c:字串型别，设定列印速度，(列印速度随机型不同而有不同的选项) 
1.0:每秒1.0吋列印速度 
1.5:每秒1.5吋列印速度 
2.0:每秒2.0吋列印速度 
3.0:每秒3.0吋列印速度 
4.0:每秒4.0吋列印速度 
5.0:每秒5.0吋列印速度 
6.0:每秒6.0吋列印速度 
d:字串型别，设定列印浓度， 
0~15，数字愈大列印结果愈黑 
e:字串型别，设定使用感应器类别 
0表示使用垂直间距感测器(gap sensor) 
1表示使用黑标感测器(black mark sensor) 
f:字串型别，设定gap/black mark垂直间距高度，单位: mm 
g:字串型别，设定gap/black mark偏移距离，单位: mm，此参数若使用一般标签时均设为0 


### 4. clearbuffer() 
说明:清除 
参数:无 

### 5. barcode(a,b,c,d,e,f,g,h,I) 
说明:使用条码机内建条码列印 
参数: 
a:字串型别，条码X方向起始点，以点(point)表示。 
(200 DPI，1点=1/8 mm, 300 DPI，1点=1/12 mm) 
b:字串型别，条码Y方向起始点，以点(point)表示。 
(200 DPI，1点=1/8 mm, 300 DPI，1点=1/12 mm) 
c:字串型别， 
128 Code 128, switching code subset A, B, C 
automatically 
128M Code 128, switching code subset A, B, C 
manually. 
EAN128 Code 128, switching code subset A, B, C 
automatically 
25 Interleaved 2 of 5 
25C Interleaved 2 of 5 with check digits 
39 Code 39 
39C Code 39 with check digits 
93 Code 93 
EAN13 EAN 13 
EAN13+2 EAN 13 with 2 digits add-on 
EAN13+5 EAN 13 with 5 digits add-on 
EAN8 EAN 8 
EAN8+2 EAN 8 with 2 digits add-on 
EAN8+5 EAN 8 with 5 digits add-on 
CODA Codabar 
POST Postnet 
UPCA UPC-A 
UPCA+2 UPC-A with 2 digits add-on 
UPCA+5 UPC-A with 5 digits add-on 
UPCE UPC-E 
UPCE+2 UPC-E with 2 digits add-on 
UPCE+5 UPC-E with 5 digits add-on 

d:字串型别，设定条码高度，高度以点来表示 
e:字串型别，设定是否列印条码码文 
0:不列印码文 
1:列印码文 
f:字串型别，设定条码旋转角度 
0:旋转0度 
90:旋转90度 
180:旋转180度 
270:旋转270度 
g:字串型别，设定条码窄bar比例因子，请参考TSPL使用手册 
h:字串型别，设定条码窄bar比例因子，请参考TSPL使用手册 
I:字串型别，条码内容 

### 6. printerfont(a,b,c,d,e,f,g) 
说明:使用条码机内建文字列印 
参数: 
a:字串型别，文字X方向起始点，以点(point)表示。 
(200 DPI，1点=1/8 mm, 300 DPI，1点=1/12 mm) 
b:字串型别，文字Y方向起始点，以点(point)表示。 
(200 DPI，1点=1/8 mm, 300 DPI，1点=1/12 mm) 
c:字串型别，内建字型名称，共12种。 
1: 8*/12 dots 
2: 12*20 dots 
3: 16*24 dots 
4: 24*32 dots 
5: 32*48 dots 
TST24.BF2:繁体中文24*24 
TST16.BF2:繁体中文16*16 
TTT24.BF2:繁体中文24*24 (电信码) 
TSS24.BF2:简体中文24*24 
TSS16.BF2:简体中文16*16 
K:韩文24*24 
L:韩文16*16 
d:字串型别，设定文字旋转角度 
0:旋转0度 
90:旋转90度 
180:旋转180度 
270:旋转270度 
e:字串型别，设定文字X方向放大倍率，1~8 
f:字串型别，设定文字X方向放大倍率，1~8 
g:字串型别，列印文字内容 

### 7. sendcommand(command) 
说明:送内建指令到条码印表机 
参数:详细指令请参考TSPL 

### 8. printlabel(a,b) 
说明:列印标签内容 
参数: 
a:字串型别，设定列印标签式数(set) 
b:字串型别，设定列印标签份数(copy) 

### 9. downloadpcx(a,b) 
说明:下载单色PCX格式图档至印表机 
参数: 
a:字串型别，档案名(可包含路径) 
b:字串型别，下载至印表机记忆体内之档名(请使用大写档名) 

### 10. formfeed() 
说明:跳页，该函式需在setup后使用 
参数:无 

### 11. nobackfeed() 
说明:设定纸张不回吐 
参数:无 

### 12. windowsfont(a,b,c,d,e,f,g,h) 
说明:使用Windows TTF字型列印文字 
参数: 
a:整数型别，文字X方向起始点，以点(point)表示。 
b:整数型别，文字Y方向起始点，以点(point)表示。 
c:整数型别，字体高度，以点(point)表示。 
d:整数型别，旋转角度，逆时钟方向旋转 
0 -> 0 degree 
90-> 90 degree 
180-> 180 degree 
270-> 270 degree 
e:整数型别，字体外形 
0->标准(Normal) 
1->斜体(Italic) 
2->粗体(Bold) 
3->粗斜体(Bold and Italic) 
f:整数型别,底线 
0->无底线 
1->加底线 
g:字串型别，字体名称。如: Arial, Times new Roman,细名体,标楷体 
h:字串型别，列印文字内容。 

### 13. about() 
说明:显示DLL版本号码 
参数:无 
  
### Visual Basic 5.0, 6.0 for Win95, 98范例 

		Private Declare Sub openport Lib "c:\windows\system\tsclib.dll" ( 
		ByVal PrinterName As String) 
		Private Declare Sub closeport Lib "c:\windows\system\tsclib.dll" () 
		Private Declare Sub sendcommand Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal command As String) 
		Private Declare Sub setup Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal LabelWidth As String, _ 
		                                     ByVal LabelHeight As String, _ 
		                                     ByVal Speed As String, _ 
		                                     ByVal Density As String, _ 
		                                     ByVal Sensor As String, _ 
		                                     ByVal Vertical As String, _ 
		                                     ByVal Offset As String) 
		Private Declare Sub downloadpcx Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal Filename As String, _ 
		                                     ByVal ImageName As String) 
		Private Declare Sub barcode Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal X As String, _ 
		                                     ByVal Y As String, _ 
		                                     ByVal CodeType As String, _ 
		                                     ByVal Height As String, _ 
		                                     ByVal Readable As String, _ 
		                                     ByVal rotation As String, _ 
		                                     ByVal Narrow As String, _ 
		                                     ByVal Wide As String, _ 
		                                     ByVal Code As String) 
		Private Declare Sub printerfont Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal X As String, _ 
		                                     ByVal Y As String, _ 
		                                     ByVal FontName As String, _ 
		                                     ByVal rotation As String, _ 
		                                     ByVal Xmul As String, _ 
		                                     ByVal Ymul As String, _ 
		                                     ByVal Content As String) 
		Private Declare Sub clearbuffer Lib "c:\windows\system\tsclib.dll" () 
		Private Declare Sub printlabel Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal NumberOfSet As String, _ 
		                                     ByVal NumberOfCopy As String) 
		Private Declare Sub formfeed Lib "c:\windows\system\tsclib.dll" () 
		Private Declare Sub nobackfeed Lib "c:\windows\system\tsclib.dll" () 
		Private Declare Sub windowsfont Lib "c:\windows\system\tsclib.dll" ( _ 
		                                     ByVal X As Integer, _ 
		                                     ByVal Y As Integer, _ 
		                                     ByVal fontheight As Integer, _ 
		                                     ByVal rotation As Integer, _ 
		                                     ByVal fontstyle As Integer, _ 
		                                     ByVal fontunderline As Integer, _ 
		                                     ByVal FaceName As String, _ 
		                                     ByVal TextContent As String) 


		Private Sub Command1_Click() 
		Call openport(“TSC TTP-243”) 
		‘Call openport(“\\server\TTP243”) 
		Call setup("100", "100", "3", "10", "0", "0", "0") 
		Call clearbuffer 
		Call downloadpcx("c:\UL.PCX", "UL.PCX") 
		‘Call downloadpcx(App.Path + "\UL.PCX", "UL") 
		'ComText = "PUTPCX 100,400," + Chr(34) + "UL" + Chr(34) 
		‘ Call sendcommand(ComText) 
		Call printerfont("10", "10", "4", "0", "1", "1", "TEST PRINTOUT") 
		Call barcode("10", "80", "39", "96", "1", "0", "2", "4", "0987654321") 
		Call sendcommand("PUTPCX 100,250,""UL.PCX""") 
		Call sendcommand("BAR 400,200,300,100") 
		Call sendcommand("BOX 10,300,300,300,5") 
		Call windowsfont(10, 500, 80, 0, 0,0, "标楷体", "标楷体字型") 
		Call printlabel("1", "1") 
		Call closeport 
		End Sub 
  
### FoxPro范例 
		declare openport in c:\windows\system\tsclib.dll string 
		declare closeport in c:\windows\system\tsclib.dll 
		declare sendcommand in c:\windows\system\tsclib.dll 
		declare setup in c:\windows\system\tsclib.dll 
		  string,string,string,string,string,string,string 
		declare downloadpcx in c:\windows\system\tsclib.dll 
		string, string 
		declare barcode in c:\windows\system\tsclib.dll 
		  string,string,string,string,string,string,string,string,string 
		declare printerfont in c:\windows\system\tsclib.dll 
		string,string,string,string,string,string,string 

		declare clearbuffer in c:\windows\system\tsclib.dll 
		declare printlabel in c:\windows\system\tsclib.dll string,string 
		declare formfeed in c:\windows\system\tsclib.dll 
		declare nobackfeed in c:\windows\system\tsclib.dll 
		declare windowsfont in c:\windows\system\tsclib.dll 
		  integer,integer,integer,integer,integer,integer,integer,string,string 

		openport(“TSC TTP-243”) 
		setup("32","25","2","10","0","0","0") 
		clearbuffer() 
		barcode("10","0","EAN13","80","1","0","2","4","123456789012") 
		windowsfont(10,100,50,0,0,0,"标楷体","标楷体字型") 
		printlabel("1","1") 
		closeport() 
  
### Delphi宣告范例 
		procedure openport(PrinterName:pchar);stdcall;far; external 'tsclib.dll'; 
		procedure closeport; external ‘tsclib.dll’; 
		procedure sendcommand(Command:pchar);stdcall;far;external 'tsclib.dll'; 
		procedure setup(LabelWidth, LabelHeight, Speed, Density, Sensor, Vertical, 
		    Offset:pchar);tsdcall; far; external 'tsclib.dll'; 
		procedure downloadpcx(Filename,ImageName:pchar);stdcall;far; 
		   external ‘tsclib.dll’; 
		procedure barcode(X, Y, CodeType, Height, Readable, Rotation, Narrow, 
		  Wide, Code :pchar); stdcall; far; external 'tsclib.dll'; 
		procedure printerfont(X, Y, FontName, Rotation, Xmul, Ymul, Content:pchar); 
		stdcall;far; external ‘tsclib.dll’; 
		procedure clearbuffer; external ‘tsclib.dll’; 
		procedure printlabel(NumberOfSet, NumberOfCopoy:pchar);stdcall; far; 
		   external ‘tsclib.dll’; 
		procedure formfeed;external ‘tsclib.dll’; 
		procedure nobackfeed; external ‘tsclib.dll’ 
		procedure windowsfont (X, Y, FontHeight, Rotation, FontStyle, 
		   FontUnderline : integer; FaceName, 
		   TextContect:pchar);stdcall;far;external 'tsclib.dll'; 

请注意:函数名称务必使用小写字母 
 
		openport('TSC TTP/TDP-243(E)'); 
		//sendcommand('Abcdvsafsfs'); 
		Setup('50', '30', '3', '10', '0', '0', '0'); 
		SendCommand('DIRECTION 0'); 
		ClearBuffer(); 
		WindowsFont(190,18, 45, 0, 0,0, 'Arial', 'ACC'); 
		printlabel(‘1’,’1'); 
		closeport; 


### PB全局函数声明： 

		Function long openport (string a) library"tsclib.dll" 
		Function long closeport () library"tsclib.dll" 
		Function long setup (string a, string b, string c ,string d, string e, string f, string g) library "tsclib.dll" 
		Function long clearbuffer () library"tsclib.dll" 
		Function long barcode (string ss1, string ss2, string ss3, string ss4, string ss5, string ss6, string ss7 ,string ss8, string ss9) library"tsclib.dll" 
		Function long printlabel (string ss1, string ss2) library"tsclib.dll" 
		Function long windowsfont (long a, long b, long c, long d,long e ,long f, string g ,string h) library "tsclib.dll" 
		Function long printerfont(string a,string b,string c,string d,string e,string f,string g) library "tsclib.dll" 

简单例程： 

		openport (“TSC TTP/TDP-243(E)”) 
		setup(“32”，“25”，“2”，“10”，“0”，“0”，“0”) 
		clearbuffer () 
		barcode(“10”, “0”, “EAN13”, “80”, “1”, “0”, “2”，“4”，“123456789012”) 
		windowsfont(10,100,50,0,0,0，“宋体”， “标楷体字型”) 
		printlabel(“1”, “1”) 
		closeport()
