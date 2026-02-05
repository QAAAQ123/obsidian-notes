- gradle 기준
- dependencyManagement에 Spirng boot 버전에 맞는 spring cloud를 import하는 구문을 작성해야 한다.
- **~={red}spring cloud가 있어야 openfeign을 사용할 수 있다=~**
```gradle
dependencyManagement {  
    imports {  
       mavenBom "org.springframework.cloud:spring-cloud-dependencies:2024.0.2"  
    }  
}

dependencies {
	implementation 'org.springframework.cloud:spring-cloud-starter-openfeign'
}
```