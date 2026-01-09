## Exercice 4 : SonarQube

1° Unused method parameters should be removed (typical code smell)
Fichier : Bank.java
Ligne : 141
	
    public void saveAccounts(Bank accManager) {
        ...
    }

accManager n'est pas utilisé, pour fix on peut juste le supprimer

    public void saveAccounts() {
        ...
    }

    et dans le main : 
	accManager.saveAccounts();


2° Try-with-resources should be used (java:S2093)

Many resources in Java need be closed after they have been used. If they are not, the garbage collector cannot reclaim the resources' memory, and they are still considered to be in use by the operating system. Such resources are considered to be leaked, which can lead to performance issues.

Fichier : BankAccount.java
Ligne : 169
FIX : 
    try (FileInputStream fis = new FileInputStream(text)){
        ...
    }
    catch {
        ...
    }
    finally {
        ...
    }

Même problème dans saveAccounts()

Fichier : Bank.java
Ligne : 142

Fix :
public void saveAccounts() {
		try (FileOutputStream fos = new FileOutputStream("C:\\Users\\jay4k\\Desktop\\stuff\\Bankaccountinfo\\BankAccountinfotext.text");
				OutputStreamWriter osw = new OutputStreamWriter(fos)) {
			for (int i = 0; i < Accounts.size(); i++) {
				BankAccount tmp = Accounts.get(i);

				osw.write(tmp.convertToText(tmp));

			}
		} catch (IOException e) {
			System.out.println("Error writing to file");
		}

	}


Les issues SonarLint apparaissent principalement dans les classes avec WMC et CBO élevés, comme Bank et BankAccount.
Ces classes concentrent la complexité et les responsabilités, ce qui génère davantage de code smells (méthodes longues, complexité élevée).
À l’inverse, les classes plus petites et moins couplées déclenchent peu ou pas d’alertes.

