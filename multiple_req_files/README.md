# Approach 1

Multiple independent requirements-<env>.txt files, one for each environment. Each requirements-<env>.txt file also includes all the internal libs that are installed directly from git repos.


# Workflow 1

If `pip install` is used as the main source of truth and libraries are updated by specifing in a version when installing/updating. In this case, the requirements-<env>.txt file is frozen once all dev work is done.

1. `pip install mylib==1.0`
2. `pip freeze > requirements-local.txt`
3. `pip install mylib==1.1`
4. `pip freeze > requirements-local.txt`
5. repeat 4. for all environments


# Workflow 2

If requirements.txt is used as a source of truth and libraries are updated by editing the file itself:

1. `pip install mylib==1.0`
2. `pip freeze > requirements-local.txt`
3. edit requirements-local.txt to update to mylib==1.1
4. `pip install -r requirements-local.txt`
5. if mylib==1.1 requires the update of one of its depedencies, an error like:
"mylib 1.1 has requirement depedencylib=x.x, but you'll have dependencylib y.y which is incompatible."
6. edit requirements-local.txt to include depedencylib=x.x
7. `pip install -r requirements-local.txt`. Repeate for all depedencies of mylib. As an example, Django 3.1.5 has 4 dependecies, but some are more stable, so rarely change.
8. repeat 4. to 6. for all `requirements-<env>.txt` files.


# Analysis

Pros:
- no pip plugins used
- fewer files

Cons:
- long list of dependecies inside each requirements-<env>.txt file
- updating the version of one lib, needs to be coordinated across multiple files
- running `pip freeze > requirements-<env>.txt` does not guarantee an order, so lines of interest might shift around
- the state of your local dev env can get out of sync with the requirements files.


# FAQ
**What if you need to have different requirements.txt files for each environment?**
Already included in the two workflows above.
