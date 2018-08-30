# sandbox-worms-blog
Blog for the Sandbox Worms team

#How to Add Posts

Assumptions: 
1. Hugo is installed
2. git is installed

Steps:
1. Clone our Blog Post repo.
2. On my computer the repo is cloned/copied on: C:\Repo\sandbox-worms-blog
3. On CMD
cd C:\Repo\sandbox-worms-blog
git checkout master #to checkout master branch or you can checkout a new or existing branch by: git checkout branch_name
git pull origin master #this could make sure that you have the latest copy of the "master" branch before making any change on your computer. If you have just cloned it, no need to do this as you will have the most up to date copy already.
hugo new article/test1.md  #creates a new articlet with the file name test1.md Hugo or this template offers other types of content, there are links, etc. refer to this https://themes.gohugo.io/bilberry-hugo-theme/#configuration
edit the test1.md using a text editor.
once done, save it. You can take a look at the C:\Repo\sandbox-worms-blog\content\article\team-setup-domain-name.md for example. 
git status #should show your new content in red
git add content/article/test1.md #adds test1.md specifically. if you have multiple files, i think you can just do git add .
git status #should show your file in green text
git commit -m "new post" #if you dont put a comment, git will complain"
git push origin master #copies the commited files to the githu