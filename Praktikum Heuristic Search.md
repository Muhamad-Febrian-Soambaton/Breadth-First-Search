# Heuristic Search
Muhamad Febrian Soambaton 5311421050
## Tujuan
1. Mahasiswa dapat memahami menyelesaikan permasalahan pada game 8-puzzle
dengan menggunakan algoritma heuristic search.
## Dasar Teori
Pencarian heuristik (heuristic search) adalah pendekatan dalam ilmu komputer dan kecerdasan buatan yang digunakan untuk menemukan solusi dalam ruang pencarian yang besar atau kompleks. Pendekatan ini menggabungkan elemen-elemen dari kecerdasan buatan dan teori pencarian.

1. Ruang Pencarian (Search Space):
Ruang pencarian adalah himpunan semua kemungkinan keadaan atau solusi yang dapat dieksplorasi oleh algoritma pencarian. Ini adalah domain di mana masalah dipecahkan.

2. Keadaan (States):
Keadaan merujuk pada konfigurasi atau representasi dari suatu masalah dalam ruang pencarian. Misalnya, dalam masalah peta, keadaan bisa menjadi lokasi atau posisi dalam peta.

3. Operasi Pindah (Move Operators):
Operasi pindah adalah tindakan atau langkah yang mengubah satu keadaan menjadi keadaan lain dalam ruang pencarian. Operasi ini mendefinisikan bagaimana kita bisa bergerak dari satu keadaan ke keadaan lain.

4. Keadaan Awal (Initial State):
Keadaan awal adalah titik awal dalam ruang pencarian, dari mana pencarian dimulai. Ini adalah representasi keadaan masalah awal.

5. Keadaan Tujuan (Goal State):
Keadaan tujuan adalah hasil yang diinginkan dari pencarian. Pencarian dilakukan untuk mencapai keadaan tujuan.

6. Fungsi Heuristik (Heuristic Function):
Fungsi heuristik adalah fungsi yang memberikan perkiraan seberapa baik atau buruknya suatu keadaan dalam mencapai keadaan tujuan. Fungsi ini digunakan untuk mengarahkan pencarian dengan cara memprioritaskan keadaan yang lebih mungkin menuju tujuan.

## Code
Code untuk EightPuzzleSerach
      package JavaApplivation2;
      import java.util.Vector;
      
      class Node {
          int[] state = new int[9];
          int cost;
          Node parent = null;
          Vector<Node> successors = new Vector<Node>();
      
          Node(int s[], Node parent) {
              this.parent = parent;
              for (int i = 0; i < 9; i++)
                  state[i] = s[i];
          }
      
          public String toString() {
              String s = "";
              for (int i = 0; i < 9; i++) {
                  s = s + state[i] + " ";
              }
              return s;
          }
      
          public boolean equals(Object node) {
              Node n = (Node) node;
              boolean result = true;
              for (int i = 0; i < 9; i++) {
                  if (n.state[i] != state[i])
                      result = false;
              }
              return result;
          }
      
          Vector<Node> getPath(Vector<Node> v) {
          v.insertElementAt(this, 0);
          if (parent != null) v = parent.getPath(v);
          return v;
        
          }
      
          Vector<Node> getPath() {
              return getPath(new Vector<Node>());
          }
      }
      
      class EightPuzzleSearch {
          EightPuzzleSpace space = new EightPuzzleSpace();
          Vector<Node> open = new Vector<Node>();
          Vector<Node> closed = new Vector<Node>();
      
          int h1Cost(Node node) {
              int cost = 0;
              for (int i = 0; i < node.state.length; i++) {
                  if (node.state[i] != i)
                      cost++;
              }
              return cost;
          }
      
          int h2Cost(Node node) {
              int cost = 0;
              int state[] = node.state;
              for (int i = 0; i < state.length; i++) {
                  int v0 = i, v1 = state[i];
                  /* tidak menghitung ubin yang kosong */
                  if (v1 == 0)
                      continue;
                  int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
                  int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
                  cost += c;
              }
              return cost;
          }
      
          /* boleh diubah dengan memakai heuristic h1 atau h2 */
          int hCost(Node node) {
              return h2Cost(node);
          }
      
          Node getBestNode(Vector nodes) {
          int index = 0, minCost = Integer.MAX_VALUE;
          for (int i = 0; i < nodes.size(); i++) {
          Node node = (Node)nodes.elementAt(i);
          if (node.cost < minCost) {
          minCost = node.cost;
          index = i; } }
          Node bestNode = (Node)nodes.remove(index);
          return(bestNode);
          }
      
          int getPreviousCost(Node node) {
              int i = open.indexOf(node);
              int cost = Integer.MAX_VALUE;
              if (i != -1) {
                  cost = open.get(i).cost;
              } else {
                  i = closed.indexOf(node);
                  if (i != -1)
                      cost = closed.get(i).cost;
              }
              return (cost);
          }
      
          void printPath(Vector path) {
              for (int i = 0; i < path.size(); i++) {
                  System.out.print(" " + path.elementAt(i) + "\n");
              }
          }
      
          void run() {
              Node root = space.getRoot();
              Node goal = space.getGoal();
              Node solution = null;
              open.add(root);
              System.out.print("\nRoot: " + root + "\n\n");
              while (open.size() > 0) {
                  Node node = getBestNode(open);
                  int pathLength = node.getPath().size();
                  closed.add(node);
                  if (node.equals(goal)) {
                      solution = node;
                      break;
                  }
                  Vector<Node> successors = space.getSuccessors(node);
                  for (int i = 0; i < successors.size(); i++) {
                      Node successor = successors.get(i);
                      int cost = hCost(successor) + pathLength + 1;
                      int previousCost;
                      previousCost = getPreviousCost(successor);
                      boolean inClosed;
                      inClosed = closed.contains(successor);
                      boolean inOpen = open.contains(successor);
                      if (!(inClosed || inOpen) || cost < previousCost) {
                          if (inClosed)
                              closed.remove(successor);
                          if (!inOpen)
                              open.add(successor);
                          successor.cost = cost;
                          successor.parent = node;
                      }
                  }
              }
              // new TreePrint(getTree(root));
              if (solution != null) {
                  Vector path = solution.getPath();
                  System.out.print("\nSolution found\n");
                  printPath(path);
              }
          }
      
          public static void main(String args[]) {
          // melakukan pencarian
         
          new EightPuzzleSearch().run();
          }
      }

