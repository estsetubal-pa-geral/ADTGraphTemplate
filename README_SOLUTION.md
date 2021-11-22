// Solution - Main 
import java.util.ArrayList;
import java.util.Collection;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Set;
import java.util.Stack;
import pt.pa.adts.graph.Edge;
import pt.pa.adts.graph.Graph;
import pt.pa.adts.graph.GraphEdgeList;
import pt.pa.adts.graph.Vertex;
import pt.pa.model.Bridge;
import pt.pa.model.Local;

public class TADGraphMain {
    public TADGraphMain() {
    }

    public static void main(String[] args) {
        Graph<Local, Bridge> graph = new GraphEdgeList();
        Vertex<Local> vA = graph.insertVertex(new Local("A"));
        Vertex<Local> vB = graph.insertVertex(new Local("B"));
        graph.insertEdge(vA, vB, new Bridge("a", 12));
        graph.insertEdge(vA, vB, new Bridge("b", 10));
        Vertex<Local> vC = graph.insertVertex(new Local("C"));
        graph.insertEdge(vA, vC, new Bridge("c", 13));
        graph.insertEdge(vA, vC, new Bridge("d", 10));
        Vertex<Local> vD = graph.insertVertex(new Local("D"));
        graph.insertEdge(vA, vD, new Bridge("e", 15));
        graph.insertEdge(vB, vD, new Bridge("f", 4));
        graph.insertEdge(vC, vD, new Bridge("g", 5));
        Vertex<Local> vX = graph.insertVertex(new Local("X"));
        graph.insertEdge(vA, vX, new Bridge("xx", 1));
        Vertex<Local> vW = graph.insertVertex(new Local("W"));
        graph.insertEdge(vW, vX, new Bridge("ww", 1));
        Vertex<Local> vZ = graph.insertVertex(new Local("Z"));
        graph.insertEdge(vW, vZ, new Bridge("zz", 1));
        graph.insertEdge(vZ, vD, new Bridge("zd", 4));
        List<Vertex<Local>> path = new ArrayList();
        double cost = minimumCostPath(vA, vD, path, graph);
        System.out.println("PATH  " + path);
        System.out.println("COST : " + cost);
    }

    public static Collection<Vertex<Local>> dfs(Vertex<Local> vertice_root, Graph<Local, Bridge> graph) {
        HashSet<Vertex<Local>> visited = new HashSet();
        Stack<Vertex<Local>> stack = new Stack();
        ArrayList<Vertex<Local>> list = new ArrayList();
        visited.add(vertice_root);
        stack.push(vertice_root);

        while(!stack.isEmpty()) {
            Vertex<Local> v = (Vertex)stack.pop();
            list.add(v);
            Iterator var6 = graph.incidentEdges(v).iterator();

            while(var6.hasNext()) {
                Edge<Bridge, Local> e = (Edge)var6.next();
                Vertex w = graph.opposite(v, e);
                if (!visited.contains(w)) {
                    stack.push(w);
                    visited.add(w);
                }
            }
        }

        return list;
    }

    public static Collection<Vertex<Local>> bfs(Vertex<Local> vertice_root, Graph<Local, Bridge> graph) {
        HashSet<Vertex<Local>> visited = new HashSet();
        Queue<Vertex<Local>> queue = new LinkedList();
        ArrayList<Vertex<Local>> list = new ArrayList();
        visited.add(vertice_root);
        queue.offer(vertice_root);

        while(!queue.isEmpty()) {
            Vertex<Local> v = (Vertex)queue.poll();
            list.add(v);
            Iterator var6 = graph.incidentEdges(v).iterator();

            while(var6.hasNext()) {
                Edge<Bridge, Local> e = (Edge)var6.next();
                Vertex w = graph.opposite(v, e);
                if (!visited.contains(w)) {
                    queue.offer(w);
                    visited.add(w);
                }
            }
        }

        return list;
    }

    private static void dijkstra(Vertex<Local> orig, Map<Vertex<Local>, Double> costs, Map<Vertex<Local>, Vertex<Local>> predecessors, Graph<Local, Bridge> graph) {
        HashSet<Vertex<Local>> unvisited = new HashSet(graph.vertices());
        Iterator var5 = graph.vertices().iterator();

        while(var5.hasNext()) {
            Vertex<Local> v = (Vertex)var5.next();
            costs.put(v, 1.7976931348623157E308D);
            predecessors.put(v, (Object)null);
        }

        costs.put(orig, 0.0D);

        while(!unvisited.isEmpty()) {
            Vertex<Local> v = findMinVertex(unvisited, costs);
            unvisited.remove(v);
            Iterator var12 = graph.incidentEdges(v).iterator();

            while(var12.hasNext()) {
                Edge<Bridge, Local> edge = (Edge)var12.next();
                Vertex<Local> w = graph.opposite(v, edge);
                double calculated_cost = (double)((Bridge)edge.element()).getCost() + (Double)costs.get(v);
                if (calculated_cost < (Double)costs.get(w)) {
                    costs.put(w, calculated_cost);
                    predecessors.put(w, v);
                }
            }
        }

    }

    private static Vertex<Local> findMinVertex(Set<Vertex<Local>> unvisited, Map<Vertex<Local>, Double> costs) {
        Vertex<Local> vMin = null;
        double min = 1.7976931348623157E308D;
        Iterator var5 = unvisited.iterator();

        while(var5.hasNext()) {
            Vertex<Local> v = (Vertex)var5.next();
            double minCost = (Double)costs.get(v);
            if (minCost < min) {
                min = minCost;
                vMin = v;
            }
        }

        return vMin;
    }

    public static double minimumCostPath(Vertex<Local> orig, Vertex<Local> dst, List<Vertex<Local>> localsPath, Graph<Local, Bridge> graph) {
        Map<Vertex<Local>, Double> costs = new HashMap();
        Map<Vertex<Local>, Vertex<Local>> predecessors = new HashMap();
        dijkstra(orig, costs, predecessors, graph);

        for(Vertex v = dst; v != orig; v = (Vertex)predecessors.get(v)) {
            localsPath.add(0, v);
        }

        localsPath.add(0, orig);
        return (Double)costs.get(dst);
    }
}
