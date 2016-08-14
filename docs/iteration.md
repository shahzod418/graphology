# Iteration

Using a `Graph` instance, it is possible to iterate on the three following things:

* [Nodes](#nodes)
* [Edges](#edges)
* [Neighbors](#neighbors)

**Iteration methods**

For a better flexibility, the library usually enables the developer to iterate through many ways:

* Methods returning arrays.
* `forEach` methods & typical reducers (`map`, `filter`, `reduce`, `some`, `find`, `findIndex`, `every`).
* Methods creating iterators for more complex, even asynchronous iteration.
* Counting methods when the implementation is able to return a number without iteration.

**On what do we iterate?**

Note that the methods will always iterate on nodes' & edges' keys and nothing more. If you need to access to attributes during iteration, you can do so using the [attributes](attributes.md) methods.

**Difference between directed iterators**

The difference between, for instance, the `#.outEdges` & the `#.outboundEdges` iterator is that the former will only traverse directed edges while the latter will traverse both directed edges & undirected edges facing towards this direction so that all edges are traversed, and never twice.

This is very useful when performing operations such as creating a subgraph etc.

## Nodes

Those methods iterate over the graph instance's nodes.

**Examples**

```js
const graph = new Graph();

graph.addNode('Thomas');
graph.addNode('Elizabeth');

// Using the array-returning method:
graph.nodes();
>>> ['Thomas', 'Elizabeth']

// Using the `forEach` methods:
graph.forEachNode(function(node) {
  console.log(node);
});
>>> 'Thomas'
>>> 'Elizabeth'

graph.someNode(node => node === 'Thomas');
>>> true

// Using the iterator-returning method:
const iterator = graph.createNodeIterator();

for (const node of iterator)
  console.log(node);
>>> 'Thomas'
>>> 'Elizabeth'
```

**Methods**

```
#.nodes

#.forEachNode
#.mapNodes
#.filterNodes
#.reduceNodes
#.someNode
#.findNode
#.findNodeIndex
#.everyNode

#.createNodesIterator
```

**Arguments**

1. **None**: iterate over every node.

**Getting the total number of nodes**

You can simply use the [`#.order`](properties.md#order) property.

## Edges

These methods iterate over the graph instance's edges.

**Examples**

```js
const graph = new Graph();

graph.addNodesFrom(['Thomas', 'Rosaline', 'Emmett', 'Catherine', 'John', 'Daniel']);
graph.addEdgeWithKey('T->R', 'Thomas', 'Rosaline');
graph.addEdgeWithKey('T->E', 'Thomas', 'Emmett');
graph.addEdgeWithKey('C->T', 'Catherine', 'Thomas');
graph.addEdgeWithKey('R->C', 'Rosaline', 'Catherine');
graph.addEdgeWithKey('J->D1', 'John', 'Daniel');
graph.addEdgeWithKey('J->D2', 'John', 'Daniel');

// Using the array-returning methods:
graph.edges();
>>> ['T->R', 'T->E', 'C->T', 'R->C']

graph.edges('Thomas');
>>> ['T->R', 'T->E', 'C->T']

graph.edges(['Rosaline', 'Catherine']);
>>> ['T->R', 'C->T', 'R->C']

graph.edges('John', 'Daniel');
>>> ['J->D1', 'J->D2']

// Using the `forEach` methods:
graph.forEachEdge('Rosaline', function(edge) {
  console.log(edge);
});
>>> 'R->C'

// Using the iterator-returning methods:
const iterator = graph.createEdgesIterator('John', 'Daniel');

for (const edge of iterator)
  console.log(edge);
>>> 'J->D1'
>>> 'J->D2'

// Using the counting methods
graph.countEdges('Thomas');
>>> 3
graph.countOutEdges('Thomas');
>>> 2
graph.countInEdges('Thomas');
>>> 1
```

**Methods**

All the typical reducers are not listed below for time's sake but you can consider them to exist (`#.mapEdges`, for instance).

```
#.edges
#.inEdges
#.outEdges
#.inboundEdges
#.outboundEdges
#.directedEdges
#.undirectedEdges

#.forEachEdge (...)
#.forEachInEdge (...)
#.forEachOutEdge (...)
#.forEachInboundEdge (...)
#.forEachOutboundEdge (...)
#.forEachDirectedEdge (...)
#.forEachUndirectedEdge (...)

#.createEdgesIterator
#.createInEdgesIterator
#.createOutEdgesIterator
#.createInboundEdgesIterator
#.createOutboundEdgesIterator
#.createDirectedEdgesIterator
#.createUndirectedEdgesIterator

#.countEdges
#.countInEdges
#.countOutEdges
#.countInboundEdges
#.countOutboundEdges
#.countDirectedEdges
#.countUndirectedEdges
```

**Arguments**

1. **None**: iterate over every edge.
2. **Using a node's key**: will iterate over the node's relevant attached edges.
  * **node** <span class="code">any</span>: the related node's key.
3. **Using a [bunch](concept.md#bunches) of nodes**: will iterate over the union of the nodes' relevant attached edge.
  * **bunch** <span class="code">bunch</span>: bunch of related nodes.
4. **Using source & target**: will iterate over the relevant edges going from source to target.
  * **source** <span class="code">any</span>: the source node's key.
  * **target** <span class="code">any</span>: the target node's key.

**Getting the total number of edges**

You can simply use the [`#.size`](properties.md#size) property.

## Neighbors

These methods iterate over the neighbors of the given node or nodes.

**Examples**

```js
const graph = new Graph();

graph.addNodesFrom(['Thomas', 'Rosaline', 'Emmett', 'Catherine', 'John', 'Daniel']);
graph.addEdge('Thomas', 'Rosaline');
graph.addEdge('Thomas', 'Emmett');
graph.addEdge('Catherine', 'Thomas');
graph.addEdge('Rosaline', 'Catherine');
graph.addEdge('John', 'Daniel');
graph.addEdge('John', 'Daniel');

// Using the array-returning methods:
graph.neighbors('Thomas');
>>> ['Rosaline', 'Emmett', 'Catherine]

graph.neighbors(['Rosaline', 'Thomas']);
>>> ['Emmett', 'Catherine']

// Using the `forEach` methods:
graph.forEachNeighbor('Rosaline', function(node) {
  console.log(node);
});
>>> 'Thomas'
>>> 'Catherine'

// Using the iterator-returning methods:
const iterator = graph.createEdgesIterator('Rosaline');

for (const edge of iterator)
  console.log(edge);
>>> 'Thomas'
>>> 'Catherine'

// Using the counting methods
graph.countNeighbors('Thomas');
>>> 3
```

**Methods**

All the typical reducers are not listed below for time's sake but you can consider them to exist (`#.mapNeighbors`, for instance).

```
#.neighbors
#.inNeighbors
#.outNeighbors
#.inboundNeighbors
#.outboundNeighbors

#.forEachNeighbor (...)
#.forEachInNeighbor (...)
#.forEachOutNeighbor (...)
#.forEachInboundNeighbor (...)
#.forEachOutboundNeighbor (...)

#.createNeighborsIterator
#.createInNeighborsIterator
#.createOutNeighborsIterator
#.createInboundNeighborsIterator
#.createOutboundNeighborsIterator

#.countNeighbors
#.countInNeighbors
#.countOutNeighbors
#.countInboundNeighbors
#.countOutboundNeighbors
```

**Arguments**

1. **Using a node's key**: will iterate over the node's relevant neighbors.
  * **node** <span class="code">any</span>: the node's key.
2. **Using a [bunch](concept.md#bunches) of nodes**: will iterate over the union of the nodes' relevant neighbors.
  * **bunch** <span class="code">bunch</span>: bunch of related nodes.