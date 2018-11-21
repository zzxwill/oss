# Installation {#concept_85288_zh .concept}

## Environment requirements {#section_kdj_d43_kfb .section}

This version of Python SDK is applicable to Python 2.6, 2.7, 3.3, 3.4 and 3.5.

-   Installation environment

    Follow the instructions on [python official website](http://www.python.org) to install an appropriate version of Python.

-   Check Python version

    Run `python -V` to check the Python version.

    In Windows, if a “Not an internal or external command” message prompts, edit the `Path` in environment variables to add the installation path of Python and pip. In general, pip is installed in the Script directory located in the installation path of Python.

    **Note:** You may need to restart your computer make sure the environment variable takes effect.


## Download SDK { .section}

-    [Download from GitHub](https://github.com/aliyun/aliyun-oss-python-sdk) 
-    [Download previous versions](https://github.com/aliyun/aliyun-oss-python-sdk/releases) 

For more information, see [OSS API documents](http://gosspublic.alicdn.com/sdks/python/apidocs/latest/zh-cn/index.html).

## Install Python-devel { .section}

Python SDK uses the crcmod library which depends on the Python.h to caculate CRC. Therefore, if the Python.h file is missing, Python SDK can be successfully installed while the installation of crcmod C extension mode fails, which significantly reduces the efficiency of operations, such as upload and download. Therefore, you must install the python-devel package if it is missing.

-   You do not need to install the python-devel package in Windows and Mac OS X because header files that Python depends on are installed together with Python.

-   For CentOS, RHEL, and Fedora, run the following command to install python-devel:

    ```language-bash
    yum install python-devel
    
    ```

-   For Debian and Ubuntu, run the following command to install python-devel:

    ```language-bash
    apt-get install python-dev
    
    ```


## Install SDK { .section}

-   Install SDK by Pip

    Run the following command:

    ```language-bash
    pip install oss2
    
    ```

    **Note:** If Pip is not installed in your environment, see [Pip official website](https://pip.pypa.io/en/stable/installing/) to install Pip.

-   Install SDK by source code

    Download the SDK package of an appropriate version from [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk) and decompress the package.Make sure that the setup.py file exists in the directory and run the following command:

    ```language-bash
    python setup.py install
    
    ```


## Verification { .section}

-   Verify the SDK version
    1.  Input `python` in the command line and press Enter to enter the Python environment.
    2.  Run the following command to verify the SDK version:

        ```language-python
        >>> import oss2
        >>> oss2.__version__
        '2.0.0'
        
        ```

        If the preceding information is displayed, it means that you have successfully installed the Python SDK 2.0.0.

-   Verify the crcmod

    Two methods are available for crcmod to calculate CRC: the C extension mode \(in which crcmod calls the extension library \_crcfunext.so to calculate CRC\) and the Python-only mode. The performance of C extension mode is far better than the Python-only mode. The crcmod library is automatically installed when oss2 is installed. For more information about crcmod, see[crcmod introduction](http://crcmod.sourceforge.net/intro.html).

    If you find that the speed of upload and download interfaces is significantly slow compared with other tools \(such as [ossutil](../../../../reseller.en-US/Utilities/ossutil/Download and installation.md#)\) or other SDKs after the Python SDK is installed, it is probably because that the C extension mode of crcmod library is not installed successfully, resulting that the Python-only mode is used to calculate CRC in upload and download.

    You can verify whether the C extension mode of crcmode is successfully installed as follows:

    1.  Input `python` in the command line and press Enter to enter the Python environment.
    2.  Input `import crcmod._crcfunext` and press Enter.
        -   If no exception is reported, it indicates that the C extension mode has been successfully installed.
        -   If the following message is displayed, it indicates that the C extension mode is not successfully installed.

            ```language-python
            >>> import crcmod._crcfunext
            Traceback (most recent call last):
            File "<stdin>", line 1, in <module>
            ImportError: No module named _crcfunext
            
            ```

            The error occurs because the Python.h file that \_crcfunext.so depends on is missing when the crcmod is compiled, resulting in that the \_crcfunext.so fails to be generated. In this case, CRC is calculated in the Python-only method. The SDK is installed successfully. However, the efficiency of operations such as upload and download is low.

            Perform the following operations if the problem occurs.

            1.  Run the following command to uninstall crcmod:

                ```
                pip uninstall crcmod
                ```

            2.  [Install python-devel.](#ul_odw_w43_kfb)
            3.  Run the following command to install crcmod again:

                ```
                pip install crcmod
                ```

                If the installation still fails, uninstall crcmod and run the following command to check detailed information:

                ```
                pip install crcmod -v
                ```


## Uninstall SDK { .section}

If the SDK fails to be installed, we recommend you to uninstall the SDK using pip and install it again. You can run the following command to uninstall the SDK.

```language-bash
pip uninstall oss2

```

