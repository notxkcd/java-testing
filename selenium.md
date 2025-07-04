# Selenium Interview Questions and Answers

## Questions Sheet

1. **Diff between findElement() and findElements()**
   - `findElement()`: Returns the first `WebElement` matching the locator. Throws `NoSuchElementException` if not found.
   - `findElements()`: Returns a `List<WebElement>` of all matching elements. Returns an empty list if none found.

2. **Diff Between Relative and Absolute XPath**
   - **Absolute XPath**: Full path from root (e.g., `/html/body/div[1]/input`). Fragile due to DOM changes.
   - **Relative XPath**: Partial path using attributes or functions (e.g., `//input[@id='username']`). More robust.

3. **Difference between navigate and get**
   - `driver.get(url)`: Loads a new page, waits for it to load completely.
   - `driver.navigate().to(url)`: Similar to `get`, but part of the navigation interface, supports additional methods like `back()`, `forward()`, `refresh()`.

4. **Explain Dynamic XPath? What is its aspects?**
   - **Dynamic XPath**: Handles elements with changing attributes (e.g., IDs with timestamps). Uses functions like `contains()`, `starts-with()`, or axes.
   - **Aspects**:
     - Robustness: Resilient to minor DOM changes.
     - Flexibility: Uses partial matches (e.g., `//div[contains(@class, 'dynamic')]`).
     - Maintainability: Reduces hardcoding.

5. **How do you handle version issues, What approach you will handle to resolve?**
   - **Issue**: Mismatch between Selenium, browser, and WebDriver versions.
   - **Approach**:
     1. Use WebDriverManager to auto-manage driver versions:
        ```java
        WebDriverManager.chromedriver().setup();
        ```
     2. Check compatibility (e.g., Selenium 4.XX with Chrome 120+).
     3. Update dependencies in `pom.xml`.
     4. Test on a staging environment before production.

6. **How to Click a Button in the Child window from parent window?**
   ```java
   String parent = driver.getWindowHandle();
   for (String window : driver.getWindowHandles()) {
       driver.switchTo().window(window);
       if (!window.equals(parent)) {
           driver.findElement(By.id("buttonId")).click();
           break;
       }
   }
   driver.switchTo().window(parent);
   ```

7. **How to handle dynamic web elements?**
   - Use dynamic locators (e.g., `contains()`, `starts-with()`).
   - Implement explicit waits:
     ```java
     WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
     wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//div[contains(@id, 'dynamic')]"))).click();
     ```

8. **How to handle exception in your project and What are the exceptions u faced in your project?**
   - **Handling**: Use try-catch blocks, explicit waits, or retry logic.
   - **Common Exceptions**:
     - `NoSuchElementException`: Element not found.
     - `StaleElementReferenceException`: DOM changed.
     - `TimeoutException`: Wait condition not met.
     - `ElementNotInteractableException`: Element not clickable.
     ```java
     try {
         driver.findElement(By.id("id")).click();
     } catch (NoSuchElementException e) {
         System.out.println("Element not found: " + e.getMessage());
     }
     ```

9. **How to handle the alert popup?**
   ```java
   Alert alert = driver.switchTo().alert();
   alert.accept(); // For OK
   // alert.dismiss(); // For Cancel
   ```

10. **How to take preceding XPath**
    - Uses `preceding` or `preceding-sibling` axes:
      ```java
      //input[@id='target']/preceding::div
      //input[@id='target']/preceding-sibling::label
      ```

11. **How will u handle stale element exception**
   ```java
   WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
   try {
       WebElement element = wait.until(ExpectedConditions.elementToBeClickable(By.id("id")));
       element.click();
   } catch (StaleElementReferenceException e) {
       element = driver.findElement(By.id("id")); // Re-locate element
       element.click();
   }
   ```

12. **How will you handle the popup window? Other than getWindowHandle?**
   - Use `getWindowHandles()` with a loop or JavaScript to open/close windows:
     ```java
     ((JavascriptExecutor) driver).executeScript("window.open('about:blank', '_blank');");
     for (String window : driver.getWindowHandles()) {
         driver.switchTo().window(window);
         // Perform actions
     }
     ```

