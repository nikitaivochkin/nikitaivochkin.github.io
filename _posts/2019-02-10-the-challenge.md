---
title: 4. The challenge.
layout: post
category: blog
date: '2019-02-10 20:30:00'
---

Almost all of this week I spent trying to solve one very interesting task. I started studying course - "Prototypes". And the first task caused me big problems.

### About this task.
A project on this course is HTML Builder. We need to create a library describing HTML using [DSL](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B5%D0%B4%D0%BC%D0%B5%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B9_%D1%8F%D0%B7%D1%8B%D0%BA).
In the first task I must be I had to create a function that should receive a data (in this case an array of HTML tags where objects is a property of tags):
```
const data = ['html', [
  ['meta', [
    ['title', 'hello, hexlet!'],
  ]],
  ['body', { class: 'container' }, [
    ['h1', { class: 'header' }, 'html builder example'],
    ['div', [
      ['span', 'span text2'],
      ['span', 'span text3'],
    ]],
  ]],
]];
```
and return HTML in a string as usual:
```
<html>
  <meta><title>hello, hexlet!</title></meta>
  <body class="container">
    <h1 class="header">html builder example</h1>
    <div>
      <span>span text2</span>
      <span>span text3</span>
    </div>
  </body>
</html>
```
But we have one small condition - without conditional constructions. Okay. The task is understood. Somehow I thought that I could to make this task quite fast ;) It was a big delusion.
For the beginning I was watched a recommended [video](https://www.youtube.com/watch?v=us8AMJKEzZg&ab_channel=Hexlet) how this library is created and I was writing a similar code using conditional constructions.  Code worked. And then I had to do this using polymorphism.
How I understood a polymorphism is call different code for different subtypes.

In general, I was solving this task !!!4 days. Every evening after my job I was trying to solve this task (my wife was very angry that I didn't pay attention to her).
Sometimes I wanted to throw everything and to see a teacher's solution. But I haven't done it. And I made this code without conditional constructions:
```
const types = [
  { type: 'tagList', check: el => el instanceof Array },
  { type: 'tag', check: el => typeof (el) === 'string' },
];

const subTypes = [undefined, 'body', 'children', 'attrs'];
const funcForSubType = {
  undefined: el => '',
  body: el => el,
  children: el => iter(el).join(''),
  attrs: el => Object.keys(el).reduce((acc, key) => [...acc, ` ${key}="${el[key]}"`], []).join(''),
};

const getFuncForSubType = (type, element) => funcForSubType[subTypes.find(el =>
  el === element || el === type)](element);

const body = {
  type: 'body',
  check: elements => elements.find(el => typeof (el) === 'string'),
  print: el => getFuncForSubType(body.type, el),
};
const children = {
  type: 'children',
  check: elements => elements.find(el => el instanceof Array),
  print: el => getFuncForSubType(children.type, el),
};
const attrs = {
  type: 'attrs',
  check: elements =>
    elements.find(el => typeof (el) !== 'string' && !(el instanceof Array)),
  print: el => getFuncForSubType(attrs.type, el),
};
const funcDependsOnLength = [
  { type: 'one',
    check: length => length === 1,
    print: ([tagName, ...elements]) => `<${tagName}></${tagName}>` },
  { type: 'moreOne',
    check: length => length > 1,
    print: ([tagName, ...elements]) =>
      [`<${tagName}${attrs.print(attrs.check(elements))}>`,
        body.print(body.check(elements)),
        children.print(children.check(elements)),
        `</${tagName}>`].join('') },
];

const getFuncDependsOnLength = (tag, length) => funcDependsOnLength.find(el => el.check(length));
const getFuncForType = {
  tagList: data => data.map(iter),
  tag: tag => (getFuncDependsOnLength(tag, tag.length).print)(tag),
};

const iter = (data) => {
  const [firstEl] = data;
  const getType = types.find(el => el.check(firstEl)).type;
  return getFuncForType[getType](data);
};

export default data => iter(data);
```
I understand that it very bad and difficult code and it should be much easier. But I was too disappointed that I haven't made a good solution and decided to research a teacher's solution. And as usual it was a quite simple solution. I don't know correctly I understood this solution. But I think the main idea is to create root object that consist of necessary us types: "name" - name of tag, "attributes" -  properties of tag, "body" - text or "childrens" - child elements. And then using dynamic dispatch to get necessary elements! I thind I inderstood it.

### My plans
I want to finish this course and then remind last four themes and to try to make a next project. Start is Mart 25.