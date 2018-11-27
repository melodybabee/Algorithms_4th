# Algorithms_4th
Notes and Exercises of Algorithms_4th from Princeton University.

## Union Find

### Dynamic connectivity

- Union command: connect two objects.
- Find/ Connected query: to determin whether there is a path connecting the two objects.
- Some kinds of connections:

    - Reflexive ：p is connected to p.

    - Symmetric : p is connected to q, q is connected to p.
    
    - Transitive : p is connected to q, q is connected to r. So p is p is connected to r.

### Quick Find

- Firstly, there is an id array which is the initial status of every node. If we call union command, it will connect one with the other, then change the entry of the second node to the entry as the first one.
E.g, union(6,5) means connectiong 6 and 5, and changing 6 to 5 as entry.

- Java inplementation
   
		public class QuickFindUF{
			private int[] id;
			
			//set id of each object to itself,O(n) = N.
			public QuickFindUF(int N){
				id = new int[N];
				for(int i = 0; i < N ;++i){
					id[i] = i;
				}
			}
			
			//check whether p and q are in the same component,whether their ID entries are equal,O(n) = 1.
			public boolean connected(int p, int q){
				return id[p] == id[q];
			}
			
			//change all entries with id[p] to id[q],O(n) = N.
			public void union(int p, int q){
				int pid = id[p];
				int qid = id[q];
				for(int i = 0; i < id.length; ++i){
					if(id[i] == pid) id[i] = qid;
				}
			}
		}
		
### Quick Union
- Using the same data structure but array represents a set of trees that's called a forest. The value in the array means the parent of this current node.
- Find: check if p and q have the same root.
- To merge components containing p and q, set the id of p's root to the id of q's root.
- Java inplementation
   
		public class QuickFindUF{
			private int[] id;
			
			//set id of each object to itself.
			public QuickFindUF(int N){
				id = new int[N];
				for(int i = 0; i < N ;++i){
					id[i] = i;
				}
			}
			
			//chase parent pointers until reach root
			private int root(int i){
				while(i != id[i])
					i = id[i];
				return i;
			}
			
			//check if p and q hava same root
			public boolean connected(int p, int q){
				return root(p) == root(q);
			}
			
			//change root of p to point to root of q
			public void union(int p, int q){
				int i = root(p);
				int j = root(q);
				id[i] = j;
			}
		}
- the find operation complexity is linear.

### Weighting
- To improve the complexity of quick find and quick union.
- Make sure that the smaller tree goes down below.
