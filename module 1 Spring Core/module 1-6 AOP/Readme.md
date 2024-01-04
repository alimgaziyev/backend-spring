<p><code>AOP</code> - aspect oriented programming (аспектно ориентированное программирование). Это подход в программировании по которому в коде определенные задачи отделяются от бизнес-логики.</p>

<p>К примеру у нас есть магазин продуктов, он состоит из одного отдела. В этом отделе работает один человек. В магазин могут входить все, но доступ к кассе есть только у работника. В ООП виде у класса <code>Store</code> был бы метод <code>enterTheCheckout</code> в котором бы проверялся пользователь на наличие соответствующих прав.</p>

<p>Магазин растет и появляются новые отделы, новые работники и новая сложная структура входов. И в каждом классе типа магазина мы постоянно пишем один и тот же метод, который проверяет наличие прав у человека.</p>

<p>АОП подход позволяет создавать обертку над магазином, назовем его <code>AccessManagement</code>. При каждом вызове <code>enterTheCheckout</code> у магазина, будет вызываться метод класса <code>AccessManagement</code>.</p>

<p>В <code>Spring</code> есть готовая реализация <code>AOP</code> подхода. В терминах он состоит из 4 ключевых слов:</p>

<ul>
	<li>Aspect - access management, logging и другие.</li>
	<li>Joint Point - участок кода на котором будет задействован аспект.</li>
	<li>Advice - код, описывающий поведение для аспекта</li>
	<li>Pointcut - связка joint point и аспекта (при вызове, после вызова)</li>
</ul>

<p>Типы <code>Advice</code>:</p>

<ul>
	<li>before: метод аспекта перед вызовом основного метода;</li>
	<li>after: метод аспекта после вызова основного метода;</li>
	<li>after-returning: метод аспекта при возврате значения из основного метода;</li>
	<li>after-throwing: метод аспекта при выводе исключения из основного метода;</li>
	<li>around;</li>
</ul>

<p>Давайте добавим в наш код из прошлого примера с <code>Reminder</code> аспект <code>Logger</code>. Который будет просто выводить информацию о вызове метода до и после и будет изменять значение выводимого значения в формат логов: <code>${date}: ${value}</code></p>

<p>Создадим наш класс для аспекта:</p>

<pre><code>// logger.java
package com.example.demo;

import java.util.Date;

public class Logger {
    public void before() {
        System.out.printf("[INFO][%s][before advice]\n", new Date());
    }

    public void after() {
        System.out.printf("[INFO][%s][after advice]\n", new Date());
    }

    public void afterReturning(Object retVal) {
        System.out.printf("[INFO][%s][afterReturning advice][%s]\n", new Date(), retVal);
    }
}</code></pre>

<p>Теперь обновим конфигурационный файл:</p>

<pre><code>&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
	    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd"&gt;

    &lt;aop:config&gt;
        &lt;!-- Aspect --&gt;
        &lt;aop:aspect id="loggerAspect" ref="logger"&gt;

            &lt;!-- Pointcut --&gt;
            &lt;aop:pointcut id="ClientAllMethods" expression="execution(* com.example.demo.Client.*(..))"/&gt;

            &lt;!-- Advice(s) --&gt;
            &lt;aop:before pointcut-ref="ClientAllMethods" method="before"/&gt;
            &lt;aop:after  pointcut-ref="ClientAllMethods" method="after"/&gt;
            &lt;aop:after-returning pointcut-ref="ClientAllMethods" returning="retVal" method="afterReturning"/&gt;
        &lt;/aop:aspect&gt;
    &lt;/aop:config&gt;

    &lt;bean id="reminder" class="com.example.demo.Reminder" scope="prototype"&gt;
    &lt;/bean&gt;

    &lt;bean id="client2" class="com.example.demo.Client"&gt;
        &lt;constructor-arg type="String" value="Client 2"/&gt;
        &lt;constructor-arg ref="reminder" /&gt;
    &lt;/bean&gt;

    &lt;bean id="client" class="com.example.demo.Client"&gt;
        &lt;constructor-arg type="String" value="Client 1"/&gt;
        &lt;constructor-arg ref="reminder" /&gt;
    &lt;/bean&gt;

    &lt;bean id="logger" class="com.example.demo.Logger"&gt;&lt;/bean&gt;

&lt;/beans&gt;</code></pre>

<pre><code>xmlns:aop="http://www.springframework.org/schema/aop"
</code></pre>

<p>подключает aop тэги.</p>

<pre><code>execution(* com.example.demo.Client.*(..)
</code></pre>

<p>указывает на все вызовы метода класса <code>Client</code></p>

<p>Запустив данную программу, в логах мы увидим:</p>

<pre><code>[INFO][Mon Mar 28 23:06:05 ALMT 2022][before advice]
Client 1 1
[INFO][Mon Mar 28 23:06:05 ALMT 2022][after advice]
[INFO][Mon Mar 28 23:06:05 ALMT 2022][afterReturning advice][null]
[INFO][Mon Mar 28 23:06:05 ALMT 2022][before advice]
Client 2 1
[INFO][Mon Mar 28 23:06:05 ALMT 2022][after advice]
[INFO][Mon Mar 28 23:06:05 ALMT 2022][afterReturning advice][null]
</code></pre>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop" rel="nofollow noopener noreferrer">Spring AOP</a></li>
</ul>