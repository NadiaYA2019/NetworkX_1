#L. Beauguitte
#Juin 2015
#Premiers pas avec NetworkX
#Voir http://groupefmr.hypotheses.org/

# coding: utf8

import networkx as nx
import matplotlib.pyplot as plt #pour la visualisation

#lecture d'une liste de liens
#non orienté par defaut d'où l'option Digraph

G=nx.read_edgelist("liens.txt", create_using=nx.DiGraph())

#visualisation
pos=nx.spring_layout(G)   			#choix de l'algorithme
nx.draw(G, pos, with_labels=True)
plt.show()
#plt.savefig("edge_colormap.png") #export dans un format donné

#analyse

print "number of nodes : %s" % G.number_of_nodes()
print "number of edges : %s" % G.number_of_edges()

print G.in_degree()
print G.out_degree()

#graphe non orienté

G=G.to_undirected()

print "degree of node a : %s" % G.degree("a") #sommet spécifique
print "degree (undirected) %s" % G.degree()   #ensemble des sommets
print "betweenness centrality %s" % nx.betweenness_centrality(G)
print "degree (normalized) %s" % nx.degree_centrality(G)

#transformer les mesures en attributs
deg = G.degree()
nx.set_node_attributes(G, 'degree', deg)

#visualisation
pos=nx.spring_layout(G)
nx.draw(G, pos, with_labels=True)
plt.show()

#recherche des cliques
cl = list(nx.find_cliques(G))
print "cliques %s" % cl

#plus courts chemins
pathlengths=[]

print("source vertex {target:length, }")
for v in G.nodes():
    spl=nx.single_source_shortest_path_length(G,v)
    print('%s %s' % (v,spl))
    for p in spl.values():
        pathlengths.append(p)

print('')
print("average shortest path length %s" % (sum(pathlengths)/len(pathlengths)))

#distribution des plus courts chemins of path lengths 
dist={}
for p in pathlengths:
    if p in dist:
        dist[p]+=1
    else:
        dist[p]=1

print('')
print("length #paths")

print("density: %s" % nx.density(G))
print("radius: %d" % nx.radius(G))
print("diameter: %d" % nx.diameter(G))
print("eccentricity: %s" % nx.eccentricity(G))
print("center: %s" % nx.center(G))
print("periphery: %s" % nx.periphery(G))

