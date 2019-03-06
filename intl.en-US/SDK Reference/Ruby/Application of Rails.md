# Application of Rails {#concept_32120_zh .concept}

This topic describes the application of Rails.

## Integration with Rails {#section_ppf_ktn_lfb .section}

To use the OSS Ruby SDK in Rails, add the following dependency to `Gemfile`:

```
gem 'aliyun-sdk', '~> 0.3.0

```

Then introduce the dependency when you use OSS:

```
require 'aliyun/oss'

```

The “rails/“ directory of the SDK provides helper code to facilitate your operation.

The following describes how to use the SDK to implement a simple OSS object manager \(oss-manager\) which provides the following functions:

-   List all the buckets of a user.
-   List all the objects in a bucket by level of directory.
-   Upload objects.
-   Download objects.

The integration process is described as follows:

1.  Create a project

    Install Rails and create a Rails application oss-manager:

    ```
    gem install rails
    rails new oss-manager
    
    ```

    We recommend, you manage your project code in GitHub:

    ```
    cd oss-manager
    git init
    git add .
    git commit -m "init project"
    
    ```

2.  Add the SDK dependency

    Add the SDK dependency to `oss-manager/Gemfile`:

    ```
    gem 'aliyun-sdk', '~> 0.3.0' 
    
    ```

    Run the following command in `oss-manager/`:

    ```
    bundle install
    
    ```

    Save the change in this step:

    ```
    git add .
    git commit -m "add aliyun-sdk dependency"
    
    ```

3.  Initialize an OSSClient

    To avoid initializing an OSSClient instance each time you use it in the project, you can add an initialization file to your project.

    ```language-ruby
    # oss-manager/config/initializers/aliyun_oss_init.rb
    require 'aliyun/oss'
    
    module OSS
      def self.client
        unless @client
          Aliyun::Common::Logging.set_log_file('./log/oss_sdk.log')
    
          @client = Aliyun::OSS::Client.new(
            endpoint:
              Rails.application.secrets.aliyun_oss['endpoint'],
            access_key_id:
              Rails.application.secrets.aliyun_oss['access_key_id'],
            access_key_secret:
              Rails.application.secrets.aliyun_oss['access_key_secret']
          )
        end
    
        @client
      end
    end
    
    ```

    The preceding code can be found in the “rails/“ directory of the SDK. The initialization file simplifies the use of the OSSClient in your project:

    ```
    buckets = OSS.client.list_buckets
    
    ```

    The endpoint, AccessKeyID, and AccessKeySecret are stored in `oss-manager/conf/secrets.yml`. For example:

    ```language-yaml
    development:
      secret_key_base: xxxx
      aliyun_oss:
        endpoint: xxxx
        access_key_id: aaaa
        access_key_secret: bbbb
    
    ```

    Save the code:

    ```
    git add .
    git commit -m "add aliyun-sdk initializer"
    
    ```

4.  Implement the “List Buckets” feature

    Firstly, use Rails to generate a controller for bucket management:

    ```
    rails g controller buckets index
    
    ```

    The following objects are generated in `oss-manager`:

    -   app/controller/buckets\_controller.rb: The bucket-related logic code
    -   app/views/buckets/index.html.erb: The bucket-related demonstration code
    -   app/helpers/buckets\_helper.rb: Some helper functions
    Edit buckets\_controller.rb. Call the OSSClient to save the `list_buckets` results to the `@buckets` variable:

    ```language-ruby
    class BucketsController < ApplicationController
      def index
        @buckets = OSS.client.list_buckets
      end
    end
    
    ```

    Edit views/buckets/index.html.erb to display the bucket list:

    ```language-html
    <h1>Buckets</h1>
    <table class="table table-striped">
      <tr>
        <th>Name</th>
        <th>Location</th>
        <th>CreationTime</th>
      </tr>
    
      <% @buckets.each do |bucket| %>
      <tr>
        <td><%= link_to bucket.name, bucket_objects_path(bucket.name) %></td>
        <td><%= bucket.location %></td>
        <td><%= bucket.creation_time.localtime.to_s %></td>
      </tr>
      <% end %>
    </table>
    
    ```

    The `bucket_objects_path` is a helper function is stored inapp/helpers/buckets\_helper.rb:

    ```language-ruby
    module BucketsHelper
      def bucket_objects_path(bucket_name)
        "/buckets/#{bucket_name}/objects"
      end
    end
    
    ```

    This is how all the buckets are listed. Before you implement the function, configure a Rails route so that the correct logic can be called after an address is entered in the address bar of the browser. Edit `config/routes.rb` and add the following:

    ```
    resources :buckets do
      resources :objects
    end
    
    ```

    Now, input `rails s` in oss-manager/ to start the Rails server, and input `http://localhost:3000/buckets/` in the address bar of the browser. The bucket list is displayed.

    Finally, save the code.

    ```
    git add .
    git commit -m "add list buckets feature"
    
    ```

