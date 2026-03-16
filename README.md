# FSAD-EXPERIMENT2
EXPERIMENT2

<project xmlns="http://maven.apache.org/POM/4.0.0"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
 http://maven.apache.org/xsd/maven-4.0.0.xsd">

 <modelVersion>4.0.0</modelVersion>
 <groupId>com.inventory</groupId>
 <artifactId>hibernate-crud</artifactId>
 <version>1.0</version>

 <dependencies>

  <dependency>
   <groupId>org.hibernate</groupId>
   <artifactId>hibernate-core</artifactId>
   <version>5.6.15.Final</version>
  </dependency>

  <dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>8.0.33</version>
  </dependency>

  <dependency>
   <groupId>javax.persistence</groupId>
   <artifactId>javax.persistence-api</artifactId>
   <version>2.2</version>
  </dependency>

  <dependency>
   <groupId>org.slf4j</groupId>
   <artifactId>slf4j-simple</artifactId>
   <version>1.7.36</version>
  </dependency>

 </dependencies>

</project>
<!DOCTYPE hibernate-configuration PUBLIC
 "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
 "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
 <session-factory>

  <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
  <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/inventorydb</property>
  <property name="hibernate.connection.username">root</property>
  <property name="hibernate.connection.password">root</property>

  <property name="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</property>
  <property name="hibernate.hbm2ddl.auto">update</property>
  <property name="hibernate.show_sql">true</property>

  <mapping class="com.inventory.Product"/>

 </session-factory>
</hibernate-configuration>
package com.inventory;

import javax.persistence.*;

@Entity
@Table(name="product")
public class Product {

 @Id
 @GeneratedValue(strategy = GenerationType.IDENTITY)
 private int id;

 private String name;
 private String description;
 private double price;
 private int quantity;

 public Product(){}

 public Product(String name,String description,double price,int quantity){
  this.name=name;
  this.description=description;
  this.price=price;
  this.quantity=quantity;
 }

 public int getId(){ return id; }
 public void setId(int id){ this.id=id; }

 public String getName(){ return name; }
 public void setName(String name){ this.name=name; }

 public String getDescription(){ return description; }
 public void setDescription(String description){ this.description=description; }

 public double getPrice(){ return price; }
 public void setPrice(double price){ this.price=price; }

 public int getQuantity(){ return quantity; }
 public void setQuantity(int quantity){ this.quantity=quantity; }
}
package com.inventory;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {

 private static SessionFactory factory;

 static{
  factory = new Configuration()
   .configure("hibernate.cfg.xml")
   .buildSessionFactory();
 }

 public static SessionFactory getSessionFactory(){
  return factory;
 }

}
package com.inventory;

import org.hibernate.Session;
import org.hibernate.Transaction;

public class ClientDemo {

 public static void main(String[] args) {

  Session session = HibernateUtil.getSessionFactory().openSession();
  Transaction tx = session.beginTransaction();

  Product p1 = new Product("Laptop","Dell Laptop",50000,10);
  Product p2 = new Product("Mouse","Wireless Mouse",500,50);

  session.save(p1);
  session.save(p2);

  Product product = session.get(Product.class,1);
  System.out.println(product.getName()+" "+product.getPrice());

  product.setPrice(52000);
  session.update(product);

  Product p = session.get(Product.class,2);
  session.delete(p);

  tx.commit();
  session.close();
 }
}
