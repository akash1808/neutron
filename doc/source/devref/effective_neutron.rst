..
      Licensed under the Apache License, Version 2.0 (the "License"); you may
      not use this file except in compliance with the License. You may obtain
      a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

      Unless required by applicable law or agreed to in writing, software
      distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
      WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
      License for the specific language governing permissions and limitations
      under the License.


      Convention for heading levels in Neutron devref:
      =======  Heading 0 (reserved for the title in a document)
      -------  Heading 1
      ~~~~~~~  Heading 2
      +++++++  Heading 3
      '''''''  Heading 4
      (Avoid deeper levels because they do not render well.)


Effective Neutron: 100 specific ways to improve your Neutron contributions
==========================================================================

There are a number of skills that make a great Neutron developer: writing good
code, reviewing effectively, listening to peer feedback, etc. The objective of
this document is to describe, by means of examples, the pitfalls, the good and
bad practices that 'we' as project encounter on a daily basis and that make us
either go slower or accelerate while contributing to Neutron.

By reading and collaboratively contributing to such a knowledge base, your
development and review cycle becomes shorter, because you will learn (and teach
to others after you) what to watch out for, and how to be proactive in order
to prevent negative feedback, minimize programming errors, writing better
tests, and so on and so forth...in a nutshell, how to become an effective Neutron
developer.

The notes below are meant to be free-form and brief by design. They are not meant
to replace or duplicate `OpenStack documentation <http://docs.openstack.org>`_,
or any project-wide documentation initiative like `peer-review notes <http://docs.openstack.org/infra/manual/developers.html#peer-review>`_
or the `team guide <http://docs.openstack.org/project-team-guide/>`_. For this
reason, references are acceptable and should be favored, if the shortcut is
deemed useful to expand on the distilled information.
We will try to keep these notes tidy by breaking them down into sections if it
makes sense. Feel free to add, adjust, remove as you see fit. Please do so,
taking into consideration yourself and other Neutron developers as readers.
Capture your experience during development and review and add any comment that
you believe will make your life and others' easier.

Happy hacking!

Developing better software
--------------------------

Database interaction
~~~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done during database development.

* `first() <http://docs.sqlalchemy.org/en/rel_1_0/orm/query.html#sqlalchemy.orm.query.Query.first>`_
  does not raise an exception.
* Do not get an object to delete it. If you can `delete() <http://docs.sqlalchemy.org/en/rel_1_0/orm/query.html#sqlalchemy.orm.query.Query.delete>`_
  on the query object. Read the warnings for more details about in-python cascades.
* ...

System development
~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when invoking system commands
and interacting with linux utils.

Eventlet concurrent model
~~~~~~~~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when using eventlet and monkey
patching.

Mocking and testing
~~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when writing tests, any test.
For anything more elaborate, please visit the testing section.

* Preferring low level testing versus full path testing (e.g. not testing database
  via client calls). The former is to be favored in unit testing, whereas the latter
  is to be favored in functional testing.

Backward compatibility
~~~~~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when extending the RPC Interfaces.

Scalability issues
~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when writing code that needs to process
a lot of data.

Translation and logging
~~~~~~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when instrumenting your code.

 * Make yourself familiar with `OpenStack logging guidelines <http://specs.openstack.org/openstack/openstack-specs/specs/log-guidelines.html#definition-of-log-levels>`_
   to avoid littering the logs with traces logged at inappropriate levels.

Project interfaces
~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when writing code that is used
to interface with other projects, like Keystone or Nova.

Documenting your code
~~~~~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when writing docstrings.

Landing patches more rapidly
----------------------------

Nits and pedantic comments
~~~~~~~~~~~~~~~~~~~~~~~~~~

Document common nits and pedantic comments to watch out for.

* Make sure you spell correctly, the best you can, no-one wants rebase generators at
  the end of the release cycle!
* Being available on IRC is useful, since reviewers can contact directly to quickly
  clarify a review issue. This speeds up the feeback loop.
