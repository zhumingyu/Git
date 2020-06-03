### chap 2 Git basics

1. init a repostitory in an existing dir

    git init, git add, git commit -m ''

2. track new files

    git add 

3. short status

    git status -s/--short

    - ??: not tracked files 未追踪

    - M:  modified files 修改文件

    - A:  new files have been added to staging area.

* ignoring file p50 *

- *~:忽略所有以～结尾的文件

- *.[oa]:忽略所有以o或a结尾的文件

！lib.a: but do track lib.a,even though you're ignoring .a files above
/TODO:only ignore the TODO file in the current dir,not subdir
build: ignore all files in the build/ dir.
doc/*.txt: ignore doc/notes.txt,but not doc/server/arch.txt
doc/**/*.pdf: ignore all .pdf files in the doc/ dir

4. viewing your staged and unstaged changes

Note:if you've staged all of your changes,will give you no output.

    git diff 

see what you've staged that will go into your next commit

    \todo git diff --cached/--staged 两者同义词,但具体区别待查

5.  commiting your changes

    git commit p54

    git config --global core.editor  配置默认编辑器 p54

6.  skipping the staging area

    git add -a 将所有修改文件自动加入stage,谨慎使用

        eg: git add -a -m 'your commit'

7.  removing files p56

    git rm 

        git rm --cached 删除所有跟踪files,硬盘上并未真正删除

    git rm log/\*.log: 删除log目录下所有以.log结尾的文件 57

    git rm \*~: 删除所有～结尾的文件
    
8.  moving files

    git mv file_from file_to

9.  viewing the commit history

    git log 

        git log -p show the difference introduced in each commit
        git log -2 limit the output to only the last two entries
        git log --stat  see some abbreviated stats for each commit简洁显示
        git log --pretty=oneline    简洁优美显示,针对许多commit
        git log --pretty=format:"%h-%an,%ar:%s" 自定义格式
                %H--commit hash
                %h--简洁commit hash
                %T--tree hash
                %t--同上
                %P--parent hashes
                %p--同上
                %an--author name
                %ae--author email
                %ad--author date
                %ar--author data,relative(相对现在时间作为比较)
                %cn--commiter name
                %ce--commiter email
                %cd--commiter date
                %cr--commiter date,relative
                %s--subject
        git log --pretty=format:"%h %s" --graph 以ASCII graph show your branch and merge history

10. limiting log output p65

    git log --since=2.weeks get the list of commits made in the last two weeks.
            --author 过滤指定author
            --grep search for keywords

11. undoing things

    git commit --amend 修改最后一次commit但并未push的commit

        eg:git commit -m 'initial commit'
           git add forgotten_file
           git commit --amend
    附:撤消最后一次commit,上面命令只是再次修改注释内容
        git reset HEAD~
        
12. unstaging a staged file 66
    git reset HEAD <filename> 谨慎使用

13. unmodifying a modified file
    git checkout -- modified_filename
        eg: git checkout -- README.md

14. showing your remotes
    git remote -v(v:显示URL链接)

15. adding remote repositories
    git remote add <shortname> <url>

16. fetching and pulling from your remotes 71
    git fetch remote-name

17. pushing to your remotes
    git push remote-name branch-name

18. inspecting(检查) a remote
    git remote show remote-name

19. removing and renaming remotes 73
    git remote rename shortname-old shortname-new
        eg:git remote rename pb paul

20. listing your tags
    git tag

21. creating tags
    2种tags:lightweight and annotated(注解)

    annotated tags
        git tag -a v1.1 -m 'my version 1.4'
        -m 添加标签中的详细信息
        git show v1.1 显示tag
        注:tag是在commit后添加

    lightweight tags
轻量标签无需添加-a -m -s 选项
        git tag v1.1-lw
        git show v1.1-lw don't see extra tag information

22. tagging later
后期添加标签
忘记tag之前注释，用如下方法来后期指定 77
    git tag -a -v1.2 9fceb02 

23. sharing tags
    git push origin tagname 默认情况并不push tags
        eg:git push origin v1.1
    git push origin --tags 一次性推送所有tags