13. **How will you validate the dropdown options in Selenium? Write code**
   ```java
   Select dropdown = new Select(driver.findElement(By.id("dropdown")));
   List<WebElement> options = dropdown.getOptions();
   List<String> actual = options.stream().map(WebElement::getText).collect(Collectors.toList());
   List<String> expected = Arrays.asList("Option1", "Option2");
   Assert.assertEquals(actual, expected);
   ```

14. **If you have not found any element in findElement and findElements, what will get returned?**
   - `findElement()`: Throws `NoSuchElementException`.
   - `findElements()`: Returns an empty `List<WebElement>`.

15. **Open an Amazon e-commerce site & search the results for mobiles in search textbox using Selenium Java**
   ```java
   WebDriver driver = new ChromeDriver();
   driver.get("https://www.amazon.com");
   driver.findElement(By.id("twotabsearchtextbox")).sendKeys("mobiles");
   driver.findElement(By.id("nav-search-submit-button")).click();
   ```

16. **Parameters in Selenium**
   - Use TestNG for parameterization:
     ```java
     @Parameters("browser")
     @Test
     public void test(String browser) {
         WebDriver driver = browser.equals("chrome") ? new ChromeDriver() : new FirefoxDriver();
     }
     ```
   - Or use properties files for configuration:
     ```java
     Properties prop = new Properties();
     prop.load(new FileInputStream("config.properties"));
     String url = prop.getProperty("url");
     ```

17. **Synchronization in Selenium? Syntax for implicit wait**
   - **Synchronization**: Ensures WebDriver waits for elements to load.
   - **Implicit Wait**:
     ```java
     driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
     ```

18. **Take an XPath in Facebook for columns, XPath should be in common usage**
   ```java
   //div[contains(@class, 'fb-content')]//table//tr/td
   ```

19. **Take screenshot syntax**
   ```java
   File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
   FileUtils.copyFile(src, new File("screenshot.png"));
   ```

