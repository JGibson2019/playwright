---
id: accessibility-testing
title: "Accessibility testing"
---

Playwright can be used to test your application for many types of accessibility issues.

A few examples of problems this can catch include:
- Text that would be hard to read for users with vision impairments due to poor color contrast with the background behind it
- UI controls and form elements without labels that a screen reader could identify
- Interactive elements with duplicate IDs which can confuse assistive technologies

The following examples rely on the [`@axe-core/playwright`](https://npmjs.org/@axe-core/playwright) package which adds support for running the [axe accessibility testing engine](https://www.deque.com/axe/) as part of your Playwright tests.

<!-- TOC -->

## Disclaimer

Automated accessibility tests can detect some common accessibility problems such as missing or invalid properties. But many accessibility problems can only be discovered through manual testing. We recommend using a combination of automated testing, manual accessibility assessments, and inclusive user testing.

For manual assessments, we recommend [Accessibility Insights for Web](https://accessibilityinsights.io/docs/web/overview/?referrer=playwright-accessibility-testing-js), a free and open source dev tool that walks you through assessing a website for [WCAG 2.1 AA](https://www.w3.org/WAI/WCAG21/quickref/?currentsidebar=%23col_customize&levels=aa) coverage.

### Example 1: Scanning an entire page

This example demonstrates how to test an entire page for automatically detectable accessibility violations. The test:
1. Imports the `axecore.playwright` package
1. Fail the test if there are any failures

```java
import com.deque.html.axecore.playwright.*;
import com.deque.html.axecore.utilities.axeresults.AxeResults;
import com.deque.html.axecore.utilities.axeresults.Rule;

public class ExampleTest {
    @Test
    void accessibilityOfH1Element() throws Exception {
        AxeBuilder axeBuilder = new AxeBuilder(page);
        AxeResults axeResults = axeBuilder
            .include(Collections.singletonList("h1"))
            .withTags(Arrays.asList("wcag2a", "wcag2aa", "wcag21a", "wcag21aa"))
            .analyze();
        new Reporter().JSONStringify(axeResults, "index-h1.json");
        assertTrue(axeResults.violationFree());
    }
}
 
````