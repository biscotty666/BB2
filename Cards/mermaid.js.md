up:: [[Obsidian]]
tags:: #note/reference #source/documentation 
X:: 

## mermaid.js

#### Flowchart

```mermaid
graph TB
%% Nodes  
1([Start])  
2[Look for lost item]  
3{Did I find it?}  
4([Stop])  
%% Node links  
1 --> 2 --> 3 -->|Yes| 4  
3 -.->|No| 2

class 1,2,3,4 nodes
classDef default stroke: white;
classDef nodes fill:blue;
```

Git graph

```mermaid
    gitGraph
       commit
       commit
       branch develop
       commit
       commit
       commit
       checkout main
       commit
       commit

```


```mermaid
pie showdata
        title /r/obsidianmd posts by type
        "Graphs" : 85
        "Dashboards" : 14
        "Tips" : 1

classDef default text: blue;
```

```mermaid
graph LR;
    A-->B[AAA];
    B-->D;

    %% Class Definitions
    %% =================
    class A cssClass;
    classDef cssClass fill:#f9f,stroke:#333,stroke-width:4px, font-size:15px;
```

```mermaid
sequenceDiagram
Alice->>John: Hello John, how are you?
John-->>Alice: Great!
Alice-)John: See you later!

```



---
### References

[Styling a Node from docs](https://github.com/mermaidjs/mermaid-gitbook/blob/master/content/flowchart.md#styling-a-node)

