# Installation {#concept_32114_zh .concept}

This topic describes how to install Ruby SDK. Install SDK with `gem` directly:

-   [GitHub](https://github.com/aliyun/aliyun-oss-ruby-sdk)
-   [API documentation](http://www.rubydoc.info/gems/aliyun-sdk/)
-   [ChangeLog](https://github.com/aliyun/aliyun-oss-ruby-sdk/blob/master/CHANGELOG.md)

## Prerequisites { .section}

-   You must have an active Alibaba Cloud OSS account. You must create an AccessKey ID and an AccessKey Secret.

## Installation { .section}

Install SDK with `gem` directly:

```
gem install aliyun-sdk

```

If the website https://rubygems.org is inaccessible, use the image source provided by Taobao.

```
gem install aliyun-sdk --clear-sources --source https://ruby.taobao.org

```

You can also install SDK by using [Bundler](http://bundler.io). Initially, add the following content to the `Gemfile` of your app:

```
gem 'aliyun-sdk', '~> 0.3.0'

```

Run the following command:

```
# (Optional). Use the image source provided by Taobao.
bundle config mirror.https://rubygems.org https://ruby.taobao.org

bundle install

```

**Note:** https://ruby.taobao.org is the complete image of rubygems.org and automatically synchronizes with the official source. You can access https://ruby.taobao.org if rubygems.org is inaccessible.

## Dependencies { .section}

-   Ruby 1.9.3 and later
-   Windows, Linux, or OS X system that supports Ruby

**Note:** 

-   Some gems on which the SDK depends are local extensions, and you must install ruby-dev to compile locally extended gems.
-   The environment for running the SDK-dependent gem \(nokogiri\) for processing XML must have the zlib library.

## Linux { .section}

The following method is used to install the preceding dependencies through Ubuntu in Linux:

```
sudo apt-get install ruby ruby-dev zlib1g-dev

```

The installation methods for various Linux releases \(each with a package management tool\) are similar.

## Windows { .section}

1.  Access [RubyInstaller](http://rubyinstaller.org/downloads/) to download RubyInstaller. Double-click the installer to install the tool. Select “Add Ruby Executables to Your PATH” during the installation process.

    **Note:** Download RubyInstaller 2.1 or an earlier version. RubyInstaller 2.2 cannot be installed due to [Issues](https://github.com/sparklemotion/nokogiri/issues/1256).

2.  Access [RubyInstaller](http://rubyinstaller.org/downloads/) to download the development kit. Make sure you select the right version. The downloaded kit is a compressed package. First create a folder named `RubyDev` in the C:\\ root directory, and decompress the package to the folder.
3.  Press the shortcut key `Windows+R`, input `cmd`, and press Enter to open the CLI. Then run the following commands:

    ```
     cd C:\RubyDev
     ruby dk.rb init
     ruby dk.rb install
    
    ```

    If “config.yml configuration error” is displayed during the last step “install”, use the text editor to open the `C:\RubyDev\config.yml` file and modify the file content as follows:

    ```
     ---
     - C:/Ruby21
    
    ```

    Save the config.yml file and continue `ruby dk.rb install`. “Ruby21” is the Ruby installation directory, which must be specified according to the actual name. Close the CLI once the operation is completed.

4.  Press the shortcut key `Windows+R`, input `cmd`, and press Enter to open the CLI. Run the following commands:

    ```
     gem install aliyun-sdk
    
    ```

    When installation is completed, input `irb` to open the Ruby interactive CLI, and input `require 'aliyun/oss'`. The SDK is successfully installed if “true” is displayed.


## OS X { .section}

1.  OS X has Ruby installed by default. To facilitate proper usage and management, we recommend you to install a development version.
2.  Run `xcode-select --install` on your terminal to install “Xcode command line tools”. If installation fails, you can manually download and install the tools \(see the steps to download\).
3.  Download Xcode command line tools from [Apple’s Developer Center](https://developer.apple.com/downloads/) using your Apple ID. Make sure that your selected version matches your system version. After download, double-click the dmg file to load the file. Double-click the installation program in the dialog box to install the tools. Enter your system password during the installation process.
4.  Run the following command on your terminal to install Brew:

    ```
     ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
    ```

5.  Run the following command on your terminal to install Ruby:

    ```
     brew install ruby
     exec $SHELL -l
    
    ```

6.  Run the following command on your terminal to install SDK:

    ```
     gem install aliyun-sdk
    
    ```

7.  Run the following command on your terminal to check the installation status of SDK.

    ```
     irb
     > require 'aliyun/oss'
     => true
    
    ```


