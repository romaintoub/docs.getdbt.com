---
title: How can I consolidate projects in dbt Cloud?
description: "Consolidating projects in dbt Cloud"
sidebar_label: 'How to consolidate projects'
id: consolidate-projects

---

There is no universal solution when consolidating projects. The needs of individual organizations can vary greatly, but there are a few considerations worth mentioning when determining the scope of such a task.  

If you have multiple projects that contain production-worthy code, there are rarely straightforward solutions to merging them. Let's suppose you have `Main Project` and `Smaller Subset Project`.

## Files and Folders

### Git and the local directory

Reference the [merge git commands](https://gist.github.com/msrose/2feacb303035d11d2d05) to help complete the migration plan. Using the commands will help retain git commit history, but you might result in duplicate folders called `models`, `tests`, etc. You will most likely still have to move files around manually.

Another option would be to use an external code editor to drag and drop folders and files from the `Smaller Subset Project` to the `Main Project`. This is what internal dbt Labs experts recommend to stay informed about what comes over to the main project and also allows you to be more aware of the incoming files, with the ability to make any minor tweaks to folder hierarchy that you might want to do at the same time.

### Manual migration with multiple browser tabs

If you only have a couple of models or macros that you want to move over, grab the raw file contents from your git provider in `Smaller Subset Project` and paste the contents into a new file in your `Main Project`, within the dbt Cloud IDE.

Alternatively, you can download those files from your git provider (`Smaller Subset Project` repo) and upload them back to your other repository (`Main Project` repo). This doesn’t scale well and could bypass change controls, so it may only be a viable solution for organizations with only a few files.

## Production Jobs
If you have multiple projects with deployment environments deploying jobs, this poses another challenge. Assuming we can bring all the models from `Smaller Subset Project` into `Main Project`, your commands within your jobs will take on a new meaning. In lieu of refactoring your global job strategy at the same time, you can add tags to the incoming project models and utilize that in your job command syntax, with the help of node selection syntax.

Main Project job command example: `dbt build --exclude tag:smaller_subset_project`
Smaller Subset Project commands: `dbt build --select tag:smaller_subset_project`