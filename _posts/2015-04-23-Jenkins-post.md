---
layout: post
title: "使用jenkins打包iOS遇到的问题"
date: 2012-05-22
tags: [iOS]
comments: true
---

近几天研究了一下使用jenkins打包iOS,遇到了很多的问题,现在把遇到的问题记录下来只当是做个笔记.

# 1.安装jenkins
可以从官网下载pkg包进行安装,装完自动运行jenkins,安装目录会在/用户/共享/jenkins,但是这样安装jenkins会遇到权限问题,从网上找修改安装目录也没有成功,于是卸载改用homebrew安装.
终端运行brew install jenkins
jenkins会被安装到/usr/local/Cellar/jenkins
运行jenkins launchctl load /usr/local/Cellar/jenkins/1.600/homebrew.mxcl.jenkins.plist
停止jenkins launchctl unload /usr/local/Cellar/jenkins/1.600/homebrew.mxcl.jenkins.plist
也可以执行/用户/用户名/资源库/LaunchAgents/homebrew.mxcl.jenkins.plist 然后会在/用户/用户名/.jenkins下生成主目录 如果执行的时候是sudo执行的,就会在/var/root/.jenkins下生成主目录

# 2.xcode插件
安装xcode插件,重启jenkins.如果使用构建本地项目,源码管理选择none,使用svn选择Subversion,Local module directory默认是在该job下的workspace,如果要改,要使用相对路径.是当前目录,..是上层目录.
如果工程文件是workspace,在配置xcode的时候要填写Xcode Schema File,Xcode Workspace File和Xcode Project Directory,如果是刚从svn服务器上copy下来的要先用xcode打开一次,否则直接在jenkins打包会出现在workspace no schema的情况.

# 3.远程访问jenkins
如果是sudo开启的jenkins远程访问没有问题,如果没有用sudo开启,默认是不能访问,可以关掉防火墙进行访问