Code untuk EightPuzzleSpace

      package JavaApplivation2;
      import java.util.Vector;
      
      class Node {
          int[] state = new int[9];
          int cost;
          Node parent = null;
          Vector<Node> successors = new Vector<Node>();
      
          Node(int s[], Node parent) {
              this.parent = parent;
              for (int i = 0; i < 9; i++)
                  state[i] = s[i];
          }
      
          public Node() {
          }
      
          public String toString() {
              String s = "";
              for (int i = 0; i < 9; i++) {
                  s = s + state[i] + " ";
              }
              return s;
          }
      
          public boolean equals(Object node) {
              Node n = (Node) node;
              boolean result = true;
              for (int i = 0; i < 9; i++) {
                  if (n.state[i] != state[i])
                      result = false;
              }
              return result;
          }
      
          Vector<Node> getPath(Vector<Node> v) {
              v.insertElementAt(this, 0);
              if (parent != null)
                  v = parent.getPath(v);
              return v;
          }
      
          Vector<Node> getPath() {
              return getPath(new Vector<Node>());
          }
          public static void main(String args[]) {
              new Node().run();
          }
      
          private void run() {
          }
      }
      
      class EightPuzzleSpace {
          Node getRoot() {
              int initialState[] ={1, 2, 3, 4, 5, 6, 7, 8, 0};
              return new Node(initialState, null);
          }
      
          Node getGoal() {
              int goalState[] = {1, 2, 3, 4, 0, 5, 6, 7, 8};
              return new Node(goalState, null);
          }
      
          Vector<Node> getSuccessors(Node parent) {
              Vector<Node> successors = new Vector<Node>();
              for (int r = 0; r < 3; r++) {
                  for (int c = 0; c < 3; c++) {
                      if (parent.state[(r * 3) + c] == 0) {
                          if (r > 0) {
                              successors.add(transformState(r - 1, c, r, c, parent));
                          }
                          if (r < 2) {
                              successors.add(transformState(r + 1, c, r, c, parent));
                          }
                          if (c > 0) {
                              successors.add(transformState(r, c - 1, r, c, parent));
                          }
                          if (c < 2) {
                              successors.add(transformState(r, c + 1, r, c, parent));
                          }
                      }
                  }
              }
              parent.successors = successors;
              return successors;
          }
      
          Node transformState(int r0, int c0, int r1, int c1, Node parent) {
              int[] s = parent.state;
              int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
              newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
              newState[(r0 * 3) + c0] = 0;
              return new Node(newState, parent);
          }
          
          public static void main(String args[]) {
              new EightPuzzleSpace().run();
          }
      
          private void run() {
          }
      
      
          
          
      }
      
      class EightPuzzleSearch {
          EightPuzzleSpace space = new EightPuzzleSpace();
          Vector<Node> open = new Vector<Node>();
          Vector<Node> closed = new Vector<Node>();
      
          int h1Cost(Node node) {
              int cost = 0;
              for (int i = 0; i < node.state.length; i++) {
              if (node.state[i] != i) cost++; }
              return cost;
              }
      
          int h2Cost(Node node) {
              int cost = 0;
              int state[] = node.state;
              for (int i = 0; i < state.length; i++) {
                  int v0 = i, v1 = state[i];
                  if (v1 == 0)
                      continue;
                  int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
                  int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
                  cost += c;
              }
              return cost;
          }
      /*boleh diubah dengan memakai heuristic h1 atau h2 */
          int hCost(Node node) {
          return h1Cost(node);
          }
          Node getBestNode(Vector nodes) {
              int index = 0, minCost = Integer.MAX_VALUE;
              for (int i = 0; i < nodes.size(); i++) {
                  Node node = (Node)nodes.elementAt(i);
                  if (node.cost < minCost) {
                      minCost = node.cost;
                      index = i;
                  }
              }
              Node bestNode = (Node)nodes.remove(index);
              return bestNode;
          }
      
          int getPreviousCost(Node node) {
              int i = open.indexOf(node);
              int cost = Integer.MAX_VALUE;
              if (i != -1) {
                  cost = open.get(i).cost;
              } else {
                  i = closed.indexOf(node);
                  if (i != -1)
                      cost = closed.get(i).cost;
              }
              return cost;
          }
      
          void printPath(Vector path) {
              for (int i = 0; i < path.size(); i++) {
                  System.out.print(" " + path.elementAt(i) + "\n");
              }
          }
      
          void run() {
              Node root = space.getRoot();
              Node goal = space.getGoal();
              Node solution = null;
              open.add(root);
              System.out.print("\nRoot: " + root + "\n\n");
              while (open.size() > 0) {
                  Node node = getBestNode(open);
                  int pathLength = node.getPath().size();
                  closed.add(node);
                  if (node.equals(goal)) {
                      solution = node;
                      break;
                  }
                  Vector<Node> successors = space.getSuccessors(node);
                  for (int i = 0; i < successors.size(); i++) {
                      Node successor = successors.get(i);
                      int cost = hCost(successor) + pathLength + 1;
                      int previousCost;
                      previousCost = getPreviousCost(successor);
                      boolean inClosed;
                      inClosed = closed.contains(successor);
                      boolean inOpen = open.contains(successor);
                      if (!(inClosed || inOpen) || cost < previousCost) {
                          if (inClosed)
                              closed.remove(successor);
                          if (!inOpen)
                              open.add(successor);
                          successor.cost = cost;
                          successor.parent = node;
                      }
                  }
              }
              if (solution != null) {
                  Vector path = solution.getPath();
                  System.out.print("\nSolution found\n");
                  printPath(path);
              }
          }
      
          public static void main(String args[]) {
              new EightPuzzleSearch().run();
          }
      }