20. **What are the exceptions you faced in Java and Selenium**
   - **Java**: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IOException`.
   - **Selenium**: See Q8.

21. **What are the locators in XPath**
   - Attributes: `id`, `class`, `name`.
   - Functions: `contains()`, `starts-with()`, `text()`.
   - Axes: `parent`, `child`, `following`, `preceding`, `sibling`.

22. **What is the return type of getWindowHandle()?**
   - `String`

23. **What is window handling and its methods**
   - **Window Handling**: Managing multiple browser windows/tabs.
   - **Methods**: `getWindowHandle()`, `getWindowHandles()`, `switchTo().window()`.

24. **Which locators can be used frequently**
   - ID, Name, CSS Selector, XPath (relative), due to reliability and speed.

25. **Which version of Selenium you use in your project?**
   - Example: Selenium 4.XX for W3C compliance and features like relative locators.

26. **Write a syntax for moveToElement?**
   ```java
   Actions actions = new Actions(driver);
   actions.moveToElement(driver.findElement(By.id("element"))).build().perform();
   ```

27. **Write dynamic XPath for eBay**
   ```java
   //input[contains(@placeholder, 'Search')]
   ```

28. **You have many Link Text in webpage and if you click one it will open new webpage and you need to find the element in that new webpage, how you can achieve that?**
   ```java
   String parent = driver.getWindowHandle();
   driver.findElement(By.linkText("Link Text")).click();
   for (String window : driver.getWindowHandles()) {
       if (!window.equals(parent)) {
           driver.switchTo().window(window);
           driver.findElement(By.id("newPageElement")).click();
           break;
       }
   }
   ```

29. **Actions class and methods**
   - **Actions Class**: Handles mouse/keyboard interactions.
   - **Methods**: `click()`, `doubleClick()`, `contextClick()`, `moveToElement()`, `dragAndDrop()`, `sendKeys()`.

30. **Assertion in Selenium**
   - Use TestNG assertions:
     ```java
     Assert.assertEquals(driver.getTitle(), "Expected Title");
     SoftAssert soft = new SoftAssert();
     soft.assertTrue(element.isDisplayed());
     soft.assertAll();
     ```

31. **Can we use chrome driver = new ChromeDriver()**
   - Yes, but prefer `WebDriver driver = new ChromeDriver()` for polymorphism and flexibility.

32. **Debugging in Selenium**
   - Use IDE breakpoints, logs, or `System.out.println()`.
   - Example:
     ```java
     WebElement element = driver.findElement(By.id("id"));
     System.out.println("Element found: " + element.getTagName());
     ```

33. **Difference between quit and close**
   - `driver.close()`: Closes the current browser window.
   - `driver.quit()`: Closes all browser windows and ends the WebDriver session.

34. **Difference between WebDriver and ChromeDriver**
   - **WebDriver**: Interface for browser automation.
   - **ChromeDriver**: Concrete class implementing WebDriver for Chrome.

35. **Difference between Click and Javascript Click?**
   - `click()`: Selenium’s method, interacts with DOM.
   - **JavaScript Click**: Uses `executeScript` for elements not interactable via Selenium:
     ```java
     ((JavascriptExecutor) driver).executeScript("arguments[0].click();", element);
     ```

36. **Disadvantages of Selenium?**
   - No support for desktop apps.
   - Limited image-based testing.
   - Requires third-party tools for reporting.
   - Slow for complex UI interactions.

37. **Driver syntax**
   ```java
   WebDriver driver = new ChromeDriver();
   ```

38. **Dynamic XPath and XPath axes**
   - **Dynamic XPath**: See Q4.
   - **Axes**: `ancestor`, `descendant`, `following`, `preceding`, `parent`, `child`, `sibling`.

39. **Exceptions in Selenium?**
   - See Q8.

40. **Explain about locators in Selenium**
   - ID, Name, ClassName, TagName, LinkText, PartialLinkText, XPath, CSS Selector.

41. **Explain about relative XPath and its types**
   - **Relative XPath**: Starts with `//`, uses attributes or functions.
   - **Types**:
     - Attribute-based: `//input[@id='username']`
     - Function-based: `//div[contains(text(), 'example')]`
     - Axes-based: `//div[@id='parent']/following-sibling::div`

42. **Explain Alerts and Types**
   - **Alerts**: JavaScript popups (alert, confirm, prompt).
   - **Types**:
     - Simple Alert: `alert.accept()`
     - Confirm Alert: `alert.accept()` or `alert.dismiss()`
     - Prompt Alert: `alert.sendKeys("text"); alert.accept()`

43. **Explain JavaScript Executor?**
   - Executes JavaScript code in the browser:
     ```java
     JavascriptExecutor js = (JavascriptExecutor) driver;
     js.executeScript("window.scrollTo(0, document.body.scrollHeight);");
     ```

44. **Find XPath for the module (link was already in open some e-commerce sites)**
   ```java
   //div[contains(@class, 'module')]//a
   ```

45. **Get window handle**
   ```java
   String handle = driver.getWindowHandle();
   ```

46. **Given a link and webelement write an XPath but don't use span or div class, use only XPath axes like following sibling or preceding sibling**
   ```java
   //input[@id='target']/following-sibling::button
   ```

47. **Given a link https://www.countries-ofthe-world.com/capitals-of-the-world.html In this write XPath for in such a way, If I give Afghanistan in XPath it should get its capital Kabul if Albania is given, it should return Tirana and so on**
   ```java
   //tr[td='Afghanistan']/td[2]
   ```

48. **Given Amazon link and asked to write XPath for a particular element which is under ul\li tag**
   ```java
   //ul/li/a[contains(text(), 'Target Element')]
   ```

49. **Have you used an interface in your framework other than Selenium interfaces?**
   - Example: `Map` for storing test data:
     ```java
     Map<String, String> testData = new HashMap<>();
     ```

50. **Headerfile**
   - Not applicable in Selenium/Java. Likely refers to HTTP headers in API testing:
     ```java
     given().header("Content-Type", "application/json").get("/api");
     ```

