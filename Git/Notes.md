# GIT->

Git is a distributed version control system.<br>

Areas-><br>
working directory -> your local machine where you have all the changes.<br>
staging area -> here,files are there with details , so that git knows which files to track and which to ignore.<br>

checksum-> an alphanumeric code with a size of 40(shows only first 7 though) to identify each commit

Commands-><br>
git init-> will directly put you on master<br>
git init -b main-> will create a branch as main will put you there<br>
git add-> adds your file in staging area<br>
git commit -m "msg"-> will commit already staged files<br>
git commit -a -m "msg"-> will stage all and will commit. but untracked or new files won't work.<br>
git diff-> shows the difference between commit and working directory<br>
git diff --staged->shows the difference between commit and staging area<br>
git rm --cached creds.txt-> to untrack or remove a tracked file<br>
git tag v1.1 -m "April Release" -- Will create a tag, mainly used to maintain version<br>