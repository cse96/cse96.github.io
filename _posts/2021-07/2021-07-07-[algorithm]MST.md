# MST

### 신장트리

그래프 내의 모든 정점을 포함하는 트리  
그래프의 최소 연결 부분 그래프이다.

- 최소연결: 간선의 수가 가장 적다.
- N개의 정점을 가지는 그래프의 최소 간선의 수는 (N-1)개이고, (N-1)개의 간선으로 연결되어 있으면 필연적으로 트리형태가 되고 이것이 바로 신장트리가 된다.

DFS,BFS탐색도중 사용된 간선만 모으면 신장트리가 된다.

### 최소 신장 트리

신장트리중에서 사용된 간선들의 가중치 합이 최소인 트리

#### MST의 특징

- 1.간선의 가중치의 합이 최소이다
- 2.N개의 정점을 가지는 그래프에 대해 반드시 (N-1)개의 간선만을 사용해야한다.
- 3.사이클이 포함되어서는 안된다.

Kruskal은 greedy방식으로 최적해답을 구하는 간선중심 알고리즘  
Prim은 임의의 시작정점부터 출발하여 단계적으로 확장하는 노드중심 알고리즘이다.

```
package study.May;

import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

/*간선을 하나씩 선택해서 MST를 찾는 알고리즘
1. 최초, 모든 간선을 가중치에 따라 오름차순 정렬
2. 가중치가 낮은 간선부터 선택하면서 트리를 만들어감
 -(사이클이 생기면 다음으로 가중치가 낮은간선 선택)
3.n-1개의 간선이 선택될때까지 2.반복
 */

/*
입력은 오픈북 보고 가져왔습니다.
입력 값
7
10
0 2 31
1 2 21
2 6 25
5 4 40
0 5 60
0 6 51
2 4 46
5 3 18
3 4 34
4 6 51

입력 값 정렬 후
5 3 18
1 2 21
2 6 25
0 2 31
3 4 34
5 4 40
2 4 46
0 6 51
4 6 51
0 5 60

선택 된 간선
5 3 18
1 2 21
2 6 25
0 2 31
3 4 34
2 4 46
 */
public class KruskalTest {
	static String input = "7\r\n" +
			"10\r\n" +
			"0 2 31\r\n" +
			"1 2 21\r\n" +
			"2 6 25\r\n" +
			"5 4 40\r\n" +
			"0 5 60\r\n" +
			"0 6 51\r\n" +
			"2 4 46\r\n" +
			"5 3 18\r\n" +
			"3 4 34\r\n" +
			"4 6 51";
	static int N; //노드(정점)의 개수
	static int E; //간선의 개수
	static PriorityQueue<Edge> pq;//간선 값을 최소정렬 우선순위큐
	static int[] parent; //union find를 위한 각 노드의 부모 노드 저장 배열
	static boolean[] visit;//방문한 노드 여부
	static int result; //결과값 저장

	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(new ByteArrayInputStream(input.getBytes())));
		N = Integer.valueOf(br.readLine());//노드 개수
		E = Integer.valueOf(br.readLine());//간선 개수

		parent = new int[N+1];//1~N번 노드 번호로 인덱스
		visit = new boolean[N+1];
		result = 0;

		pq = new PriorityQueue<Edge>();
		int from = 0, to = 0, cost = 0;
		for(int i = 0; i < E; i++) {//간선 수 만큼 저장
			StringTokenizer st = new StringTokenizer(br.readLine());
			from = Integer.valueOf(st.nextToken());//시작정점
			to = Integer.valueOf(st.nextToken());  //끝 정점
			cost = Integer.valueOf(st.nextToken());//비용
			pq.add(new Edge(from,to,cost));//우선순위 큐에 저장 & 비용순으로 정렬
		}//입력 끝

		for(int i = 1; i <= N; i++) {
			parent[i] = i;
		}//union-find의 초기화(자기 자신의 부모노드는 자기 자신)

		for(int i = 0; i < E; i++) {//모든 간선에 대해
			Edge minedge = pq.poll();// 우선순위 큐에서 최소비용간선 꺼냄

			int start = minedge.s;
			int end = minedge.e;
			//간선 선택시 사이클 여부 판단을 위해 부모노드를 찾는다
			int a = find(start);
			int b = find(end);
			if(a == b) continue; //부모노드가 같으면 넘어감

			union(start,end);//두개의 부모노드가 다르면 한쪽의 부모노드를 다른 한쪽의 부모노드로 설정
			result += minedge.v;//선택된 간선이므로 간선비용을 더함
			System.out.println("선택된간선:" + start + " " + end + " " + minedge.v);
		}

		System.out.println("최종 비용:" + result);
	}//main

	private static void union(int a, int b) {//union에서는 부모노드가 다른경우만 들어옴
		int aRoot = find(a);
		int bRoot = find(b);
		if(aRoot != bRoot) {
			parent[aRoot] = b;
		}
		else {
			return;
		}
	}

	private static int find(int a) {
		if(a==parent[a]) return a; //초기화된 상태면 자신을 리턴
		parent[a] = find(parent[a]);//find할 때마다 부모는 최상위 부모로 설정

		return parent[a];
	}

	static class Edge implements Comparable<Edge>{
		int s;//시작 정점
		int e;//끝 정점
		int v;//비용

		public Edge(int s, int e, int v) {
			super();
			this.s = s;
			this.e = e;
			this.v = v;
		}

		@Override
		public int compareTo(Edge o) {
			// TODO Auto-generated method stub
			return this.v - o.v;
		}

	}
}

```