5.  Implement the “List Objects” feature

    First generate a controller for object management:

    ```
    rails g controller objects index
    
    ```

    Edit app/controllers/objects\_controller.rb as follows:

    ```language-ruby
    class ObjectsController < ApplicationController
      def index
        @bucket_name = params[:bucket_id]
        @prefix = params[:prefix]
        @bucket = OSS.client.get_bucket(@bucket_name)
        @objects = @bucket.list_objects(:prefix => @prefix, :delimiter => '/')
      end
    end
    
    ```

    The preceding code obtains the bucket name from the URL parameters first. An additional prefix is required to list objects by directory level. Call the list\_objects interface of the OSSClient to obtain the object list. Note that the preceding procedure provide a list of the objects which are delimited by the slash \(/\) and whose names contain a specified prefix. This also aims to display the object list by directory level. See [Manage objects](reseller.en-US/SDK Reference/Ruby/Manage objects.md#).

    Next, edit app/views/objects/index.html.erb as follows:

    ```language-html
    <h1>Objects in <%= @bucket_name %></h1>
    <p> <%= link_to 'Upload file', new_object_path(@bucket_name, @prefix) %></p>
    <table class="table table-striped">
      <tr>
        <th>Key</th>
        <th>Type</th>
        <th>Size</th>
        <th>LastModified</th>
      </tr>
    
      <tr>
        <td><%= link_to '../', with_prefix(upper_dir(@prefix)) %></td>
        <td>Directory</td>
        <td>N/A</td>
        <td>N/A</td>
      </tr>
    
      <% @objects.each do |object| %>
      <tr>
        <% if object.is_a?( Aliyun::OSS::Object) %>
        <td><%= link_to remove_prefix(object.key, @prefix),
                @bucket.object_url(object.key) %></td>
        <td><%= object.type %></td>
        <td><%= number_to_human_size(object.size) %></td>
        <td><%= object.last_modified.localtime.to_s %></td>
        <% else  %>
        <td><%= link_to remove_prefix(object, @prefix), with_prefix(object) %></td>
        <td>Directory</td>
        <td>N/A</td>
        <td>N/A</td>
        <% end  %>
      </tr>
      <% end %>
    </table>
    
    ```

    In the preceding code, the main logic for listing objects according to the directory structure is as follows:

    1.  “../“ always appears at the beginning to indicate the upper-level directory.
    2.  Objects with a common prefix in their names are displayed as directories.
    3.  Objects are displayed as files.
    The preceding code uses `with_prefix`, `remove_prefix`, and other helper functions which are defined in app/helpers/objects\_helper.rb:

    ```language-ruby
    module ObjectsHelper
      def with_prefix(prefix)
        "? prefix=#{prefix}"
      end
    
      def remove_prefix(key, prefix)
        key.sub(/^#{prefix}/, '')
      end
    
      def upper_dir(dir)
        dir.sub(/[^\/]+\/$/, '') if dir
      end
    
      def new_object_path(bucket_name, prefix = nil)
        "/buckets/#{bucket_name}/objects/new/#{with_prefix(prefix)}"
      end
    
      def objects_path(bucket_name, prefix = nil)
        "/buckets/#{bucket_name}/objects/#{with_prefix(prefix)}"
      end
    end
    
    ```

    Once you run the preceding code, run `rails s`, and enter `http://localhost:3000/buckets/my-bucket/objects/` in the address bar of the browser. The object list is displayed.

    Save the code as usual:

    ```
    git add .
    git commit -m "add list objects feature"
    
    ```

6.  Download objects

    Note the the preceding code adds a link to each listed object:

    ```language-html
    <td><%= link_to remove_prefix(object.key, @prefix),
            @bucket.object_url(object.key) %></td>
    
    ```

    The `Bucket#object_url` method is used to generate a temporary URL for an object. For more information, see [Download objects](reseller.en-US/SDK Reference/Ruby/Download objects.md#).

7.  Upload files

    You can use either of the following methods to upload files with a server app such as Rails:

    1.  Upload a file to the Rails server that uploads the object to the OSS. In this method, the Rails server works as a transit server by copying the object to the OSS. The upload process is inefficient.
    2.  Upload a file to the OSS directly with the form and temporary credentials generated by the Rails server.
    The first method is relatively simple, similar to the common file upload method. The following describes how to use the second method to upload a file:

    Add a `#new` method to app/controllers/objects\_controller.rb to generate an upload form.

    ```language-ruby
    def new
      @bucket_name = params[:bucket_id]
      @prefix = params[:prefix]
      @bucket = OSS.client.get_bucket(@bucket_name)
      @options = {
        :prefix => @prefix,
        :redirect => 'http://localhost:3000/buckets/'
      }
    end
    
    ```

    Then edit app/views/objects/new.html.erb as follows:

    ```language-html
    <h2>Upload object</h2>
    
    <%= upload_form(@bucket, @options) do %>
      <table class="table table-striped">
        <tr>
          <td><label>Bucket:</label></td>
          <td><%= @bucket.name  %></td>
        </tr>
        <tr>
          <td><label>Prefix:</label></td>
          <td><%= @prefix  %></td>
        </tr>
    
        <tr>
          <td><label>Select file:</label></td>
          <td><input type="file" name="file" style="display:inline" /></td>
        </tr>
    
        <tr>
          <td colspan="2">
            <input type="submit" class="btn btn-default" value="Upload" />
            <span>&nbsp;&nbsp</span>
            <%= link_to 'Back', objects_path(@bucket_name, @prefix) %>
          </td>
        </tr>
      </table>
    <% end %>
    
    ```

    Specifically, the `upload_form` is a helper function provided by SDK to generate an upload form. The function is stored in rails/aliyun\_oss\_helper.rb of the SDK. You must copy the function to the app/helpers/ directory, run `rails s`, and input the `http://localhost:3000/buckets/my-bucket/objects/new` in the address bar of the browser. The file is uploaded.

    Do not forget to save the code:

    ```
    git add .
    git commit -m "add upload object feature"
    
    ```

8.  Add a style

    You can add some styles \(CSS\) to refine the page outlook.

    Download [bootstrap](http://getbootstrap.com/). Unzip the package and then copy `bootstrap.min.css` to `app/assets/stylesheets/`.

    Open `app/views/layouts/application.html.erb` and modify the `yield` line as follows:

    ```language-html
      <div id="main">
        <%= yield %>
      </div>
    
    ```

    In this way, each page is added with a `<div>` with main as the ID. Then modify `app/assets/stylesheets/application.css` and add the following content:

    ```language-css
    body {
        text-align: center;
    }
    
    div#main {
        text-align: left;
        width: 1024px;
        margin: 0 auto;
    }
    
    ```

    The preceding modification displays the body content of a webpage in a centered layout. The new style makes the page layout more elegant.

    So far, a simple demo has been completed. For a complete demo, see [Alibaba Cloud OSS Rails Demo](https://github.com/aliyun/alicloud-oss-rails-demo).