## Tugas

1. Pelajari class EightPuzzleSearch, EightPuzzleSpace, dan Node.
2. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya
Gambar 5.8. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal
state. Analisa dan bedakan dengan solusi pada point 1.
3. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya
Gambar 5.9. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai
goal state. Analisa dan bedakan dengan solusi pada point 1 dan 2.
4. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya
Gambar 5.10. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai
goal state. Analisa dan bedakan dengan solusi pada point 1, 2, dan 3.
5. Ubahlah initial dan goal state dari program dan class-class di atas sehingga bentuk initial dan
goal statenya Gambar 5.11. Kemudian tentukan langkah-langkah mana saja sehingga
puzzlenya mencapai goal state.

## Jawaban

1. EightPuzzleSearch adalah untuk menjalankan algoritma heuristic search, EightPuzzleSpace adalah tempat untuk membuat ruang puzzle, node digunakan untuk membuat node dalam puzzle.
2. Dalam kode Eight Puzzle 5.8 tidak ada solusi yang di temukan hal ini dapat di karenakan beberapa kondisi seperti initial state yang infeasible sehingga tidak ada pola yang cocok serta dapat juga di karenakan fungsi heuristic yang di gunakan tidak mirip dengan fungsi aslinya. langkah-langkah untuk mencapai goal state dari initial state ditemukan di dalam class EightPuzzleSearch. Class EightPuzzleSearch mencari solusi menggunakan algoritma pencarian A* dengan pilihan penggunaan heuristic h1 atau h2. Berikut adalah langkah-langkah untuk mencapai goal state:

            1. Inisialisasi initial state dan goal state.
            2. Membuat node awal (root) dengan initial state.
            3. Memasukkan node awal ke dalam daftar open.
            4. Selama daftar open tidak kosong, lakukan langkah-langkah berikut:
                     a. Pilih node dengan biaya terkecil (biaya yang dihitung berdasarkan heuristic dan panjang path).
                     b. Tambahkan node terpilih ke dalam daftar closed untuk menghindari pengulangan.
                     c. Jika node terpilih sama dengan goal state, maka solusi ditemukan, dan proses berakhir.
                     d. Dapatkan semua node penerus dari node terpilih.
                     e. Untuk setiap node penerus, hitung biaya baru berdasarkan heuristic, path length, dan biaya sebelumnya.
                     f. Jika node penerus belum pernah ada di daftar open atau biayanya lebih rendah, tambahkan node penerus ke daftar open dengan biaya yang diupdate.
   
