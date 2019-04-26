# VSCODE

download : https://code.visualstudio.com/docs/?dv=linux64_deb


sudo dpkg -i ~/Downloads/code_*.deb; sudo apt -f install


- python PATH : file -> preferences -> settings (User Settings)

```
#/home/adioshun/.vscode/extensions/ms-python.python-2018.6.0/node_modules/vscode-uri/.vscode/settings.json
{
    "python.pythonPath":"/home/cvr/.pyenv/shims/python"
}
```

Press Ctrl+Shift+P to hit “task” to find Tasks: Configure Default Build Task, modify the contents of 
tasks.json (task.json)
    "command":"/home/cvr/.pyenv/shims/python"
    
    
> http://robotslam.blogspot.com/2018/03/open3d-modern-library-for-3d-data.html


## [[Python] Visual Studio Code를 파이썬 IDE 로 이용해 보기](http://egloos.zum.com/mcchae/v/11262544)