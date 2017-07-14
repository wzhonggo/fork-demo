---
layout: post
title: Restricting IP addresses for Jetty and Solr
date: 2017-06-15 18:00:00.000000000 +08:00
---

# 环境
* Solr6.3.0 (jetty-server-9.3.8.v20160314)
* Solr6.6.0 (jetty-server-9.3.14.v20161028)

#
* Solr默认外网直接可以通过8983端口访问， Solr限制访问的ip为127.0.0.1, 这样外网ip可以访问8983端口，但是api访问，response http code 都是403, jetty xml配置语法参考[Jetty IoC XML format](http://www.eclipse.org/jetty/documentation/9.4.x/quick-start-configure.html)
* Solr6.3.0 修改$solrpath/server/etc/jetty.xml添加下面 Restricting IP addresses for Jetty and Solr 之间的代码

``` xml
 <!-- =========================================================== -->
<!-- Set handler Collection Structure                            -->
<!-- =========================================================== -->
<Set name="handler">
    <New id="Handlers" class="org.eclipse.jetty.server.handler.HandlerCollection">
        <Set name="handlers">
            <Array type="org.eclipse.jetty.server.Handler">
                <Item>
                    <Ref id="RewriteHandler"/>
                </Item>

                <Item>
                    <New id="Contexts" class="org.eclipse.jetty.server.handler.ContextHandlerCollection"/>
                </Item>

                <Item>
                    <New id="DefaultHandler" class="org.eclipse.jetty.server.handler.DefaultHandler"/>
                </Item>
                <Item>
                    <New id="RequestLog" class="org.eclipse.jetty.server.handler.RequestLogHandler"/>
                </Item>

                <!-- Restricting IP addresses for Jetty and Solr -->
                <Item>
                    <New id="IPAccessHandler" class="org.eclipse.jetty.server.handler.IPAccessHandler">
                        <Set name="white">
                            <Array type="String">
                                <Item>127.0.0.1</Item>
                            </Array>
                        </Set>
                        <Set name="whiteListByPath">false</Set>
                        <Set name="handler">
                            <Ref refid="Contexts"/>
                        </Set>
                    </New>
                </Item>
                <!-- Restricting IP addresses for Jetty and Solr   -->

            </Array>
        </Set>
    </New>
</Set>

```


# Solr6.6.0 修改$solrpath/server/etc/jetty.xml添加下面 Restricting IP addresses for Jetty and Solr 之间的代码
```xml
<!-- =========================================================== -->
<!-- Set handler Collection Structure                            -->
<!-- =========================================================== -->
<Set name="handler">
    <New id="Handlers" class="org.eclipse.jetty.server.handler.HandlerCollection">
        <Set name="handlers">
            <Array type="org.eclipse.jetty.server.Handler">

                <Item>
                    <New id="Contexts" class="org.eclipse.jetty.server.handler.ContextHandlerCollection"/>
                </Item>

                <Item>
                    <New id="InstrumentedHandler" class="com.codahale.metrics.jetty9.InstrumentedHandler">
                        <Arg>
                            <Ref refid="solrJettyMetricRegistry"/>
                        </Arg>
                        <Set name="handler">
                            <New id="DefaultHandler" class="org.eclipse.jetty.server.handler.DefaultHandler"/>
                        </Set>
                    </New>
                </Item>
                <Item>
                    <New id="RequestLog" class="org.eclipse.jetty.server.handler.RequestLogHandler"/>
                </Item>

                <!-- Restricting IP addresses for Jetty and Solr -->
                <Item>
                    <New id="InetAccessHandler" class="org.eclipse.jetty.server.handler.InetAccessHandler">

                        <Call name="include">
                            <Arg>
                                127.0.0.1
                            </Arg>
                        </Call>
                        <Set name="handler">
                            <Ref refid="Contexts"/>
                        </Set>
                    </New>
                </Item>
                <!-- Restricting IP addresses for Jetty and Solr   -->

            </Array>
        </Set>
    </New>
</Set>

```
##### 参考文档
* [Restricting IP addresses for Jetty and Solr](https://stackoverflow.com/questions/8924102/restricting-ip-addresses-for-jetty-and-solr/16615064#16615064)
* [Jetty IoC XML format](http://www.eclipse.org/jetty/documentation/9.4.x/quick-start-configure.html)
* [InetAccessHandler](http://www.eclipse.org/jetty/javadoc/9.3.18.v20170406/index.html?org/eclipse/jetty/server/handler/InetAccessHandler.html)
* [IPAccessHandler](http://www.eclipse.org/jetty/javadoc/9.3.18.v20170406/index.html?org/eclipse/jetty/server/handler/IPAccessHandler.html)