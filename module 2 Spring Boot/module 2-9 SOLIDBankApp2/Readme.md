<p>Вам нужно будет дополнить функционал небольшого банковского приложения на основе диаграммы классов <code>UML</code>.</p>

<h3>Какие навыки вы приобретете, выполнив данный проект:</h3>

<ul>
	<li>Чтение UML диаграмм;</li>
	<li>Знание основ принципов SOLID;</li>
	<li>Сборка Spring приложения через авто-конфигурацию;</li>
</ul>

<h2>Описание:</h2>

<p>Ваша прошлая работа устраивает требованиям и вам дают следующее задание. Команде не нравится каждый раз читать <code>xml</code> файл для информации о связях между компонентами и теперь вам нужно использовать авто-конфигурацию. Дополнительно, команда просит вас реализовать остальные компоненты из <code>uml</code> диаграммы.</p>

<p><img alt="" height="1979" name="image.png" src="https://ucarecdn.com/9c83ab15-eac1-4e21-85a2-ca2770fc2d90/" width="2008"></p>

<p>Архитектор проекта отправил вам новое сообщение:</p>

<p>Приветствую! Вижу твой прогресс по последней работе, надеюсь у тебя уже есть основное понимание принципов <code>SOLID</code> и чистой архитектуры, они тебе понадобятся в реальной работе. Давай рассмотрим функционал, связанный с транзакциями и операциями со счетами.</p>

<p>Вместе с доменом <code>Account</code> у нас есть домен <code>Transaction</code>. Мы всегда должны хранить историю всех операций для избежания ошибок при работе со счетами.</p>

<p><code>TransactionDAO</code> - это интерфейс, который описывает операции с базой данных. В нашем случае все транзакции в памяти - <code>MemoryTransactionDAO</code>.</p>

<p>Каждый отдельный бизнес процесс имеет свой интерфейс - <code>AccountWithdrawService</code>, <code>AccountDepositService</code> занимаются снятием и добавлением денег на счета.</p>

<p><code>TransactionWithdraw</code>, <code>TransactionDeposit</code> это классы, которые расширяют наши сервисы. Их основная функция - добавить транзакцию в базу после вызова одного из сервисов: <code>AccountWithdrawService</code>, <code>AccountDepositService</code>.</p>

<p><code>TransactionDepositCLI</code>, <code>AccountBasicCLI</code>, <code>TransactionWithdrawCLI</code> - это классы, которые работают исключительно с чтением данных от пользователя. Они хранят различные интерфейсы <code>WithdrawDepositCLIUI</code>, <code>CreateAccountOperationUI</code> интерфейсы, в реализации которых от пользователя ожидается ввод данных. Конечная реализации ввода имеется в классе <code>MyCLI</code>. Здесь использован принцип <code>ISP</code>. Мы разделили <code>CLIUI</code> на мелкие интерфейсы, где каждый из <code>TransactionDepositCLI</code>, <code>AccountBasicCLI</code>, <code>TransactionWithdrawCLI</code> использует только то, что ему нужно.</p>

<p>К примеру, нам нужно снять с депозита <code>20$</code>:</p>

<ul>
	<li>Вызывается метод <code>TransactionWithdrawCLI</code> - <code>withdrawMoney()</code>.</li>
	<li>Метод <code>withdrawMoney()</code> вызывает <code>requestClientAccountNumber()</code> у <code>WithdrawDepositOperationCLIUI</code>.</li>
	<li>После ввода номера, проверяется наличие данного <code>AccountWithdraw</code> счета (мы не можем снять из <code>Fixed</code> счета).</li>
	<li>Вызывается метод <code>getClientWithdrawAccount()</code>.</li>
	<li>Метод <code>withdrawMoney()</code> вызывает <code>requestClientAmount()</code> у <code>WithdrawDepositOperationCLIUI</code>.</li>
	<li>Вызывается метод <code>execute()</code> у <code>transactionWithdraw</code>.</li>
	<li>Метод <code>execute</code> вызывает метод <code>withdraw</code> у сервиса <code>accountWithdrawService</code>.</li>
	<li>Вызывается метод <code>addTransaction()</code> у <code>transactionDAO</code>.</li>
	<li>Выводится сообщение об успешном снятии или ошибка.</li>
</ul>

