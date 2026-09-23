<div align="center">

# isams

[![License](https://img.shields.io/badge/LICENSE-MIT-5C9E31?style=for-the-badge)](LICENSE)
[![Python](https://img.shields.io/badge/PYTHON-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org)
[![Built by](https://img.shields.io/badge/BUILT%20BY-JGALEA-8A2BE2?style=for-the-badge&logo=github&logoColor=white)](https://github.com/jgalea)

**Read your school's iSAMS parent portal from the terminal: timetable, school calendar, after-school clubs, attendance, teachers, homework, reports, documents and forms.**

</div>

It signs in the way the portal does and calls the same JSON API the portal's web app uses. It's read-only and unofficial, not made by or connected to iSAMS.

Needs Python 3.9+ and nothing else. Runs on macOS, Linux and Windows.

## Setup

On a Mac or Linux:

```
mkdir -p ~/.local/bin
curl -fsSL https://raw.githubusercontent.com/jgalea/isams-cli/main/isams -o ~/.local/bin/isams
chmod +x ~/.local/bin/isams
isams login
```

If `isams` isn't found afterwards, add `~/.local/bin` to your PATH: `echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc` and open a new terminal.

On Windows, open PowerShell and install Python first if you don't have it:

```
winget install Python.Python.3.12
```

Then open a new PowerShell window and paste:

```
mkdir "$HOME\bin" -Force
curl.exe -fsSL https://raw.githubusercontent.com/jgalea/isams-cli/main/isams -o "$HOME\bin\isams.py"
Set-Content "$HOME\bin\isams.cmd" '@py "%USERPROFILE%\bin\isams.py" %*'
[Environment]::SetEnvironmentVariable("Path", [Environment]::GetEnvironmentVariable("Path", "User") + ";$HOME\bin", "User")
```

Open one more new PowerShell window so it picks up the PATH change, then run `isams login`. The Windows steps haven't been tested on a real Windows machine yet; if something breaks, open an issue.

`login` asks for the school (the `SCHOOL` in `SCHOOL.parents.isams.cloud`) and your portal username and password. The password goes only to the school's iSAMS sign-in page. What's saved, in `~/.config/isams/` (mode 600), is the refresh token and the sign-in cookies, so you aren't asked again until the school ends the session.

To sign in without prompts, set `ISAMS_SCHOOL` and point `ISAMS_OP_ITEM` at a 1Password item with `username` and `password` fields, e.g. `op://Private/iSAMS`.

## Commands

```
isams whoami                      who's signed in, and the children
isams timetable [child]           today's lessons (-d tomorrow|fri|YYYY-MM-DD, -w for the week)
isams calendar                    school calendar, next 14 days (--days, --from, -s text)
isams activities [child]          after-school clubs from SOCS: day, staff, next session (--sessions for every date)
isams attendance [child]          this term's absences and lates
isams teachers [child]            subjects, teachers and their emails
isams homework [child]            homework (--status outstanding|submitted|completed)
isams reports [child]             published reports and assessments
isams docs                        latest documents (-s search, -f folder id)
isams folders                     the document folder tree
isams download <id> [--dir DIR]   save a document
isams forms                       electronic forms: pending, available, submitted
isams invoices                    invoices on the portal
isams raw <path>                  GET any API path, e.g. portals/schools/terms
```

`child` matches part of a name or a form (`sam`, `3A`). Leave it out for all children. Every command takes `--json`.

Which sections return data depends on what your school has switched on in the portal.

`activities` works for schools that run clubs in SOCS and link it from the iSAMS parent portal. SOCS signs you in through the same iSAMS account, so there's no second login. The SOCS school number is read from the portal; set `ISAMS_SOCS_ID` if it can't be found. SOCS doesn't hold club descriptions, only names, staff and dates.
