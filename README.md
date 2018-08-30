# sandbox-worms-blog
Blog for the Sandbox Worms team

#How to Add Posts

**Assumptions:**


1. Hugo is installed
2. Git is installed

**Steps:**

1. Clone our Blog Post repo. On my computer the repo is cloned/copied on: C:\Repo\sandbox-worms-blog
2. Go to to the directory of the repo from CMD or Bash Ex: C:\Repo\sandbox-worms-blog
3. Git checkout master #to checkout master branch or you can checkout a new or existing branch by: git checkout branch_name
4. Git pull origin master #this could make sure that you have the latest copy of the "master" branch before making any change on your computer. If you have just cloned it, no need to do this as you will have the most up to date copy already.
5. hugo new article/test1.md  #creates a new articlet with the file name test1.md this template offers other types of content, there are links, etc. refer to this https://themes.gohugo.io/bilberry-hugo-theme/#configuration
6. edit the test1.md using a text editor or markdown editor such as MarkdownPad 2.
7. once done, save it. You can take a look at the C:\Repo\sandbox-worms-blog\content\article\team-setup-domain-name.md for example. 
8. git status #should show your new content in red
9. git add content/article/test1.md #adds test1.md specifically. if you have multiple files, i think you can just do git add \.
10. git status #should show your file in green text
11. git commit -m "new post" #if you dont put a comment, git will complain"
12. git push origin master #copies the commited files to the github