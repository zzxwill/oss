# Standalone deployment {#concept_py5_2hh_wdb .concept}

Standalone deployment supports Linux and Windows.

## Download {#section_dpf_t9v_xzi .section}

Download the tool for standalone deployment: [ossimport-2.3.4.zip](http://gosspublic.alicdn.com/ossimport/international/standalone/ossimport-2.3.4.zip). Download the tool to a local directory and use a tool or run the `unzip` command to unzip the files. The file structure after unzipping is as follows:

```
ossimport
├── bin
│   └── ossimport2.jar  # The JAR including master, worker, tracker, and console modules
├── conf
│   ├── local_job.cfg   # The job configuration file
│   └── sys.properties  # Configuration file of the system running parameters
├── console.bat         # Windows command line, which can run distributed call-in tasks
├── console.sh          # Linux command line, which can run distributed call-in tasks
├── import.bat          # The configuration file for one-click import and execution in Windows is the data migration job configured in conf/local_job.cfg, including start, migration, validation, and retry
├── import.sh           # The configuration file for one-click import and execution in Linux is the data migration job configured in conf/local_job.cfg, including start, migration, validation, and retry
├── logs                # Log directory
└── README.md           # Description documentation. We recommend that you carefully read the documentation before using the feature
```

## Configuration {#section_exh_pn9_ury .section}

The standalone version has two configuration files: `conf/sys.properties` and `conf/local_job.cfg`.

-   Do not change the configuration items in `conf/sys.properties`: `workingDir`, `workerUserName`, `workerPassword`, and `privateKeyFile`.

-   Do not change the name and location of `conf/local_job.cfg` and the `jobName` configuration item in it.

Configure other items appropriately.

**Note:** Confirm the parameters in `sys.properties` and `local_job.cfg` before submitting the job. The parameters in the job are not allowed to be changed after the job is submitted.

## Running {#section_xkn_rfo_sk4 .section}

In standalone mode, a data migration job has two execution modes: one-click import and step-by-step execution.

One-click import encapsulates all the steps and data migration can then be completed following the prompts of the script.

**Note:** We recommend you use one-click import if you use ossimport for the first time.

Step-by-step execution includes executing the starting service, submitting the job and retrying failed tasks.

-   One-click import
    1.  To run one-click import, run `import.bat` in cmd.exe in Windows, and run `bash import.sh` in Linux.
    2.  If you previously run this job, you are asked if you want to continue the job from the last breakpoint or if you want to run a new synchronization job. If you initiate a new data migration job, or have modified the synchronized source end/destination end, run the synchronization job again.
    3.  After a job starts in Windows, a new cmd window appears showing the synchronization job in progress and the log. The job status in the old window is refreshed every 10 seconds. Do not close these two windows during the data migration process. In Linux, the preceding process is run in the background.
    4.  When the job is complete, if a task failed, you are asked if you want to retry. Enter y to retry or n to skip this step and exit.
    5.  To see why the upload failed, open the file `master/jobs/local_test/failed_tasks/<tasktaskid>/audit.log` and check the cause of the failure.
-   Step-by-step execution

    1.  Clear jobs with the same name. If you have run job with the same name before and want to run the job again, first clear the job with the same name. If you have never run the job or you want to retry a failed job, do not run the clear command. In Windows, run `console.bat clean` in **cmd.exe**. In Linux, run `bash console.sh clean`.
    2.  Submit the data migration job. OssImport does not support submitting jobs of the same name. If jobs with the same name exist, clear the job with the same name first. The configuration file for the submitted job is `conf/local_job.cfg`, and the default job name is `local_test`. To submit a job, run `console.bat submit` in **cmd.exe** in Windows, and run `bash console.sh submit` in Linux.
    3.  Start the service. Run `console.bat start` in **cmd.exe** in Windows, and run `bash console.sh start` in Linux.
    4.  View the job status. Run `console.bat start` in **cmd.exe** in Windows, and run `bash console.sh start` in Linux.
    5.  Retry a failed task. Tasks may fail due to network issues or other causes. Only failed tasks are retried. Run `console.bat retry` in **cmd.exe** in Windows, and run `bash console.sh retry`.
    6.  Stop the service. Close the **%JAVA\_HOME%/bin/java.exe** window in Windows, and run `bash console.sh stop` in Linux.
    **Note:** We recommend that you use one-click import for data migration if you have no special requirements.

-   Common causes of failure
    -   A file in the source directory was modified during the upload process. This cause is indicated by a `SIZE_NOT_MATCH` error in log/audit.log. In this case, the old file has been uploaded successfully, but the changes have not been synchronized to the OSS.
    -   A source file was deleted during the upload process, leading to download failure.
    -   A source file name does not conform to naming rules of the OSS \(file name cannot start with / or be empty\), leading to upload failure.
    -   The data source file failed to be downloaded.
    -   The program exited unexpectedly and the job status is Abort. If this happens, contact after-sales technical support.
-   Job statuses and logs

    After a job is submitted, the master splits the job into tasks, the workers run the tasks and the tracker collects the task statuses. After a job is completed, the ossimport directory contains the following:

    ```
    ossimport
    ├── bin
    │   └── ossimport2.jar    # The standalone version JAR
    ├── conf
    │   ├── local_job.cfg     # The job configuration file
    │   └── sys.properties    # Configuration file of the system running parameters
    ├── console.sh            # The command line tool
    ├── import.sh             # One-click import script
    ├── logs
    │   ├── import.log        # Migration logs
    │   ├── job_stat.log      # Job status record
    │   ├── ossimport2.log    # Running log of the standalone version
    │   └── submit.log        # Job submission record
    ├── master
    │   ├── jobqueue                # Store jobs that have not been fully split
    │   └── jobs                    # Store the job running status
    │       └── local_test          # Job name
    │           ├── checkpoints     # The checkpoint record of the master splitting the job to tasks
    │           │   └── 0
    │           │       └── 034DC9DD2860B0CFE884242BC6FF92E7.cpt
    │           ├── dispatched      # Tasks that have been assigned to the workers but haven't been fully run
    │           │   └── localhost
    │           ├── failed_tasks    # Tasks that failed to run
    │           ├── pending_tasks   # Tasks that have not been assigned
    │           └── succeed_tasks   # Tasks that run successfully
    │               └── A41506C07BF1DF2A3EDB4CE31756B93F_1499744514501@localhost
    │                   ├── audit.log   # The task running log. You can view the error causes in the log
    │                   ├── DONE        # Mark of successful tasks
    │                   ├── error.list  # The task error list. You can view the error file list
    │                   ├── STATUS      # The task status marker file. The content is Failed or Completed
    │                   └── TASK        # The task description information
    └── worker      # Status of the task being run by the worker. After running, tasks are managed by the master
        └── jobs
            └── local_test
                └── tasks
    ```

    **Note:** 

    -   For job running information, view logs/ossimport2.log or logs/import.log.
    -   For the task failure cause, view master/jobs/$\{JobName\}/failed\_tasks/$\{TaskName\}/audit.log.
    -   For failed task files, view master/jobs/$\{JobName\}/failed\_tasks/$\{TaskName\}/error.list.
    -   The preceding log files are for reference only. Do not deploy your services and applications entirely based on them.

## FAQ {#section_zf0_q32_tir .section}

See [FAQ](reseller.en-US/Tools/ossimport/FAQ.md#).

