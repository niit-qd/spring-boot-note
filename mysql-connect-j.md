# mysql:mysql-connector-java to com.mysql:mysql-connector-j

1. mysql
    Functionality Added or Changed: [Changes in MySQL Connector/J 8.0.31 (2022-10-11, General Availability)](https://dev.mysql.com/doc/relnotes/connector-j/8.0/en/news-8-0-31.html)

    `Connector/J` 已经改为：
    - groupId: com.mysql
    - artifactId: mysql-connector-j

    ``` txt
    Functionality Added or Changed
    Important Change: To comply with proper naming guidelines, the Maven groupId and artifactId for Connector/J have been changed to the following starting with this release:

    groupId: com.mysql

    artifactId: mysql-connector-j

    The old groupId and artifactId can still be used for linking the Connector/J library, but they will point to a Maven relocation POM, redirecting users to the new coordinates. Please switch to the new coordinates as soon as possible, as the old coordinates could be discontinued anytime without notice. See Installing Connector/J Using Maven.

    Also, to go with these changes, the .jar library for Connector/J has been renamed to mysql-connector-j-x.y.z for all channels of distribution by Oracle, not just the Maven repository. (WL #15259)
    ```

2. Spring-Boot-2.7-Release-Notes
    [Spring Boot 2.7 Release Notes](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.7-Release-Notes)
    [MySQL JDBC Driver](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.7-Release-Notes#mysql-jdbc-driver)
    从`Spring Boot 2.7.8`开始，`MySQL JDBC driver`已经升级到`com.mysql:mysql-connector-j` `8.0.32`。
    ``` txt
    MySQL JDBC Driver
    The coordinates of the MySQL JDBC driver have changed. 8.0.31 is published to com.mysql:mysql-connector-j in addition to mysql:mysql-connector-java. In 8.0.32 and later it is only published to com.mysql:mysql-connector-j. Spring Boot 2.7.8 upgraded to 8.0.32. If you are using the MySQL JDBC driver, update its coordinates accordingly when upgrading to Spring Boot 2.7.8 and later.
    ```
    从实际验证来看，
    - 从`Spring Boot 2.7.8`开始，已经不再默认支持`mysql:mysql-connector-java`，只能使用`mysql-connector-j`。
    - 从`Spring Boot 2.7.5`开始，即便显示使用`mysql:mysql-connector-java`，也会自动relocate到`com.mysql:ysql-connector-j`。即，使用引用的是`com.mysql:mysql-connector-j`。
        ``` xml
        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>

        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.31</version>

        <distributionManagement>
            <relocation>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <message>MySQL Connector/J artifacts moved to reverse-DNS compliant Maven 2+ coordinates.</message>
            </relocation>
        </distributionManagement>
        </project>
        ```
        ``` log
        [WARNING] The artifact mysql:mysql-connector-java:jar:8.0.31 has been relocated to com.mysql:mysql-connector-j:jar:8.0.31: MySQL Connector/J artifacts moved to reverse-DNS compliant Maven 2+ coordinates.
        ```
    - 在`Spring Boot 2.7.4`及之前，使用`mysql:mysql-connector-java`，不会relocate到`com.mysql:mysql-connector-j`。即，使用引用的是`mysql:mysql-connector-java`。
3. Spring-Boot-3.0-Migration-Guide
    [Spring-Boot-3.0-Migration-Guide](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guid)
    [MySQL JDBC Driver](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide#mysql-jdbc-driver)
    升级到`Spring Boot 3.0`后，`MySQL JDBC driver`已经从`mysql:mysql-connector-java`改变为`com.mysql:mysql-connector-j`。
    ``` txt
    MySQL JDBC Driver
    The coordinates of the MySQL JDBC driver have changed from mysql:mysql-connector-java to com.mysql:mysql-connector-j. If you are using the MySQL JDBC driver, update its coordinates accordingly when upgrading to Spring Boot 3.0.
    ```
    如上描述，实际上，从`Spring Boot 2.7.8`开始，`MySQL JDBC driver`已经不再支持`mysql:mysql-connector-java`，只支持`com.mysql:mysql-connector-j`。

