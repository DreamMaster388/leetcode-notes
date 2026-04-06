// 图的深度优先搜索问题

无论题目如何变化，图的深度优先搜索问题需要分为以下五个步骤：
1. 建图，将输入数据转换为邻接表（带权图需额外存权重）	           List<List<Integer>>（邻接表）、Map（节点映射）
2. 状态初始化，定义节点访问状态（防止重复遍历 / 检测环）	         int[] visited（0 = 未访问，1 = 访问中，2 = 已访问）或 boolean[] visited
3. 遍历启动，遍历所有节点，对未访问节点启动 DFS	                 for 循环 + dfs(u)
4. DFS，处理当前节点 → 递归 / 迭代探索邻居 → 回溯（如需要）	     递归函数或 Stack（迭代模拟）
5. 结果处理，根据问题需求返回结果（如是否有环、路径值等）	       全局变量或返回值传递

```java
class Solution {
    List<List<Integer>> graph;   // 邻接表：graph[u] 存储 u 的所有邻居
    int[] visited;                // 访问状态：0=未访问，1=访问中，2=已访问
    boolean hasCycle = false;     // 全局结果：是否有环

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // 1. 初始化邻接表
        graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        // 2. 填充边（根据题意调整方向，如 prerequisites[i] = [a, b] 表示 b→a）
        for (int[] p : prerequisites) {
            int a = p[0], b = p[1];
            graph.get(b).add(a);  // b 指向 a
        }
        // 3. 初始化访问状态
        visited = new int[numCourses];
        // 4. 遍历所有节点，启动 DFS
        for (int i = 0; i < numCourses; i++) {
            if (visited[i] == 0) {
                dfs(i);
                if (hasCycle) return false;  // 提前终止
            }
        }
        return true;
    }

    // DFS 递归函数：处理节点 u
    private void dfs(int u) {
        visited[u] = 1;  // 标记为「访问中」（递归栈中）
        // 遍历所有邻居
        for (int v : graph.get(u)) {
            if (visited[v] == 1) {
                hasCycle = true;  // 找到环（邻居在递归栈中）
                return;
            } else if (visited[v] == 0) {
                dfs(v);  // 递归探索未访问邻居
                if (hasCycle) return;
            }
        }
        visited[u] = 2;  // 标记为「已访问」（递归栈退出）
    }
}
