Glassbreakers Spec Guide
=======

Fundamentals 
--------
###*At Glassbreakers we value a well tested app and take pride in our test suite.*###

####Coverage####
Glassbreakers is a fast moving company that pushes code up frequently as we build out new features. The development team relies on a well tested app to have the confidence to push code to production without causing errors and/or negatively affecting a user's experience.

####Balance####
Although it would be nice to have every edge case covered in our specs, our test suite should be there as a tool to help us write code productively and confidently and should not inhibit us from getting features out.

  When deciding what to cover in your specs consider the following:
  - Black box, test what goes in and assert what comes out. Don't test private methods.
  - Is there a 90% probability the edge case will happen?
  - What type of impact will it have on the user?
    - A nice to have, but not a necessity (low risk, cover most common cases)
    - A slight inconvenience (cover majority of cases)
    - A critical feature to proceed (cover as many as feasible)
    - A security risk (cover all cases)

####Consistentancy####
A consistent test suite is an outcome of a developer team that cares about the product, is able to work together and practices good communication.
Bonuses of a consistent testsuite
- Being able to easily navigate a codebase
- Allows a developer to focus on the logic and not worry about about how to setup the tests
- No need to spend time explaining why we did what we did.

####Best Judgement####
These are best practices for going forward. If you are writing new specs or updating an existing spec, do your best to follow these guides unless you have good reason not to. We don't want to break what is already working so use your best judgement on what to update and what to leave alone for now. We as a team own this spec guide, so if you see something that is more than a one time exception, please open a pull request with the needed changes.  

High Level Spec Setup
--------
###*These conventions are best practices across all spec types*###

####Outside in Testing####
  * pattern for writing tests
  * start writing test at highest level
  * assert on behavior that user can see
  
####Naming####
Print out all the specs titles to include with your code review, all names should be readable and clear of what the code covers and what edge cases are being tested.

Remove duplicate phrasing and weak words like 'should'. 

it "should add the item to the list" do - BAD
it "add the item to the list" do - GOOD
  
####File Stucture####
  - It is nice to have files under 100 lines but the file size is lowest on priority over code being well tested
  - All specs go in their spec type folders
   * ie. Model, Feature ect.
  - Feel free to make nested sub-folders for features.
   * Ie. Subfolder for pending users, active users, deactivated users

####Hound Issues####
We are more flexible with hound issues on Spec tests with the highest priority being clear of what is being tested. If a spec name is longer than 80 characters, see if there is a quick way to shorten the name without losing the meaning, if not don't let this hold you back and move on.  

Spec Specifications 
--------
###*We now know what to test, where to test it, and why it's important but let's dig into the details and see how!*###

####Arrange-Act-Assert pattern####
  * Arrange: setup all nessary preconditions and inputs
  * Act: on the object or method being tested
  * Assert: the expected results have occurred.

####Describes, Features and Contexts... on my!####
The purpose of “describe” is to wrap a set of tests against one functionality while “context” is to wrap a set of tests against one functionality under the same state. 
 * Use Describe for things (top level and methods).
 * Use context for states.
 * User Features for tope level of Feature tests


####before, let, instance variables####
 - Avoid instance variables, http://stackoverflow.com/questions/5359558/when-to-use-rspec-let/5359979#5359979 
 - before eagerly runs code before each test.
 - let lazily runs code when it is first used which is better then automatically running unneeded logic.  
 - Make sure the lets aren't to distant so it is still readable easy to track what is going on.

####Factory Girl and Fixtures####
At any time possible, use Factory girl and not fixtures. Factory Girl is reliable and requires little maintance. With Factory Girl, a model updates with your development as you merge in updates to your models.

A case for fixtures would be if you need to pass in a CSV file, or create an object to pass in that is not a Model.  
http://www.hiringthing.com/2012/08/17/rails-testing-factory-girl.html#sthash.h7s1sDju.dpuf
https://github.com/thoughtbot/factory_girl

####JavaScript####
By default Capybara emulates a headless browser which is fast but does not run JavaScript. If you need to enable JavaScript in a test, include in `js:  true`. 
-'js: true' can be added to a describe block, context or single spec
- It does take more time to run a JS enabled spec, only apply it to the smallest block possible. 
- Ex: `it "tests using javascript" js:  true, do`
http://tutorials.jumpstartlab.com/topics/capybara/capybara_with_selenium_and_webkit.html

####Shared examples####
https://www.relishapp.com/rspec/rspec-core/docs/example-groups/shared-examples
http://testdrivenwebsites.com/2011/08/17/different-ways-of-code-reuse-in-rspec/
http://modocache.io/shared-examples-in-rspec
  
Shared examples allow us to execute the same group of expectations against several classes. Only use if there are more then 2 casese that could call them. If all of the calls to the shared example are in one file, place the shared example up top of that file, otherwise place in spec/support/examples. 
  
####Helper Methods####
http://testdrivenwebsites.com/2011/08/17/different-ways-of-code-reuse-in-rspec/

The primary use of helper methods is to hide implementation details by grouping several low-level statements into more meaningful, higher level abstractions (with clear, expressive names). If you have an urge to write a comment explaing what you are doing, this would be a great case to make a helper method with a clear name. 

If the helper method is only being accessed in one file, place at the bottom of the file. If they are shared, place in the appropriate spec type subfolder under support. Ex: spec/support/features/feature_name_helpers.rb



Feature Specs
---------
Head with Feature `"User sends a message" do`

High level tests that walk through entire application. Think of it as an actual user testing out the web interface. Only focus on what the user will see, do not test any logic in the feature tests that should be behind the scenes to the user. 

Controller Specs
----------
Most controller specs should be hit through the feature specs. Use your best judgement if additional controller specs are needed.  

Model/Unit/Services
-----
Head with `Describe "User" do`
Great talk by Sandi Metz: https://www.youtube.com/watch?v=URSWYvyc42M
Narrow the focus down until the entire universe is a single object, that object is all the unit test knows about
-every cell works correctly
-thorough, stable, fast, and few
-test the interface, not the implementation (allows for refactoring)
- Do not test private methods, they do not allow for refactoring. If you have a complicated private algorithm, this may require testing to get it running but should be segregated and noted that they can be deleted with refactoring. 
