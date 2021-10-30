# ADT Map Template
Este repositório consiste num projeto IntelliJ de suporte à lecionação dos tipos abstratos de dados na linguagem Java, no contexto da unidade curricular de Programação Avançada - ESTSetúbal.

Os exercícios solicitados são os seguintes:

##Exercícios
Este projeto contem integrada a api ADT_Graph.jar que disponibiliza uma implementação do ADT Graph.
Pretende-se manipular o ADT Graph. Para tal define-se instância um Grafo usando para vértice objectos do tipo `Local`e para arestas obejectos do tipo `Bridge`.

1. Complete o método `main`de forma a:
   1. Construir  o grafo ilustrado na figura.
   2. Mostrar todas as pontes que saiem do Local `A`
   3. Mostrar todos os locais vizinhos do Local `D`
   4. Determinar o número de pontes que partem de `C`
   5. Dado dois locais verificar se existe uma ponte que os liga diretamente.

![Graph](images/BridgesGraph.png)
 
##Exercícios 2
Relembrando o pseudocodigo da travessia em profundidade - DFS

```pascal
Algorithm: DFS(Graph,vertice_root)
BEGIN
    setAsVisited(vertice_root)
    push(s,vertice_root)
    WHILE s is not EMPTY
        BEGIN
            v <- pop(s)     
            process(v)
            FOR EACH w adjacents(v)
                IF w is not visited THEN
                    BEGIN
                        setAsVisited(w)
                        push(s,w)
                    END
        END                    
END

```
Elabore um método na classe `Main` que implemente o método DFS.

**Nota** : Use a classe `java.util.Stack` para criar a instância do stack necessária.
