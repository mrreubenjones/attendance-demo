# Git Branching and Merging Summary Notes
Working with branches in Git is very powerful as it forms the basis of collaboration. It also enables developers to try features and work on features without causing errors to the main working code.

## How to Create a Branch
To branch in Git simply use `checkout` command with `-b` option:
```shell
git checkout -b my_awesome_branch
```
This will create a new branch and move you to it, so you should see:
```shell
Switched to a new branch 'my_awesome_branch'
```

## Checking the Branch you're on
To check what branch you're on, you can simply type:
```shell
git status
```
You will see a print out like:
```shell
On branch my_awesome_branch
nothing to commit, working directory clean
```
You can also do:
```shell
git branch -v
```
You will see a print out like:
```shell
master            aa042dd 1. answer to q11 2. removed semicolons in q7
* my_awesome_branch aa042dd 1. answer to q11 2. removed semicolons in q7
```

## Merging Branches
To merge a branch into another, first make sure you are on the branch that you want to merge to. Let's say you want to merge the `my_awesome_branch` branch into master. First make sure you're on the master brach:
```shell
git checkout master
```
Notice that if the branch already exists, no need to use the `-b` option.

After you're on the branch that you want to merge to, use the `git merge` command:
```shell
git merge my_awesome_branch
```
This will merge the `my_awesome_branch` branch into the `master` branch. You will see an output like:
```
Fast-forward
 q8/blog.rb | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
```
The example above shows a case of `Fast-forward` merging where there are no conflicts between commits so Git just adds the new commits to the top of the master branch.

## Solving Conflicts
Sometimes you merge a branch with another and there are conflicts within the same lines so it's not a simple `Fast-forward`. This is a case when Git will show you conflicts and you will have to solve the conflicts before continuing. If you try to merge:
```shell
git merge my_awesome_branch
```
You get:
```
Auto-merging q6/even.rb
CONFLICT (content): Merge conflict in q6/even.rb
Automatic merge failed; fix conflicts and then commit the result.
```
If you open up `q6/even.rb` you see something like:
```ruby
<<<<<<< HEAD
def even(x)
  a = Array.new
  i = 0

  until a.length == x do
=======
def even(number)
  a = Array.new
  i = 0

  until a.length == number do
>>>>>>> my_awesome_branch
```
From the code above you can note that `HEAD` which is master in this case uses `x` as the variable name while `my_awesome_branch` uses `number`. In this case it's up to you to choose which one you'd like to use. Let's say we choose that we want to use `number` you can remove the `<<<<<<< HEAD`, `=======` and `>>>>>>> my_awesome_branch` lines and only keep the second block of code so it looks like:
```ruby
def even(number)
  a = Array.new
  i = 0

  until a.length == number do
```
After that you will need to make a commit for solving the conflicts so you can do:
```shell
git add .
git commit -m "solve conflicts of merging with my_awesome_branch"
```
You will see an output like:
```
[master 6d497a5] solve conflicts of merging with my_awesome_branch
```
Note that we will see a new commit added as a result of the merge because of the
conflicts.