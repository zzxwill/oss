# Quick start {#concept_32070_zh .concept}

This topic describes how to quickly perform OSS operations \(such as view the bucket list or upload objects\) in the Node.js environment.

**Note:** To facilitate changes, a file named app.js is created to demonstrate the code used to perform the following operations.

## View the bucket list { .section}

Add the following code to the end of app.js, and then call the listBuckets interface to view the bucket list.

```language-js
async function listBuckets () {
  try {
    let result = await client.listBuckets();
  } catch(err) {
    console.log(err)
  }
}

listBuckets();
			
```

You can run `node app.js` to view the result.

For more information about bucket-related APIs, see [GitHub](https://github.com/ali-sdk/ali-oss/blob/master/test/node/bucket.test.js).

## View the object list { .section}

Modify the `app.js` file as follows and call the list interface to view the object list.

```
client.useBucket('Your bucket name');
async function list () {
  try {
    let result = await client.list({
      'max-keys': 5
    })
    console.log(result)
  } catch (err) {
    console.log (err)
  }
}
list();
```

You can run `node app.js` to view the result.

## Upload an object { .section}

Modify the `app.js` file as follows and call the put interface to upload a single object.

```language-js
client.useBucket('Your bucket name');

async function put () {
  try {
    let result = await client.put('object-name', 'local file');
    console.log(result);
   } catch (err) {
     console.log (err);
   }
}

put();
			
```

## Download an object { .section}

Modify the `app.js` file as follows and call the get interface to download a single object.

```language-js
async function get () {
  try {
    let result = await client.get('object-name');
    console.log(result);
  } catch (err) {
    console.log (err);
  }
}

get();
			
```

## Delete an object { .section}

Modify the `app.js` file and call the delete interface to delete an object.

```language-js
async function delete () {
  try {
    let result = await client.delete('object-name');
    console.log(result);
  } catch (err) {
    console.log (err);
  }
}

delete();
			
```

For more information about object-related interfaces, see [GitHub](https://github.com/ali-sdk/ali-oss/blob/master/test/node/object.test.js).

