# Distributed deployment {#concept_axx_n3h_wdb .concept}

## Download {#section_cwl_43h_wdb .section}

Distributed deployment currently only supports Linux, and does not support Windows.

 Download the tool for distributed deployment: [ossimport-2.3.2.tar.gz](http://gosspublic.alicdn.com/ossimport/international/distributed/ossimport-2.3.2.tar.gz). Download the tool to a local directory and use the command `tar -zxvf ossimport-2.3.2.tar.gz -C $HOME/ossimport ` to unzip the files. The file structure after the unzipping is as follows:

```
Ossimport
├── bin
│ ├── console.jar # The JAR package of the console module
│ ├── master.jar # The JAR package of the master module
│ ├── tracker.jar # The JAR package of the tracker module
│ └── worker.jar # The JAR package of the worker module
├── conf
│ ├── job.cfg # The template of the job configuration file
│ ├── sys.properties # Configuration file of the system running parameters
│ └── workers # Worker list
├── console.sh # The command line tool. Currently it only supports Linux
├── logs # Log directory
└── README.md # Description documentation. Read it carefully before use
```

Note:

-   OSS\_IMPORT\_HOME: The root directory of the OssImport. By default the directory is the `$HOME/ossimport`   in the unzip command. You can also run the command  `export OSS_IMPORT_HOME=<dir>` or modify the system configuration file `$HOME/.bashrc` to set the directory.
-   OSS\_IMPORT\_WORK\_DIR: The OssImport working directory. You can specify the directory through the configuration item `workingDir` in   `conf/sys.properties`. The recommended values is  `$HOME/ossimport/workdir`.
-   Use absolute paths for OSS\_IMPORT\_HOME or OSS\_IMPORT\_WORK\_DIR, such as  `/home/<user>/ossimport` or  `/home/<user>/ossimport/workdir`.

## Configuration {#section_smx_s3h_wdb .section}

The distributed version has three configuration files: `conf/sys.properties`, `conf/job.cfg` and `conf/workers`. For descriptions of the configuration items, see the Introduction chapter.

-   `conf/job.cfg`: The configuration file template for the job in distributed mode. Modify the values according to the actual parameters before data migration.
-   `conf/sys.properties`: The configuration file for the system run parameters, such as the working directory and the worker running parameters
-   `conf/workers`: The worker list.

Note:

-   Confirm the parameters in `sys.properties` and `job.cfg` before submitting the job.  The parameters in the job are not allowed to be changed after the job is submitted.
-   Determine the worker list `workers` before starting the service. After the service is started, workers are not allowed to be added or deleted.

## Running {#section_m1y_1jh_wdb .section}

-   Run commands

    In distributed deployment, the general steps for job execution are as follows: modify the job configuration file, deploy the service, clear jobs of the same name, submit the data migration job, start the migration service, view the job state,  retry failed tasks, and stop the migration job. Detailed instructions are as follows:

    -   Deploy the service. Run bash console.sh deploy in Linux.

        **Note:** Make sure the configuration files  Conf/job. cfg and CONF/workers have been modified before deployment.

    -   Clear jobs of the same name. If you ran a job of the same name before and want to run the job again, clear the job with the same name first.  If you have never run the job or you want to retry a failed job, do not run the clear command.  Run  `bash console.sh clean job_name` in Linux.
    -   Submit the data migration job. The OssImport does not support submitting jobs of the same name. If jobs with the same name exist, use the `clean` command to clean the job with the same name first.  To submit a job, you must specify the job configuration file. The job’s configuration file template is at  `conf/job.cfg`. We recommend that you modify the settings based on the template.  Run `bash console.sh submit [job_cfg_file]` in Linux and submit the job with the configuration file job\_cfg\_file.  The `job_cfg_file`  is an optional parameter. If not specified, the parameter is  `$OSS_IMPORT_HOME/conf/job.cfg` by default. The `$OSS_IMPORT_HOME`  is by default the directory where the  `console.sh` file is located.
    -   Start the migration service. Run `bash console.sh start` in Linux.
    -   View the job state. Run `bash console.sh stat` in Linux.
    -   Retry failed tasks. Tasks may fail to run because of network issues or other causes.  Only failed tasks are retried.  Run  `bash console.sh retry [job_name]` in Linux. The job\_name  is an optional parameter which specifies to retry tasks of the job named `job_name`. If the `job_name` parameter is not specified, failed tasks of all jobs are retried.
    -   Stop the migration job. Run `bash console.sh stop` in Linux.
    Note:

    -   When the `bash console.sh` parameter has an error, the console.sh automatically prompts the command format.
    -   We recommend that you use absolute paths for directories of the configuration file and submitted jobs.
    -   The configuration for jobs \(that is, the configuration items in `job.cfg` \) cannot be modified after submitted.

        **Note:** The configuration item cannot be modified after it is submitted, please confirm before submitting.

-   Common causes of job failure
    -   A file in the source directory was modified during the upload process. This cause is indicated by a `SIZE_NOT_MATCH` error in `log/audit.log`. In this case, the old file has been uploaded successfully, but the changes have not been synchronized to the OSS.
    -   A source file was deleted during the upload process, leading to the download failure.
    -   A source file name does not conform to naming rules of the OSS \(file name cannot start with / or be empty\), leading to the upload failure to the OSS.
    -   The data source file fails to be downloaded.
    -   The program exits unexpectedly and the job state is Abort. If this happens, contact after-sales technical support.
-   Job states and logs

    After a job is submitted, the master splits the job into tasks, the workers run the tasks and the tracker collects the task states.  After a job is completed, the workdir directory contains the following:

    ```
    workdir
    ├── bin
    │ ├── console.jar # The JAR package of the console module
    │ ├── master.jar # The JAR package of the master module
    │ ├── tracker.jar # The JAR package of the tracker module
    │ └── worker.jar # The JAR package of the worker module
    ├── conf
    │ ├── job.cfg # The template of the job configuration file
    │ ├── sys.properties # Configuration file of the system running parameters
    │ └── workers # Worker list
    ├── logs
    │ ├── import.log # Archive logs
    │ ├── master.log # Master logs
    │ ├── tracker.log # Tracker logs
    │ └── worker.log # Worker logs
    ├── master
    │ ├── jobqueue # Store jobs that have not been fully split
    │ └── jobs # Store the job running state
    │ └── xxtooss # Job name
    │ ├── checkpoints # The checkpoint record that the master splits the job to tasks
    │ │ └── 0
    │ │ └── ED09636A6EA24A292460866AFDD7A89A.cpt
    │ ├── dispatched # Tasks that have been assigned to the workers but haven't been fully run
    │ │ └── 192.168.1.6
    │ ├── failed_tasks # Tasks that failed to run
    │ │ └── A41506C07BF1DF2A3EDB4CE31756B93F_1499348973217@192.168.1.6
    │ │ ├── audit.log # The task running log. You can view the error causes in the log
    │ │ ├── DONE # Mark of successful tasks. If the task fails, the mark is empty
    │ │ ├── error.list # The task error list. You can view the error file list
    │ │ ├── STATUS # The task state mark file. The content is Failed or Completed, indicating that the task failed or succeeded
    │ │ └── TASK # The task description information
    │ ├── pending_tasks # Tasks that have not been assigned
    │ └── succeed_tasks # Tasks that run successfully
    │ └── A41506C07BF1DF2A3EDB4CE31756B93F_1499668462358@192.168.1.6
    │ ├── audit.log # The task running log. You can view the error causes in the log
    │ ├── DONE # Mark of successful tasks
    │ ├── error.list # Task error list. If the task is successful, the list is empty
    │ ├── STATUS # The task state mark file. The content is Failed or Completed, indicating that the task failed or succeeded
    │ └── TASK  # The task description information
    └── worker # state of the task being run by the worker. After running, tasks are managed by the master
        └── jobs
            ├── local_test2
            │ └── tasks
            └── local_test_4
                └── tasks
    ```

    Note:

    -   For the job running state, view `logs/tracker.log`. For the worker running logs, view  `logs/worker.log`. For the master running logs, view  `logs/master.log`. 
    -   For the task failure cause, view `master/jobs/${JobName}/failed_tasks/${TaskName}/audit.log`.
    -   For failed task files, view `master/jobs/${JobName}/failed_tasks/${TaskName}/error.list`.

## FAQs {#section_rxm_rjh_wdb .section}

See [FAQs](intl.en-US/Utilities/ossimport/FAQs.md#).

