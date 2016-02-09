# deepanshpahwa.github.io

##What is it?##
Selendroid is a test automation framework which drives off the UI of Android
native and hybrid applications (apps) and the mobile web. Tests are written
using the Selenium 2 client API - that's it!
Selendroid can be used on emulators and real devices and can be integrated as
a node into the Selenium Grid for scaling and parallel testing.
The documentation specifies how to install and use Selendroid itself. The use
of Selendroid requires knowledge about how to use Selenium. The official
documentation of the WebDriver client API can be found [here](http://docs.seleniumhq.org/docs/03_webdriver.jsp).


##How to add it in your Android Studio Project?##
Working with Android studio 1.5.1 (with gradle)

Permissions required:
```xml
<uses-permission android:name="android.**permission.INTERNET"/>
```

Make a new module and choose 'java library for testing'.
In the gradle file of that module
```Gradle
testCompile 'junit:junit:4.12'
testCompile 'io.selendroid:selendroid-standalone:0.17.0'
testCompile 'io.selendroid:selendroid-client:0.17.0'
```

rename the ‘main’ folder under your test module to ‘test’ and start writing your test under that!

##Making the first test!##

```Java
public class ExampleUnitTest {

    private static SelendroidLauncher selendroidServer = null;
    private static SelendroidDriver driver = null;

    @BeforeClass
    public static void startSelendroidServer() throws Exception {
        if (selendroidServer != null) {
            selendroidServer.stopSelendroid();
        }
        SelendroidConfiguration config = new SelendroidConfiguration();
        config.addSupportedApp("app-debug.apk");
        selendroidServer = new SelendroidLauncher(config);
        selendroidServer.launchSelendroid();

        SelendroidCapabilities capa = SelendroidCapabilities.emulator("com.madebyatomicrobot.web:1.0"); // change from 'emulator' to 'device' if running on device
        capa.setLaunchActivity("com.madebyatomicrobot.web.MainActivity");

        driver = new SelendroidDriver(capa);
    }

    @AfterClass
    public static void stopSelendroidServer() {
        if (driver != null) {
            driver.quit();
        }
        if (selendroidServer != null) {
            selendroidServer.stopSelendroid();
        }
    }

    @Test
    public void testShouldBeAbleToEnterText() {

        WebElement inputField = driver.findElement(By.id("editText"));
        WebElement button = driver.findElement(By.id("button"));
        WebElement textView = driver.findElement(By.id("textView"));

        inputField.clear();
        inputField.sendKeys("Hello World");

        button.click();
        
        hideKeyboard();

        String text = textView.getText().toString();
        Assert.assertEquals(text, "Hello World");

    }

    private void hideKeyboard() {
        new Actions(driver).sendKeys(SelendroidKeys.BACK).perform();
    }
}
```
##Used methods:##

This method must be there before all the tests:

```
@Test
```
This method runs only once at the start:

```
@BeforeClass
```
This method runs only once after all the tests are completed:

```
@AfterClass
```
This method runs before every test and will run as many times as the number of the test in the Class:

```
@Before
```

To find elements in your layout, you should use
        '''Java
        WebElement inputField = driver.findElement(By.id("editText"));
        '''
You can find elements by id, by Class name, by Link text, by Css selector,By name, By partial Link Text, By tag name or By Xpath.

## Errors##

If you are getting errors like these repeatedly:
```
Feb 09, 2016 11:56:47 AM io.selendroid.standalone.android.impl.AbstractDevice isSelendroidRunning
INFO: Checking if the Selendroid server is running: http://localhost:8080/wd/hub/status
Feb 09, 2016 11:56:47 AM org.apache.http.impl.execchain.RetryExec execute
INFO: I/O exception (org.apache.http.NoHttpResponseException) caught when processing request to {}->http://localhost:8080: The target server failed to respond
Feb 09, 2016 11:56:47 AM org.apache.http.impl.execchain.RetryExec execute
INFO: Retrying request to {}->http://localhost:8080
Feb 09, 2016 11:56:47 AM org.apache.http.impl.execchain.RetryExec execute
INFO: I/O exception (org.apache.http.NoHttpResponseException) caught when processing request to {}->http://localhost:8080: The target server failed to respond
Feb 09, 2016 11:56:47 AM org.apache.http.impl.execchain.RetryExec execute
INFO: Retrying request to {}->http://localhost:8080
Feb 09, 2016 11:56:47 AM org.apache.http.impl.execchain.RetryExec execute
INFO: I/O exception (org.apache.http.NoHttpResponseException) caught when processing request to {}->http://localhost:8080: The target server failed to respond
Feb 09, 2016 11:56:47 AM org.apache.http.impl.execchain.RetryExec execute
INFO: Retrying request to {}->http://localhost:8080
Feb 09, 2016 11:56:47 AM io.selendroid.standalone.android.impl.AbstractDevice isSelendroidRunning
INFO: Can't connect to Selendroid server, assuming it is not running.
Feb 09, 2016 11:56:49 AM io.selendroid.standalone.io.ShellCommand exec
INFO: Executing shell command: [/Users/deepanshpahwa/Library/Android/sdk/platform-tools/adb, -s, emulator-5554, shell, echo, $EXTERNAL_STORAGE]
Feb 09, 2016 11:56:49 AM io.selendroid.standalone.io.ShellCommand exec
INFO: Shell command output
```
Then you havent added the internet permission.


