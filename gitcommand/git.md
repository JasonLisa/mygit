# **git命令**

**1.本地创建版本库后,同步到github步骤**

	create a new repository on the command line


>
>echo # LinuxAndGit_command >> README.md
>git init  //初如化当前目录为git 库。
>
>git add README.md
>
>git commit -m "first commit"
>
>git remote add origin https://github.com/JasonLisa/LinuxAndGit_command.git
>
>git push -u origin master

	…or push an existing repository from the command line

>git remote add origin https://github.com/JasonLisa/LinuxAndGit_command.git
>
>git push -u origin master

	…or import code from another repository

>You can initialize this repository with code from a Subversion, Mercurial, or TFS project.