<h3>Какие навыки вы приобретете, выполнив данный проект:</h3>

<ul>
	<li><code>REST API</code></li>
	<li><code>JWT</code></li>
	<li><code>Spring Security</code></li>
	<li><code>Swagger</code></li>
</ul>

<h2>Описание:</h2>

<p>В этом проекте вам необходимо реализовать аутентификацию и авторизацию пользователей вместе с дополнительным функционалом по работе со счетами.</p>

<p>Основой для решения, является прошлый проект, в котором есть только один пользователь и все необходимые операции со счетами. Вам нужно расширить функционал, создав <code>CRUD</code> для пользователей.</p>

<p>В базе должна быть таблица <code>users</code> в которой хранится уникальный <code>id</code> пользователя, уникальный <code>username</code> и хэш пароля <code>password</code>.</p>

<h2>Auth</h2>

<p>Аутентификация должна происходить при <code>POST</code> запросе на ресурс <code>/authenticate</code>.</p>

<pre><code>curl -v -XPOST user:password@localhost:8080/authenticate</code></pre>

<pre><code>Результат:
* Connected to localhost (127.0.0.1) port 8080 (#0)
* Server auth using Basic with user 'user'
&gt; POST /token HTTP/1.1
&gt; Host: localhost:8080
&gt; Authorization: Basic dXNlcjpwYXNzd29yZA==
&gt; User-Agent: curl/7.79.1
&gt; Accept: */*
&gt;
* Mark bundle as not supporting multiuse
&lt; HTTP/1.1 200
&lt; X-Content-Type-Options: nosniff
&lt; X-XSS-Protection: 1; mode=block
&lt; Cache-Control: no-cache, no-store, max-age=0, must-revalidate
&lt; Pragma: no-cache
&lt; Expires: 0
&lt; X-Frame-Options: DENY
&lt; Content-Type: text/plain;charset=UTF-8
&lt; Content-Length: 464
&lt; Date: Thu, 21 Apr 2022 10:06:30 GMT
&lt;
* Connection #0 to host localhost left intact
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzZWxmIiwic3ViIjoidXNlciIsImV4cCI6MTY1MDU3MTU5MCwiaWF0IjoxNjUwNTM1NTkwLCJzY29wZSI6ImFwcCJ9.IBLiLpv7yeF_FTB4u8IrHUfft3M3dc-AyHkWT_11E1E
</code></pre>

<p>При успешной аутентификации, пользователь должен получить <code>JWT</code> токен, который служит идентификатором для сессии.</p>

<p><img alt="" height="2258" name="image.png" src="https://ucarecdn.com/483a4fb7-e7f5-42e5-935f-70add187e9fe/" width="3034"></p>

<p>в <code>JWT</code> обязательно должны быть <code>sub</code> - <code>username</code>, <code>scope</code> - роль.</p>

<p>Для регистрации пользователя используется ресурс <code>/register</code>, который принимает <code>POST</code> запрос с <code>JSON</code> объектом, в котором есть параметры: <code>username</code> и <code>password</code>. <code>password</code> пользователя должен хэшироваться функцией <code>bcrypt</code>.</p>

<p>Доступ к ресурсам для работы со счетами имеет только авторизированный пользователь.</p>

<pre>export TOKEN=`curl -XPOST user:password@localhost:8080/token`
curl -H "Authorization: Bearer $TOKEN" localhost:8080/accounts`</pre>

<p>Ответ:</p>

<pre><code>[]
</code></pre>

<p>Запрос с невалидным токеном:</p>

<pre><code>curl -v -H "Authorization: Bearer Invalid" localhost:8080/accounts</code></pre>

<p>Ответ:</p>

<pre><code>*   Trying 127.0.0.1:8080...
* Connected to localhost (127.0.0.1) port 8080 (#0)
&gt; GET / HTTP/1.1
&gt; Host: localhost:8080
&gt; User-Agent: curl/7.79.1
&gt; Accept: */*
&gt; Authorization: Bearer Invalid
&gt;
* Mark bundle as not supporting multiuse
&lt; HTTP/1.1 401
&lt; WWW-Authenticate: Bearer error="invalid_token", error_description="An error occurred while attempting to decode the Jwt: Invalid JWT serialization: Missing dot delimiter(s)", error_uri="https://tools.ietf.org/html/rfc6750#section-3.1"
&lt; X-Content-Type-Options: nosniff
&lt; X-XSS-Protection: 1; mode=block
&lt; Cache-Control: no-cache, no-store, max-age=0, must-revalidate
&lt; Pragma: no-cache
&lt; Expires: 0
&lt; X-Frame-Options: DENY
&lt; Content-Length: 0
&lt;
* Connection #0 to host localhost left intact
</code></pre>

<p>Пользователь не может смотреть информацию о счетах, транзакциях других пользователей.</p>

<h2>Функционал приложения:</h2>

<ol>
	<li>Приложение должно принимать запросы на 8080 порту.</li>
	<li>Приложение должно использовать <code>h2</code> как базу данных.</li>
	<li>У приложения должна быть <code>swagger</code> документация по <code>URN</code> - <code>/swagger-ui/index.html</code>.</li>
</ol>

<p>Конечная программа должна выполнять традиционные банковские операции.</p>

<p><code>/swagger-ui/index.html</code> - должна быть общедоступна.</p>

<p>Операции с пользователями:</p>

<ol>
	<li>Регистрация пользователя</li>
	<li>Аутентификация пользователя</li>
	<li>Получение информации о пользователе</li>
</ol>

<p>К основным операциям, выполняемыми пользователем со счетами относятся:</p>

<ol>
	<li>Получение информации о всех счетах</li>
	<li>Создание счета</li>
	<li>Пополнение счета (debit)</li>
	<li>Снятие денег со счета (withdraw)</li>
	<li>Получение списка всех операций по счету</li>
	<li>Перевод на счет пользователя (transfer)</li>
</ol>

<p>Ключевые факторы при проверке проекта:</p>

<ul>
	<li>Воспользуйтесь репозиторием <a href="https://classroom.github.com/a/85eCW3uN" rel="noopener noreferrer nofollow" target="_blank">GitHub Classroom</a> для этого задания. Работайте на ветке <code>feature/jwt</code>. После выполнения задания пришлите ссылку на репозиторий ревьюеру. После успешной приемки проекта не забудьте смерджить ветку <code>feature/jwt</code> с <code>master</code>. Это понадобится вам для следующего проекта.</li>
	<li>В репозитории должен быть <code>README.md</code> с описанием запуска программы.</li>
	<li>В проекте не должна происходить инициализация через <code>xml</code> - используй Аннотации.</li>
	<li>Пользователь может выполнять запросы на аутентификацию и регистрацию без активной сессии.</li>
	<li>При регистрации пользователя создается новая строка в таблице <code>users</code>.</li>
	<li><code>userID</code> - начинается с 1</li>
	<li>Пользователь может создать счет. К счетам относятся:
	<ol>
		<li>Текущий счет (CHECKING)</li>
		<li>Сберегательный счет (SAVING)</li>
		<li>Фиксированный счет (Fixed)</li>
	</ol>
	</li>
	<li>Пользователь может удалить свой счет.</li>
	<li>Пользователь может получить список всех своих счетов.</li>
	<li>Пользователь может получить список всех транзакций, проведенных со своим счетом.</li>
	<li>Пользователь может отправить деньги со своего счета на другой счет.</li>
	<li>Пользователь не может выполнять любые операции с чужими счетами.</li>
	<li>Каждый счет имеет уникальный <code>AccountID</code> и ссылку на <code>users</code>.</li>
	<li>Нумерация счетов должна начинаться с 1.</li>
	<li>Сумма на счете не может быть меньше нуля.</li>
	<li>Снимать деньги можно только с <code>Checking</code> и <code>Saving</code> счетов. Формат для <code>AccountID</code> счета:</li>
</ul>

<pre><code>String accountNumber = String.format("%03d%06d", 1, accountID);</code></pre>

<ul>
	<li>Все данные должны сохраняться в базе данных.</li>
	<li>Программа должна обрабатывать запросы по данным <code>URN</code></li>
</ul>

<pre><code>GET     /accounts                               Получение списка счетов
POST    /accounts                               Создание нового счета
GET     /accounts/{account_id}                  Получение информации об одном счете
DELETE  /accounts/{account_id}                  Удаление счета
POST    /accounts/{account_id}/withdraw         Снятие денег со счета
POST    /accounts/{account_id}/deposit          Внесение денег на счет
GET     /accounts/{account_id}/transactions     Получение списка всех транзакций
POST    /accounts/{account_id}/transfer         Отправка средств на другой счет (принимает в `body` - `destination_account_id`)
POST    /authenticate                           Аутентификация пользователя
POST    /register                               Регистрация пользователя
</code></pre>

<ul>
	<li>Программа должна обрабатывать ошибки и выводить соответствующее сообщение и <code>HTTP</code> статус.</li>
	<li>Каждый запрос должен возвращать ответ с информацией о произведенном действии.</li>
</ul>

<p>Например при успешном удалении:</p>

<pre><code>DELETE /accounts/1
Authorization: Bearer jwt.token

200
{"message": "Account 1 deleted"}
</code></pre>

<p>Или при невалидной отправке средств на счет должна быть ошибка:</p>

<pre><code>POST /accounts/1/transfer
Authorization: Bearer jwt.token
{"destination_account_id": "2222"}

404
{"message": "Account 2222 does not exist"}
</code></pre>

<blockquote>
<p>Примечание: важно, чтобы все операции могли выполняться через <code>swagger</code>. Для реализации авторизации по <code>JWT</code> токену в <code>header</code> можно использовать данную аннотацию для вашего <code>main</code> класса:</p>
</blockquote>

<pre><code>@SpringBootApplication
@SecurityScheme(name = "basicauth", scheme = "bearer", type = SecuritySchemeType.HTTP, in = SecuritySchemeIn.HEADER, bearerFormat = "JWT")
public class SchoolApplication {
    // main
}</code></pre>

<p>Полезные ссылки:</p>

<ul>
	<li><a href="https://jwt.io/introduction" rel="nofollow noopener noreferrer">JWT explained</a></li>
	<li><a href="https://docs.spring.io/spring-security/reference/index.html" rel="nofollow noopener noreferrer">Spring Security Reference Doc</a></li>
	<li><a href="https://stackoverflow.com/questions/41480102/how-spring-security-filter-chain-works" rel="nofollow noopener noreferrer">How Spring Security Filter Chain works</a></li>
	<li><a href="https://www.synopsys.com/glossary/what-is-csrf.html" rel="nofollow noopener noreferrer">CSRF explained</a></li>
	<li><a href="https://en.wikipedia.org/wiki/Cross-origin_resource_sharing" rel="nofollow noopener noreferrer">CORS explained</a></li>
	<li><a href="https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/oauth2/jwt/package-summary.html" rel="nofollow noopener noreferrer">Spring JWT</a></li>
	<li><a href="https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/bcrypt/BCrypt.html" rel="nofollow noopener noreferrer">Bcrypt</a></li>
	<li><a href="https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/userdetails/UserDetailsService.html" rel="nofollow noopener noreferrer">UserDetailsService</a></li>
</ul>