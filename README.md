# Recent

Recent is a more structured way to log your bash history.

The standard `~/.bash_history` file is inadequate in many ways, its
worst fault is to by default log only 500 history entries, with no timestamp.
You can alter your bash `HISTFILESIZE` and `HISTTIMEFORMAT` variables but it
is still a unstructured format with limited querying ability.

Recent does the following.

1. Logs current localtime, command text, current pid, command return value,
   working directory to an sqlite database in `~/.recent.db`.
2. Logs history immediately, rather than at the close of the session.
3. Provides a command called `recent` for searching bash history.

## Installation instructions

You need will need sqlite installed.

Install the recent pip package.

`pip install recent`

Add the following to your `.bashrc` or `.bash_profile`.

`export PROMPT_COMMAND='log-recent -r $? -c "$(HISTTIMEFORMAT= history 1)" -p $$'`

And start a new shell.

## Usage

Look at your current history using recent. Here are some examples on how to use recent.

```sh
# Help
recent -h
# Look for all git commands
recent git
# Look for git commit commands.
# Query via regexp mode.
recent -re git.*commit
# Look for git commands that are not commits.
# Query via sql mode.
recent -sql 'command like "%git%" and command not like "%commit%"'
```

By default `recent` commands are not displayed in the output. To see the `recent` commands pass
the `return_self` argument as follows.

`recent git --return_self`

For more information run `recent -h`


**Note**: currently recent doesn't integrate with bash commands such as Ctrl-R,
but this is in the pipeline.

You can directly query your history running `sqlite3 ~/.recent.db "select * from commands limit 10"`

## Dev installation instructions


```
git clone https://github.com/dotslash/recent && cd recent
pip install -e .
```

## Security

Please note, recent does not take into account enforcing logging
for security purposes. For this functionality on linux, have a
look at auditd http://people.redhat.com/sgrubb/audit/.