Langkah-langkah ini berulang hingga solusi ditemukan atau semua kemungkinan solusi dieksplorasi. Hasil akhir akan mencetak langkah-langkah solusi pada output.
3. Pada initial satate dan goal state seperti gambar 5.9 di hasilkan output
![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/0862b61f-5db3-4d4e-ad07-d567c3d5a990)
Didapatkan output tersebut dengan 

      1. Inisialisasi: Algoritma dimulai dengan menginisialisasi keadaan awal permainan (initial_state) dan keadaan tujuan (goal_state). Ini adalah konfigurasi awal dan konfigurasi yang ingin Anda capai.
      
      2. Heuristik (Manhattan Distance): Heuristik yang digunakan dalam contoh ini adalah jarak Manhattan. Heuristik ini mengukur seberapa jauh setiap angka pada papan permainan berada dari posisinya yang benar di keadaan tujuan. Semakin kecil heuristik, semakin mendekati solusi.
      
      3. A Search*: Algoritma A* adalah algoritma pencarian yang menggabungkan biaya aktual yang diperlukan untuk mencapai suatu keadaan dengan heuristik. Algoritma ini mempertimbangkan dua faktor: biaya sejauh ini (g) dan estimasi biaya yang tersisa (h) ke tujuan.
      
      4. Open List dan Closed Set: Algoritma A* menggunakan dua struktur data: open list (daftar terbuka) dan closed set (daftar tertutup). Open list berisi keadaan yang masih harus dieksplorasi, sementara closed set berisi keadaan yang sudah dieksplorasi.
      
      5. Ekspansi Node: Algoritma A* mengambil keadaan yang memiliki nilai terkecil (g + h) dari open list. Ini adalah keadaan yang paling berjanji untuk mencapai solusi. Kemudian, ini dikeluarkan dari open list dan dimasukkan ke dalam closed set.
      
      6. Generate Penerus: Untuk keadaan saat ini, algoritma menghasilkan semua kemungkinan penerus dengan melakukan semua gerakan yang valid. Ini adalah semua keadaan yang mungkin dihasilkan dengan memindahkan kotak kosong satu langkah ke atas, bawah, kanan, atau kiri.
      
      7. Evaluasi Heuristik: Algoritma mengevaluasi heuristik untuk setiap keadaan penerus. Ini melibatkan perhitungan jarak Manhattan antara setiap angka dalam keadaan saat ini dan keadaan tujuan. Jumlah jarak Manhattan ini adalah estimasi biaya tersisa (h) untuk mencapai solusi.
      
      8. Penyortiran dan Penyimpanan: Keadaan penerus disortir berdasarkan biaya sejauh ini (g) dan heuristik biaya tersisa (h + g). Mereka kemudian dimasukkan ke dalam open list untuk dieksplorasi lebih lanjut.
      
      9. Iterasi: Algoritma berlanjut dengan mengulangi proses ini hingga menemukan solusi. Setiap iterasi, ia memilih keadaan berikutnya dari open list, mengevaluasi penerusnya, dan memutuskan langkah mana yang harus diambil untuk mencapai tujuan.
      
      10. Rekonstruksi Solusi: Setelah menemukan keadaan tujuan, algoritma A* dapat merekonstruksi solusi dengan melacak keadaan-keadaan yang telah dilewati dari tujuan ke awal menggunakan informasi parent.

