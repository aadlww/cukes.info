# Step Definitions

Every Gherkin [step](steps) must have a matching Step Definition in order to pass.
Step Definitions are written in one of Cucumber's supported programming languages,
and although each programming language is different, a Step Definition always consists
of two parts --- a pattern (regular expression) and a body of code.

The best way to explain this is with an example. Consider the following Gherkin step:

```gherkin_en
Given I have 5 cukes in my belly
```

A matching step definition would look like the following:

<TABS>
#### Ruby
```ruby
Given /^I have (\d+) cukes in my belly$/ do |n|
  puts n
end
```

#### Java
```java
@Given("^I have (\\d+) cukes in my belly$")
public void cukes_in_my_belly(int n) {
    System.out.println(n);
}
```

#### JavaScript
```javascript
Given(/^I have (\d+) cukes in my belly$/, function (n) {
    console.log(n);
}
```

#### Groovy
```java
Given(~'^I have (\\d+) cukes in my belly$') { int n ->
    println(n)
}
```

#### Clojure
```javascript
(Given #"^I have (\d+) cukes in my belly$" [n]
  (println n))
```

#### Python
```python
@Given('I have (\d+) cukes in my belly')
def something_in_the_belly(self, n):
  print(n)
```

#### C++
```cpp
GIVEN("^I have (\\d+) in my belly$") {
    REGEX_PARAM(int, n);
    USING_CONTEXT(MyCtx, context);
    cout << n;
}
```

#### CSHARP
```csharp
[Then(@"I have (\d+) cukes in my belly")]
public void cukes_in_my_belly(int n)
{
    Console.WriteLine(n);
}
```

#### Lua
```javascript
Given("I have (%d+) cukes in my belly", function (n)
  print(n)
end)
```

#### Scala
```scala
Given("""^I have (\d+) cukes in my belly$""") { (n:Int) =>
  println(n)
}
```

</TABS>

When Cucumber executes a Gherkin step it will find a step definition with a matching pattern and execute the associated body.
If the pattern has capture groups, those will be passed to the body as parameters.

The step definitions above will print `5` for the step `Given I have 5 cukes in my belly`.

Cucumber will instantiate a new object for each [scenario](scenarios), so any variables you assign in a step will stick around
to the following steps in the same scenario, but they will be gone when a new scenario starts.