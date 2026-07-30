# Framecut build runners

Public GitHub-hosted runner definitions for building and validating Framecut's
private GPU worker images. The application source remains in the private
`SakethSripada/VideoEditor` repository and is checked out at an explicit commit
with a read-only deploy key.

Workflows are manual-only. They do not run for pushes or pull requests, do not
upload private source as Actions artifacts, and accept only constrained inputs.
