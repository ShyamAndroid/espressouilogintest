# espressouilogintest
EspressoUiTesting
One of the problems with manual testing is that it can be time-consuming and tedious to perform. For example, to test a login screen (manually) in an Android app, you will have to do the following:

Launch the app. 
Navigate to the login screen. 
Confirm if the usernameEditText and passwordEditText are visible. 
Type the username and password into their respective fields. 
Confirm if the login button is also visible, and then click on that login button.
Check if the correct views are displayed when that login was successful or was a failure. 
Instead of spending all this time manually testing our app, it would be better to spend more time writing code that makes our app stand out from the rest! And, even though manual testing is tedious and quite slow, it is still error-prone and you might miss some corner cases. 

Some of the advantages of automated testing include the following:   

Automated tests execute exactly the same test cases every time they are executed. 
Developers can quickly spot a problem quickly before it is sent to the QA team. 
It can save a lot of time, unlike doing manual testing. By saving time, software engineers and the QA team can instead spend more time on challenging and rewarding tasks. 
Higher test coverage is achieved, which leads to a better quality application. 

Prerequisites
To be able to follow this tutorial, you'll need:

a basic understanding of core Android APIs and Java
Android Studio 3.1.3 or higher

Set Up Espresso and AndroidJUnitRunner
After creating a new project, make sure to add the following dependencies from the Android Testing Support Library in your build.gradle (although Android Studio has already included them for us). In this tutorial, we are using the latest Espresso library version 3.0.2 (as of this writing). 


android {
    //...
      defaultConfig {
        //...
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    //...
}
 
dependencies {
    //...
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test:rules:1.0.2'
 
}

The following types of annotations can be applied to the methods used inside the test class.

@BeforeClass: this indicates that the static method this annotation is applied to must be executed once and before all tests in the class. This could be used, for example, to set up a connection to a database. 
@Before: indicates that the method this annotation is attached to must be executed before each test method in the class.
@Test: indicates that the method this annotation is attached to should run as a test case.
@After: indicates that the method this annotation is attached to should run after each test method. 
@AfterClass: indicates that the method this annotation is attached to should run after all the test methods in the class have been run. Here, we typically close out resources that were opened in @BeforeClass. 

Find a View Using onView()
In our MainActivity layout file, we just have one widget—the Login button. Let's test a scenario where a user will find that button and click on it.

To find widgets in Espresso, we make use of the onView() static method (instead of findViewById()). The parameter type we supply to onView() is a Matcher. Note that the Matcher API doesn't come from the Android SDK but instead from the Hamcrest Project. Hamcrest's matcher library is inside the Espresso library we pulled via Gradle. 

The onView(withId(R.id.btn_login)) will return a ViewInteraction that is for a View whose ID is R.id.btn_login. In the example above, we used withId() to look for a widget with a given id. Other view matchers we can use are: 

withText(): returns a matcher that matches TextView based on its text property value.
withHint(): returns a matcher that matches TextView based on its hint property value.
withTagKey(): returns a matcher that matches View based on tag keys.
withTagValue(): returns a matcher that matches Views based on tag property values.
First, let's test to see if the button is actually displayed on the screen. 

onView(withId(R.id.btn_login)).check(matches(isDisplayed()))
Here, we are just confirming if the button with the given id (R.id.btn_login) is visible to the user, so we use the check() method to confirm if the underlying View has a certain state—in our case, if it is visible.

The matches() static method returns a generic ViewAssertion that asserts that a view exists in the view hierarchy and is matched by the given view matcher. That given view matcher is returned by calling isDisplayed(). As suggested by the method name, isDisplayed() is a matcher that matches Views that are currently displayed on the screen to the user. For example, if we want to check if a button is enabled, we simply pass isEnabled() to matches(). 

Other popular view matchers we can pass into the matches() method are:

hasFocus(): returns a matcher that matches Views that currently have focus.
isChecked(): returns a matcher that accepts if and only if the view is a CompoundButton (or subtype of) and is in checked state. The opposite of this method is isNotChecked(). 
isSelected(): returns a matcher that matches Views that are selected.

Perform Actions on a View
On a ViewInteraction object which is returned by calling onView(), we can simulate actions a user can perform on a widget. For example, we can simulate a click action by simply calling the click() static method inside the ViewActions class. This will return a ViewAction object for us. 

The documentation says that ViewAction is:

Responsible for performing an interaction on the given View element.

@Test
public void clickLoginButton_opensLoginUi() {
    // ...
 
    onView(withId(R.id.btn_login)).perform(click())
}
We perform a click event by first calling perform(). This method performs the given action(s) on the view selected by the current view matcher. Note that we can pass it a single action or a list of actions (executed in order). Here, we gave it click(). Other possible actions are:

typeText() to imitate typing text into an EditText.
clearText() to simulate clearing text in an EditText.
doubleClick() to simulate double-clicking a View.
longClick() to imitate long-clicking a View.
scrollTo() to simulate scrolling a ScrollView to a particular View that is visible. 
swipeLeft() to simulate swiping right to left across the vertical center of a View.
Many more simulations can be found inside the ViewActions class. 

Validate With View Assertions
Let's complete our test, to validate that the LoginActivity screen is shown whenever the Login button is clicked. Though we have already seen how to use check() on a ViewInteraction, let's use it again, passing it another ViewAssertion. 
@Test
public void clickLoginButton_opensLoginUi() {
    // ... 
     
    onView(withId(R.id.tv_login)).check(matches(isDisplayed()))
}
Inside the LoginActivity layout file, apart from EditTexts and a Button, we also have a TextView with ID R.id.tv_login. So we simply do a check to confirm that the TextView is visible to the user. 