* The odd pep8 error may cause an entire CI run to be wasted. Consider running
  validation (pep8 and/or tests) before submitting your patch. If you keep forgetting
  consider installing a git `hook <https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks>`_
  so that Git will do it for you.
* Sometimes, new contributors want to dip their toes with trivial patches, but we
  at OpenStack *love* bike shedding and their patches may sometime stall. In
  some extreme cases, the more trivial the patch, the higher the chances it fails
  to merge. To ensure we as a team provide/have a frustration-free experience
  new contributors should be redirected to fixing `low-hanging-fruit bugs <https://bugs.launchpad.net/neutron/+bugs?field.tag=low-hanging-fruit>`_
  that have a tangible positive impact to the codebase. Spelling mistakes, and
  docstring are fine, but there is a lot more that is relatively easy to fix
  and has a direct impact to Neutron users.

Reviewer comments
~~~~~~~~~~~~~~~~~

* Acknowledge them one by one by either clicking 'Done' or by replying extensively.
  If you do not, the reviewer won't know whether you thought it was not important,
  or you simply forgot. If the reply satisfies the reviewer, consider capturing the
  input in the code/document itself so that it's for reviewers of newer patchsets to
  see (and other developers when the patch merges).
* Watch for the feedback on your patches. Acknowledge it promptly and act on it
  quickly, so that the reviewer remains engaged. If you disappear for a week after
  you posted a patchset, it is very likely that the patch will end up being
  neglected.

Commit messages
~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when writing commit messages.
For more details see `Git commit message best practices <https://wiki.openstack.org/wiki/GitCommitMessages>`_.
This is the TL;DR version with the important points for committing to Neutron.


* One liners are bad, unless the change is trivial.
* Remember to use DocImpact, APIImpact, UpgradeImpact appropriately.
* Make sure the commit message doesn't have any spelling/grammar errors. This
  is the first thing reviewers read and they can be distracting enough to
  invite -1's.
* Describe what the change accomplishes. If it's a bug fix, explain how this
  code will fix the problem. If it's part of a feature implementation, explain
  what component of the feature the patch implements. Do not just describe the
  bug, that's what launchpad is for.
* Use the "Closes-Bug: #BUG-NUMBER" tag if the patch addresses a bug. Submitting
  a bugfix without a launchpad bug reference is unacceptable, even if it's
  trivial. Launchpad is how bugs are tracked so fixes without a launchpad bug are
  a nightmare when users report the bug from an older version and the Neutron team
  can't tell if/why/how it's been fixed. Launchpad is also how backports are
  identified and tracked so patches without a bug report cannot be picked to stable
  branches.
* Use the "Implements: blueprint NAME-OF-BLUEPRINT" or "Partially-Implements:
  blueprint NAME-OF-BLUEPRINT" for features so reviewers can determine if the
  code matches the spec that was agreed upon. This also updates the blueprint
  on launchpad so it's easy to see all patches that are related to a feature.
* If it's not immediately obvious, explain what the previous code was doing
  that was incorrect. (e.g. code assumed it would never get 'None' from
  a function call)
* Be specific in your commit message about what the patch does and why it does
  this. For example, "Fixes incorrect logic in security groups" is not helpful
  because the code diff already shows that you are modifying security groups.
  The message should be specific enough that a reviewer looking at the code can
  tell if the patch does what the commit says in the most appropriate manner.
  If the reviewer has to guess why you did something, lots of your time will be
  wasted explaining why certain changes were made.


Dealing with Zuul
~~~~~~~~~~~~~~~~~

Document common pitfalls as well as good practices done when dealing with OpenStack CI.

* When you submit a patch, consider checking its `status <http://status.openstack.org/zuul/>`_
  in the queue. If you see a job failures, you might as well save time and try to figure out
  in advance why it is failing.
* Excessive use of 'recheck' to get test to pass is discouraged. Please examine the logs for
  the failing test(s) and make sure your change has not tickled anything that might be causing
  a new failure or race condition. Getting your change in could make it even harder to debug
  what is actually broken later on.
