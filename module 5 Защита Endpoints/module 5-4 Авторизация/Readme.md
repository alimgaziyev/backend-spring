<p><code>Spring Security</code> обеспечивает всестороннюю поддержку авторизации. Авторизация - это определение того, кому разрешен доступ к определенному ресурсу. <code>Spring Security</code> обеспечивает глубокую защиту, предоставляя авторизацию на основе запросов и авторизацию на основе методов.</p>

<p>Расширенные возможности авторизации в <code>Spring Security</code> представляют собой одну из наиболее веских причин ее популярности. Независимо от того, как вы используете <code>Spring Security</code> аутентификацию, модуль авторизации работает простым способом.</p>

<p><img alt="" height="594" name="image.png" src="https://ucarecdn.com/8eaad68e-a60f-4a8f-8e7e-d48d56dd9f6b/" width="1106"></p>

<ol>
	<li><code>AuthorizationFilter</code> получает интерфейс <code>Authentication</code>, в котором есть методы для получения ролей активного пользователя.</li>
	<li><code>AuthorizationFilter</code> создает <code>FilterInvocation</code> из <code>HttpServletRequest</code>, <code>HttpServletResponse</code> и <code>FilterChain</code>.</li>
	<li>Далее <code>Authentication</code> и <code>AuthorizationManager</code> отправляется на выполнение к <code>AuthorizationManager</code></li>
	<li>Если авторизация запрещена, то срабатывает <code>AccessDeniedException</code>.</li>
	<li>Если авторизация разрешена, то <code>AuthorizationFilter</code> продолжает выполнение других элементов <code>FilterChain</code>.</li>
</ol>

<pre><code>@Bean
SecurityFilterChain web(HttpSecurity http) throws Exception {
	http
		// ...
		.authorizeHttpRequests(authorize -&gt; authorize                                       // 1
			.mvcMatchers("/resources/**", "/signup", "/about").permitAll()              // 2
			.mvcMatchers("/admin/**").hasRole("ADMIN")                                  // 3
			.mvcMatchers("/db/**").access("hasRole('ADMIN') and hasRole('DBA')")        // 4 
			.anyRequest().denyAll()                                                     // 5
		);

	return http.build();
}</code></pre>

<ol>
	<li>Указано несколько правил авторизации. Каждое правило рассматривается в том порядке, в котором оно было объявлено. т.е. сверху вниз.</li>
	<li>Мы указали несколько шаблонов <code>URL</code>, к которым может получить доступ любой пользователь. В частности, любой пользователь может получить доступ к запросу, если URL-адрес начинается с <code>/resources/</code>, равен <code>/signup</code> или равен <code>/about</code>.</li>
	<li>Любой <code>URL-адрес</code>, начинающийся с <code>/admin/</code>, будет доступен только пользователям с ролью <code>ROLE_ADMIN</code>. Поскольку мы вызываем метод <code>hasRole</code>, нам не нужно указывать префикс <code>ROLE_</code>.</li>
	<li>Любой <code>URL-адрес</code>, начинающийся с <code>/db/</code>, требует, чтобы у пользователя были как <code>ROLE_ADMIN</code>, так и <code>ROLE_DBA</code>. Поскольку мы используем выражение <code>hasRole</code>, нам не нужно указывать префикс <code>ROLE_</code>.</li>
	<li>Любому <code>URL-адресу</code>, который еще не был сопоставлен, будет отказано в доступе. Это хорошая стратегия, если вы не хотите случайно забыть обновить правила <code>авторизации</code>.</li>
</ol>

<p>Далее мы рассмотрим пример интеграции <code>Spring Security</code> в наше приложение.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-security/reference/servlet/authorization/index.html" rel="nofollow noopener noreferrer">Spring Security Authorization</a></li>
</ul>