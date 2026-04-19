## Java (konzolos)
### Fájlbeolvasás
```
import java.io.File;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public class Adat {
        public String adat1;
        public String adat2;
        public String adat3;

        public Adat(String sor) {
            String[] s = sor.split(";");
            adat1 = s[0];
            adat2 = s[1];
            adat3 = s[2];
        }
    }

    private ArrayList<Adat> adatok = new ArrayList<>();

    public Main() {
        // --- 0. feladat ---
        betolt("szovegfajl.txt");
        System.out.printf("0) %d sor adata beolvasva\n", adatok.size());

        // --- 1. feladat ---
        [...]
    }

    private void betolt(String fajlnev) {
        Scanner be = null;
        try {
            be = new Scanner(new File(fajlnev), "UTF-8");
            while (be.hasNextLine()) {
                adatok.add(new Adat(be.nextLine()));
            }
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            if (be != null) be.close();
        }
    }

    public static void main(String[] args) {
        new Main();
    }
}
```
