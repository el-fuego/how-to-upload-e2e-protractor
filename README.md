How to test upload at protractor
============================

The ways to test files uploading at e2e tests on JavaScript

## General

```js
describe('File uploader', function () {
  // absolute path to single file
  var filePath = '/home/youe-user/test-file.txt';
    
  it('should select files', function () {
    // find file by css-selector and put filePath
    $('input[type=file]').sendKeys(filePath);

    // verify filePath
    expect($('input[type=file]').getAttribute('value')).toBeEqual(filePath);
  });
});
```

## How to select many files

It works on Chrome. Firefox is select first file only

```js
filePath = '/home/youe-user/test-file.txt\n/home/youe-user/another-file.png';
```

## How to put web url
Cool idea for tests runnded on virtual machine and you can`t use absolute path

It works on Chrome, Firefox, Safari but not IE <br/>
Windows, Mac, Linux is supported
```js
filePath = '//my-server.com/test-file.txt';
```

For local server you can use
```js
filePath = '//127.0.0.1/test-file.txt';
```

## How to select all files at local folder

```js
// nodeJS modules
var fs = require('fs'),
  path = require('path');
  
/**
* Get paths for uploading and expectsation params
* @param folder {string} local directory with files. Example "../your-path-for-files-to-upload/"
* @return {
    paths: {string},
    names: [{string}],
    count: {int}
  }
*/
function getFilesToUpload(folder) {
  // get absolute path from current folder
  var uploadsDir = path.resolve(__dirname + '/' + folder),
  
    // find names of files at folder
    filesNames = fs.readdirSync(uploadsDir),
    
    // get absolute paths of files
    fileList = filesNames.map(
      function (fileName) {
        return uploadsDir + '/' + fileName;
      }
    );

  return {
    // use it at .sendKeys()
    paths: fileList.join('\n'),
    // use it for expect(..).toBe(..)
    names: filesNames,
    count: fileList.length
  };
}
```