51. **How Authentication popup is handled**
   ```java
   driver.get("https://username:password@website.com");
   ```

52. **How many ways to define WebDriver**
   - Direct: `WebDriver driver = new ChromeDriver();`
   - Remote: `WebDriver driver = new RemoteWebDriver(new URL("http://hub:4444/wd/hub"), new ChromeOptions());`
   - Factory pattern:
     ```java
     WebDriver driver = BrowserFactory.getDriver("chrome");
     ```

53. **How many ways we can upload a file through Selenium, if button tag What will be the way**
   - **Ways**: `sendKeys()`, Robot class, AutoIt.
   - **Button Tag**:
     ```java
     driver.findElement(By.xpath("//input[@type='file']")).sendKeys("C:/file.txt");
     ```

54. **How to check if element is enabled if Selenium enabled method is not working**
   ```java
   boolean isEnabled = ((JavascriptExecutor) driver).executeScript("return arguments[0].disabled;", element).equals(false);
   ```

55. **How to handle Chrome WebDriver**
   ```java
   WebDriverManager.chromedriver().setup();
   WebDriver driver = new ChromeDriver();
   ```

56. **How to handle date drop-down values**
   ```java
   Select day = new Select(driver.findElement(By.id("day")));
   day.selectByValue("15");
   ```

57. **How to handle Dropdown in Selenium? Write code for that**
   ```java
   Select dropdown = new Select(driver.findElement(By.id("dropdown")));
   dropdown.selectByVisibleText("Option");
   ```

58. **How to handle keyboard events**
   ```java
   Actions actions = new Actions(driver);
   actions.sendKeys(Keys.ENTER).build().perform();
   ```

59. **How to handle notification popup**
   ```java
   ChromeOptions options = new ChromeOptions();
   options.addArguments("--disable-notifications");
   WebDriver driver = new ChromeDriver(options);
   ```

60. **How to handle static and dynamic XPath**
   - **Static**: Fixed path (e.g., `//input[@id='static']`).
   - **Dynamic**: Use `contains()`, `starts-with()` (e.g., `//input[contains(@id, 'dynamic')]`).

61. **How to handle the webpage which takes more time to load?**
   ```java
   driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(30));
   WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
   wait.until(ExpectedConditions.jsReturnsValue("return document.readyState === 'complete'"));
   ```

62. **How to open a new window in Selenium? Write code for that**
   ```java
   ((JavascriptExecutor) driver).executeScript("window.open('about:blank', '_blank');");
   ```

63. **How to handle window in Selenium**
   ```java
   String parent = driver.getWindowHandle();
   for (String window : driver.getWindowHandles()) {
       driver.switchTo().window(window);
   }
   ```

64. **How to switch from one window to another**
   - Same as Q63.

65. **How to switch the driver in your project**
   - Use a factory or conditional logic:
     ```java
     WebDriver driver = browser.equals("chrome") ? new ChromeDriver() : new FirefoxDriver();
     ```

66. **How WebDriver is initialized**
   ```java
   WebDriver driver = new ChromeDriver();
   ```

67. **How will read/write data in JSON file in Selenium Java**
   ```java
   // Read
   ObjectMapper mapper = new ObjectMapper();
   JsonNode json = mapper.readTree(new File("data.json"));
   String value = json.get("key").asText();
   // Write
   ObjectNode node = mapper.createObjectNode();
   node.put("key", "value");
   mapper.writeValue(new File("data.json"), node);
   ```

68. **How will u handle element not interactable exception**
   ```java
   WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
   wait.until(ExpectedConditions.elementToBeClickable(By.id("id"))).click();
   ```

69. **How will you handle CAPTCHA**
   - Manual intervention, disable CAPTCHA in test environment, or use third-party services (not recommended for production).

70. **How you handle frames**
   ```java
   driver.switchTo().frame("frameName");
   driver.findElement(By.id("element")).click();
   driver.switchTo().defaultContent();
   ```

71. **How you locate the elements in Selenium**
   - See Q40.

