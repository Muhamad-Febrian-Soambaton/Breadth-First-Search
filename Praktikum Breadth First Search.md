# Breadth-First-Search Algorithm
Muhamad Febrian Soambaton 5311421050
## Tujuan
1. Mahasiswa mampu memahami konsep blind search dan dapat mengimplementasikan program
salah satu algoritma blind search pada kasus tree.
## Dasar Teori
Breadth first search (BFS) adalah algoritma pencarian yang dimulai dari satu node dan menyelidiki semua node yang dapat dicapai dari node tersebut sebelum menyelidiki node yang lebih jauh. BFS sering digunakan untuk menemukan jalur terpendek antara dua node dalam sebuah graph.
BFS menggunakan queue untuk menyimpan node-node yang belum dieksplorasi. Node akar dimasukkan ke dalam queue, dan kemudian node yang paling depan dalam queue dieksplorasi. Setelah node dieksplorasi, semua node yang berdekatan dengan node tersebut dimasukkan ke dalam queue. Proses ini diulangi hingga semua node dalam graph telah dieksplorasi.
## Code
Pada code di bawah "addEdge(Node n1, Node n2)" adalah method untuk menghubungkan dua buah edge sedangkan method "bfs(Node s)" adalah method untuk menentukan titik dimulai teknik pencarian menggunakan algoritma Breadth First Search.

    package JavaApplication1;
            
    public class JavaApplication1
    {  
      public static void main(String[] args) 
      {
      }         
    }

code di atas adalah main file pada code java. Untuk code algoritma BFS ditunjukkan oleh code di bawah.

    package JavaApplication1;
    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;
    
    public class AdjacencyList {
        public enum NodeColour 
        {
            WHITE, GRAY, BLACK
    }
    
    public static class Node {
        int data;
        int distance;
        Node predecessor;
        NodeColour colour;
    
    public Node(int data)
    
    {
          this.data = data;
    }
    
        public String toString() {
            return "(" + data + ",d=" + distance + ")";
        }
    }
    
    Map<Node, List<Node>> nodes;
    
    public AdjacencyList()
    {
    nodes = new HashMap<Node, List<Node>>();
    }
    
    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }
    
    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.remove();
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }
    
    public static void main(String[] args)
    
    {
        AdjacencyList graph = new AdjacencyList();
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        Node n7 = new Node(7);
        Node n8 = new Node(8);
        
        graph.addEdge(n1, n2);
        
        graph.addEdge(n2, n1);
        graph.addEdge(n2, n3);
        
        graph.addEdge(n3, n4);
        graph.addEdge(n3, n2);
        
        graph.addEdge(n4, n3);
        graph.addEdge(n4, n5);
        graph.addEdge(n4, n6);
        
        graph.addEdge(n5, n4);
        graph.addEdge(n5, n6);
        graph.addEdge(n5, n7);
        
        graph.addEdge(n6, n4);
        graph.addEdge(n6, n5);
        graph.addEdge(n6, n7);
        graph.addEdge(n6, n8);
        
        graph.addEdge(n7, n5);
        graph.addEdge(n7, n6);
        graph.addEdge(n7, n8);
        
        graph.addEdge(n8, n6);
        graph.addEdge(n8, n7);
        
        graph.bfs(n3);
      }
    }

## Tugas
1. Tentukan bagaimana algoritma BFS di atas dapat menentukan node ke 8, 6, dan 7.
2. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.5 dapat dibentuk.
Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 5.

