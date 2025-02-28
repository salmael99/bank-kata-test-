# bank-kata-test-
public class Account {
    public void deposit(int amount) { }
    public void withdraw(int amount) { }
    public void printStatement() { }
}
cd /chemin/vers/votre/projet
import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.*;

public class AccountTest {
    @Test
    void should_print_statement_containing_all_transactions() {
        TransactionRepository transactionRepository = mock(TransactionRepository.class);
        StatementPrinter statementPrinter = mock(StatementPrinter.class);
        Account account = new Account(transactionRepository, statementPrinter);

        account.deposit(1000);
        account.deposit(2000);
        account.withdraw(500);
        account.printStatement();

        verify(statementPrinter).print( /* ici, on vérifiera l'affichage des transactions */ );
    }
}import java.util.ArrayList;
import java.util.List;

public class TransactionRepository {
    private final List<Transaction> transactions = new ArrayList<>();

    public void addDeposit(int amount) {
        transactions.add(new Transaction(amount));
    }

    public void addWithdrawal(int amount) {
        transactions.add(new Transaction(-amount));
    }

    public List<Transaction> allTransactions() {
        return transactions;
    }
}import java.util.List;

public class StatementPrinter {
    public void print(List<Transaction> transactions) {
        System.out.println("DATE | AMOUNT | BALANCE");
        int balance = 0;
        for (Transaction transaction : transactions) {
            balance += transaction.getAmount();
            System.out.println(transaction.getDate() + " | " + transaction.getAmount() + " | " + balance);
        }
    }
}import java.time.LocalDate;

public class Transaction {
    private final int amount;
    private final LocalDate date;

    public Transaction(int amount) {
        this.amount = amount;
        this.date = LocalDate.now(); // Pour simplifier
    }

    public int getAmount() {
        return amount;
    }

    public LocalDate getDate() {
        return date;
    }
}public class Account {
    private final TransactionRepository transactionRepository;
    private final StatementPrinter statementPrinter;

    public Account(TransactionRepository transactionRepository, StatementPrinter statementPrinter) {
        this.transactionRepository = transactionRepository;
        this.statementPrinter = statementPrinter;
    }

    public void deposit(int amount) {
        transactionRepository.addDeposit(amount);
    }

    public void withdraw(int amount) {
        transactionRepository.addWithdrawal(amount);
    }

    public void printStatement() {
        statementPrinter.print(transactionRepository.allTransactions());
    }
}
