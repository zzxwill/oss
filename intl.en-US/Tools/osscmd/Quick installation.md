# Quick installation {#concept_jv4_ssb_wdb .concept}

## Overview {#section_b4n_tsb_wdb .section}

osscmd is a command line tool based on Python 2.x, supporting Bucket management and file management.

**Note:** We recommend that you use [ossutil](reseller.en-US/Tools/ossutil/Quick start.md#) instead of osscmd unless necessary.

-   Application scenarios
    -   API development and debugging, for example, sending a request with a specific format and performing multipart upload step by step.
    -   Bucket configuration if the console is unavailable, for example, logging/website/lifecycle.
-   Restrictions

    -   osscmd supports Python 2.5/2.6/2.7, but does not support Python 3.x.
    -   osscmd does not support new functions, such as low-frequency storage/archive storage, cross-region replication, and image origin retrieval. It only supports debugging.
    We recommend that you use ossutil instead of osscmd. ossutil has the following advantages:

    -   Supports Windows/Linux/Mac.
    -   Implemented based on the Go SDK, which features simple installation and superior performance.
    -   Provides easy commands and rich help information, and supports Chinese/English.

## Environment requirement {#section_i4r_dtb_wdb .section}

Python SDK requires a Python-ready environment. Python versions: Version 2.5 to Version 2.7. SDK is applicable to Windows and Linux, but as Python3.0 is not fully compatible with SDK Version 2.x, SDK does not support Python3.0 or later.

After Python is installed:

-   Input python in Linux shell and press Enter to view the Python version as follows:

    ```
    Python 2.5.4 (r254:67916, Mar 10 2010, 22:43:17) 
    [GCC 4.1.2 20080704 (Red Hat 4.1.2-46)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    					
    ```

-   Input python in Windows cmd and press Enter to view the Python version, as follows:

    ```
    C:\Documents and Settings\Administrator>python
    Python 2.7.5 (default, May 15 2013, 22:43:36) [MSC v. 1500 32 bit (Intel)] on win
    32
    Type "help", "copyright", "credits" or "license" for more information.
    					
    ```

    The preceding code shows the Python has been installed successfully.

-   Exception: After entering python in Windows cmd and pressing Enter, the system prompts Not an internal or external command. In such a case, check the configuration Environment variables Path and add the Python installation path.

    If the Python is not installed, you can get its installer from [Python official website](http://www.python.org/). The website provides detailed instructions and guidance for installing and using Python.


## Installation and usage {#section_wl4_qvb_wdb .section}

Unzip the downloaded Python SDK to the directory of the osscmd and then run `python osscmd + operation`. For example, upload an object to the bucket:

```
python osscmd put myfile.txt oss://mybucket
```

Please note that in osscmd, oss://bucket or oss://bucket/object is used to indicate a bucket or an object. oss:// is merely a way to indicate the resource with no other meanings.

If you need the detailed command list, enter `python osscmd`.

If you need the detailed parameter list instructions, enter `python osscmd help`.

