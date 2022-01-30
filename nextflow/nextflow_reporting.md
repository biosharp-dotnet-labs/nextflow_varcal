---
layout: main
title: NextFlow Reporting
categories: [nextflow]
tags: [cluster,nextflow,workflow,bioinformatics,tutorial]
permalink: /nextflow_varcal/nextflow/nextflow_reporting
---
{% include _nextflow_nextflow_reporting_toc.html %}


<hr>
<center>This is part 13 of 14 of a <a href="/nextflow_varcal/nextflow/" target="_blank">Introduction to NextFlow</a>.</center>
<hr>

<br>


## Log

When provided with a run name or session ID, the log command can return useful information about a pipeline execution. This can be composed to track the provenance of a workflow result.

To find the run names or session IDs we can use the Nextflow log command.

```bash
nextflow log
```

>```bash
>TIMESTAMP          	DURATION	RUN NAME               	STATUS	REVISION ID	SESSION ID                          	COMMAND
>```

This will print a summary of the executions log and runtime information for all pipelines run. By default, included in the summary, are the date and time it ran, how long it ran for, the run name, run status, a revision ID, the session id and the command run on the command line.

If we want to get more information about an individual run we can add the run name or session ID to the log command. This will lists all the work directories.

For example:

```bash
nextflow log tiny_fermat
```
This will list the work directory for each process.

>```bash
>/data/.../work/7b/3753ff13b1fa5348d2d9b6f512153a
>/data/.../work/c1/56a36d8f498c99ac6cba31e85b3e0c
>/data/.../work/f7/659c65ef60582d9713252bcfbcc310
>/data/.../work/82/ba67e3175bd9e6479d4310e5a92f99
>/data/.../work/e5/2816b9d4e7b402bfdd6597c2c2403d
>/data/.../work/3b/3485d00b0115f89e4c202eacf82eba
>```

### Task ID

A task’s unique ID is generated as a 128-bit hash number obtained from a composition of the task’s:
*   Inputs values
*   Input files
*   Command line string
*   Container ID
*   Conda environment
*   Environment modules
*   Any executed scripts in the bin directory


## Fields

If we want to print more metadata we can use log command and the option -f (fields) followed by a comma delimited list of fields.

For example:


```bash
nextflow log tiny_fermat -f 'process,exit,hash,duration'
```

Will output the process name, exit status, hash and duration of the process for the tiny_fermat run to the terminal.

>```bash
>index	0	7b/3753ff	2s
>fastqc	0	c1/56a36d	9.3s
>fastqc	0	f7/659c65	9.1s
>quant	0	82/ba67e3	2.7s
>quant	0	e5/2816b9	3.2s
>multiqc	0	3b/3485d0	6.3s
>```

The complete list of available fields can be retrieved with the command:

```bash
nextflow log -l
```

>```bash
>attempt
>complete
>container
>cpus
>disk
>duration
>env
>error_action
>exit
>hash
>inv_ctxt
>log
>memory
>module
>name
>native_id
>pcpu
>peak_rss
>peak_vmem
>pmem
>process
>queue
>rchar
>read_bytes
>realtime
>rss
>scratch
>script
>start
>status
>stderr
>stdout
>submit
>syscr
>syscw
>tag
>task_id
>time
>vmem
>vol_ctxt
>wchar
>workdir
>write_bytes
>```

### Script

If we want a log of all the commands executed in the pipeline we can use the script field. It is important to note that the resultant output can not be used to run the pipeline steps.

Filtering

The output from the log command can be very long. We can subset the output using the option -F (filter) specifying the filtering criteria. This will print only those tasks matching a pattern using the syntax ~=/<pattern>/.

For example to filter for process with the name fastqc we would run:


```bash
nextflow log tiny_fermat -F 'process =~ /fastqc/'
```

>```bash
>/data/.../work/c1/56a36d8f498c99ac6cba31e85b3e0c
>/data/.../work/f7/659c65ef60582d9713252bcfbcc310
>```

This can be useful to locate specific tasks work directories.

<details>
  <summary>Exercise</summary>
View run log

Use the Nextflow log command specifying a run name and the fields. name, hash, process and status

```groovy

```  
<details>
  <summary>Solution</summary>
  
```bash
nextflow log elegant_descartes -f name,hash,process,status
```
</details>
</details>

<details>
  <summary>Exercise</summary>
Filter pipeline run log

Use the -F option and a regular expression to filter the for a specific process.

```groovy

```  
<details>
  <summary>Solution</summary>
  
```bash
nextflow log elegant_descartes -f name,hash,process,status -F 'process =~ /multiqc/'
```
</details>
</details>

---

## Templates

The -t option allows a template (string or file) to be specified. This makes it possible to create a custom report in any text based format.

For example you could save this markdown snippet to a file:

```
## $name

script:

    $script

exist status: $exit
task status: $status
task folder: $folder
```

Then, the following log command will output a markdown file containing the script, exit status and folder of all executed tasks:

```bash
nextflow log elegant_descartes -t my-template.md > execution-report.md
```

Or, the template file can also be written in HTML.

For example:

```html
<div>
<h2>${name}</h2>
<div>
Script:
<pre>${script}</pre>
</div>

<ul>
    <li>Exit: ${exit}</li>
    <li>Status: ${status}</li>
    <li>Work dir: ${workdir}</li>
    <li>Container: ${container}</li>
</ul>
</div>
```

By saving the above snippet in a file named template.html, you can run the following command:

```bash
nextflow log elegant_descartes -t template.html > provenance.html
```
To view the report open it in a browser.

<details>
  <summary>Exercise</summary>
Generate an HTML run report

Generate an HTML report for a run using the -t option and the template.html file.

```groovy

```  
<details>
  <summary>Solution</summary>
  
```bash
nextflow log elegant_descartes -t template.html > provenance.html
```
</details>
</details>

> Key Points
>*  Nextflow can produce a custom execution report with run information using the log command.
>*  You can generate a report using the -t option specifying a template file.

---

<h5><a href="/nextflow_varcal/nextflow/nextflow_sub_workflows" style="float: left"><b>Back to:</b>NextFlow SubWorkflows</a>

<a href="/nextflow_varcal/nextflow/nextflow_caching" style="float: right"><b>Next:</b>NextFlow Caching</a></h5>
