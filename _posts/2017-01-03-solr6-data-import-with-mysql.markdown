---
layout: post
title: solr6 data import with mysql
date: 2017-01-03 16:00:00.000000000 +08:00
---

# 环境
* Ubuntu 12.04 
* Solr 6.3.0
* JDK8

1. 数据库表foo, user
```java
public class Foo {
    private long id;
    private String name;
    private Date updateDate;
    private long userId;
}
public class User {
    private long id;
    private String username;
    private Date updateDate;
}
```

2. 从$solr_home/dist 目录复制solr-dataimporthandler-6.3.0.jar，solr-dataimporthandler-extras-6.3.0.jar 到$solr_home/server/solr-webapp/webapp/WEB-INF/lib,然后复制mysql的java驱动包 mysql-connector-java-5.1.30.jar 到相同目录

3. 复制$solr_home/server/solr/configsets/data_driven_schema_configs 到 $solr_home/server/solr/ ,重命名为foo

4. 在foo/conf目录下面修改solrconfig.xml， 在<requestHandler name="/select" class="solr.SearchHandler">之上添加下面代码
```xml
<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">  
　     <lst name="defaults">  
　        <str name="config">data-config.xml</str>  
　     </lst>  
　</requestHandler>  
```

5. 在foo/conf下新建data-config.xml文件, 里面内容如下
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<dataConfig>  
    <dataSource name="ds" type="JdbcDataSource" driver="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/testdb" user="root" password="root" batchSize="-1" />  
  <document>  
        <entity name="foo" pk="id"  dataSource="ds"   
                query="select * from  foo"  
                 deltaImportQuery="select * from foo where id='${dih.delta.id}'"  
                deltaQuery="select id from foo where updateDate > '${dataimporter.last_index_time}'">  
			<field column="id" name="id"/>  
			<field column="name" name="name"/>              
            <field column="updateDate" name="updateDate"/>  
			 <entity name="user" pk="userId" query="username from user where userId='${foo.userId}'">
               <field column="username" name="username"/>  
            </entity>
     </entity>  
  </document>  
</dataConfig>  
```

6. 在foo/conf下managed-schema.xml 配置field信息：
```xml
<field name="id" type="long" indexed="true" stored="true" required="true" multiValued="false" />
<field name="name" type="string" indexed="true" stored="true" required="true" multiValued="false" />
<field name="username" type="string" indexed="true" stored="true" required="true" multiValued="false" />
<field name="updateDate" type="date" indexed="true" stored="true" required="true" multiValued="false" /> 
```


7. 启动solr后，添加刚才配置foo core，


8. corntab里面添加下面脚本，让solr delta import data， 没五分钟导入一次数据
```bash
*/5 * * * *	  /home/ubuntu/crontab_script/solr_foo.sh
```
solr_foo.sh 里面的内容
```bash
curl -d "command=delta-import&verbose=false&clean=false&commit:true&optimize:false&core:foo&name:dataimport" "http://127.0.0.1:8983/solr/foo/dataimport?_="+$(date +%s)+"&indent=on&wt=json"
```

##### 参考文档
* [ Solr之搭建Solr6.0服务并从Mysql上导入数据](http://blog.csdn.net/linzhiqiang0316/article/details/51464461)