/TODO 检出标签，后期待查阅update
24. checking out tags
    git checkout -b branchname tagname

25.git aliases 

创建别名方便操作 p78

-----------------------------------------------------
/NOTE chap3
chap 3 p81
1. creating a new branch 创建分支
    git branch newbranch
查看各分支当前所指的对象
    git log --oneline --decorate: show where branch pointers are pointing
2.  switch branches 切换分支
    git checkout branchname
输出提交历史，各分支指向情况,项目分支分叉情况
    git log --oneline --decorate --graph -all:print out history of commit,show where your branch pointers and how your history has diverged.
/FIXME test issus53
/FIXME fix hotfix
3.  basic branching and merging p90
4.  barch management
    git branch --merged/--no-merged:查看整合/未整合分支
    git branch -d branchname:删除分支 -D 选项强制删除
创建新分支整合后删除基本流程:
        1.git checkout -b branchname
        2.modify
        3.git commit -a -m 'commitname'
        4.git checkout master
        5.git merge branchname
        6.git branch -d branchname

/NOTE 分支流介绍
branching workflows

remote branches
git remote show 
git ls-remote 显示远程引用完整列表 

5.  pushing 
    git push remote branch
6.  tracking branches跟踪分支,可以指定跟踪分支
    git checkout --track origin/serverfix
        serverfix set up to track remote branch serverfix from origin,switched to a new branch 'serverfix'
指定本地名sf的分支指定远程serverfix分支
    git checkout -b sf origin/serverfix
7.  pulling
    deleting remote branches删除远程分支
        git push origin --delete branchname
8.  rebasing中文翻译:变基
   /TODO 仔细研究 p115 
============================================================
/NOTE chap4
chap 4
1.  generating ssh public key p134
============================================================
chap 5
略
============================================================
chap 6 github基本使用 略
============================================================
/NOTE chap7 p253
chap 7  git tools
1.  简洁显示commit简短显示，默认7个字符
    git log --abbrev-commit --pretty=oneline
branch references 分支引用
2.  显示所有commit
    git reflog 查看引用日志
        eg:git show HEAD@{5} HEAD中在5次前所指向的提交
        git show master@{yesterday} 查看master分支在昨天指向了哪个提交
3.  ancestry(祖先) Reference
    git log --pretty=format:'%h %s' --graph
    git show HEAD^
eg:查看在testing分支但未在master分支commit,以后者为准 p259
git log master..testing
4.  interactive staging p262 交互式暂存/TODO以后待查
    git add -i/--interactive
5.  stashing and cleaning
    git stash
    查看stash list
    git stash list
    重新应用
    git stash apply
    应用指定的list,若不指定则使用最近的储藏
    git stash apply stash@{2}
    reapply the staged changes文件变更后,使暂存文件重新暂存
    git stash apply --index/FIXME待查
    remove stash
    git stash drop 删除stash
    apply stash and drop it from stack
    git stash pop
6.  not stash anything that you're already staged with git add.
    git stash --keep-index 显示所有stash的东西
7.  stash any untracked files you've created
    git stash --include-untracked/ -u 储藏任何创建未跟踪的文件
8.  not stash everything that is modified but will prompt you
    git stash --patch不会储藏任何修改过的文件，但会提示哪些会储存，哪些需要保存在工作目录中
9.  creating a branch from a stash
    git stash branch branch_name
10. cleaning your working directory 清理工作目录
    git clean
    remove everything but save it in a stash
    git stash --all移除每一样东西并存放在栈中
    remove any files and any subdirectories that become empty
    git clean -f -d 移除工作目录中所有未跟踪的文件及空的子目录
    do a dry run and tell me what you would have removed
    git clean -d -n
11. search
    git grep -n
            -n:print out the line number where matches
            --count:how many matches there were in each file
            -p:see what method or function that found
12. git log searching p279
13. changing the last commit
    git commit --amend修改最后一次提交信息
14. changing multiple commit messages
    git rebase -i HEAD~3
p298

chap 8 customizing git P381
