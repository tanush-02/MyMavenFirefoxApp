# 🚀 Maven Selenium Project – Firefox Browser Automation

## 🎯 Objective

This project demonstrates how to build a **Maven-based Selenium automation project** using the Firefox browser. The automation performs:

- Login to SauceDemo website
- Search and add a product to cart on Automation Exercise
- Login to Practice Test Automation website
- Multi-tab browser automation using Selenium WebDriver

---

## 🌐 Websites Used

- SauceDemo: https://www.saucedemo.com/
- Automation Exercise: https://automationexercise.com/products
- Practice Test Automation: https://practicetestautomation.com/practice-test-login/

---

## 🛠️ Step 1: Create Maven Project

```bash
mvn archetype:generate \
-DgroupId=com.example \
-DartifactId=MyMavenFirefoxApp \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DinteractiveMode=false
```

---

## 📁 Project Structure

```bash
MyMavenFirefoxApp/
│── src/
│   ├── main/java/com/example/App.java
│   └── test/java/com/example/AppTest.java
│── target/
│── pom.xml
```

---

## 🌐 Step 2: Install Firefox

Check version:

```bash
firefox --version
```

If not installed:

```bash
sudo apt install firefox
```

---

## 🧰 Step 3: Install GeckoDriver

Download:

```bash
wget https://github.com/mozilla/geckodriver/releases/latest/download/geckodriver-v0.34.0-linux64.tar.gz
```

Extract:

```bash
tar -xvzf geckodriver-v0.34.0-linux64.tar.gz
```

Move to system path:

```bash
sudo mv geckodriver /usr/local/bin/
sudo chmod +x /usr/local/bin/geckodriver
```

Verify:

```bash
geckodriver --version
```

---

## 📄 Step 4: Configure pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
         http://maven.apache.org/maven-v4_0_0.xsd">

<modelVersion>4.0.0</modelVersion>

<groupId>com.example</groupId>
<artifactId>MyMavenFirefoxApp</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>jar</packaging>

<name>MyMavenFirefoxApp</name>

<dependencies>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.29.0</version>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <source>11</source>
                <target>11</target>
            </configuration>
        </plugin>

        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>3.1.0</version>
            <configuration>
                <mainClass>com.example.App</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>

</project>
```

---

## 🔍 Step 5: Inspect Web Elements

Use browser inspect tool to find element IDs.

### SauceDemo
| Field | ID |
|------|----|
| Username | user-name |
| Password | password |
| Login Button | login-button |

### Automation Exercise
| Field | ID |
|------|----|
| Search Box | search_product |
| Search Button | submit_search |
| Product Button | data-product-id |

### Practice Test Automation
| Field | ID |
|------|----|
| Username | username |
| Password | password |
| Login Button | submit |

---

## 💻 Step 6: App.java

```java
package com.example;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.WindowType;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class App {

    public static void main(String[] args) throws InterruptedException {

        FirefoxOptions options = new FirefoxOptions();
        WebDriver driver = new FirefoxDriver(options);

        driver.manage().window().setSize(new org.openqa.selenium.Dimension(1920,1080));

        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(15));
        Actions actions = new Actions(driver);

        // SauceDemo Login
        driver.get("https://www.saucedemo.com/");
        driver.findElement(By.id("user-name")).sendKeys("standard_user");
        driver.findElement(By.id("password")).sendKeys("secret_sauce");
        driver.findElement(By.id("login-button")).click();
        System.out.println("SauceDemo login successful");

        // New Tab - Automation Exercise
        driver.switchTo().newWindow(WindowType.TAB);
        driver.get("https://automationexercise.com/products");

        driver.findElement(By.id("search_product")).sendKeys("Men Tshirt");
        driver.findElement(By.id("submit_search")).click();

        WebElement product = wait.until(
                ExpectedConditions.visibilityOfElementLocated(
                        By.cssSelector("a[data-product-id='2']")
                )
        );

        ((org.openqa.selenium.JavascriptExecutor) driver)
                .executeScript("arguments[0].scrollIntoView({block:'center'});", product);

        ((org.openqa.selenium.JavascriptExecutor) driver)
                .executeScript("arguments[0].click();", product);

        WebElement viewCart = wait.until(
                ExpectedConditions.elementToBeClickable(
                        By.cssSelector("#cartModal a[href='/view_cart']")
                )
        );

        ((org.openqa.selenium.JavascriptExecutor) driver)
                .executeScript("arguments[0].click();", viewCart);

        System.out.println("Product added to cart");

        // New Tab - Practice Test Automation
        driver.switchTo().newWindow(WindowType.TAB);
        driver.get("https://practicetestautomation.com/practice-test-login/");

        Thread.sleep(2000);

        driver.findElement(By.id("username")).sendKeys("student");
        driver.findElement(By.id("password")).sendKeys("Password123");
        driver.findElement(By.id("submit")).click();

        System.out.println("Practice Test login successful");

        Thread.sleep(5000);
        driver.quit();
    }
}
```

---

## ⚙️ Step 7: Build Project

```bash
mvn clean compile
```

---

## ▶️ Step 8: Run Application

```bash
mvn exec:java
```

---

## ✅ Output

- SauceDemo login successful
- Product added to cart
- Practice Test login successful

---

## 📌 Key Concepts Covered

- Maven project setup
- Selenium WebDriver with Firefox
- GeckoDriver configuration
- Multi-tab automation
- Explicit waits (WebDriverWait)
- DOM element handling
- JavaScript execution in Selenium

---

## 📚 References

- https://maven.apache.org/
- https://www.selenium.dev/
- https://github.com/mozilla/geckodriver

---

## 👨‍💻 Author

Dept. of CSE, BIT  
Academic Year: 2025-26
