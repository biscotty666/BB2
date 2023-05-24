- [x] Root task ✅ 2023-04-17
    - [x] task 1.1 ✅ 2023-04-17
	    - [x] task 1.1.1 ✅ 2023-04-17
	    - [x] task 1.1.2 ✅ 2023-04-17
		    - [x] one more level ✅ 2023-04-17
    - [x] task 1.2 ✅ 2023-04-17
        - [x] task 1.2.1 ✅ 2023-04-17
        - [x] task 1.2.2 ✅ 2023-04-17
        - [x] task 1.2.3 ✅ 2023-04-17
    - [x] task 1.3 ✅ 2023-04-17
        - [x] task 1.3.1 ✅ 2023-04-17

```dataviewjs
function crawl(subtasks) {
  let size = subtasks.length > 0 ? 0 : 1; //if no children then a leaf with size 1
  for (let task of subtasks) {
    task["size"] = crawl(task.subtasks);
    size += task.size;
  }
  return size;
}

const tasks = dv.page("Demo.md").file.tasks[0];
tasks["size"] = crawl(tasks.subtasks);

const width = 200;
const height = 300;
const ea = ExcalidrawAutomate;
ea.reset();

function buildMindmap(subtasks, depth, offset, parentObjectID) {
  if (subtasks.length == 0) return;
  for (let task of subtasks) {
    if (depth == 1) ea.style.strokeColor = '#'+(Math.random()*0xFFFFFF<<0).toString(16).padStart(6,"0");
    task["objectID"] = ea.addText(depth*width,(task.size/2+offset)*height,task.text,{box:true})
    ea.connectObjects(parentObjectID,"right",task.objectID,"left",{startArrowHead: 'dot'});
    buildMindmap(task.subtasks, depth+1,offset,task.objectID);
    offset += task.size;
  }
}

tasks["objectID"] = ea.addText(0,(tasks.size/2)*height,tasks.text,{box:true});    
buildMindmap(tasks.subtasks, 1, 0, tasks.objectID);

ea.createSVG().then((svg)=>dv.span(svg.outerHTML));
```
