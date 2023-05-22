# Spring-Boot中DAO包和Repository包的区别



在Spring Boot中，DAO和Repository包都是用于封装数据访问逻辑的层次，它们的主要区别在于不同的实现方式和面向对象的不同思想。

## DAO

DAO（Data Access Object）是一种**面向过程的设计思想**，它将数据访问逻辑与业务逻辑分离。

DAO通常包含了数据库的连接和SQL语句的执行，以及**数据的持久化**和查询等操作。

使用DAO时，通常需要**手动编写SQL语句**或**使用ORM框架**（如**MyBatis**）来**简化数据库操作**。

## Repository

Repository是一种**面向对象的设计思想**，它`将数据访问逻辑封装成对象的方法`，使得数据的访问更加直观和易于使用。

> Repository 将数据访问逻辑封装成为对象的方法，指的是**将数据库的操作（如增删改查）封装在一个 Java 类中，以对象的方式进行调用**，从而简化对数据库的访问。
>
> 例如，如果一个项目中需要实现用户登录的功能，我们可以定义一个 UserRepository 类来管理用户数据，该类中定义了一些方法，比如 addUser、removeUser、getUserById 等，来实现对用户信息的增删改查操作。这样，Service 层就可以通过调用 UserRepository 类的这些方法来实现对用户信息的访问和管理，而不必关心具体的数据库操作细节。
>
> 使用 Repository 的方法还可以避免 SQL 注入等安全问题，因为 Repository 会将所有的 SQL 语句参数化，从而保证了应用的安全性。

在Spring Boot中，Repository通常是基于Spring Data JPA实现的，它提供了一种简单而强大的方式来处理与数据库的交互，通过使用一些**预定义的方法和查询语言**，使得数据访问更加易于使用和灵活。

与DAO不同，使用Repository时，我们**不需要手动编写SQL语句**，而是通过**定义方法来完成数据访问操作**。

## Repository 将数据访问逻辑封装成为对象的方法-实例解释

假设我们有一个**用户信息管理系统**，需要实现对用户信息的增删改查功能。

在该系统中，我们需要定义一个用户实体类（User），并将用户信息存储到数据库中。

在传统的开发方式中，我们通常需要编写数据库访问代码，包括连接数据库、执行 SQL 语句等。

但是，在 Spring 框架中，我们可以使用 **Repository 将数据访问逻辑封装成为对象的方法**。

假设我们使用 **Spring Data JPA** 来实现用户信息的增删改查操作，我们可以**定义一个 UserRepository 接口**，该接口**继承 JpaRepository 接口**。

在 UserRepository 接口中，我们可以定义一些方法，如 findByUsername(String username)、save(User user)、delete(User user) 等，用来实现对用户信息的查询、保存和删除操作。

```Java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
    User save(User user);
    void delete(User user);
}
```

在上述代码中，@Repository 注解表示该接口是一个 Repository，**JpaRepository 是 Spring Data JPA 提供的一个基础 Repository**，继承该接口可以获得一些基础的 CRUD（增删改查）操作方法。

UserRepository 接口定义了 findByUsername、save 和 delete 等方法，可以实现对用户信息的查询、保存和删除操作。

### 在 UserRepository 接口中定义的方法不需要进行具体的实现

因为 UserRepository 接口继承了 JpaRepository 接口，而 JpaRepository 接口**已经提供了一些基础的 CRUD（增删改查）操作方法的默认实现**。

例如，JpaRepository 接口中**已经定义了一个 save() 方法**，用于**保存一个实体对象到数据库中**。

因此，当我们在 UserRepository 接口中**声明了一个 save() 方法**时，其实是**继承了 JpaRepository 接口的 save() 方法**，**不需要再进行具体的实现**。

