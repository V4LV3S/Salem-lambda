How to issue a salem release.

1. Ensure your master branch is synced to upstream:

       git pull upstream master

2. Look over whats-new.rst and the docs. Make sure "What's New" is complete
   (check the date!) and add a brief summary note describing the release at the
   top.
3. If you have any doubts, run the full test suite one final time!

       pytest --mpl .

4. On the master branch, commit the release in git:

       git commit -a -m 'Release v0.X.Y'

5. Tag the release:

       git tag -a v0.X.Y -m 'v0.X.Y'

6. Build source and binary wheels for pypi:

       git clean -xdf  # this deletes all uncommited changes!
       python setup.py bdist_wheel sdist

7. Use twine to register and upload the release on pypi. Be careful, you can't take this back!
     
       twine upload dist/salem-0.X.Y*
   
   You will need to be listed as a package owner at
   https://pypi.python.org/pypi/salem for this to work.
8. Push your changes to master:

       git push origin master
       git push origin --tags

9. Update the stable branch (used by ReadTheDocs) and switch back to master:

       git checkout stable
       git rebase master
       git push origin stable
       git checkout master
    
    It's OK to force push to 'stable' if necessary.
    We also update the stable branch with `git cherrypick` for documentation
      only fixes that apply the current released version.
10. Add a section for the next release (`v.X.(Y+1)`) to `doc/whats-new.rst`.
11. Commit your changes and push to master again:

       git commit -a -m 'Revert to dev version'
       git push origin master

12. Issue the release on GitHub. Click on "Draft a new release" at
    https://github.com/fmaussion/salem/releases. Type in the version number, but
    don't bother to describe it -- we maintain that on the docs instead.
13. Update the docs. Login to https://readthedocs.org/projects/salem/versions/
    and switch your new release tag (at the bottom) from "Inactive" to "Active".
    It should now build automatically.
14. Issue the release announcement!
