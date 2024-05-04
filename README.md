Graphs Questions:

package pages;

import org.framework.BaseSetup;
import org.openqa.selenium.*;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;

import java.time.Duration;

public class Graphs extends BaseSetup {

    WebDriver driver;

    By LinearGraphpointA = By.xpath("//*[local-name()='svg']//*[name()='g' and @class='Zelcpf']//*[name()='rect']");
    By PRICE = By.xpath("//div[@class='hSGhwc']/p[@jsname='BYCTfd']");
    By DATE = By.xpath("//div[@class='hSGhwc']/p[@jsname='LlMULe']");


    @BeforeTest
    void setUp() {
        this.driver = initSetup();
    }


    private  WebElement retriveFreshWebElement(By loc) {

        WebElement ele = null;
        int attempts = 0;
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(120));
        while (attempts < 3) {
            try {
                ele = wait.until(ExpectedConditions.elementToBeClickable(loc));
                break;
            } catch (StaleElementReferenceException e) {
                attempts++;
            }
        }
      return ele;
    }

    @Test
    void graph() throws InterruptedException {


        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        WebElement rectElement = wait.until(ExpectedConditions.visibilityOfElementLocated(LinearGraphpointA));
        double rectWidth = Double.parseDouble(rectElement.getAttribute("width"));
        double rectHeight = Double.parseDouble(rectElement.getAttribute("height"));
        System.out.println(rectWidth+" "+rectHeight);
        double topLeftY = (rectHeight)/2 -rectHeight;
        double topLeftX = (rectWidth)/2 - rectWidth;
        System.out.println(topLeftX+"  "+topLeftY);
        Actions actions = new Actions(driver);
        for(int i=0;i<=596;i++){
            double translateX = topLeftX+i;
            actions.moveToElement(retriveFreshWebElement(LinearGraphpointA), (int) translateX, (int)topLeftY).perform();
            wait.until(ExpectedConditions.visibilityOfElementLocated(PRICE));
            String price =   driver.findElement(PRICE).getText().trim();
            String dateTime = driver.findElement(DATE).getText().trim();
            System.out.println(dateTime+"  "+price);
            driver.findElement(PRICE).getRect();
        }

    }

}
