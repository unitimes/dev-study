# User guide
## About Velocity
#### What Velocity is
**Velocity is a Java-based template engine.**
#### What for
web pages, SQL, PostScript etc.
## Using jar
### Maven
#### for Velocity engine
```xml
<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-core</artifactId>
  <version>x.x.x</version>
</dependency>
```
#### Connecting Velocity log to Commons Logging
Choose one of the below
```xml
<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-commons-logging</artifactId>
  <version>x.x.x</version>
</dependency>

<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-slf4j</artifactId>
  <version>x.x.x</version>
</dependency>

<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-log4j</artifactId>
  <version>x.x.x</version>
</dependency>

<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-servlet</artifactId>
  <version>x.x.x</version>
</dependency>
```
## VTL(Velocity Template Language)
#### VTL statement
Directive start with `#`
```
#set()
```
#### Reference
Start with `$`
> #### String value and Quotes
> Single quotes(`''`) ensure the value will be as is
> Double quotes(`""`) allow to use velocity references and directives
> - `"Hello $name"`
> #### `#[[don't parse me!]]#` syntax
> Unparse the block

## References
### **Three types**
#### Variables
```$foo```
$ + VTL Identifier    
VTL Identifier must start with an alphabetic character    
#### Properties
```
$customer.Address
```
Returning the value of of the key 'Address' on the hashtable 'cutomer' or using method(```$customer.getAddress()```)
#### Methods
```
$purchase.getTotal()
$page.setTitle( "My Home Page" )
$person.setAttributes( ["Strange", "Weird", "Excited"] )
```
### Formal Reference Notation
```
${foo}

$foo
```
Both above are same
```
${vice}manicac

$vicemanicac
```
Both are different
### Quiet Reference Notation
```
$!foo
```
When the reference is undefined, a blank value is displayed
## Directives
Using formal reference notation is possible
#### `#set`
Setting the value of a reference    

When the RHS is a property or method and evaluates null, it will not be assigned to the LHS. Depending the configuration, this mechanism can not remove an existing reference.
