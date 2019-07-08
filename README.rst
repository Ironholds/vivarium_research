=================
Vivarium Research
=================

.. image:: https://readthedocs.org/projects/vivarium-research/badge/?version=latest
   :target: https://vivarium-research.readthedocs.io/en/latest/?badge=latest
   :alt: Documentation Status

Here you'll find a variety of useful information about working on ``vivarium``
research projects.

The hosted documentation can be found
`here <https://vivarium-research.readthedocs.io/en/latest/>`_.

.. contents::

Contributing
------------

If you want to add or modify documentation, you'll first need to clone and
install this repository.  As always, you should do this in a clean environment.

::

   $> conda create -y --name=vivarium_research python=3.6
   $> source activate vivarium_research
   (vivarium_research) $> git clone https://github.com/ihmeuw/vivarium_research.git
   (vivarium_research) $> cd vivarium_research
   (vivarium_research) $> pip install -r requirements.txt

Updating the Documentation
++++++++++++++++++++++++++

Make a new git branch
^^^^^^^^^^^^^^^^^^^^^

The first step to modifying the documentation is to checkout a new branch
with ``git``::

   (vivarium_research) $> git checkout -b YOUR-BRANCH-NAME

The ``YOUR-BRANCH-NAME`` is the name of your branch, and you should try to
be descriptive.  For example, if you're adding new documentation about an
ischemic heart disease model, you might call your branch ``ihd-model``.

Modify the documentation
^^^^^^^^^^^^^^^^^^^^^^^^

To make updates to the documentation, you should add or modify the
``.rst`` files in the ``docs/source/`` directory.  Our documentation is written
using `reStructuredText <http://docutils.sourceforge.net/docs/user/rst/quickref.html>`_
and `sphinx <http://www.sphinx-doc.org/en/master/contents.html>`.  Keep those
links handy for reference.

Once you've made your modifications, you can build them to see how they look.

::

   (vivarium_research) $> cd docs
   (vivarium_research) $> make html

This will create a new ``build`` sub-directory with the new documentation
rendered in html.  You can open the file `docs/build/html/index.html` in your
browser to view your changes.

Push your changes
^^^^^^^^^^^^^^^^^

Once you're satisfied, you should push your changes to the remote repository
(the one on GitHub).  Make sure you're in the main `vivairum_research`
directory and run::

   (vivarium_research) $> git add .
   (vivarium_research) $> git commit -m "Your commit message here."
   (vivarium_research) $> git push

Your commit message should describe the changes you made to the documentation.
You can make multiple commits to your branch, and that's frequently a very good
idea.  The first time you push your branch to the remote repository, you'll
have to tell ``git`` which branch to push to.  Instead of just running
``git push``, you'll run::

   (vivarium_research) $> git push --set-upstream origin YOUR-BRANCH-NAME

Don't worry if you forget.  ``git`` will remind you.

Submit a pull request for review
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The last part of the process is to submit a pull request.  You do this on
GitHub itself.  Open up the
`vivarium_research repository <https://github.com/ihmeuw/vivarium_research>`_
in your browser.  Click on the ``Branches`` tab right above the directory tree.
You should see a listing for your branch with a button that says
``Make pull request``.  Click that.  **Add reviewers**, then click the
``Create Pull Request`` button and notify the people you tagged that you
have a documentation PR for review.

People should respond with either approval, changes, or comments.  You should
respond to all the feedback and make updates to your pull request if necessary
and re-request reviews. Once everyone has responded and is happy (or has, at
least, marked your PR as approved), you can click the ``Merge Pull Request``
button and add your docs to the master branch.
