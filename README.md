# Spring Boot ModelMapper Starter
> A Spring Boot starter that let you use [ModelMapper](http://modelmapper.org) within your Spring Boot application.

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/6c1928b5776c44ec9af0b044f1a68e92)](https://www.codacy.com/app/rozidan/modelmapper-spring-boot-starter?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=rozidan/modelmapper-spring-boot-starter&amp;utm_campaign=Badge_Grade)

## Features
Register the ModelMapper to your Spring Boot application and allows to configure it and register object mappings.

## Setup
In order to add ModelMpaeer to your project simply add this dependency to your classpath:
```xml
<dependency>
    <groupId>com.github.rozidan</groupId>
    <artifactId>modelmapper-spring-boot-starter</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

```groovy
compile 'com.github.rozidan:modelmapper-spring-boot-starter:1.0.0-SNAPSHOT'
```

### Change ModelMapper global configuration
In order to set change ModelMapper global configuration, simply register within Spring application context instance of  `MapperConfigurer`:
```java
@Component
public class GlobalConfiguration extends MapperConfigurer {
    @Override
    public void configure(Configuration configuration) {
        configuration.setSkipNullEnabled(true);
    }
}
```

### Overriding the default mapping
In order to override the default mapping of some object types, lets say User -> UserDto, simply register within Spring application context instance of `TypeMapConfigurer`:
```java
@Component
public class MapperConfigurer extends TypeMapConfigurer<User, UserDto> {
    @Override
    public void configure(TypeMap<User, UserDto> typeMap) {
        typeMap.addMapping(User::getAge, UserDto::setAge);
        typeMap.addMappings(mapping -> mapping.skip(UserDto::setMiddleName));
        typeMap.setPreConverter(context -> {
            String[] name = context.getSource().getName().split(" ");
            context.getDestination().setFirstName(name[0]);
            context.getDestination().setLastName(name[1]);
            return context.getDestination();
        });
        
    }
}
```

###Testing
Scan for mapping components with the `WithModelMapper` annotation 
```java
@RunWith(SpringRunner.class)
@WithModelMapper(basePackage = "com.company.program.mapping")
public class MapperTest {
    
    @Test
    public void someTest() {
        
    }
}
```

## License
[Apache-2.0](http://www.apache.org/licenses/LICENSE-2.0)
