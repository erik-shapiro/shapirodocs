# Local Setup

* Download [Git](https://git-scm.com/downloads).
* Install [Python](https://www.python.org/downloads/release/python-371) (be sure to "Add Python 3.7 to PATH"). It should include pip by default.
* Go into your favorite directory.
* Right-click in whitespace and click "Git Bash Here".
* A command prompt will come up. Run the following:
    * pip install mkdocs (this will install mkdocs, which you need to compile the documentation into HTML pages)
    * git clone //shap-exp-360pro/c$/inetpub/wwwroot/shapiro-cms/templates/rt_metamorph/shapirodocumentaion
    * cd shapirodocumentaion
    * git checkout master
* Now make your changes. Once you are done, commit your raw changes to the master branch:
    * git pull (make sure you have the latest version of the repository)
    * git status (make sure your files show up as edited)
    * git add {filename} (stage your files for a commit)
    * git commit (be sure to add a commit message on the next screen, or else your commit will not take)
    * git push (this pushes your raw changes to the master branch)
    * mkdocs gh-deploy (this compiles the documentation site and syncs it to 360)
* Please remember to do both: push to the master branch and deploy the new site. These are two distinct branches, and updating one will not update the other.