4. Pada soal ini dengan initial state dan goal state 5.10 di dapatkan outuput

   ![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/28b94d95-cc45-4279-8fb6-e3682fe37259)

Sama seperti metode sebumnya didapatkan solusi dengan cara.

            1. Inisialisasi: Pada awalnya, algoritma dimulai dengan mengatur keadaan awal permainan dan keadaan tujuan. Inisialisasi ini menentukan konfigurasi permulaan dan tujuan yang ingin dicapai.
            
            2. Heuristik (Jarak Manhattan): Algoritma ini menggunakan heuristik jarak Manhattan. Heuristik ini menghitung seberapa jauh setiap angka dalam permainan berada dari posisinya yang benar dalam keadaan tujuan. Semakin kecil nilai heuristiknya, semakin mendekati solusi.
            
            3. A Search*: A* adalah sebuah algoritma pencarian yang menggabungkan biaya aktual yang telah dikeluarkan sejauh ini dengan estimasi biaya tersisa menuju tujuan. Algoritma ini mempertimbangkan dua faktor: biaya aktual (g) dan estimasi biaya tersisa (h) ke tujuan.
            
            4. Open List dan Closed Set: A* menggunakan dua struktur data, yaitu daftar terbuka (open list) dan daftar tertutup (closed set). Daftar terbuka berisi keadaan yang masih harus dieksplorasi, sedangkan daftar tertutup berisi keadaan yang sudah diperiksa.
            
            5. Ekspansi Node: A* memilih keadaan dengan nilai terkecil (g + h) dari daftar terbuka. Keadaan ini adalah yang paling menjanjikan untuk mencapai solusi. Kemudian, keadaan ini dipindahkan dari daftar terbuka ke dalam daftar tertutup.
            
            6. Generate Penerus: Algoritma menghasilkan semua kemungkinan penerus dari keadaan saat ini dengan melakukan gerakan yang valid. Ini mencakup semua keadaan yang dapat diperoleh dengan memindahkan kotak kosong satu langkah ke atas, bawah, kanan, atau kiri.
            
            7.Evaluasi Heuristik: A* mengevaluasi heuristik untuk setiap keadaan penerus. Ini melibatkan perhitungan jarak Manhattan antara setiap angka dalam keadaan saat ini dan keadaan tujuan. Hasil jarak Manhattan ini adalah estimasi biaya tersisa (h) untuk mencapai solusi.
            
            Penyortiran dan Penyimpanan: Keadaan penerus disortir berdasarkan biaya aktual (g) dan estimasi biaya tersisa (h + g), kemudian dimasukkan ke dalam daftar terbuka untuk dieksplorasi lebih lanjut.
            
            Iterasi: Algoritma A* mengulangi proses ini sampai menemukan solusi. Setiap iterasi, algoritma memilih keadaan berikutnya dari daftar terbuka, mengevaluasi penerusnya, dan menentukan langkah mana yang harus diambil untuk mencapai tujuan.
            
            8. Rekonstruksi Solusi: Setelah menemukan keadaan tujuan, algoritma A* dapat merekonstruksi solusi dengan melacak semua keadaan yang telah dilewati dari tujuan kembali ke keadaan awal menggunakan informasi parent.

5. Pada soal 5 didapatkan output
   ![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/e9d31783-5d2b-45cd-8b22-d209b5d8e604)
   Dapat dilihat bahwa tidak ada hasil yang ditemukan pada initial dan goal yang diberikan. Hal ini dapat terjadi karena.
   beberapa kondisi seperti initial state yang infeasible sehingga tidak ada pola yang cocok serta dapat juga di karenakan fungs            heuristic yang di gunakan tidak mirip dengan fungsi aslinya. langkah-langkah untuk mencapai goal state dari initial state ditemukan      di dalam class EightPuzzleSearch



