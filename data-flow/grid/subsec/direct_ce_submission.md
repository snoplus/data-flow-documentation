---
layout: default
title: Direct CE submission
nav_exclude: true
---

## Direct CE submission

These instructions are likely deprecated. Proceed with caution...
Sites typically run either Cream Computing Elements (cream-CE) or Arc Computing Elements (arc-CE), an exception being UCSD which runs htcondor. At present we only have documentation for direct submission to cream-CEs. It should be apparent which type of CE you want to test by running “lcg-infosites ce” and inspecting the CE name (the suffix will typically contain either “cream” or “nordugrid-Condor”.

To submit to a cream-CE create a job file, e.g.: 

```bash
[
Executable = "/bin/echo";
Arguments = "Hello world!";
StdOutput = "hello.out";
StdError = "hello.err";
OutputSandbox = {"hello.out","hello.err"};
OutputSandboxBaseDestURI = "gsiftp://localhost";
]
```

and then submit, check, and retrieve outputs with:

```bash
# Where site is the queue name (can be found from lcg-infosites ce)
glite-ce-job-submit -a -r [site] ce.jdl
# This will output a job id (which is the site name + some ID)
glite-ce-job-status [job ID]
# Once complete, retrieve outputs with:
glite-ce-job-output [job ID]
```
