<p>Spring Security интегрируется на уровне&nbsp;<code>Servlet</code>ов. Это означает, что его методы встраиваются перед&nbsp;<code>DispatcherServlet</code>.&nbsp;<code>DispatcherServlet</code>&nbsp;принимает запросы и распределяет их по&nbsp;<code>RestController</code>.</p>

<p>Ресурс&nbsp;<code>/private</code>&nbsp;выполняет бизнес-логику. Перед и после выполнения логики в&nbsp;<code>/private</code>&nbsp;может быть неограниченное количество правил, фильтров, встроенных&nbsp;<code>Spring Security</code>.</p>

<p><img alt="" height="462" name="image.png" src="https://ucarecdn.com/ecfb3c4f-489e-4af1-bb3d-cb2f769fab87/" width="320" /></p>

<p>При выполнении клиентом запроса информации по ресурсу&nbsp;<code>/private</code>&nbsp;<code>Spring Container</code>&nbsp;создает список из ранее объявленных в коде правил/фильтров и самого&nbsp;<code>Servlet</code>, который нашем примере является&nbsp;<code>DispatcherServlet</code>.</p>

<p>Далее, каждое правило/фильтр выполняются. Они принимают&nbsp;<code>ServletRequest</code>,&nbsp;<code>ServletResponse</code>.</p>

<pre>
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	// Операции перед выполнением главного запроса
    chain.doFilter(request, response); // Выполнить операцию
    // Операции после завершения главного запроса
}</pre>

<p><code>Filter</code>&nbsp;может быть встроен из&nbsp;<code>ApplicationContext</code>&nbsp;приложения:</p>

<pre>
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	Filter delegate = getFilterBean(someBeanName);
	delegate.doFilter(request, response);
}</pre>

<p><code>Spring Security</code>&nbsp;- предоставляет большой список готовых&nbsp;<code>Filter</code>, которые можно использовать в приложении.</p>

<ul>
	<li>ChannelProcessingFilter</li>
	<li>WebAsyncManagerIntegrationFilter</li>
	<li>SecurityContextPersistenceFilter</li>
	<li>HeaderWriterFilter</li>
	<li>CorsFilter</li>
	<li>CsrfFilter</li>
	<li>LogoutFilter</li>
	<li>OAuth2AuthorizationRequestRedirectFilter</li>
	<li>Saml2WebSsoAuthenticationRequestFilter</li>
	<li>X509AuthenticationFilter</li>
	<li>AbstractPreAuthenticatedProcessingFilter</li>
	<li>CasAuthenticationFilter</li>
	<li>OAuth2LoginAuthenticationFilter</li>
	<li>Saml2WebSsoAuthenticationFilter</li>
	<li>UsernamePasswordAuthenticationFilter</li>
	<li>OpenIDAuthenticationFilter</li>
	<li>DefaultLoginPageGeneratingFilter</li>
	<li>DefaultLogoutPageGeneratingFilter</li>
	<li>ConcurrentSessionFilter</li>
	<li>DigestAuthenticationFilter</li>
	<li>BearerTokenAuthenticationFilter</li>
	<li>BasicAuthenticationFilter</li>
	<li>RequestCacheAwareFilter</li>
	<li>SecurityContextHolderAwareRequestFilter</li>
	<li>JaasApiIntegrationFilter</li>
	<li>RememberMeAuthenticationFilter</li>
	<li>AnonymousAuthenticationFilter</li>
	<li>OAuth2AuthorizationCodeGrantFilter</li>
	<li>SessionManagementFilter</li>
	<li>ExceptionTranslationFilter</li>
	<li>FilterSecurityInterceptor</li>
	<li>SwitchUserFilter</li>
</ul>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-security/reference/index.html" rel="nofollow noopener noreferrer">Spring Security Core</a></li>
</ul>
