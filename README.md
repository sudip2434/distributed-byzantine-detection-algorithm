# Distributed-Byzantine-Fault-Detection
Wireless ad-hoc networks (WANET) with multi-hop communication are subject to a variety of faults and attacks, and detecting the source of any fault is highly important to maintain the quality of service, confidentiality, and reliability of an entire network operation. Intermediate byzantine nodes in WANET could subvert the system by altering sensitive routed information unintentionally due to many reasons such as power depletion, software bug, malware, and environmental obstacles. This thesis highlights some of the research studies done in the area of distributed fault detection (DFD) and proposes a solution to detect Byzantine behavior cooperatively. The present research will focus on designing a scalable distributed fault detection (DFD) algorithm to detect byzantine nodes who permanently try to distort or reroute information while relaying a message from one node to another, complimentary to that, a symmetric distributed cryptography scheme will be employed to continuously validates the data integrity of a routed message. 

# Installation Requirement

* Python 2.7.11

# Python Packages 

* python-igraph (http://igraph.org/python/)
* PyCrypto 2.6.1 (https://pypi.python.org/pypi/pycrypto)
* NumPy 1.9.3 (https://pypi.python.org/pypi/numpy)
* matplotlib 1.4.3 (https://pypi.python.org/pypi/matplotlib) 

# Testing the code

To test the code run sampleExperiment.py file by going to project root folder and running the following command

''' Python code
python sampleExperiment
'''
# Sample usage

''' Using DFD Library
import numpy as np
import decisionNodeWithGraph as DN
import randomNetworkGenerator as GEN
import recursiveRandomRouting as DFD
import matplotlib.pyplot as plt
import utility as util

# =============== Initialize parameters ===========
sinkLocation = 0
meshSize = 10
numClasses = 3
numMessages = 900
numberHops = 10
numByzantine = np.floor((meshSize**2)/5) # 1/5 of the entire network size
mobility = False

g = GEN.generate_random_wanet_mesh_graph(meshSize, numClasses, numByzantine, 40, 12, sinkLocation)

byzNodes, healthyNodes, byzListFormat, messageSizeComplexity, routedHopCount, energyConsum, energyConsumWithout = DFD.RecursiveRandomRouting(g, numClasses, meshSize, numberHops, sinkLocation)
        
for testCommunication in range(0, numMessages):
    if mobility:
        g = util.random_mobility(g, meshSize**2);
    byzNodes2, healthyNodes2, byzListFormat2, messageSizeComplexity2, routedHopCount2, energyConsum2, energyConsumWithout2 = DFD.RecursiveRandomRouting(g, numClasses, meshSize, numberHops, sinkLocation)
    # print "Message" + str(testCommunication) + " : " + str(g.vs[0]["msg"])
    byzNodes.extend(byzNodes2)
    healthyNodes.extend(healthyNodes2)
    byzListFormat.extend(byzListFormat2)
    routedHopCount += routedHopCount2
    messageSizeComplexity.extend(messageSizeComplexity2)
    energyConsum.extend(energyConsum2)
    energyConsumWithout.extend(energyConsumWithout2)

# no line highlights (false)
util.plot_save_graph(g, "graph.png", healthyNodes)
    
DN.result_analysis(g, healthyNodes, byzNodes, routedHopCount, messageSizeComplexity, meshSize, numClasses, energyConsum, energyConsumWithout)
'''