72. **How you will find Broken links in Selenium, write a code for that?**
   ```java
   List<WebElement> links = driver.findElements(By.tagName("a"));
   for (WebElement link : links) {
       String url = link.getAttribute("href");
       HttpURLConnection conn = (HttpURLConnection) new URL(url).openConnection();
       conn.setRequestMethod("HEAD");
       int code = conn.getResponseCode();
       if (code >= 400) {
           System.out.println("Broken: " + url);
       }
   }
   ```

73. **How you will get the 8th search Result in Google search when you are typing Selenium**
   ```java
   driver.get("https://www.google.com");
   driver.findElement(By.name("q")).sendKeys("Selenium");
   driver.findElement(By.name("btnK")).click();
   WebElement result = driver.findElements(By.cssSelector("div.g")).get(7);
   System.out.println(result.findElement(By.tagName("a")).getAttribute("href"));
   ```

74. **Inspect an element from Flipkart and write XPath**
   ```java
   //div[contains(@class, 'product')]//a
   ```

75. **Java executor for disabled elements**
   ```java
   JavascriptExecutor js = (JavascriptExecutor) driver;
   js.executeScript("arguments[0].removeAttribute('disabled');", driver.findElement(By.id("disabled")));
   ```

76. **Mobile application**
   - Use Appium for mobile automation:
     ```java
     DesiredCapabilities caps = new DesiredCapabilities();
     caps.setCapability("platformName", "Android");
     caps.setCapability("app", "app.apk");
     AppiumDriver driver = new AndroidDriver(new URL("http://localhost:4723/wd/hub"), caps);
     ```

77. **moveToElement, doubleClick uses**
   ```java
   Actions actions = new Actions(driver);
   actions.moveToElement(element).build().perform(); // Hover
   actions.doubleClick(element).build().perform(); // Double-click
   ```

78. **Object repository in Selenium**
   - Stores locators in a centralized file (e.g., properties or Excel) or uses Page Factory:
     ```java
     @FindBy(id = "username") WebElement username;
     ```

79. **Open Amazon & go to ALL tab write XPath for all hyperlinks**
   ```java
   //div[@id='nav-hamburger-menu']//a
   ```

80. **POM in Selenium usage**
   - Page Object Model: Encapsulates page elements and methods in classes for maintainability and reusability.

81. **Rating about yourself in Java, Selenium**
   - Example: Java: 4/5 (OOPs, collections, exception handling). Selenium: 4/5 (UI automation, frameworks).

82. **Return type of findElement and findElements**
   - `findElement()`: `WebElement`
   - `findElements()`: `List<WebElement>`

83. **Robot class and methods?**
   ```java
   Robot robot = new Robot();
   robot.keyPress(KeyEvent.VK_ENTER); // Press key
   robot.mouseMove(100, 100); // Move mouse
   ```

84. **Select a link from drop-down, navigate to new tab, enter username and password in new tab, click on submit button. The page will navigate to the base window again. Write scripting for this**
   ```java
   String parent = driver.getWindowHandle();
   Select dropdown = new Select(driver.findElement(By.id("dropdown")));
   dropdown.selectByVisibleText("Link");
   for (String window : driver.getWindowHandles()) {
       if (!window.equals(parent)) {
           driver.switchTo().window(window);
           driver.findElement(By.id("username")).sendKeys("user");
           driver.findElement(By.id("password")).sendKeys("pass");
           driver.findElement(By.id("submit")).click();
           break;
       }
   }
   driver.switchTo().window(parent);
   ```

85. **Selenium artifacts**
   - Dependencies, test reports, screenshots, or logs generated during execution.

86. **Selenium Grid usage**
   - Runs tests on multiple browsers/machines:
     ```java
     WebDriver driver = new RemoteWebDriver(new URL("http://hub:4444/wd/hub"), new ChromeOptions());
     ```

87. **Switch to and Navigate to**
   - `switchTo()`: Switches to frames, windows, or alerts.
   - `navigate().to()`: Loads a URL, supports `back()`, `forward()`.

