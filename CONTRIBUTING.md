# Contributing teaching materials
To contribute teaching materials:
1. prepare teaching materials in a self-contained folder, ideally using `git` while developing the materials
2. add links to the session to `schedule.md`
3. add folder to Github repository


### Clone Github repository
```bash
# Clone the repository
git clone https://github.com/NBISweden/course-Bioinformatics-for-PIs.git
cd /course-Bioinformatics-for-PIs

# Checkout feature branch to work on
git checkout -b session-example
```

### Prepare a self-contained folder
Prepare a folder with teaching materials for a given session, e.g. `session-example`.

**e.g. folder structure with .md and .Rmd**

```bash
e.g. session-example
.
+-- session-example-files
|   +-- figures
|       +-- figure.1.png
+-- session-example.md
```

#### Code and commit
``` bash
 # Code & commit changes while working on the materials
 git add session-feature.md
 git commit -m "commit message"
 ```

 _Note Git commit good practices_

 **Git commits good practices**
 - Commit messages should contain relevant information regarding the feature(s) you add, what type of analyses they can be used for, *etc.*.
 - The subject line should be written in an imperative, e.g. *Fix typos* and be 50 characters or less
 - The body, if any, should be wrapped at 72 characters.
 - The subject and body should be separated by a blank line, start with a capital letter, and the subject line should not end with a period.
 - More about [good commit messages][git-commits]

### Add link(s) to the session in `schedule.md`

### Add to Github

#### Push feature branch to repo

 ``` bash
  # Push to feature when ready
  git push
  ```

#### Make a pull request to master branch when ready
Go to course repository [https://github.com/NBISweden/course-Bioinformatics-for-PIs.git](https://github.com/NBISweden/course-Bioinformatics-for-PIs.git) and create a pull request


## Course website
https://nbisweden.github.io/course-Bioinformatics-for-PIs/

_*renders from master branch_


### Questions?
Create an issue or contact Olga Dethlefsen <<olga.dethlefsen@nbis.se>>


## [Back to main](index.md)


[git-commits]: https://chris.beams.io/posts/git-commit/
