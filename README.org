👉 [[https://github.com/sponsors/xenodium][Support this work via GitHub Sponsors]]

* sqlite-mode-extras

An experimental package to extend Emacs 29's sqlite-mode.

⚠️ Experimental package ⚠️

I've received enough requests to put this on MELPA from folks that have found the package stable and helpful.

Having said that, I want to be upfront that I haven't tested the code thoroughly, so please use at own risk (please back up your database).

* Recommended bindings and installation

Install from MELPA and enable default bindings via =sqlite-extras-minor-mode=.

#+begin_src emacs-lisp :lexical no
  (use-package sqlite-mode-extras
    :ensure t
    :hook ((sqlite-mode . sqlite-extras-minor-mode)))
#+end_src

* What kind of extras?

- =sqlite-mode-extras-tab-dwim=: Tab navigation.

#+ATTR_HTML: :width 75%
[[file:images/sqlite-forward.gif]]

#+ATTR_HTML: :width 75%
[[file:images/sqlite-previous.gif]]

- =sqlite-mode-extras-ret-dwim=: Edit field.

#+ATTR_HTML: :width 90%
[[file:images/sqlite-edits.gif]]

- =sqlite-mode-extras-compose-and-execute=: Compose and execute a query.

#+ATTR_HTML: :width 90%
[[file:images/compose-execute.gif]]
- =sqlite-mode-extras-execute=: Execute a query.

#+ATTR_HTML: :width 90%
[[file:images/execute.gif]]
- =sqlite-mode-extras-add-row=: Add row to table at point.

#+ATTR_HTML: :width 90%
[[file:images/add-row.gif]]
- =sqlite-mode-extras-delete-row-dwim=: Similar to =sqlite-mode-delete= but also enables deleting range in region.

#+ATTR_HTML: :width 90%
[[file:images/delete-rows.gif]]
- =sqlite-mode-extras-refresh=: Refreshes the buffer re-querying the database.
- =sqlite-mode-extras-ret-dwim=: If on table, toggle expansion. If on row, edit it.
- =sqlite-mode-extras-execute-and-display-select-query=: Executes a query and displays results.

#+ATTR_HTML: :width 90%
[[file:images/select-earth.gif]]

* Customizations

#+BEGIN_SRC emacs-lisp :results table :colnames '("Custom variable" "Description") :exports results
  (let ((rows))
    (mapatoms
     (lambda (symbol)
       (when (and (string-match "^sqlite-mode-extras"
                                (symbol-name symbol))
                  (custom-variable-p symbol))
         (push `(,symbol
                 ,(car
                   (split-string
                    (or (get (indirect-variable symbol)
                             'variable-documentation)
                        (get symbol 'variable-documentation)
                        "")
                    "\n")))
               rows))))
    rows)
#+END_SRC

#+RESULTS:
| Custom variable                          | Description                             |
|------------------------------------------+-----------------------------------------|
| sqlite-mode-extras-auto-complete-enabled | Enable value lookup on database column. |

* Commands
#+BEGIN_SRC emacs-lisp :results table :colnames '("Binding" "Command" "Description") :exports results
  (let ((rows))
    (mapatoms
     (lambda (symbol)
       (when (and (string-match "^sqlite-mode-extras"
                                (symbol-name symbol))
                  (commandp symbol))
         (push `(,(string-join
                   (seq-filter
                    (lambda (symbol)
                      (not (string-match "menu" symbol)))
                    (mapcar
                     (lambda (keys)
                       (key-description keys))
                     (or
                      (where-is-internal
                       (symbol-function symbol)
                       comint-mode-map
                       nil nil (command-remapping 'comint-next-input))
                      (where-is-internal
                       symbol sqlite-extras-minor-mode-map nil nil (command-remapping symbol))
                      (where-is-internal
                       (symbol-function symbol)
                       sqlite-extras-minor-mode-map nil nil (command-remapping symbol)))))  " or ")
                 ,(symbol-name symbol)
                 ,(car
                   (split-string
                    (or (documentation symbol t) "")
                    "\n")))
               rows))))
    rows)
#+END_SRC

#+RESULTS:
| Binding        | Command                                             | Description                                       |
|----------------+-----------------------------------------------------+---------------------------------------------------|
| g              | sqlite-mode-extras-refresh                          | Refresh all listings and table queries.           |
| E              | sqlite-mode-extras-execute                          | Execute a query.                                  |
| C              | sqlite-mode-extras-compose-and-execute              | Compose and execute a query.                      |
| S              | sqlite-mode-extras-execute-and-display-select-query | Execute a SELECT QUERY.                           |
|                | sqlite-mode-extras-edit-row-field                   | Edit current row's field.                         |
|                | sqlite-mode-extras-next-column                      | Navigate to next column.                          |
|                | sqlite-mode-extras-mode                             |                                                   |
| +              | sqlite-mode-extras-add-row                          | Add a row to current table.                       |
| DEL or D       | sqlite-mode-extras-delete-row-dwim                  | Delete current row or rows in region.             |
| <backtab> or b | sqlite-mode-extras-backtab-dwim                     | Like `sqlite-mode-extras-tab-dwim' but backwards. |
| RET            | sqlite-mode-extras-ret-dwim                         | DWIM binding for RET.                             |
| <tab> or f     | sqlite-mode-extras-tab-dwim                         | DWIM binding for TAB.                             |

👉 Find this useful? [[https://github.com/sponsors/xenodium][Support this work via GitHub Sponsors]]
