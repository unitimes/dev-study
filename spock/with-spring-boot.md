#Gradle setting
##`build.gradle`
```groovy
apply plugin 'groovy'
dependencies {
	testCompile "org.codehaus.groovy:groovy-all:2.1.7"
	testCompile "org.spockframework:spock-core:0.7-groovy-2.0"
}
```
#Using spring beans
```groovy
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.boot.test.SpringApplicationContextLoader
import org.springframework.test.context.ContextConfiguration
import spock.lang.Specification

@ContextConfiguration(loader = SpringApplicationContextLoader, classes = Application)
class SearchMaemulServiceTest extends Specification {
    @Autowired
    SearchMaemulService searchMaemulService;

    def "Feature method description"() {
		// feature method code
    }
}
```