<p>Что касается инициализации всех этих классов, тебе будет нужно использовать автоматическую конфигурацию. Для этого ты можешь дополнительно изучить <code>Spring</code> аннотации.</p>

<p>Там где это возможно, используй <code>lombok</code>, так код станет более читабельным, скрывая очевидные операции.</p>

<p>С наилучшими пожеланиями, Джон Доу</p>

<h2>Задача:</h2>

<p>Конечная программа должна выполнять традиционные банковские операции.</p>

<p>К основным операциям, выполняемым пользователем со счетами в данной версии относятся:</p>

<ol>
	<li>Получение информации о всех счетах</li>
	<li>Создание счета</li>
	<li>Пополнение счета (debit)</li>
	<li>Снятие денег со счета (withdraw)</li>
</ol>

<p>Ключевые факторы при проверке проекта:</p>

<ul>
	<li>Воспользуйтесь ссылкой на <a href="https://classroom.github.com/a/85eCW3uN" rel="noopener noreferrer nofollow">GitHub Classroom</a> для создания репозитория для этого задания. После выполнения задания пришлите ссылку на репозиторий ревьюеру.</li>
	<li>В репозитории должен быть <code>README.md</code> с описанием запуска программы.</li>
	<li>Классы, интерфейсы и связи должны соответствовать <code>UML</code> диаграмме.</li>
	<li>В проекте не должна происходить инициализация через <code>xml</code>, используй Аннотации.</li>
	<li>Программа должна работать в командной строке.</li>
	<li>Пользователь может создать счет. К счетам относятся:
	<ol>
		<li>Текущий счет (CHECKING)</li>
		<li>Сберегательный счет (SAVING)</li>
		<li>Фиксированный счет (Fixed)</li>
	</ol>
	</li>
	<li>При вызове команды для создания счета, программа должна вывести результат выполнения команды (успешно, неуспешно).</li>
	<li>Пользователь может получить список всех счетов.</li>
	<li>Каждый счет имеет уникальный <code>AccountID</code>.</li>
	<li>Нумерация счетов должна начинаться с 1.</li>
	<li>Сумма на счете не может быть меньше нуля.</li>
	<li>Снимать деньги можно только с <code>Checking</code> и <code>Saving</code> счетов. Формат для <code>AccountID</code> счета:</li>
</ul>

<pre>String accountNumber = String.format("%03d%06d", 1, accountID);</pre>

<p>Пример работы:</p>

<p><img alt="" height="693" name="image.png" src="https://ucarecdn.com/b25038ef-e370-46f7-aacb-3357cffa4138/" width="681"></p>

<p>Пример точки входа программы:</p>

<pre><code>package com.example.demo;

import com.example.demo.delivery.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class DemoApplication implements CommandLineRunner {
	@Autowired
	private ApplicationContext context;

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class);
	}

	@Override
	public void run(String... arg0) throws Exception {
		boolean running = true;
		String clientID = "1";

		MyCLI myCLI = context.getBean(MyCLI.class);
		AccountBasicCLI accountBasicCLI = context.getBean(AccountBasicCLI.class);
		TransactionDepositCLI transactionDepositCLI = context.getBean(TransactionDepositCLI.class);
		TransactionWithdrawCLI transactionWithdrawCLI = context.getBean(TransactionWithdrawCLI.class);

		String helpMessage = "1 - show accounts\n2 - create account\n3 - deposit\n4 - withdraw\n5 - transfer\n6 - this message\n7 - exit\n";
        System.out.printf("Welcome to CLI bank service\n");
        System.out.printf("Enter operation number: \n");
        System.out.printf(helpMessage);
        while(running){
            switch(myCLI.getScanner().nextLine()){

                case "1":
                    accountBasicCLI.getAccounts(clientID);
                    break;

                case "2":
                    accountBasicCLI.createAccountRequest(clientID);
                    break;

                case "3":
                    transactionDepositCLI.depositMoney(clientID);
                    break;

                case "4":
                    transactionWithdrawCLI.withdrawMoney(clientID);
                    break;

                case "6":
                    System.out.printf(helpMessage);
                    break;

                case "7":
                    System.out.printf("Application Closed\n");
                    running = false;
                    break;

                default:
                    System.out.printf("Command not recognized!\n");
                    break;
            }
        }
        myCLI.getScanner().close();
	}
}</code></pre>