88. **Syntax of JavascriptExecutor and its methods**
   ```java
   JavascriptExecutor js = (JavascriptExecutor) driver;
   js.executeScript("arguments[0].click();", element); // Click
   js.executeScript("window.scrollTo(0, 1000);"); // Scroll
   ```

89. **Types of waits in Selenium**
   - Implicit: `driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));`
   - Explicit: `WebDriverWait` for specific conditions.
   - Fluent: Custom polling with `FluentWait`.

90. **WebDriver verification methods**
   - `isDisplayed()`, `isEnabled()`, `isSelected()`, TestNG assertions.

91. **What are all the Selenium interfaces?**
   - `WebDriver`, `WebElement`, `TakesScreenshot`, `JavascriptExecutor`, `Alert`.

92. **What are components of Selenium**
   - Selenium WebDriver, Selenium Grid, Selenium IDE.

93. **What are package you will import for screenshot**
   ```java
   import org.apache.commons.io.FileUtils;
   import org.openqa.selenium.OutputType;
   import org.openqa.selenium.TakesScreenshot;
   ```

94. **What are the different frameworks in Selenium Java**
   - Data-Driven, Keyword-Driven, Hybrid, POM, BDD (Cucumber).

95. **What are the major changes from version to version**
   - Selenium 4 vs 3:
     - W3C-compliant WebDriver.
     - Relative locators.
     - Enhanced Selenium Grid.
     - Chrome DevTools integration.

96. **What are the OOPs concepts in Java and what are the areas it is implemented in Selenium**
   - **Concepts**: Encapsulation (POM classes), Inheritance (base classes), Polymorphism (WebDriver methods), Abstraction (interfaces).
   - **Areas**: POM, framework design, test utilities.

97. **What are the types to navigate**
   - `navigate().to(url)`, `navigate().back()`, `navigate().forward()`, `navigate().refresh()`.

98. **What is action class and syntax?**
   - Same as Q29.

99. **What is Java AWT**
   - Abstract Window Toolkit for GUI programming, used in `Robot` class for keyboard/mouse events.

100. **What is no such driver exception**
    - `NoSuchDriverException`: Thrown when WebDriver binary (e.g., chromedriver) is missing or incompatible.

101. **What is overloading methods in Selenium?**
    - Methods like `findElement()` can be overloaded for different locators:
      ```java
      driver.findElement(By.id("id"));
      driver.findElement(By.xpath("//input"));
      ```

102. **What is Selenium WebDriver**
    - API for browser automation, supporting multiple browsers via drivers (e.g., ChromeDriver).

103. **What is Selenium?**
    - Open-source tool for automating web browsers, includes WebDriver, Grid, and IDE.

104. **What is the alternative for "click" in Selenium?**
    - JavaScript click:
      ```java
      ((JavascriptExecutor) driver).executeScript("arguments[0].click();", element);
      ```

105. **What is the common locator using in your project**
    - ID, CSS Selector, relative XPath for reliability.

106. **What is the difference between Selenium 3 and Selenium 4**
    - See Q95.

107. **What is the direct method for file uploading in Selenium**
   ```java
   driver.findElement(By.xpath("//input[@type='file']")).sendKeys("C:/file.txt");
   ```

108. **What is the use JavascriptExecutor**
    - Executes JavaScript for actions Selenium can’t handle (e.g., scrolling, clicking disabled elements).

109. **What to do if two objects have same XPath?**
    - Use indexing or unique attributes:
      ```java
      (//input[@type='text'])[2]
      //input[@type='text' and @name='unique']
      ```

110. **When chromedriver updates frequently & throws IO exception, how you handle it?**
    - Use WebDriverManager:
      ```java
      WebDriverManager.chromedriver().setup();
      ```

111. **Which is the fastest locator**
    - ID, followed by CSS Selector.

112. **While taking common XPath if any changes in DOM structure in future, how will you handle**
    - Use dynamic XPath with `contains()`, `starts-with()`, or axes to make it robust.

113. **Why and when we go for XPath**
    - **Why**: Flexible, supports complex traversals.
    - **When**: No unique ID/Name, dynamic elements, or specific DOM navigation.

