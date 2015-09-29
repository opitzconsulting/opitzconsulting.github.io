--- 
layout: page
header: no
title: "Automated Unittesting of XQueries in Oracle OSB"
author: Pascal Brokmeier
categories:
- Oracle
- OSB
- SOA
- XQuery

---

In order to support a more agile project environment, OPITZ CONSULTING developed a Java/JUnit based SOA Unit Testing Framework. Among others, the framework was capable of using the Oracle libraries to run XQuery tests locally and verify their correctness. Over the course of time, Oracle released 12c and with it, changed its xquery files. A new extension (.xqy instead of .xq) was introduced and with it additional syntax. Oracle added a `(:: OracleAnnotationVersion "1.0" ::)` . After this, the testing framework needed to be updated to support such tags. Without an update, executing a query with the above annotation gave the following error: 

```
java.lang.RuntimeException: weblogic.xml.query.exceptions.XQueryStaticException: line 3, column 1: {err}XP0003: Invalid expression: either 'pragma' or 'extension' is required inside (:: ::)
	at com.opitzconsulting.soa.testing.AbstractXQueryTest.setQuery(AbstractXQueryTest.java:56)
	at de.home24.SalesOrderErrorListener.TransformationTest.testPrepareResponseTransformation(TransformationTest.java:25)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:78)
	at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:212)
	at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:68)
	at com.intellij.rt.execution.application.AppMain.main(AppMain.java:140)
Caused by: weblogic.xml.query.exceptions.XQueryStaticException: line 3, column 1: {err}XP0003: Invalid expression: either 'pragma' or 'extension' is required inside (:: ::)
	at weblogic.xml.query.exceptions.XQueryStaticException.create(XQueryStaticException.java:579)
	at weblogic.xml.query.exceptions.XQueryException.create(XQueryException.java:127)
	at weblogic.xml.query.exceptions.XQueryException.create(XQueryException.java:175)
	at weblogic.xml.query.compiler.parser.ParserUtil.lineColException(ParserUtil.java:69)
	at weblogic.xml.query.compiler.parser.ParserUtil.makeXQueryExceptionFrom(ParserUtil.java:91)
	at weblogic.xml.query.compiler.parser.XQueryMainLexer.reportError(XQueryMainLexer.java:106)
	at weblogic.xml.query.compiler.parser.XQueryMainLexer.mEXPR_COMMENT(XQueryMainLexer.java:3495)
	... 29 more
```
Updating the Testing Framework, we were able to continue unit testing xqueries  using the following syntax:

```java
OXQDataSource dataSource = new OXQDataSource();
XQConnection connection = dataSource.getConnection();
OXQEntity entity = new OXQEntity(query);
OXQConnection ocon = OXQView.getConnection(connection);
XQPreparedExpression expression = ocon.prepareExpression(entity);
expression.executeQuery();
```

However, in queries there are also other functions allowed. Among others `{fn-bea:($soapbody)}` would be a valid bea function. The above code however is not capable of handling such custom functions, resulting in the following error:

```
javax.xml.xquery.XQQueryException: line 49, column 27: {http://www.w3.org/2005/xqt-errors}XPST0081: The prefix "fn-bea" used in the qualified name "fn-bea:serialize" can not be resolved
	at oracle.xml.xquery.xqjimpl.OXQCUtils.createXQQueryException(OXQCUtils.java:992)
	at oracle.xml.xquery.xqjimpl.OXQCPreparedExpression.<init>(OXQCPreparedExpression.java:92)
	at oracle.xml.xquery.xqjimpl.OXQCConnection.prepareExpressionImpl(OXQCConnection.java:421)
	at oracle.xml.xquery.xqjimpl.OXQCConnection.prepareExpression(OXQCConnection.java:210)
	at oracle.xml.xquery.xqjimpl.OXQCConnection$OXQConnectionView.prepareExpression(OXQCConnection.java:528)
	at com.opitzconsulting.soa.testing.AbstractOXQueryTest.setOXQuery(AbstractOXQueryTest.java:32)
	at com.opitzconsulting.soa.testing.selftest.OXQuerySelfTest.testsetOXQuery(OXQuerySelfTest.java:29)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:78)
	at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:212)
	at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:68)
	at com.intellij.rt.execution.application.AppMain.main(AppMain.java:140)
Caused by: oracle.xml.xquery.exceptions.XQueryStaticException: line 49, column 27: {http://www.w3.org/2005/xqt-errors}XPST0081: The prefix "fn-bea" used in the qualified name "fn-bea:serialize" can not be resolved
	at oracle.xml.xquery.exceptions.XQueryStaticException.create(XQueryStaticException.java:696)
	at oracle.xml.xquery.exceptions.XQueryException.create(XQueryException.java:92)
...
```

Our theory: Oracle created a superset of xquery implementing both the above mentioned Oracle `(:: ::)` annotations as well as the `{fn-bea: ... }` custom functions which were created by bea. 

Analyzing the files opened by jDeveloper indicated the correctness of this theory. 

![]({{ site.baseurl }}/assets/img/2015-09-29-Automated-Unittesting-of-XQueries-in-Oracle-OSB/1.png)

As can be seen in the above image, the jDeveloper Process opened several .jar files while opening a .xqy xQuery. Jar files with `oxquery` as well as `bea` can be spotted. The former contained the Classes used in the above java snippet, which enabled us to parse the Oracle specific annotations. We suspect the latter to be capable of parsing xQueries with bea specific functions. However we could not find a way to parse a query that contains both forms.

Now the interesting part. The file activity list also includes an `oxquery-xmlbeans-interop.jar` which can be expected to be capable to interoperate with *something*, probably the bea xquery parsing utilities. 

What is the problem? Well this is as far as we got. We looked into the .jar files trying to find Classes and Methods that suggest being capable of parsing queries that both contain *bea* and *Oracle* specific syntax but we were not able to perform a successful test. 

If anyone has also tried Unit-Testing the new 12c xQueries we'd be happy to hear their solution.  Below is a view of the contents in the `interop.jar` files. Somehow we suspect there has to be a solution. However without proper documentation or an API, it's hard to create a successful piece of code

![]({{ site.baseurl }}/assets/img/2015-09-29-Automated-Unittesting-of-XQueries-in-Oracle-OSB/2.png) 