```
package study.May;

import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class PrimTest {
/*
 * 임의의 정점을 하나 선택해서 시작
 * 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점 선택
 * 모든 정점이 선택될때까지 위 과정 반복
 * 서로소인 2개의 집합 정보 유지
 * 트리 정점-MST만들기 위해 선택된 정점
 * 비트리 정점-선택되지 않은 정점들
 */
	/*
	입력은 오픈북 보고 가져왔습니다.
	입력 값
	7
	10
	0 2 31
	1 2 21
	2 6 25
	5 4 40
	0 5 60
	0 6 51
	2 4 46
	5 3 18
	3 4 34
	4 6 51

	입력 값 정렬 후
	5 3 18
	1 2 21
	2 6 25
	0 2 31
	3 4 34
	5 4 40
	2 4 46
	0 6 51
	4 6 51
	0 5 60

	선택 된 간선
	5 3 18
	1 2 21
	2 6 25
	0 2 31
	3 4 34
	2 4 46
	 */

	static String input = "7\r\n" +
			"10\r\n" +
			"0 2 31\r\n" +
			"1 2 21\r\n" +
			"2 6 25\r\n" +
			"5 4 40\r\n" +
			"0 5 60\r\n" +
			"0 6 51\r\n" +
			"2 4 46\r\n" +
			"5 3 18\r\n" +
			"3 4 34\r\n" +
			"4 6 51";
	static int N;//정점의 수
	static int E;//간선의 수
	static int result = 0, cnt = 0;//최종 비용값result, 정점만큼 순회를 카운트위한 cnt
	static ArrayList<ArrayList<Edge>> graph;//간선리스트
	static boolean[] visit;
	static PriorityQueue<Edge> pq;

	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(new ByteArrayInputStream(input.getBytes())));
		N = Integer.valueOf(br.readLine());//노드 개수
		E = Integer.valueOf(br.readLine());//간선 개수

		pq = new PriorityQueue<Edge>();
		graph = new ArrayList<>();
		visit = new boolean[N];

		for(int i = 0; i < N; i++) {
			graph.add(new ArrayList<>());//각 정점마다 리스트 초기화 생성
		}
		StringTokenizer st;
		for(int i = 0; i < E; i++) {//간선 수 만큼 간선정보 입력
			st = new StringTokenizer(br.readLine());
			//ArrayList의 ArrayList라 0번 인덱스부터 시작(입력값도 0번정점부터시작)
			//입력값이 1부터시작이면 A,B값에 -1해주면 됩니다.
			int A = Integer.valueOf(st.nextToken());
			int B = Integer.valueOf(st.nextToken());
			int cost = Integer.valueOf(st.nextToken());

			graph.get(A).add(new Edge(B,cost));
			graph.get(B).add(new Edge(A,cost));
		}
		//임의의 정점(0)부터 시작.
		//0번정점, 거리 0(혼자이므로)
		pq.add(new Edge(0,0));
		/*
			처음에 0번정점이 poll되고 0번이 방문처리 됩니다.
			0번정점과 연결된 노드중 방문 안햇으므로 다 pq에 넣어주고 while문 시작
		*/
		while(!pq.isEmpty()) {
			Edge edge = pq.poll();
			//방문했다면 continue;
			if(visit[edge.node]) continue;
			visit[edge.node] = true;
			//선택했으므로 거리 증가
			result += edge.cost;
			System.out.println("연결된 간선: " + edge.node + " " + edge.cost);

			//그래프에 연결된 노드를 돌면서 방문하지 않으면 pq에 넣어준다.
			for(Edge next : graph.get(edge.node)) {//모든리스트에 저장된 정점들
				if(!visit[next.node]) {
					pq.add(next);
				}
			}
			//모든 정점을 순회
			if(++cnt == N) break;
		}
		System.out.println("최소비용:"+result);
	}



	static class Edge implements Comparable<Edge>{
		int node; //정점번호
		int cost; //비용


		public Edge(int node, int cost) {
			super();
			this.node = node;
			this.cost = cost;
		}


		@Override
		public int compareTo(Edge o) {
			return this.cost - o.cost;
		}

	}
}

```
