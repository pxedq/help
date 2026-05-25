# Java - CLI
### Osztály létrehozása
```
private class Allat {
    public String fajta;
    public int magas;
    public int suly;
    public int kor;
    public Allat(String sor) {
        String[] s = sor.split(";");
        fajta = s[0];
        magas = Integer.parseInt(s[1]);
        suly = Integer.parseInt(s[2]);
        kor = Integer.parseInt(s[3]);
    }
}
```
### Fájlbeolvasás
```
public void betolt(String fajlnev) {
    Scanner be = null;
    try {
        be = new Scanner(new File(fajlnev), "utf-8");
        be.nextLine();
        while (be.hasNextLine()) allatok.add(new Allat(be.nextLine()));
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (be != null) be.close();
    }
}
```
### Teljes minta
```
public class Main {
    private class Adat {
        public String adat1;
        public int adat2;
        //...

        public Adat(String sor) {
            String[] s = sor.split(";");
            adat1 = s[0];
            adat2 = Integer.parseInt(s[1);
            //...
        }
    }

    private ArrayList<Adat> adatok = new ArrayList<>();

    public Main() {
        // --- 0. feladat ---
        betolt("szovegfajl.txt");
        System.out.printf("0) %d sor adata beolvasva\n", adatok.size());

        // --- 1. feladat ---
        //...
    }

    private void betolt(String fajlnev) {
        Scanner be = null;
        try {
            be = new Scanner(new File(fajlnev), "UTF-8");
            while (be.hasNextLine()) {
                adatok.add(new Adat(be.nextLine()));
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (be != null) be.close();
        }
    }

    public static void main(String[] args) {
        new Main();
    }
}
```
### Kiíratás külön fájlba
```
PrintWriter ki = null;
try {
    ki = new PrintWriter(new File("kicsi.csv"), "utf-8");
    for (Allat a : allatok) {
        if (a.magas < 100) ki.printf("%s;%d;%d;%d\r\n", a.fajta, a.magas, a.suly, a.kor);
    }
} catch (Exception e) {
    e.printStackTrace();
} finally {
    if (ki != null) ki.close();
}
```
### TreeMap példa
```
TreeMap<Integer, Integer> stat = new TreeMap<>();
for (Fovaros f : fovarosok) {
    int kat = f.folakos / 5_000_000;
    if (!stat.containsKey(kat)) {
        stat.put(kat, 1);
    } else {
        stat.put(kat, stat.get(kat) + 1);
    }
}
System.out.printf("5) Fővárosok népesség szerint csoportosítva (5 milló fő):\n");
for (Integer kat : stat.keySet()) {
    System.out.printf("   %,10d - %,10d: %d\n", kat*5_000_000, (kat+1)*5_000_000-1, stat.get(kat));
}
```
