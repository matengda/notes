github.com/Microsoft/Terminal

```json
// C:\Users\sa\AppData\Local\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json

{
    "defaultProfile": "{bd8fb4d7-511c-4e60-98df-c64140a36921}",
    "copyOnSelect": true, // 自动复制
    "requestedTheme": "system", // 设置亮暗主题
    // 以上都是全局属性
    "profiles":
    {
        "list":
        /*
            环境入口
            一个列表，其中包含有 Windows Terminal 下拉菜单中唤起的各种环境（比如打开                   PowerShell 环境）与各种环境里 Windows Terminal 的显示方案（比如字体、背景、色             彩方案等）
        */
        [
            {
                "guid": "{bd8fb4d7-511c-4e60-98df-c64140a36921}",
                "name": "pwsh",
                "commandline": "C:\\Program Files\\PowerShell\\7\\pwsh.exe -nologo",
                // "source": "Windows.Terminal.PowershellCore",
                "useAcrylic" : true, // 使用亚克力效果
                "acrylicOpacity" : 0.75, // 亚克力效果不透明度,值越大越不透明
                "startingDirectory": "~", // 终端默认路径
                "hidden": false
            },
            {
                // Make changes here to the powershell.exe profile.
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": true
            },
            {
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "name": "命令提示符",
                "commandline": "cmd.exe",
                "hidden": false
            },
        ]
    },
    
    "schemes": [],
    
    "keybindings":
    [
        { "command": "find", "keys": "ctrl+shift+f" },
    ]
}
```

