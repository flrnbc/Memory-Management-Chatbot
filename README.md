# Memory Management Chatbot

This project is my submission for the final project in the Memory Management course of the [Udacity C++ Nanodegree Program](https://www.udacity.com/course/c-plus-plus-nanodegree--nd213). The user can ask the chatbot about basic terms of the memory management in C++, for example:

<img src="images/chatbot_demo.gif"/>

The basic idea of the chatbot is to use a [Knowledge graph](https://en.wikipedia.org/wiki/Knowledge_graph). Each node contains answers/output of the chatbot. The edges are labelled with keywords. The edge, whose keywords are closest to the user's query (with respect to the [Levensthein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)), determines the next node (child node). In this way, the chatbot traverses the knowledge graph until a node with no child nodes is reached. Then the dialog starts again from the so-called root node which typically contains a greeting of the chatbot to the user. 

## Dependencies 
* cmake >= 3.11
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* wxWidgets >= 3.0
  * Linux: `sudo apt-get install libwxgtk3.0-dev libwxgtk3.0-0v5`. If you are facing unmet dependency issues, refer to the [official page](https://wiki.codelite.org/pmwiki.php/Main/WxWidgets30Binaries#toc2) for installing the unmet dependencies.
  * Mac: There is a [homebrew installation available](https://formulae.brew.sh/formula/wxmac).
  * Installation instructions can be found [here](https://wiki.wxwidgets.org/Install). Some version numbers may need to be changed in instructions to install v3.0 or greater.

## Basic Build Instructions
1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./membot`.


## Implementation
As described above, the basic idea of the chatbot is to traverse a knowledge graph according to the user's queries. This is mainly achieved through five classes and their most important attributes: 

* _GraphEdge_ 
    - **_keywords**: basic terms of memory management in C++.

* _GraphNode_ 
    - **_answers**: determined by keywords of the corresponding edges. 
    - **_childEdges** and *_parentEdges*: the outgoing and ingoing edges of a node respectively.
    - *_chatBot*: instance of the ChatBot class ('current position of the chatbot').

* _ChatBotPanelDialog_ to display the chatbot output and receiving the user's input (mainly based on wxWidgets).
    - **_chatLogic** instance of _ChatLogic_ below. 

* _ChatLogic_ intermediates between the panel and chatbot. 
    - **_nodes** of the knowledge graph.
    - *_currentNode* of the chatbot.
    - *_panelDialog*: keeps track of the corresponding chatbot panel.
    - *_chatBot*: the actual chatbot (see below).

* _ChatBot_
    - **_image**: displayed as the avatar of the chatbot.
    - *_currentNode* of chatbot in the knowledge graph. 
    - *_rootNode*: root of the knowledge graph (containing a greeting to the user)
    - *_chatLogic* handls the interface between panel and chatbot.

 Here the bold attributes are owned by the corresponding class, i.e. they are [unique pointers](https://en.cppreference.com/w/cpp/memory/unique_ptr) to a class instance (on the heap). For example, it makes sense that an object of ChatLgoic owns all the nodes of the knowledge graph. In contrast, the current node is not owned but moved (using _move semantics_) when the chatbot does.

The following shows an overview of the classes in this project (provided by Udacity instructors):
<img src="images/udacity-memory-management-final-project-overview.png"/>


The smaller boxes inside the larger ones are methods of the corresponding classes. For example, _LoadAnswerGraphFromFile_ creates the knowledge graph from a text file. The dotted arrows show where a method maps to or to which class an attribute belongs.


## On starter code
Most part of this code has been provided by [Udacity](https://github.com/udacity/CppND-Memory-Management-Chatbot). The main challenge of the project (since it is about memory management) was to figure out the ownership of the various attributes and to adapt functions correspondingly.
