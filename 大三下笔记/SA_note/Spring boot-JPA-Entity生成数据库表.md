# Spring boot-JPA数据库操作

## 根据Entity生成数据库表-初建项目场景

### application.properties

```properties
# auto create table from entity when start 
spring.jpa.hibernate.ddl-auto=update
```

### pom.xml

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
</dependency>
```

### Entity添加注解

以student为例添加如下注解

```java
@Entity
@Data
@Builder
@Table(name="数据库表名", schema="数据库名", catelog="")
public class student{
    @Id
    @Column(name = "id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    
    @Column(name = "student_name")
    private String studentName;
    
    ...
}
```

之后在代码运行过程中数据库表就会随Entity类进行更新