![image](https://github.com/Muhamad-Febrian-Soambaton/Breadth-First-Search/blob/main/Screenshot%202023-10-30%20003834.png)

4. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.6 dapat dibentuk.
Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 9.

![image](https://github.com/Muhamad-Febrian-Soambaton/Breadth-First-Search/blob/main/Screenshot%202023-10-30%20003937.png)

6. Ubahlah kode program di atas sehingga bentuk tree seperti Gambar 4.7 dapat dibentuk. Kemudian
tentukan bagaimana algoritma BFS dapat menemukan node C.

![image](https://github.com/Muhamad-Febrian-Soambaton/Breadth-First-Search/blob/main/Screenshot%202023-10-30%20004003.png)


## Jawaban
1. Pada code yang diberikan di hasilkan output

   (3,d=0) (4,d=1) (2,d=1) (5,d=2) (6,d=2) (1,d=2) (7,d=3) (8,d=3)

   Hasil di atas di dapatkan di karenakan code "graph.bfs(n3);" yang berarti mulai melakukan algoritma BFS dari node 3 sehingga node 3 merupakan tingkat atau di kedalaman palinga atas maka node 3 depth 0. node ke 8, 6, dan 7 ditentukan mempunyai depth (6,d=2), (7,d=3), (8,d=3) dengan menggunakan array queue dan visited. Pertama dimulai dari node 3 disimpan dalam queue setelah sudah di datangi makan di simpan kedalam array visited lalu akan pindah ke depth ke-1 dari depth ke-0 karena tetangga 3 adalah 4 dan 2 maka node 4 dan 2 masuk ke queue ketika node 4 sudah di datangi maka node 4 di simpan dalam array visited dengan depth ke-1 setelah node 4 queue selanjutnya adalah 2 ketika dua sudah di datangi maka node 2 akan dimasukkan ke dalam array visited dengan depth 1. Dari node 2 pencarian berlanjut ke depth 2 dimulai dari tetangga 4 yaitu 3, 5, dan 6 karena 3 sudah pernah di datangi maka queue akan berisi 5 dan 6 di tambah dengan tetangga dari 2 yaitu 1 dan 3 karena 3 sudah di datangi maka queue dari tetangga 2 hanya node 1. Queue dari depth 2 adalah 5, 6, dan 1 setelah semua di datangi maka node 5, 6, dan 1 akan di masukkan kedalam array visited. Selanjutnya pencarian dilakukan di depth ke 3 dengan tetangga dari node 5 adalah 4, 6, dan 7. Tetangga dari node 6 adalah 4, 5, 7, 8. Tengga dari node 1 adalah 2 maka queue pada depth 3 adalah 7 dan 8 saja karena node lain sudah pernh di datangi ketika 7 dan 8 sudah di datangi maka node 7 dan 8 akan di masukkan kedalam array visited dengan depth 3.

2. Code di ubah menjadi

           public static void main(String[] args)
            
            {
                AdjacencyList graph = new AdjacencyList();
                Node n1 = new Node(1);
                Node n2 = new Node(2);
                Node n3 = new Node(3);
                Node n4 = new Node(4);
                Node n5 = new Node(5);
                Node n6 = new Node(6);
                Node n0 = new Node(0);
            
                
                 graph.addEdge(n0, n1);
                graph.addEdge(n0, n2);
                
                graph.addEdge(n2, n5);
                graph.addEdge(n2, n6);
                graph.addEdge(n2, n0);
            
                graph.addEdge(n1, n3);
                graph.addEdge(n1, n4);
                graph.addEdge(n1, n0);
            
                graph.addEdge(n3, n1);
            
                graph.addEdge(n4, n1);
            
                graph.addEdge(n5, n2);
            
                graph.addEdge(n6, n2);
                
                graph.bfs(n0);
              }
   Pada graf di atas node 0 menjadi titik awal algoritma pencarian BFS sesuai dengan gambar. Output dari code di atas adalah.
   
(0,d=0) (1,d=1) (2,d=1) (3,d=2) (4,d=2) (5,d=2) (6,d=2) 

dari output di atas didapatkan node 5 dengan depth 2 adalah dengan pertama node 0 masuk kedalam queue dan dimaukkan kedalam visited array. lalu lanjut ke depth 1 terdapat node 1 dan node 2 masuk kedalam queue dan setelah di lewati akan di masukkan ke visited array sete;ah di depth 1 makan akan lanjut ke depth 2 dengan tetangga 3, 4, 5, dan 6 masuk kedalam queue ketika sudah di lewati di dapatkan node 3, 4, 5, dan 6 masuk ke depth 2.

2. Code diubah menjadi.

            public static void main(String[] args)
            
            {
                AdjacencyList graph = new AdjacencyList();
                Node n1 = new Node(1);
                Node n2 = new Node(2);
                Node n3 = new Node(3);
                Node n4 = new Node(4);
                Node n5 = new Node(5);
                Node n6 = new Node(6);
                Node n7 = new Node(7);
                Node n8 = new Node(8);
                Node n9 = new Node(9);
                Node n10 = new Node(10);
                Node n11 = new Node(11);
                Node n12 = new Node(12);
            
            
            
                
                graph.addEdge(n2, n5);
                graph.addEdge(n2, n6);
                graph.addEdge(n2, n1);
            
                graph.addEdge(n1, n2);
                graph.addEdge(n1, n3);
                graph.addEdge(n1, n4);
            
                graph.addEdge(n3, n1);
            
                graph.addEdge(n4, n1);
                graph.addEdge(n4, n7);
                graph.addEdge(n4, n8);
            
                graph.addEdge(n5, n2);
                graph.addEdge(n5, n9);
                graph.addEdge(n5, n10);
            
                graph.addEdge(n6, n2);
            
                graph.addEdge(n7, n4);
                graph.addEdge(n7, n11);
                graph.addEdge(n7, n12);
            
                graph.addEdge(n8, n4);
            
                graph.addEdge(n9, n5);
            
                graph.addEdge(n10, n5);
            
                graph.addEdge(n11, n7);
                
                graph.addEdge(n12, n7);
            
                graph.bfs(n1);
              }
            }
   Code di atas mempunyai hasil

   (1,d=0) (2,d=1) (3,d=1) (4,d=1) (5,d=2) (6,d=2) (7,d=2) (8,d=2) (9,d=3) (10,d=3) (11,d=3) (12,d=3)

   node 9 didapatkan dengan depth 3 adalah dengan di mulai pada depth 0 dengan node 1 diamsukkan ke dalam queue dan setelah di lewati akan di pindah ke dalam array visited dan berlanjut pada depth selanjutnya dengan queue 2, 3, dan 4 seletal di lewati akan masuk ke dalam array visited dan terus berlanjut hingga depth ke 3 di mana 9 berada dan diteruskan sampai semua node di lewati.

3. Code di ubah menjadi.
                    
            /*
             * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
             * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
             */
            
            
            package JavaApplication1;
            import java.util.ArrayDeque;
            import java.util.ArrayList;
            import java.util.HashMap;
            import java.util.List;
            import java.util.Map;
            import java.util.Queue;
            import java.util.Set;
            
            public class AdjacencyList {
                public enum NodeColour {
                    WHITE, GRAY, BLACK
            }
            
            public static class Node {
                char data;
                int distance;
                Node predecessor;
                NodeColour colour;
            
            public Node(int data)
            
            {
                  this.data = data;
            }
            
                public String toString() {
                    return "(" + data + ",d=" + distance + ")";
                }
            }
            
            Map<Node, List<Node>> nodes;
            
            public AdjacencyList()
            {
            nodes = new HashMap<Node, List<Node>>();
            }
            
            public void addEdge(Node n1, Node n2) {
                if (nodes.containsKey(n1)) {
                    nodes.get(n1).add(n2);
                } else {
                    ArrayList<Node> list = new ArrayList<Node>();
                    list.add(n2);
                    nodes.put(n1, list);
                }
            }
            
            public void bfs(Node s) {
                Set<Node> keys = nodes.keySet();
                for (Node u : keys) {
                    if (u != s) {
                        u.colour = NodeColour.WHITE;
                        u.distance = Integer.MAX_VALUE;
                        u.predecessor = null;
                    }
                }
                s.colour = NodeColour.GRAY;
                s.distance = 0;
                s.predecessor = null;
                Queue<Node> q = new ArrayDeque<Node>();
                q.add(s);
                while (!q.isEmpty()) {
                    Node u = q.remove();
                    List<Node> adj_u = nodes.get(u);
                    if (adj_u != null) {
                        for (Node v : adj_u) {
                            if (v.colour == NodeColour.WHITE) {
                                v.colour = NodeColour.GRAY;
                                v.distance = u.distance + 1;
                                v.predecessor = u;
                                q.add(v);
                            }
                        }
                    }
                    u.colour = NodeColour.BLACK;
                    System.out.print(u + " ");
                }
            }
            
            public static void main(String[] args)
            
            {
                AdjacencyList graph = new AdjacencyList();
                Node n1 = new Node('A');
                Node n2 = new Node('B');
                Node n3 = new Node('C');
                Node n4 = new Node('D');
                Node n5 = new Node('E');
                Node n6 = new Node('F');
                Node n7 = new Node('G');
                Node n8 = new Node('H');
                Node n9 = new Node('I');
                
                graph.addEdge(n1, n2);
                graph.addEdge(n1, n4);
            
                graph.addEdge(n2, n6);
                graph.addEdge(n2, n1);
                graph.addEdge(n2, n4);
                
                graph.addEdge(n3, n4);
                graph.addEdge(n3, n5);
                
                graph.addEdge(n4, n2);
                graph.addEdge(n4, n3);
                graph.addEdge(n4, n5);
                
                
                graph.addEdge(n5, n4);
                graph.addEdge(n5, n3);
                
                
                graph.addEdge(n6, n2);
                graph.addEdge(n6, n7);
               
                
                
                graph.addEdge(n7, n6);
                graph.addEdge(n7, n9);
                
                graph.addEdge(n8, n9);
                
                graph.addEdge(n9, n8);
                
                graph.bfs(n6);
            }}

Output dari code adalah.    

(F,d=0) (B,d=1) (G,d=1) (A,d=2) (D,d=2) (I,d=2) (C,d=3) (E,d=3) (H,d=3) 

sama dengan algoritma yang sebelumnya BFS di gunakan dari inisial condition pada node F seperti gambar. proses pertama menaruh node 1 pada queue setealh di lewati akan masuk kedalam visited array dan berlanjutke depth selanjutnya. pada proses selanjutnya juga di lakukan pngantrian dan penyimpanan sampai di dapatkan node C.

## Kesimpulan

Dengan BFS dapat di lakukan algoritma pencarian yang pasti terdapat jawabnaya namun harus memiliki sumberdaya yang banyak karena besarnya memori yang di gunakan oleh algoritma BFS 