114. **Why we go for Selenium**
    - Open-source, supports multiple browsers, integrates with frameworks (TestNG, Cucumber), and automates web testing.

115. **Window handle and window handles**
    - `getWindowHandle()`: Returns current window ID (`String`).
    - `getWindowHandles()`: Returns all window IDs (`Set<String>`).

116. **What does the FileUtils.copyFile denote?**
   ```java
   FileUtils.copyFile(src, new File("destination.png")); // Copies screenshot file
   ```

117. **Write a program of Dynamic table Selenium program code**
   ```java
   List<WebElement> rows = driver.findElements(By.xpath("//table//tr"));
   for (WebElement row : rows) {
       List<WebElement> cells = row.findElements(By.tagName("td"));
       for (WebElement cell : cells) {
           System.out.print(cell.getText() + "\t");
       }
       System.out.println();
   }
   ```

118. **Write a program of Window Handling and finally switch to parent window**
   ```java
   String parent = driver.getWindowHandle();
   driver.findElement(By.linkText("Open Window")).click();
   for (String window : driver.getWindowHandles()) {
       if (!window.equals(parent)) {
           driver.switchTo().window(window);
           driver.findElement(By.id("element")).click();
           driver.close();
       }
   }
   driver.switchTo().window(parent);
   ```

119. **Write an XPath for search content in Google**
   ```java
   //input[@name='q']
   ```

120. **Write an XPath of the dynamic locator**
   ```java
   //div[contains(@id, 'dynamic')]
   ```

121. **Write the dependency which you add in pom.xml for Selenium in notepad**
   ```xml
   <dependency>
       <groupId>org.seleniumhq.selenium</groupId>
       <artifactId>selenium-java</artifactId>
       <version>4.XX.0</version>
   </dependency>
   ```

122. **Write XPath for "I am feeling lucky" available in Google home page**
   ```java
   //input[@value="I'm Feeling Lucky"]
   ```

123. **XPath finding in Google home**
   ```java
   //input[@name='q']
   ```

124. **XPath is changing dynamically, how will you handle this scenario?**
   - Use dynamic XPath with `contains()`, `starts-with()`, or axes:
     ```java
     //button[contains(text(), 'Submit')]
     ```

125. **XPath syntax**
   - Absolute: `/html/body/div/input`
   - Relative: `//input[@id='username']`

126. **XPath uses**
   - Locate elements, traverse DOM, handle dynamic elements.

127. **Link: selectorshub.com/xpath-practice-page/ "Scenario Based Question: If you are copying from correct username and password from notepad and paste it into login fields and click submit it alerts 'username or password is wrong' why?"**
   - **Reasons**:
     - Hidden characters (e.g., spaces, newlines) copied from notepad.
     - Case sensitivity mismatch.
     - Incorrect field locators.
     - CAPTCHA or session issues.
   - **Solution**:
     ```java
     String username = "user".trim();
     String password = "pass".trim();
     driver.findElement(By.id("username")).sendKeys(username);
     driver.findElement(By.id("password")).sendKeys(password);
     driver.findElement(By.id("submit")).click();
     ```

128. **Selenium - Given this url "https://www.geico.com/" and asked to pass numbers to zipcode textbox and click go and validate the result "thanks message"**
   ```java
   driver.get("https://www.geico.com");
   driver.findElement(By.id("zip")).sendKeys("12345");
   driver.findElement(By.xpath("//button[contains(text(), 'Go')]")).click();
   WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
   String message = wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//div[contains(text(), 'Thanks')]"))).getText();
   Assert.assertTrue(message.contains("Thanks"));
   ```

129. **WebDriver driver = new ChromeDriver() Vs ChromeDriver driver = new ChromeDriver()**
   - `WebDriver`: Interface, promotes polymorphism, supports multiple browsers.
   - `ChromeDriver`: Concrete class, specific to Chrome, less flexible.

130. **Why we use 3.141.59 version in Selenium**
   - Likely a typo; it refers to Selenium 3.141.59 (last stable Selenium 3 release). Used for compatibility with older systems before upgrading to Selenium 4.
