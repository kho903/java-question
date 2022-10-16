### [Regular expressions - bussiness applications](https://howto.webcon.com/regular-expressions-business-application/)
- Bank account number
- E-mail address
- TAX ID 
- Personal ID number(pesel)


### [Using Regular Expressions](https://docs.appdynamics.com/appd/21.x/21.2/en/application-monitoring/configure-instrumentation/configure-instrumentation-overview/using-regular-expressions)
- Matching Non-Adjacent URL Segments
- Matching Any digit
- Requiring a Digit
- Handling Letter a Casing
- Backend Discovery Rules


### [Regular Expressions (Regex) ?](https://www.merkle.com/in/blog/regex-guide-seo-regular-expression-fundamentals-and-use-cases)
> Regex is used to find text matching a pre-defined pattern.


### [Regex For SEO : How to use Regular Expressions (with Examples)](https://www.jcchouinard.com/regex-for-seo/)

### [Regex Guide for SEO - Regular Expression Fundmentals and Use cases](https://www.merkle.com/in/blog/regex-guide-seo-regular-expression-fundamentals-and-use-cases)


### [Java Regular Expressions - w3shcools](https://www.w3schools.com/java/java_regex.asp)

- Pattern class - Defines a pattern (to be a used in a search)
- Matcher class - Used to search for the pattern
- PatternSyntaxException class - Indicates syntax error in a regular expression pattern

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
  public static void main(String[] args) {
    Pattern pattern = Pattern.compile("w3schools", Pattern.CASE_INSENSITIVE);
    Matcher matcher = pattern.matcher("Visit W3Schools!");
    boolean matchFound = matcher.find();
    if(matchFound) {
      System.out.println("Match found");
    } else {
      System.out.println("Match not found");
    }
  }
}
```

### [Regex exam](https://codechacha.com/ko/java-regex/)

### [AppDynamics - cisco](https://docs.appdynamics.com/appd/22.x/22.2/en/application-monitoring/configure-instrumentation/backend-detection-rules/java-backend-detection)
