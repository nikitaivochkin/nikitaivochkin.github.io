---
title: 3. Trees and something else
layout: post
category: blog
date: '2019-02-02 22:22:00'
---

This week wasn't such intensively like last month. I returned to my job and became to have less free time.

### 1. What I did.

I finished course about [trees](https://ru.hexlet.io/courses/js-trees). 

Everyone meets tree structures in their life. This maybe the file structure of your computer:

![1. file structure](/assets/images/file%20structure.png)

or the computer network in your company:

![2. computer network](/assets/images/computer%20network.png)

or HTML and much more:
 ![3. HTML1](/assets/images/HTML1.png)
 
Any hierarchy is tree.
All trees are consist of nodes. Nodes are internal (has children) and leaf (hasn't children).

In programming one of the most important tasks with trees is tree walk.
In this course I met depth-first search. The task of this algorithm is go deep into one subtree how this is possible. In the lessons of this course, it needed to use depth-first search to manipulation on nodes.

I created these functions for trees: Map, Filter and Reduce and also search function and aggregation function. 

I won't write here my code just I want to describe what I what I learned.

This course was almost without problems. I understand that it was only  ''tip of the iceberg'' in the world of trees and the most interesting waiting for me in a real work. 

But in the additional tasks I met a real difficult task. It took **!!! 3 days.**

### 2. About difficult task.

This [task](https://ru.hexlet.io/challenges/js_trees_dependencies) was about sorting dependencies.  I recommend this task to anyone who wants to break their brains;) (of course, this applies to beginners, but I think not every experienced developer will be able to solve this task). Okay. Task is we need to sort graph using the topological sort.

In the comments of task I found this [topik on habr](https://habr.com/ru/post/100953/) and to try to understood what I should do.
In the first case I had this tree:
```
const deps1 = {
  mongo: [],
  tzinfo: ['thread_safe'],
  uglifier: ['execjs'],
  execjs: ['thread_safe', 'json'],
  redis: [],
};
```

I painted a graph corresponding to the my tree (I don't know if it was correctly):
![4. graph](/assets/images/graph.png)

As a result I should get this ``` ['mongo', 'thread_safe', 'tzinfo', 'json', 'execjs', 'uglifier', 'redis'] ```.
The main problem was - how to add key after walk of values.
After two days and a big numbers of attempts I got a code which passed the tests:
```
export default (deps) => {
  const add = (currendEl, currentValue, cAcc) => {
    const findByElement = (el, acc) =>
      (deps[el] === undefined ? [...acc, el] : [...acc, ...add(el, deps[el], cAcc)]);
    return currentValue.reduce((acc, e) => findByElement(e, acc), cAcc).concat(currendEl);
  };
  return Object.keys(deps).reduce((acc, e) =>
    (deps[e].length === 0 ? [...acc, e] : add(e, deps[e], acc)), [])
    .reduce((acc, e) => (acc.includes(e) ? acc : [...acc, e]), []);
};
```
When I saw the teacher's solution I was really surprised. I didn't understand it completely. There is something to strive for. But my code worked and I was really happy! ;).

I'll definitely returned to trees because it's really important theme.
Further it's waiting for me a prototipes.