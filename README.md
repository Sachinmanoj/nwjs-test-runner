# nwjs-test-runner
Test runner for node webkit app with mocha and istanbul

Why nwjs-test-runner?
-------------------

We were working on a node-webkit app and we couldn't find a good way to write unit tests. Our unit tests initially were written with mocha and separated as client and node side tests. Client side tests were run with mocha-phantomjs and mocha used in node side tests. But, it was not enough. The tests that we wrote, though they passed in their respective environments we cannot be sure that they will pass in node-webkit. Also, node-webkit allowed us to write browser side and node side scripts intertwined, which means we had to mock a lot of things to get the test running. Hence, we decided to run the unit tests in the node-webkit environment. But, there were no good libraries that allowed us to do so. Hence, nwjs-test-runner was created.



How to use nwjs-test-runner?
--------------------------

Install to your project folder using npm:
> npm install nwjs-test-runner --save-dev

Create a config file in your project folder from where you are going to run your unit tests. The name of the file should be <B>nwtest.config.json</B>

Here is a sample file:-
```javascript
{
    "files": [
        "src/app/angular.js",
        "src/app/angular-mocks.js"
    ],
    "src": "src/**/*.js",
    "mock":"tests/**/*mock.js",
    "deps":"tests/**/*deps.js",
    "test":"tests/**/*test.js",
    "output": "test-results",
    "nwpath": "nodewebkit/nw",
	"testFolder": "unitTests/",
    "ext": "spec",
    "covReport": ['cobertura', 'html' ],
    "ignoreCoverageForUntested": true
}
```

'<B>files</B>' - Files to include in all your tests.These will be loaded before all your test, mock and source files

'<B>src*</B>' - The source file pattern to match and load

'<B>mock</B>' - The mock file pattern to match and load

'<B>deps</B>' - The dependencies file pattern to match and load. These are the files that you can specify as dependencies if you decide not to mock

'<B>test*</B>' - The test file pattern to match and load

'<B>output</B>' - The output folder to which all your test results will be published.

'<B>nwpath*</B>' - The path to the nw.exe.

'<B>testFolder*</B>' - The path to the unit test folder from current working directory.

'<B>ext</B>' - The extension you use for your test file name. Could be *.spec.js or *.test.js.

'<B>covReport</B>' - Array of coverage reports you need to be generated

'<B>ignoreCoverageForUntested</B>' - This will check if coverage needs to be run for source files that have no tests

How does it work?
-----------------

The nwjs-test-runner goes through your list of test files that you have added. It picks a test file and then tries to find the corresponding src, mock and dependency file by matching the name.

For ex:-

if you test file is named <B>app.test.js</B>, the src file should be named <B>app.js</B>, the dep file should be named <B>app.deps.js</B>  and your mock file should be named <B>app.mock.js</B>. Having a corresponding src and mock file is optional.

If you want to include some other source files to support your test, it can be done by adding the following code in your deps file. If you have to repeatedly mock something for your tests, you can create a single file with the mock and include it using deps file.


```javascript
module.exports = ['/path/to/file1.js', '/path/to/file2.js'];
```

It is recommended to follow the source code directory structure in unit test and mention the relative path from current working directory to the unit test directory in 'testFolder' parameter.

How to run the tests?
--------------------

As of now, you have to do 
> node node_modules/nwjs-test-runner

or, alternatively you can make this command to be executed for 'npm test' in package.json

In case, you want to run only a perticular test file, you can pass an optional command line argument. The test files that contain the string that you passed in their name only will be run

> node node_modules/nwjs-test-runner app.service.js


Command Line Options
--------------------

<B>'-h', '--help'</B> - Show help message and exit

<B>'-v', '--version'</B> - Show version number and exit

<B>'-d', '--directory'</B> - Specify the directory and run the unit test only for the source files inside the directory

<B>'-f', '--file'</B> - Specify a file or a set of files(comma seperated and without white-space) and run the unit test only for the files

<B>'-i', '--ignoreCoverageForUntested'</B> - Assign true to ignore coverage for source files that have no tests