当我们调用 UserRepository 接口中定义的方法时，**Spring Data JPA 会根据方法的名称和参数自动生成相应的 SQL 语句**，并将其转换成为对应的数据库操作。`

这样，我们就可以通过简单的接口调用来完成复杂的数据库操作，避免了手动编写 SQL 语句的繁琐和复杂。

### 定义除CRUD外其他的自定义查询方法

Spring Data JPA 的 JpaRepository 接口已经提供了一些基础的 CRUD（增删改查）操作方法的默认实现，包括保存、删除、查询等操作。如果我们**只需要实现这些基础的操作**，可以**直接继承 JpaRepository 接口**，在自定义的 Repository 接口中**不需要重新定义这些方法**。

如果需要**定义其他的自定义查询方法**，可以**在自定义的 Repository 接口(例如上例中的UserRepository)中声明这些方法**，`根据方法名的命名规则，Spring Data JPA 会自动根据方法名生成对应的 SQL 语句`。

如果在自定义的 Repository 接口中定义的方法与 JpaRepository 接口中已有的**方法重名**，Spring Data JPA 会**优先使用自定义的方法实现**。

在实际开发中，我们可以通过**查看 JpaRepository 接口的源码来确定其提供的基础方法，根据需要选择继承该接口还是自定义 Repository 接口**。同时，在自定义 Repository 接口中，应该**尽量避免定义与基础方法重名的方法**，以避免出现不必要的歧义和错误。

### Spring Data JPA 如何根据方法的名称和参数自动生成相应的 SQL 语句

Spring Data JPA 根据方法的名称和参数自动生成相应的 SQL 语句是通过一种名为**方法名解析**的技术实现的。

在 Spring Data JPA 中，我们可以通过在 Repository 接口中定义一些**特定命名规则**的方法，来让 Spring Data JPA **自动生成对应的 SQL 语句**。

这些**命名规则基于方法名的语义，以及参数类型、返回值类型、方法的参数数量等信息**，通过一定的规则**映射成对应的数据库操作**。

下面是一些常用的命名规则：

- **根据属性**查询

  - **findBy{属性名}**：根据**单个属性查询**，例如 findByUsername(String username)；

  - **findBy{属性名}And{属性名}**：根据**多个属性查询**，例如 findByUsernameAndAge(String username, Integer age)；

  - **findBy{属性名}In**：**根据属性值集合查询**，例如 findByUsernameIn(Collection<String> usernames)；

  - **findBy{属性名}Like**：根据属性**模糊查询**，例如 findByUsernameLike(String username)。

- 根据关联属性查询

  - findBy{关联属性名}.{属性名}：根据关联属性的属性名查询，例如 findByDepartmentName(String name)；


  - findBy{关联属性名}.{属性名}And{属性名}：根据关联属性的属性名和自身属性名查询，例如 findByDepartmentNameAndUsername(String name, String username)；

  - findBy{关联属性名}.{属性名}In：根据关联属性的属性名和属性值集合查询，例如 findByDepartmentNameIn(Collection<String> names)。

- 分页查询

  - findBy{属性名}：分页查询，例如 findByUsername(String username, Pageable pageable)；


  - findBy{属性名}OrderBy{属性名}Desc：按照属性排序查询，例如 findByUsernameOrderByAgeDesc(String username, Pageable pageable)。

- 其他查询

  - countBy{属性名}：统计数量，例如 countByUsername(String username)；


  - deleteBy{属性名}：根据属性删除，例如 deleteByUsername(String username)。

以上仅是一些常用的命名规则，Spring Data JPA 还支持更多的命名规则和复杂查询语句的生成方式。在实际使用中，我们可以根据具体需求和文档说明来选择合适的方法和命名规则。

## Jpa和Repository

JPA（Java Persistence API）是一种**Java ORM（对象关系映射）标准**，它提供了一种通用的方式来处理对象与关系数据库之间的映射，使得开发者可以**使用面向对象的方式进行数据库操作**。

Spring Data JPA是**基于JPA标准**的一个扩展框架，它为我们提供了一些**预定义的方法和查询语言**来简化数据访问的操作。

Repository通常是基于Spring Data JPA实现的。

### 总结

DAO和Repository都是用于**封装数据访问逻辑**的层次，它们的主要区别在于**不同的实现方式**和**面向对象的不同思想**。

在Spring Boot中，我们可以选择使用DAO或Repository来处理数据访问逻辑，而**Repository通常是基于Spring Data JPA实现**的，提供了一种更加方便和易于使用的方式来处理数据访问。

