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

rename the ‘main’ folder under your test module to ‘test’.

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
        config.addSupportedApp("app-debug.apk");  // TODO
        selendroidServer = new SelendroidLauncher(config);
        selendroidServer.launchSelendroid();

        SelendroidCapabilities capa = SelendroidCapabilities.emulator("com.madebyatomicrobot.web:1.0");
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

    @Before
    public void setup() {
        driver.switchTo().window("NATIVE_APP");
    }

    @Ignore
    @Test
    //SAMPLE test with google.com:WORKS FINE
    public void testShouldBeAbleToEnterText() {

        WebElement inputField = driver.findElement(By.id("editText"));
        WebElement button = driver.findElement(By.id("button"));
        WebElement textView = driver.findElement(By.id("textView"));

        inputField.clear();
        inputField.sendKeys("Hello World");
        //waitFor(pageTitleToBe(driver(), "We Arrive Here"), 15, TimeUnit.SECONDS))

        button.sendKeys("  ");
        hideKeyboard();


        String text = textView.getText().toString();
        Assert.assertEquals(text, "Hello World");

    }

    private void hideKeyboard() {
        new Actions(driver).sendKeys(SelendroidKeys.BACK).perform();
    }

    private void delay(int seconds) {
        long end3 = System.currentTimeMillis()+(seconds*1000);
        while(System.currentTimeMillis()<end3)
        {

        }
    }
}
```

