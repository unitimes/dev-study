#Template
```groovy
import spock.lang.*

class SpecificationName extends Specification {
	// fields
	// fixture methods
	// feature methods
	// helper methods
}
```
##Fields
```
def obj = new ClassUnderSpecification()
def coll = new Collaborator()
@Shared res = new VeryExpensiveResource()
static final PI = 3.14
```
- Instance fields are not shared between feature methods.
- If you want to share some instance fields, declare field with ```@Shared```.
- You can also use `static`, but think about meaning whether constant or share.

##Fixture Methods
```groovy
def setup() {}          // run before every feature method
def cleanup() {}        // run after every feature method
def setupSpec() {}     // run before the first feature method
def cleanupSpec() {}   // run after the last feature method
```
- These are for setting up and cleaning up the environment
- Fixture methods are optional.

##Feature Methods
```groovy
def "feature description"() {
	// blocks
}
```
- Features methods' name are String literals.

###Blocks
- There are six kinds of blocks
	- setup, when, then, expect, cleandup, where

####Setup Blocks
```groovy
setup:
def stack = new Stack()
def elem = "push me"
```
- Any statements between the beginning of the method and the first explicit block belong to an implicit `setup` block.
- `given` block is an alias for `setup` block.

####When and Then Blocks
```groovy
when:
stack.push(elem)

then:
!stack.empty
stack.size() == 1
stack.peek() == elem

when:
stack.pop()

then:
thrown(EmptyStackException)
stack.empty
def e = thrown(EmptyStackException)
// or EmptyStackException e = thrown()
e.cause == null
notThrown(NullPointerException)
```
- Both always occur together.
- A feature method can contain multiple pairs of `when-then` blocks.

####Expect Blocks
```groovy
when:
def x = Math.max(1, 2)

then:
x == 2
```
```groovy
expect:
Math.max(1, 2) == 2
```
- Both are semantically same.

####Cleanup Blocks
```groovy
setup:
def file = new File("/some/path")
file.createNewFile()

cleanup:
file.delete()
```
- Cleanup blocks are run even if the feature method has produced an exception.
- It may be used to free resources.

####Where Blocks
```groovy
def "computing the maximum of two numbers"() {
  expect:
  Math.max(a, b) == c

  where:
  a << [5, 3]
  b << [1, 9]
  c << [5, 9]
}
```
- `where` block always comes last in a method.
- Above block creates two "versions' of the feature method: One where a is 5, b is 1, and c is 5, and another one where a is 3, b is 9, and c is 9.

##Helper Methods
```groovy
void matchesPreferredConfiguration(pc) {
  assert pc.vendor == "Sunny"
  assert pc.clockRate >= 2333
  assert pc.ram >= 4096
  assert pc.os == "Linux"
}
```
- Helper methods can be used for any purpose.
- If you use for condition check, `return void` and using `assert` way is preferable to `return boolean` way.


#Something good to know
- Spock uses JUnit runner, so Spock specifications can be run by most modern Java IDEs.

#Wanna be an advanced Spocker?
##[Data table](http://spockframework.github.io/spock/docs/1.0/data_driven_testing.html#data-tables)
##[Interaction Based Testing](http://spockframework.github.io/spock/docs/1.0/interaction_based_testing.html)
##[Refer Docs](http://spockframework.github.io/spock/docs/1.0/index.html)