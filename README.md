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
### Fájlbetöltés
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
# JavaFX - GUI
### initialize
```
public void initialize() {
    fc.setInitialDirectory(new File("./"));
    fc.getExtensionFilters().add(new FileChooser.ExtensionFilter("CSV fájlok", "*.csv"));
}
```
### Fájlmegnyitás - ListView
```
@FXML private void onMegnyitasClick() {
    File fbe = fc.showOpenDialog(lsGyartok.getScene().getWindow());
    if (fbe != null) {
        betolt(fbe);
        lsGyartok.getItems().clear();
        TreeSet<String> gyartok = new TreeSet<>();
        for (Repulo repulo : repulok) gyartok.add(repulo.tipus.split(" ")[0]);
        for (String gyarto : gyartok) lsGyartok.getItems().add(gyarto);
        lsGyartok.getSelectionModel().select(0);
        //onGyartoPressed(); plusz függvény meghívása
    }
}
```
### Fájlmegnyitás - ComboBox
```
@FXML private void onMegnyitasClick() {
    File fbe = fc.showOpenDialog(lsLista.getScene().getWindow());
    if (fbe != null) {
        filmek.clear(); cmEvek.getItems().clear(); //csak ez változik a ListView-hoz képest asszem
        betolt(fbe);
        TreeSet<Integer> evek = new TreeSet<>();
        for (Film f : filmek) evek.add(f.ev);
        for (Integer ev : evek) cmEvek.getItems().add(ev);
        cmEvek.getSelectionModel().select(0);
        //onEvekChange(); plusz függvény meghívása
    }
}
```
### Kilépés
```
@FXML private void onKilepesClick() {
    Platform.exit();
}
```
### Névjegy
```
@FXML private void onNevjegyClick() {
    Alert info = new Alert(Alert.AlertType.INFORMATION);
    info.setTitle("Névjegy");
    info.setHeaderText(null);
    info.setContentText("Repülők v1.0.0\n(C) Kandó");
    info.showAndWait();
}
```
### Fájlbetöltés
```
private void betolt(File fajl) {
    Scanner be = null;
    try {
        be = new Scanner(fajl, "utf-8");
        be.nextLine();
        while (be.hasNextLine()) filmek.add(new Film(be.nextLine()));
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (be != null) be.close();
    }
}
```
### Teljes minta
```
public class HelloController {

    @FXML private ListView<String> lsGyartok;
    @FXML private ListView<String> lsTipusok;

    private class Repulo {
        public String tipus;
        public float hossz;
        public int suly;
        public int ferohely;
        public int tank;

        public Repulo(String sor) {
            String[] s = sor.split(";");
            tipus = s[0];
            hossz = Float.parseFloat(s[1]);
            suly = Integer.parseInt(s[2]);
            ferohely = Integer.parseInt(s[3]);
            tank = Integer.parseInt(s[4]);
        }
    }

    private ArrayList<Repulo> repulok = new ArrayList<>();
    private FileChooser fc = new FileChooser();

    public void initialize() {
        //...
    }

    @FXML private void onMegnyitasClick() {
        //...
    }

    //A feladat plusz/egyéni függvénye(i)
    @FXML private void onGyartoPressed() {
        int i = lsGyartok.getSelectionModel().getSelectedIndex();
        if (i != -1) {
            String gyarto = lsGyartok.getSelectionModel().getSelectedItem();
            lsTipusok.getItems().clear();
            for (Repulo repulo : repulok) {
                if (repulo.tipus.split(" ")[0].equals(gyarto)) lsTipusok.getItems().add(repulo.tipus);
            }
        }
    }

    @FXML private void onKilepesClick() {
        Platform.exit();
    }

    @FXML private void onNevjegyClick() {
        //...
    }

    private void betolt(File fajl) {
        //...
    }
}
```
