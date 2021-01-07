# Approach 1

Use `pip-tools` and split out all the internal libs that are installed directly from git repos into a different different, using the layered requirements feature.


# Workflow

Note: for the purpose of this example, I will not use a full git repo URL, but rather a lib named mylib. Repos and commit hashes would work similarly to lib versions.

1. create a `requirements-internal-repos.in` that would include `mylib==1.0`.
2. create a `requirements.in` that would include everything else and would import `requirements-repos.in`.
```
-r requirements-internal-repos.in

django
```
3. `pip-compile requirements.in` which generates `requirements.txt` from the two .in files above.
4. `pip install -r requirements.txt`
5. update `mylib==1.1` in `requirements-internal-repos.in`.
6. `pip-compile requirements.in`
7. `pip install -r requirements.txt`


# Analysis

Pros:

Cons:


# FAQ

**What if you need to have different requirements.txt files for each environment?**
Don't.
