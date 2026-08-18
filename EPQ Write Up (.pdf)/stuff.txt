import copy
import random

import matplotlib.pyplot as plt
import networkx as nx

CONST_BIGNUMBER = 10000000
savedMazes = [[[-1, -1, -1, 4, -1, 3, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, 3, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, -1, 2, -1, -1, -1],
[4, -1, -1, -1, 5, 4, -1, -1, -1, -1],
[-1, -1, -1, 5, -1, -1, -1, -1, -1, -1],
[3, 3, -1, 4, -1, -1, 3, 1, -1, -1],
[-1, -1, 2, -1, -1, 3, -1, 7, -1, 6],
[-1, -1, -1, -1, -1, 1, 7, -1, 2, 1],
[-1, -1, -1, -1, -1, -1, -1, 2, -1, -1],
[-1, -1, -1, -1, -1, -1, 6, 1, -1, -1],],
[[-1, -1, -1, -1, 1, -1, -1, 6, -1, -1, 1, -1, -1, -1, -1],
[-1, -1, -1, 3, -1, -1, -1, -1, -1, -1, 3, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, 7, -1, -1, -1, -1, 3, 4],
[-1, 3, -1, -1, -1, -1, -1, -1, 1, -1, -1, -1, -1, -1, -1],
[1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 1, 2, -1],
[-1, -1, -1, -1, -1, -1, 5, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, 5, -1, 3, -1, -1, 1, -1, -1, -1, -1],
[6, -1, -1, -1, -1, -1, 3, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, 7, 1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 1, 2, -1, -1, -1],
[1, 3, -1, -1, -1, -1, 1, -1, -1, 1, -1, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, -1, 2, -1, -1, -1, -1, -1],
[-1, -1, -1, -1, 1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 2],
[-1, -1, 3, -1, 2, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, 4, -1, -1, -1, -1, -1, -1, -1, -1, -1, 2, -1, -1]],
[[-1, -1, -1, -1, -1, -1, -1, -1, -1, 2, -1, -1, 8, -1],
[-1, -1, -1, 5, 9, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, -1, 1, 1, 2, -1, -1, -1, -1, 2, -1, -1],
[-1, 5, -1, -1, -1, -1, -1, -1, -1, -1, 6, -1, -1, 4],
[-1, 9, 1, -1, -1, -1, -1, -1, 4, -1, -1, -1, -1, -1],
[-1, -1, 1, -1, -1, -1, -1, -1, -1, 5, -1, -1, -1, -1],
[-1, -1, 2, -1, -1, -1, -1, -1, -1, -1, -1, -1, 2, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, -1, 3, 3, -1, -1, -1],
[-1, -1, -1, -1, 4, -1, -1, -1, -1, -1, -1, -1, -1, 3],
[2, -1, -1, -1, -1, 5, -1, 3, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, 6, -1, -1, -1, 3, -1, -1, -1, -1, -1, -1],
[-1, -1, 2, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[8, -1, -1, -1, -1, -1, 2, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, 4, -1, -1, -1, -1, 3, -1, -1, -1, -1, -1]],
[[-1, -1, -1, -1, 1, 3, -1, 1, -1, 2, -1, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, 3, -1, -1, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 1],
[-1, -1, -1, -1, -1, -1, 1, -1, -1, -1, 3, -1],
[1, -1, -1, -1, -1, -1, -1, -1, 3, -1, -1, -1],
[3, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, 1, -1, -1, -1, 1, -1, -1, -1, 4],
[1, -1, -1, -1, -1, -1, 1, -1, -1, -1, 8, -1],
[-1, 3, -1, -1, 3, -1, -1, -1, -1, -1, 2, -1],
[2, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, 3, -1, -1, -1, 8, 2, -1, -1, -1],
[-1, -1, 1, -1, -1, -1, 4, -1, -1, -1, -1, -1]],
[[-1, -1, -1, 8, -1, 2, 6, -1, -1, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, -1, 3],
[-1, -1, -1, -1, -1, 6, -1, -1, 5, -1],
[8, -1, -1, -1, -1, -1, -1, -1, -1, -1],
[-1, -1, -1, -1, -1, 2, -1, -1, 4, -1],
[2, -1, 6, -1, 2, -1, -1, -1, -1, -1],
[6, -1, -1, -1, -1, -1, -1, -1, 7, -1],
[-1, -1, -1, -1, -1, -1, -1, -1, -1, 1],
[-1, -1, 5, -1, 4, -1, 7, -1, -1, 5],
[-1, 3, -1, -1, -1, -1, -1, 1, 5, -1]]]
def output2DArray(array):
    #This function was primarily for outputting the adjacency matrix for testing and debugging.
    for row in array:
        print(row)

def printCopyableMatrix(graph):
    #This function was used when I was testing my dijkstra's algorithm. The online software I was using didn't accept -1 for no arc so I had to replace it with a large number (formatted for that software).
    for row in graph:
        rowString = ""
        for column in row:
            if column == -1:
                rowString = rowString + str(1000) + ","
            else:
                rowString = rowString + str(column) + ","
        #This line removes the last character from a string, which would in this case be an additional comma.
        rowString=rowString [:-1]
        print(rowString)

def convertAdjacencyMatrixToGraph(graph):
    #This function converts an adjacency matrix into the graph data type that is used by networkx. The function determines the weight of the arc between any two nodes and if it is not -1, it adds it to the graph.
    G = nx.Graph()
    for rowIndex, row in enumerate(graph):
        for columnIndex, column in enumerate(row):
            weight = graph[rowIndex][columnIndex]
            if weight != -1 and weight != CONST_BIGNUMBER:
                G.add_edge(columnIndex, rowIndex, weight=weight)
    return G

def demonstrateDijkstras(stringVisitedNodes):
    #This function is used to demonstrate the forward pass of the dijkstra's algorithm. At each stage of the algorithm, the previous fixed nodes are highlighted in yellow.
    #This for loop identifies all the nodes visited by the algorithm.
    visitedNodes = []
    for stringNode in stringVisitedNodes:
        visitedNodes.append(int(stringNode))
    #This loop stores the colour of each node depending on whether or not the node has been visited by the algorithm. Yellow is for a visited fixed node and lightblue is for a non fixed node.
    nodeColours = []
    for node in G.nodes():
        if node in visitedNodes:
            nodeColours.append("yellow")
        else:
            nodeColours.append("lightblue")
    #These are functions that are part of matplotlib.pyplot and networkx libraries used to display the graph. First the previous graph is cleared, and the new graph is then displayed with the updated visited nodes. The weight of each arc are added to this new graph.
    #I added a slight delay of 0.5 seconds so that the user can see the changes to the graph as the algorithm progresses.
    plt.clf()
    nx.draw(
        G, pos, with_labels=True,
        node_color=nodeColours,
        node_size=200
    )
    labels = nx.get_edge_attributes(G, 'weight')
    nx.draw_networkx_edge_labels(G, pos, edge_labels=labels)
    plt.pause(0.5)

def showPath(path, isFinalPath):
    #This function is used to display the shortest path found by the dijkstra's algorithm and the paths taken by the AI to find this optimal path. This function is similar to the demonstrateDijkstras function however additionally displays the arcs that are part of the shortest path.
    #Any arcs and nodes that are passed through during the training of the AI will be highlighted in green. Any arcs and nodes that are part of dijkstra's final path or that are part of the AI's final path found during testing will be highlighted in red.
    stringVisitedNodes = path.split(",")
    #This for loop identifies all the nodes visited by the algorithm.
    visitedNodes = []
    for stringNode in stringVisitedNodes:
        visitedNodes.append(int(stringNode))
    nodeColours = []
    for node in G.nodes():
        if node in visitedNodes:
            if (isFinalPath == False):
                nodeColours.append("green")
            else:
                nodeColours.append("red")
        else:
            nodeColours.append("lightblue")
    #This for loop identifies all the edges visited by the algorithm.
    edgesInPath = []
    for i in range(len(visitedNodes) - 1):
        edgesInPath.append((visitedNodes[i], visitedNodes[i + 1]))
    edgeColours = []
    for edge in G.edges():
        if edge in edgesInPath or (edge[1], edge[0]) in edgesInPath:
            if (isFinalPath == False):
                edgeColours.append("green")
            else:
                edgeColours.append("red")
        else:
            edgeColours.append("lightgray")
    #As described previously, these functions are part of matplotlib.pyplot and networkx libraries used to display the graph.
    plt.clf()
    nx.draw(
        G, pos, with_labels=True,
        node_color=nodeColours,
        edge_color=edgeColours,
        node_size=200
    )
    labels = nx.get_edge_attributes(G, 'weight')
    nx.draw_networkx_edge_labels(G, pos, edge_labels=labels)
    if (isFinalPath == False):
        plt.pause(0.01)
    else:
        plt.pause(1) 
        input()

#This class is responsible for algorithmically generating and solving a maze.
class Maze:
    def __init__(self):
        #This function is called when an instance of the class Maze is created. It will creat new public variables. These are the maze as an adjacency matrix, the shortest path found by dijkstra's algorithm, and the length of the shortest path.
        self.maze = []
        self.shortestPath = ""
        self.shortestPathLength = 0

        #To begin maze generation, a random graph is created. If the maze is not connected, validated using my personalised breadth first search algorithm, then a new random graph is created.
        g = self.generateRandomGraph()
        while self.isConnectedBFS(g) == False :
            g = self.generateRandomGraph()
        output2DArray(g)
        #The convert to maze function uses prim's algorithm to convert the graph into a minimum spanning tree. A few arcs are re-added to this minimum spanning tree to ensure that the maze generated is more difficult to solve.
        #At the same time a tempGraph is produced from the new maze generated. This is so that I can use networkx's check_planarity function to ensure that the maze is planar. This is crucial for the displaying of the maze (to minimise overlapping arcs).
        self.maze = self.convertGraphToMaze(g)

        tempGraph = convertAdjacencyMatrixToGraph(self.maze)
        while nx.check_planarity(tempGraph)[0] == False:
            self.maze = self.convertGraphToMaze(g)
            tempGraph = convertAdjacencyMatrixToGraph(self.maze)
        self.shortestPath = "NOT FOUND"
        self.shortestPathLength = -1
    def generateRandomGraph(self):
        #This function generates a random graph of size between 10 and 20 nodes. The weight of each arc is between 1 and 10 and there is a 50% chance that the arc is not present (so is -1.)
        graph = []
        numberOfNodes = random.randint(30,31)
        for i in range(numberOfNodes):
            row = []
            for j in range(numberOfNodes):
                row.append(-1)
            graph.append(row)
        for i in range(numberOfNodes):
            for j in range(numberOfNodes):
                if i != j:
                    if graph[i][j] == -1 and graph[j][i] == -1:
                        isNegative = random.randint(0,1)
                        if isNegative==1:
                            insert = random.randint(1,10)
                            graph[i][j] = insert
                            graph[j][i] = insert
        return graph
    def isConnectedBFS(self, graph):
        #This function performs the breadth first search (for the randomly generated graph) tpo determine if the graph is connected.
        #These nested for loops convert the adjacency matrix into an adjacency list.
        adjacencyList = {}
        count = 0
        for row in graph:
            tempList = []
            for index, item in enumerate(row):
                if (item != -1):
                    tempList.append(index)
            adjacencyList[count] = tempList
            count += 1
        #The queue stores the nodes in the graph that are yet to be visited. The visited list stores the nodes that have already been visited.
        queue = [0]
        visited = []
        #As long as the queue is not empty, the first node in front of the queue is removed. This node is added to the back of visited and its neighbours are added to the back of the queue. The neighbours are only added if they have not already been visited and are not already in the queue.
        while (len(queue) > 0):
            currentIndex = queue.pop(0)
            visited.append(currentIndex)
            nN=adjacencyList[currentIndex]
            for n in nN:
                if (n not in visited and n not in queue):
                    queue.append(n)
        if (len(visited) == len(graph)):
            return True
        else:
            return False
    def convertGraphToMaze(self, graph):
        #Prims algorithm is used to convert the random graph into a minimum spanning tree. Another function is used to determine which arcs have not been used in the minimum spanning tree. Finally, a few unused arcs are re-added to the minimum spanning tree to increase the difficulty of the maze.
        graph = copy.deepcopy(graph)
        result = self.primsAlgorithm(graph)
        mst = result[0]
        addedArcs = result[1]
        unusedArcs = self.identifyUnusedArcs(graph, addedArcs)
        #RE-ADDING A FEW ARCS TO THE MAZE
        self.reAddArcs(mst, unusedArcs)
        return mst
    def primsAlgorithm(self,graph):
        addedArcs = {}
        availableColumns = [0]
        count = 1
        #The while loop runs until all the nodes have been added to the minimum spanning tree. For each loop, the smallest arc from the available columns is found and added to the addedArcs dictionary.
        while (len(availableColumns) != len(graph)):
            smallestArc = CONST_BIGNUMBER
            rowOfSmallest = -1
            columnOfSmallest = -1
            for i, row in enumerate(graph):
                if i not in availableColumns:
                    for j, column in enumerate(row):
                        if j in availableColumns:
                            if graph[i][j] != -1 and graph[i][j] < smallestArc:
                                smallestArc = graph[i][j]
                                rowOfSmallest = i
                                columnOfSmallest = j
            availableColumns.append(rowOfSmallest)
            newInfo = [columnOfSmallest,rowOfSmallest, smallestArc]
            addedArcs[count] = newInfo
            count += 1
        #An empty matrix is created with the dimensions of the original graph.
        mst = []
        for i in range(len(graph)):
            row = []
            for j in range(len(graph)):
                row.append(-1)
            mst.append(row)
        #The contents of the addedArcs dictionary are inserted into the empty matrix to create adjacency matrix for the minimum spanning tree.
        for index in addedArcs:
            mst[addedArcs[index][0]][addedArcs[index][1]] = addedArcs[index][2]
            mst[addedArcs[index][1]][addedArcs[index][0]] = addedArcs[index][2]
        return [mst, addedArcs]
    def identifyUnusedArcs(self, graph, addedArcs):
        #A new list containing the unused arcs is created. The arcs are only added to the list if they have not been used in the minimum spanning tree.
        #Ignore I's and ignore J's is used to prevent the same arc being re-added twice.
        unusedArcs = []
        ignoreIs = []
        ignoreJs = []
        #The function loops through every item in the matrix (excluding items that have already been visited in the other half of the symmetrical matrix). If the item isn't in the addedArcs dictionary, it is added to the unusedArcs list.
        for i,row in enumerate(graph):
            for j, column in enumerate(row):
                if (graph[i][j] != -1):
                    if (i not in ignoreIs and j not in ignoreJs):
                        isInAddedArcs = False
                        for index in addedArcs:
                            if addedArcs[index][0] == i and addedArcs[index][1] == j:
                                isInAddedArcs = True
                            elif addedArcs[index][0] == j and addedArcs[index][1] == i:
                                isInAddedArcs = True
                        if isInAddedArcs == False:
                            unusedArcs.append([i,j,graph[i][j]])
                            ignoreIs.append(j)
                            ignoreJs.append(i)
        return unusedArcs
    def reAddArcs(self, mst, unusedArcs):
        #The function loops through the unusedArcs list. If the random number generated between 0 and 1000 is less than 125, the arc is re-added to the minimum spanning tree.
        for arc in unusedArcs:
            if (random.randint(0,1000) <125):
                mst[arc[0]][arc[1]] = arc[2]
                mst[arc[1]][arc[0]] = arc[2]
    def dijkstrasAlgorithm(self):
        #This function runs the Dijkstras algorithm on the maze.
        #The function first runs a forward pass where labels are produced for each node.
        #The labels can immediately be used to determine

        
        labels = self.performForwardPass(self.maze)      
        self.shortestPathLength = labels[len(self.maze)-1][1]
        self.shortestPath = self.performBackwardsPass(labels, self.maze)
        #self.optimalDijkstras(self.maze)

    def optimalDijkstras(self, graph):
        #Any usage of vNodes is used to display dijkstra's algorithm on the screen as the forward pass is being carried out.
        vNodes = []
        labels = {}
        for index, rowNum in enumerate(graph):
            #ORDER NUMBER, PERMANENT LABEL, TEMPORARY LABEL, PREVIOUS NODE
            labels[index] = [-1,-1,CONST_BIGNUMBER, -1]
        currentNode = 0
        vNodes.append(str(currentNode))
        #Node 0 (starting node) is given order number 1 and a permanent and temporary label of 0.
        ordering = 1
        labels[currentNode] = [ordering,0,0, -1]
        ordering += 1
        #The function loops through each node and updates temporary labels of neighbors, then selects the next node with the smallest temporary label (that isn't already fixed) to become a fixed node.
        for i in range(len(graph)):
            for rowIndex, row in enumerate(graph):
                for columnIndex, column in enumerate(row):
                    if columnIndex == currentNode:
                        if column != -1:
                            newPotentialTempLabel = column + labels[currentNode][1]
                            if (newPotentialTempLabel < labels[rowIndex][2]):
                                labels[rowIndex][2] = newPotentialTempLabel
                                labels[rowIndex][3] = currentNode
            labelIndexToBecomeNextNode = -1
            newSmallestLength =CONST_BIGNUMBER
            for labelIndex in labels:
                if labels[labelIndex][0] == -1:
                    if (labels[labelIndex][2] < newSmallestLength):
                        newSmallestLength = labels[labelIndex][2]
                        labelIndexToBecomeNextNode = labelIndex
            if labelIndexToBecomeNextNode != -1:
                labels[labelIndexToBecomeNextNode] = [ordering,labels[labelIndexToBecomeNextNode][2], labels[labelIndexToBecomeNextNode][2], labels[labelIndexToBecomeNextNode][3]]
                currentNode = labelIndexToBecomeNextNode
                vNodes.append(str(currentNode))
                ordering += 1
                demonstrateDijkstras(vNodes)
        print(labels)
        reversedOrder = []
        currentIndex = len(labels)-1
        while currentIndex != 0:
            print(currentIndex)
            reversedOrder.append(str(currentIndex))
            currentIndex = labels[currentIndex][3]
        reversedOrder.append("0")
        shortestPath = []
        for index, i in enumerate(reversedOrder):
            shortestPath.append(reversedOrder[len(reversedOrder)-1-index])
        print("NewAlgorithmShortestPath:")
        print(shortestPath)




    def performForwardPass(self, graph):
        #Any usage of vNodes is used to display dijkstra's algorithm on the screen as the forward pass is being carried out.
        vNodes = []
        labels = {}
        for index, rowNum in enumerate(graph):
            #ORDER NUMBER, PERMANENT LABEL, TEMPORARY LABEL
            labels[index] = [-1,-1,CONST_BIGNUMBER]
        currentNode = 0
        vNodes.append(str(currentNode))
        #Node 0 (starting node) is given order number 1 and a permanent and temporary label of 0.
        ordering = 1
        labels[currentNode] = [ordering,0,0]
        ordering += 1
        #The function loops through each node and updates temporary labels of neighbors, then selects the next node with the smallest temporary label (that isn't already fixed) to become a fixed node.
        for i in range(len(graph)):
            for rowIndex, row in enumerate(graph):
                for columnIndex, column in enumerate(row):
                    if columnIndex == currentNode:
                        if column != -1:
                            newPotentialTempLabel = column + labels[currentNode][1]
                            if (newPotentialTempLabel < labels[rowIndex][2]):
                                labels[rowIndex][2] = newPotentialTempLabel
            labelIndexToBecomeNextNode = -1
            newSmallestLength =CONST_BIGNUMBER
            for labelIndex in labels:
                if labels[labelIndex][0] == -1:
                    if (labels[labelIndex][2] < newSmallestLength):
                        newSmallestLength = labels[labelIndex][2]
                        labelIndexToBecomeNextNode = labelIndex
            if labelIndexToBecomeNextNode != -1:
                labels[labelIndexToBecomeNextNode] = [ordering,labels[labelIndexToBecomeNextNode][2], labels[labelIndexToBecomeNextNode][2]]
                currentNode = labelIndexToBecomeNextNode
                vNodes.append(str(currentNode))
                ordering += 1
                demonstrateDijkstras(vNodes)
        return labels    
    def performBackwardsPass(self, labels, graph):
        #This function now takes in the labels produced by the forward pass and uses them alongside the adjacency matrix to determine the shortest path.
        reversedOrder = []
        currentIndex = len(graph)-1
        reversedOrder.append(str(currentIndex))
        nodeFound = False
        #The function loops through the adjacency matrix. It then calculates the difference between the permanent label of the current node and the permanent label of all neighbours (to determine the previous node in the path).
        #If this difference is equal to the length of the arc between these nodes (found using the matrix), then the previous node is now taken as the current node. The previous node is now added to the reversedOrder list.
        while currentIndex != 0:
            for rowIndex, row in enumerate(graph):
                for columnIndex, column in enumerate(row):
                    if (nodeFound == False):
                        if columnIndex == currentIndex:
                            if (graph[rowIndex][columnIndex] != -1):
                                difference = labels[currentIndex][1] - labels[rowIndex][1] - graph[rowIndex][columnIndex]
                                if difference == 0:
                                    currentIndex = rowIndex
                                    reversedOrder.append(str(currentIndex))
                                    nodeFound = True
            nodeFound = False
        #At this point the reversedOrder list contains the order of the nodes in the shortest path in reverse.
        #This for loop reverses the order of the nodes in the reversedOrder to find the actual shortest path.
        correctOrder = ""
        for index,i in enumerate(reversedOrder):
            if  index == 0:
                correctOrder = str(reversedOrder[len(reversedOrder)-1-index])
            else:
                correctOrder = correctOrder + "," +  str(reversedOrder[len(reversedOrder)-1-index])
        return correctOrder

class AIMazeSolver:
    #Generally I would advise to avoid modifying the constants as it leads to unexpected results. For instance, if the learning rate is too high (more than 0.5) the Q function will not converge to a solution.
    #Adjustable constants
    CONST_EPSILONDECAYRATE = 0.995
    CONST_LEARNINGRATE = 0.1
    CONST_DISCOUNTFACTOR = 0.9
    #End of adjustable constants
    def __init__(self, mazeToSolve):
        #This function is called when an instance of the class Maze is created. It will create new public variables. These are the tabular Q function, the path found by the Q function, and the adjacency matrix of the maze to solve.
        self.tabularQFunction = []
        self.pathFound = ""
        self.mazeToSolve = mazeToSolve
    def getNeighbours(self, graph, index):
        #This function is used to find the neighbours of a node in the graph using the matrix.
        neighbours = []
        for rowIndex, row in enumerate(graph):
            for columnIndex, column in enumerate(row):
                if (columnIndex == index):
                    if (column != -1):
                        neighbours.append(rowIndex)
        return neighbours
    def getNodeWithHighestQFunctionValue(self, tQF, node):
        #This function passes through the tabular Q function and returns the state action pair for the specified node with the highest Q function value.
        greatestQV = "Not found"
        potNextNode = "Not found"
        for row in tQF:
            if row[0] == node:
                if greatestQV == "Not found":
                    potNextNode = row[1]
                    greatestQV = row[2]
                else:
                    if row[2] > greatestQV:
                        potNextNode = row[1]
                        greatestQV = row[2]
        return [greatestQV, potNextNode]
    def trainQFunction(self):
        #This function is used to train the tabular Q function for the AI.
        #Epsilon is responsible for slowing causing the AI to shift from exploration of new nodes to the exploitation of previous know paths.
        epsilon = 1
        epsilonMinimum = 0.05
        #At the beginning of the function the tabular Q function is populated with the initial values.
        for rowIndex, row in enumerate(self.mazeToSolve):
            for columnIndex,column in enumerate(row):
                if column != -1:
                    # [currentNode, nextNode, Q-value]
                    qFunctionInsert = [rowIndex, columnIndex, 0]
                    self.tabularQFunction.append(qFunctionInsert)
        #Q(s,a) = Q(s,a) + alpha*(r + gamma*max(Q(s',a')) - Q(s,a))
        count = 1
        self.pathFound = ""
        keepLooping = True
        pathHistory = []
        #Each loop in this while loop represents an episode in the training of the agent.
        while keepLooping == True:
            self.pathFound = ""
            count += 1
            currentNode = 0
            self.pathFound = str(currentNode)
            #In each episode, the AI starts at the start node. This while loop ensures that the AI keeps moving until it reaches the end node.
            while (currentNode != len(self.mazeToSolve)-1):
                nextNode = -1
                #In each episode, the AI will either perform an explore or an exploit step. In an explore step, the agent will make a completely random decision as to where it will move. In an exploit step, the agent will move to the node with the highest maximum Q function value.
                if (random.random() < epsilon):
                    #Explore step
                    neighbours = self.getNeighbours(self.mazeToSolve, currentNode)
                    nextNode = neighbours[random.randint(0,len(neighbours)-1)]
                else:
                    #Exploit step
                    nextNode = self.getNodeWithHighestQFunctionValue(self.tabularQFunction, currentNode)[1]
                #After each step, the Q function is updated so that the Q value for the current state action pair better suits the bellman equation.
                for row in self.tabularQFunction:
                    if row[0] == currentNode and row[1] == nextNode:
                        row[2] = row[2] + self.CONST_LEARNINGRATE*(-self.mazeToSolve[currentNode][nextNode] + self.CONST_DISCOUNTFACTOR*self.getNodeWithHighestQFunctionValue(self.tabularQFunction, nextNode)[0] - row[2])
                currentNode = nextNode
                self.pathFound = self.pathFound + "," + str(currentNode)
            #After each episode, the path found by the AI is displayed.
            if (count % 1 ==0):
                showPath(self.pathFound, False)
            #The AI keeps running more training episodes until the training yields the same path for 20 episodes in a row.
            if len(pathHistory) < 20:
                pathHistory.append(self.pathFound)
            else:
                pathHistory.pop(0)
                pathHistory.append(self.pathFound)
                consistent = True
                firstPath = pathHistory[0]
                for path in pathHistory:
                    if path != firstPath:
                        consistent = False
                if consistent == True:
                    keepLooping = False
            epsilon = max(epsilonMinimum,epsilon*self.CONST_EPSILONDECAYRATE)
    def testQFunction(self, newMaze):
        #This function is used to provide the AI with data for a new potential maze. The AI now uses the tabular Q function to find the shortest path to the end node. It does this by only performing exploit steps.
        #The Q-Values are not updated in this function.
        cNode = 0
        nNode = -1
        pFound = str(cNode)
        while (cNode != len(newMaze)-1):
            nNode = -1
            nNode = self.getNodeWithHighestQFunctionValue(self.tabularQFunction, cNode)[1]
            cNode = nNode
            pFound = pFound + "," + str(cNode)
        showPath(pFound, True)
        return pFound
#This creates a new instance of the Maze class - new maze is randomly generated.
MazeCl = Maze()
maze = MazeCl.maze
#This maze is converted into a networkx graph to be displayed.
G = convertAdjacencyMatrixToGraph(maze)
#This positions the start and end nodes of the maze so that the start node is on the left hand side of the screen and the end node is on the right hand side.
fixed_positions = {
        0: (-1.5, 0),
        len(maze) - 1: (1.5, 0)
    }
#This is the layout of the graph I decided to use.
pos = nx.spring_layout(G, pos=fixed_positions, fixed=fixed_positions.keys())
#This is the figure size of the graph.
plt.figure(figsize=(15, 9))
#The maze is now solved using dijkstras algorithm. Once dijkstras algorithm is complete, the shortest path is displayed.
MazeCl.dijkstrasAlgorithm()
showPath(MazeCl.shortestPath, True)
#The maze is now solved using the AI.
AI = AIMazeSolver(maze)
AI.trainQFunction()
AI.testQFunction(maze)
