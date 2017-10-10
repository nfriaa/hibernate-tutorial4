# hibernate-tutorial4
Hibernate tutorial 4 : **"Many To One"** association

[![contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg?style=flat)](https://github.com/nfriaa/hibernate-tutorial4/issues) [![Travis](https://img.shields.io/travis/rust-lang/rust.svg)](https://github.com/nfriaa/hibernate-tutorial4) [![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/nfriaa/hibernate-tutorial4/blob/master/LICENSE)

## Description
A sample code to learn how to map **"Many To One"** relationship between two entities using the Hibernate ORM.
* JavaSE 8
* Hibernate 5 / Annotations
* Hibernate **"Many To One"** association
* Maven 4
* MySQL 5

## 1. Database
Create only database, don't create tables (tables will be created by Hibernate)
* database name : **persist_db**

## 2. Maven "pom.xml" dependencies
```
<dependencies>
    <!-- MySQL connector -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>6.0.6</version>
    </dependency>
    <!-- Hibernate -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>5.2.11.Final</version>
    </dependency>
</dependencies>
```

## 3. Hibernate configuration file "hibernate.cfg.xml"
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!-- MySQL -->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/persist_db?useTimezone=true&amp;serverTimezone=UTC</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">root</property>

        <!-- Hibernate -->
        <property name="show_sql">true</property>
        <property name="format_sql">false</property>
        <property name="hibernate.hbm2ddl.auto">create</property>

        <!-- Entities -->
        <mapping class="net.isetjb.hibernatetutorial4.Product"/>
        <mapping class="net.isetjb.hibernatetutorial4.Category"/>
    </session-factory>
</hibernate-configuration>
```
* hibernate.hbm2ddl.auto : "create" => creates the schema necessary for defined entities, destroying any previous data
* don't forget to map the two entities in this XML config file (Product and Category)

## 4. "Many To One" association
`Source entity : Product.java`
```
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.ForeignKey;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;

@Entity
@Table(name = "product")
public class Product
{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id", nullable = false)
    private int id;

    @Column(name = "name", length = 255, nullable = true)
    private String name;

    @Column(name = "price", nullable = true)
    private int price;

    @ManyToOne
    @JoinColumn(name = "category_id", foreignKey = @ForeignKey(name = "CATEGORY_ID_FK"))
    private Category category;

    // Getters and Setters here...
}
```
- @ManyToOne : is equivalent to foreign key relationship in a database
- @JoinColumn : name of the foreign key column in the *source entity*
- @ForeignKey : name of the constraint (foreign key) in the *destination entity*

`Destination entity : Category.java`
```
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "category")
public class Category
{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id", nullable = false)
    private int id;

    @Column(name = "name", length = 255, nullable = true)
    private String name;

    // Getters and Setters here...
}
```

## 5. Main Class "Application.java"
* create main class to test the code
* example :
```
    // new category
    Category category_a = new Category();
    category_a.setName("Cat a");
    session.save(category_a);

    // new product
    Product product_x = new Product();
    product_x.setName("Prod x");
    product_x.setPrice(456);
    product_x.setCategory(category_a);
    session.save(product_x);
```