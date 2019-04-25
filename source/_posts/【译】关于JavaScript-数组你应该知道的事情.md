---
title: 【译】关于JavaScript 数组你应该知道的事情
tags:
  - javascript
originContent: "首先做一个粗体声明：**循环经常是无用的，并且使得代码很难阅读**。\n当谈到迭代一个数组的时候，无论你想去查找元素，排序或者任何其他的事，都有可能存在一个数组的方法供你使用。\n\n然而，尽管它们有用，但其中一些仍然不被人了解。我会努力为你展示一些有用的方法。把这篇文章当做对 JavaScript 数组方法的指引吧。\n注意： 在开始之前，不得不了解一件事：我比较偏爱函数式编程。所以我倾向于使用的方法不会直接改变原来的数组。这种方法，我避免了副作用。我不是说不应该改变数组，但至少要了解那些方法会改变，那些会有副作用。副作用导致不想要的改变，而不想要的改变带来bugs！\n了解到这里，我们可以开始正文了。\n\n<!-- more -->\n\n### 必不可少的\n\n当跟数组打交道时，有四件事你应该清楚：`map,filter,reduce`和 展开操作符。它们富有力量。\n\n#### map\n\n你可以在很多种情况下使用它。基本地，每次你需要修改数组的元素时，考虑使用 `map`。\n\n它接受一个参数：一个方法，在每一个数组元素上调用。然后返回一个**新的数组**，所以没有副作用。\n\n```javascript\nconst numbers = [1, 2, 3, 4]\n\nconst numbersPlusOne = numbers.map(n => n + 1) // 每个元素 +1\nconsole.log(numbersPlusOne) // [2, 3, 4, 5]\n```\n\n你也能创建一个新数组，用于保留对象的一个特殊属性：\n\n```javascript\nconst allActivities = [\n  { title: 'My activity', coordinates: [50.123, 3.291] },\n  { title: 'Another activity', coordinates: [1.238, 4.292] },\n  // etc.\n]\n\nconst allCoordinates = allActivities.map(activity => activity.coordinates)\nconsole.log(allCoordinates) // [[50.123, 3.291], [1.238, 4.292]]\n```\n\n所以，请记住，当你需要去**转换**数组时，考虑使用**map**。\n\n#### filter\n\n这个方法的名字在这里十分准确的：当你想去过滤数组的时候使用它。\n\n如同`map`所做，它接受一个函数作为它的唯一参数，在数组的每个元素上调用。这个方法返回一个布尔值：\n\n- `true` 如果你需要在数组中保留元素\n- `false` 如果你不想保留它\n\n接着你会得到一个带有你想要保留的元素的新数组。\n\n举个例子，你可以在数组中只保留奇数：\n\n```javascript\nconst numbers = [1, 2, 3, 4, 5, 6]\nconst oddNumbers = numbers.filter(n => n % 2 !== 0)\nconsole.log(oddNumbers) // [1, 3, 5]\n```\n\n或者你可以在数组中移除特殊的项：\n\n```javascript\nconst participants = [\n  { id: 'a3f47', username: 'john' },\n  { id: 'fek28', username: 'mary' },\n  { id: 'n3j44', username: 'sam' },\n]\n\nfunction removeParticipant(participants, id) {\n  return participants.filter(participant => participant.id !== id)\n}\n\nconsole.log(removeParticipant(participants, 'a3f47')) //  [{ id: 'fek28', username: 'mary' }, { id: 'n3j44', username: 'sam' }];\n```\n\n#### reduce\n\n个人认为是最难理解的方法。但是如果你一旦掌握它，很多疯狂的事情你都可以用它做到。\n\n基本地， `reduce` 使用有值的数组然后组合成一个新的值。它接受两个参数，一个回调方法就是我们的 **reducer** 和一个可选的初始化的值（默认是数组的第一个项）。这个 reducer 自己使用四个参数：\n\n- 累计：在你的 reducer 中累积的返回值\n- 当前数组的值\n- 当前索引\n- 当前调用 reduce 的数组\n\n大多数时候，你只需要使用前两个参数：累计值和当前值。\n\n抛开这些理论。来看看常见的一个 `reduce` 的例子。\n\n```javascript\nconst numbers = [37, 12, 28, 4, 9]\nconst total = numbers.reduce((total, n) => total + n)\nconsole.log(total) // 90\n```\n\n在第一个遍历时，这个累计值，也就是 `total`，使用了初始化为 37 的值。它返回的值是 37 + `n` 并且 `n` 等于 12，因此得到 49.在第二次遍历时，累加值是 49，返回值是 49 + 28 = 77。如此继续直到第四次。\n\n`reduce` 是很强大的，你可以实际使用它去构建很多数组的方法，比如 `map` 或者 `filter`：\n\n```javascript\nconst map = (arr, fn) => {\n  return arr.reduce((mappedArr, element) => {\n    return [...mappedArr, fn(element)]\n  }, [])\n}\n\nconsole.log(map([1, 2, 3, 4], n => n + 1)) // [2, 3, 4, 5]\n\nconst filter = (arr, fn) => {\n  return arr.reduce((filteredArr, element) => {\n    return fn(element) ? [...filteredArr] : [...filteredArr, element]\n  }, [])\n}\n\nconsole.log(filter([1, 2, 3, 4, 5, 6], n => n % 2 === 0)) // [1, 3, 5]\n```\n\n根本上看，我们给 `reduce` 一个初始默认值 `[]`：我们的累计值。对于 `map`，我们运行一个方法，它的结果是累加到最后，多亏了 **展开操作符**（不必担心，后面讨论）。对于 `filter`，几乎是相似的，除了我们在元素上运行过滤函数。如果返回 true，我们返回前一个数组，否则在数组最后添加当前元素。\n\n我们来看一个更高级的例子：深度展开数组，也就是说把 `[1, 2, 3, [4, [[[5, [6, 7]]]], 8]]` 样的数组转换成 `[1, 2, 3, 4, 5, 6, 7, 8]` 样的。\n\n```javascript\nfunction flatDeep(arr) {\n  return arr.reduce((flattenArray, element) => {\n    return Array.isArray(element)\n      ? [...flattenArray, ...flatDeep(element)]\n      : [...flattenArray, element]\n  }, [])\n}\n\nconsole.log(flatDeep([1, 2, 3, [4, [[[5, [6, 7]]]], 8]])) // [1, 2, 3, 4, 5, 6, 7, 8]\n```\n这个例子有点像 `map`，除了我们用到了递归。我不想去解释这个用法，它超出了这篇文章的范围。但是，如果你想了解更多的关于递归的知识，请参考这篇[优质的文章](https://guillaumebogard.dev/blog/what-is-recursion/)。\n\n### 展开操作（ES2015）\n\n我知道这不是一个方法。但是，在处理数组时，使用展开操作可以帮助你做很多事情。事实上，你可以在另一个数组中使用它展开一个数组的值。从这一点来说，你可以复制一个数组，或者连接多个数组。\n\n```javascript\nconst numbers = [1, 2, 3]\nconst numbersCopy = [...numbers]\nconsole.log(numbersCopy) // [1, 2, 3]\n\nconst otherNumbers = [4, 5, 6]\nconst numbersConcatenated = [...numbers, ...otherNumbers]\nconsole.log(numbersConcatenated) // [1, 2, 3, 4, 5, 6]\n```\n注意：：展开操作符对原数组做了一次浅拷贝。但什么是 浅拷贝？\U0001F914\n\n额，浅拷贝是尽可能少的复制原数组。当你有一个数组包含数字，字符串或者布尔值（基本类型），它们是没问题的，这些值被真正复制。然而，对于 **对象和数组** 而言，这是不同的。只有 **对原值的引用** 会被复制！因此，如果你创建一个包含对象的数组的浅拷贝，然后在拷贝的数组中修改了对象，它也会修改原数组的对象，因为它们是 **同一个引用**。\n\n```javascript\nconst arr = ['foo', 42, { name: 'Thomas' }]\nlet copy = [...arr]\n\ncopy[0] = 'bar'\n\nconsole.log(arr) // No mutations: [\"foo\", 42, { name: \"Thomas\" }]\nconsole.log(copy) // [\"bar\", 42, { name: \"Thomas\" }]\n\ncopy[2].name = 'Hello'\n\nconsole.log(arr) // /!\\ MUTATION [\"foo\", 42, { name: \"Hello\" }]\nconsole.log(copy) // [\"bar\", 42, { name: \"Hello\" }]\n```\n\n所以，如果你想去“真正地”靠谱一个包含对象或者数组的是维护组，你可以使用 lodash 的方法 cloneDeep。但是不要觉得必须做这样的事。这里的目标是 意识到事情是如何运作的。\n\n### 最好了解的\n\n下面你看到的方法，是最好了解一下的，同时它们能帮助你解决某些问题，比如在数组中搜索一个元素，取出数组的部分或者更多。\n\n你曾经尝试用过 `indexOf` 去查找一个数组中是否存在某个东西吗？这是一个糟糕的方式对吧？幸运的是，`includes` 为我们做到了这些。给 `includes` 一个参数，然后会在数组里面搜索它，如果一个元素存在的话。\n\n```javascript\nconst sports = ['football', 'archery', 'judo']\nconst hasFootball = sports.includes('football')\nconsole.log(hasFootball) // true\n```\n#### concat\n\nconcat 方法可以用来合并两个或者更多的数组。\n\n```javascript\nconst numbers = [1, 2, 3]\nconst otherNumbers = [4, 5, 6]\n\nconst numbersConcatenated = numbers.concat(otherNumbers)\nconsole.log(numbersConcatenated) // [1, 2, 3, 4, 5, 6]\n\n// You can merge as many arrays as you want\nfunction concatAll(arr, ...arrays) {\n  return arr.concat(...arrays)\n}\n\nconsole.log(concatAll([1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12])) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]\n```\n\n#### forEach\n\n无论何时你想为数组的每个元素执行一些事情时，可以使用 forEach。它使用一个函数作为参数，然后给它三个参数：当前值，索引，和当前数组。\n\n```javascript\nconst numbers = [1, 2, 3, 4, 5]\nnumbers.forEach(console.log)\n// 1 0 [ 1, 2, 3 ]\n// 2 1 [ 1, 2, 3 ]\n// 3 2 [ 1, 2, 3 ]\n```\n\n#### indexOf\n\n这个用来在给定的数组中找出第一个被发现的元素的索引。 `indexOf` 也广泛用于检查元素是否在一个数组中。不过老实说，我如今已经不这样使用了。\n\n```javascript\nconst sports = ['football', 'archery', 'judo']\n\nconst judoIndex = sports.indexOf('judo')\nconsole.log(judoIndex) // 2\n```\n\n#### find\n`find` 方法十分类似于 `filter` 方法。你必须提供一个函数用于测试数组的元素。然而，`find` 一旦发现有一个元素通过测试，就立即停止测试其他元素。不用于 `filter`，`filter` 将会迭代整个数组，无论情况如何。\n\n```javascript\nconst users = [\n  { id: 'af35', name: 'john' },\n  { id: '6gbe', name: 'mary' },\n  { id: '932j', name: 'gary' },\n]\n\nconst user = users.find(user => user.id === '6gbe')\nconsole.log(user) // { id: '6gbe', name: 'mary' }\n```\n\n所以使用 `filter`，当你想去过滤整个数组时。使用 `find` 在当你确定在数组中找某个唯一元素的时候。\n\n#### findIndex\n\n这个方法完全跟 find 相同除了它返回第一个发现元素的索引，而不是直接返回元素。\n\n```javascript\nconst users = [\n  { id: 'af35', name: 'john' },\n  { id: '6gbe', name: 'mary' },\n  { id: '932j', name: 'gary' },\n]\n\nconst user = users.findIndex(user => user.id === '6gbe')\nconsole.log(user) // 1\n```\n\n你或许认为 `findIndex` 跟 `indexOf` 是相同的。额……不完全是。`indexOf` 的第一个元素是基本值（布尔，数字，字符串，null，undefined或者一个 symbol）而`findIndex`的第一个元素是一个回调方法。\n所以当你需要搜索在数组中的一个元素的基本值时，使用 `indexOf`。如果有更复杂的元素，比如object，使用 `findIndex`。\n\n#### slice\n\n当你需要取出或者复制数组的一部分，可以使用 `slice`。但是注意，像展开操作符一样， slice 返回部分的浅拷贝！\n\n```javascript\nconst numbers = [1, 2, 3, 4, 5]\nconst copy = numbers.slice()\n```\n\n我在文章的开始谈到，循环是没有什么用的。来用一个例子说明你如何摆脱它。\n\n假设你想去从 API 中去除一定量的聊天记录里，然后展示它们中的 5 条。有两种方式实现：一种是循环，另一种是 `slice`。\n\n```javascript\n// 传统方式\n// 用循环来决定消息的数量\nconst nbMessages = messages.length < 5 ? messages.length : 5\nlet messagesToShow = []\nfor (let i = 0; i < nbMessages; i++) {\n  messagesToShow.push(posts[i])\n}\n\n// 假设 arr 少于 5 个元素\n// slice 将会返回原数组的整个浅拷贝\nconst messagesToShow = messages.slice(0, 5)\n```\n\n#### some\n\n如果你想测试数组中 至少有一个元素 通过测试，那么可以使用 `some`。就像是 `map`,`filter`,和 `find`，`some` 用回调函数作为参数。它返回 `ture`，如果至少一个元素通过测试，返回 `true` 否则返回 `false`。\n\n当你处理权限问题的时候，可以使用 `some`：\n\n```javascript\nconst users = [\n  {\n    id: 'fe34',\n    permissions: ['read', 'write'],\n  },\n  {\n    id: 'a198',\n    permissions: [],\n  },\n  {\n    id: '18aa',\n    permissions: ['delete', 'read', 'write'],\n  },\n]\n\nconst hasDeletePermission = users.some(user =>\n  user.permissions.includes('delete')\n)\nconsole.log(hasDeletePermission) // true\n```\n\n#### every\n\n类似 `some`，不同的是 `ever` 测试了所有的元素是否满足条件（而不是 至少一个）。\n\n```javascript\nconst users = [\n  {\n    id: 'fe34',\n    permissions: ['read', 'write'],\n  },\n  {\n    id: 'a198',\n    permissions: [],\n  },\n  {\n    id: '18aa',\n    permissions: ['delete', 'read', 'write'],\n  },\n]\n\nconst hasAllReadPermission = users.every(user =>\n  user.permissions.includes('read')\n)\nconsole.log(hasAllReadPermission) // false\n```\n#### flat(ES2019)\n这是一个即将到来的招牌方法， 在JavaScript 世界中。大致而言，`flat` 创建一个新数组，通过组合所有的子数组元素。接受一个参数，数值类型，代表你想展开的深度。\n```javascript\nconst numbers = [1, 2, [3, 4, [5, [6, 7]], [[[[8]]]]]]\n\nconst numbersflattenOnce = numbers.flat()\nconsole.log(numbersflattenOnce) // [1, 2, 3, 4, Array[2], Array[1]]\n\nconst numbersflattenTwice = numbers.flat(2)\nconsole.log(numbersflattenTwice) // [1, 2, 3, 4, 5, Array[2], Array[1]]\n\nconst numbersFlattenInfinity = numbers.flat(Infinity)\nconsole.log(numbersFlattenInfinity) // [1, 2, 3, 4, 5, 6, 7, 8]\n```\n#### flatMap(ES2019)\n\n猜猜这个方法干什么？我打赌你可以做到顾名思义。\n\n首先在每个元素上运行一个 mapping 方法。接着一次性展示数据。十分简单！\n\n```javascript\nconst sentences = [\n  'This is a sentence',\n  'This is another sentence',\n  \"I can't find any original phrases\",\n]\n\nconst allWords = sentences.flatMap(sentence => sentence.split(' '))\nconsole.log(allWords) // [\"This\", \"is\", \"a\", \"sentence\", \"This\", \"is\", \"another\", \"sentence\", \"I\", \"can't\", \"find\", \"any\", \"original\", \"phrases\"]\n```\n这个例子中，数组里有一些句子，然而我们想得到所有的单词。不使用 `map` 去把所有的句子分割成单词然后展开数组，你可以直接使用 `flatMap`。\n与 `flatMap` 无关的，你可以使用 `reduce` 方法来计算单词的数量（只是展示另一种 `reduce` 的用法）\n\n```javascript\nconst wordsCount = allWords.reduce((count, word) => {\n  count[word] = count[word] ? count[word] + 1 : 1\n  return count\n}, {})\nconsole.log(wordsCount) // { This: 2, is: 2, a: 1, sentence: 2, another: 1, I: 1, \"can't\": 1, find: 1, any: 1, original: 1, phrases: 1, }\n```\nflatMap 经常用于响应式编程，这里有个[例子](https://link.juejin.im/?target=http%3A%2F%2Freactivex.io%2Fdocumentation%2Foperators%2Fflatmap.html)。\n\n#### join\n如果你需要基于数组元素创建字符串，`join` 正是你所寻找的。它允许通过链接数组元素来创建一个新的字符串，通过提供的分割符分割。\n\n举个例子，你可以使用 `join` 一眼展示活动的参与者：\n\n```javascript\nconst participants = ['john', 'mary', 'gary']\nconst participantsFormatted = participants.join(', ')\nconsole.log(participantsFormatted) // john, mary, gary\n```\n下面的例子更真实，在于你想先过滤参与者然后得到他们的名字。\n\n```javascript\nconst potentialParticipants = [\n  { id: 'k38i', name: 'john', age: 17 },\n  { id: 'baf3', name: 'mary', age: 13 },\n  { id: 'a111', name: 'gary', age: 24 },\n  { id: 'fx34', name: 'emma', age: 34 },\n]\n\nconst participantsFormatted = potentialParticipants\n  .filter(user => user.age > 18)\n  .map(user => user.name)\n  .join(', ')\n\nconsole.log(participantsFormatted) // gary, emma\n```\n\n#### from\n\n这是一个静态方法，从类数组中创建新的数组，或者像例子中的字符串一样遍历对象。当处理 dom 时，这个方法十分有用。\n\n```javascript\nconst nodes = document.querySelectorAll('.todo-item') // 这是一个 nodeList 实例\nconst todoItems = Array.from(nodes) // 现在你能使用 map filter 等等，就像在数组中那样!\n```\n你曾经见到过我们使用 `Array` 代替数组实例吗？这就是问什么 `from` 被称作静态方法。\n\n接着可以愉快处理这些节点，比如用 `forEach` 在每个节点上注册事件监听：\n\n```javascript\ntodoItems.forEach(item => {\n  item.addEventListener('click', function() {\n    alert(`You clicked on ${item.innerHTML}`)\n  })\n})\n```\n### 最好了解突变\n\n是的，sort 修改了原数组。事实上，在这里进行了数组元素排序。默认的排序方法把所有的元素转换成字符串，然后按照字母表排序它们。\n\n```javascript\nconst names = ['john', 'mary', 'gary', 'anna']\nnames.sort()\nconsole.log(names) // ['anna', 'gary', 'john', 'mary']\n```\n\n如果你有 Python 背景的话，要小心了。使用 sort 在数字数组中不会得到你想要的结果。\n\n```javascript\nconst numbers = [23, 12, 17, 187, 3, 90]\nnumbers.sort()\nconsole.log(numbers) // [12, 17, 187, 23, 3, 90] \U0001F914\n```\n那么如何对一个数组排序？额，`sort` 接受一个函数，一个比较函数。这个函数接受两个参数：第一个元素（我们称呼为 `a`）和第二个元素作比较（`b`）。这两个元素之间的比较需要返回一个数字。\n\n- 如果为负，`a` 排序在 `b` 之前。\n- 如果为正，`b` 排序在 `a` 之前。\n- 如果是0，没有任何改变。\n\n那么你可以使用下面的方式排序数组：\n\n```javascript\nconst numbers = [23, 12, 17, 187, 3, 90]\nnumbers.sort((a, b) => a - b)\nconsole.log(numbers) // [3, 12, 17, 23, 90, 187]\n```\n\n或者通过最近时间排序：\n\n```javascript\nconst posts = [\n  {\n    title: 'Create a Discord bot under 15 minutes',\n    date: new Date(2018, 11, 26),\n  },\n  { title: 'How to get better at writing CSS', date: new Date(2018, 06, 17) },\n  { title: 'JavaScript arrays', date: new Date() },\n]\nposts.sort((a, b) => a.date - b.date) // Substracting two dates returns the difference in millisecond between them\nconsole.log(posts)\n// [ { title: 'How to get better at writing CSS',\n//     date: 2018-07-17T00:00:00.000Z },\n//   { title: 'Create a Discord bot under 15 minutes',\n//     date: 2018-12-26T00:00:00.000Z },\n//   { title: 'Learn Javascript arrays the functional way',\n//     date: 2019-03-16T10:31:00.208Z } ]\n```\n#### fill\n`fill` 修改或者填充了数组的所有元素，从开始索引到结束索引，使用一个静态值。`fill` 最有用的作用是使用静态值填充一个新数组。\n\n```javascript\n//  Normally I would have called a function that generates ids and random names but let's not bother with that here.\nfunction fakeUser() {\n  return {\n    id: 'fe38',\n    name: 'thomas',\n  }\n}\n\nconst posts = Array(3).fill(fakeUser())\nconsole.log(posts) // [{ id: \"fe38\", name: \"thomas\" }, { id: \"fe38\", name: \"thomas\" }, { id: \"fe38\", name: \"thomas\" }]\n```\n#### reverse\n\n这个方法名在这里显而易见。然而，像留意 sort 那样，reverse 会反转数组的位置。\n\n```javascript\nconst numbers = [1, 2, 3, 4, 5]\n\nnumbers.reverse()\nconsole.log(numbers) // [5, 4, 3, 2, 1]\n```\n\n### 你可以替换的方法\n\n终于，在这个最后的部分，你将发现改变原数组的方法，同时可以很容易替换其中一些。我不是说你应该抛弃这些方法。只是想要你意识到一些数组方法有副作用，并且这里有可选择的其他方法。\n\n#### push\n\n处理数组时这是使用最多的方法。事实上，push 允许你在数组中添加一个或者多个元素。它也通常基于一个旧数组构建一个新数组。\n\n```javascript\nconst todoItems = [1, 2, 3, 4, 5]\n\nconst itemsIncremented = []\nfor (let i = 0; i < items.length; i++) {\n  itemsIncremented.push(items[i] + 1)\n}\n\nconsole.log(itemsIncremented) // [2, 3, 4, 5, 6]\n\nconst todos = ['Write an article', 'Proofreading']\ntodos.push('Publish the article')\nconsole.log(todos) // ['Write an article', 'Proofreading', 'Publish the article']\n```\n如果你需要像 `itemsIncremented` 一样构建一个数组，很多方法都是机会，像我们的朋友 `map`,`filter`或者`reduce`。事实上我们可以使用 `map` 同样做到：\n\n```javascript\nconst itemsIncremented = todoItems.map(x => x + 1)\n```\n\n并且如果你需要使用 `push`，当你要添加新元素的时候，展开操作符为你撑腰。\n\n```javascript\nconst todos = ['Write an article', 'Proofreading']\nconsole.log([...todos, 'Publish the article']) // ['Write an article', 'Proofreading', 'Publish the article']\n```\n\n#### splice\n\n`splice` 常常用于作为移除某个索引元素的方法。你可以同样使用 `filter` 做到。\n\n```javascript\nconst months = ['January', 'February', 'March', 'April', ' May']\n\n// With splice\nmonths.splice(2, 1) // remove one element at index 2\nconsole.log(months) // ['January', 'February', 'April', 'May']\n\n// Without splice\nconst monthsFiltered = months.filter((month, i) => i !== 3)\nconsole.log(monthsFiltered) // ['January', 'February', 'April', 'May']\n```\n\n你可能会想，如果我需要移除多个元素呢？额，使用 `slice`：\n\n```javascript\nconst months = ['January', 'February', 'March', 'April', ' May']\n\n// With splice\nmonths.splice(1, 3) // remove thirds element starting at index 1\nconsole.log(months) // ['January', 'May']\n\n// Without splice\nconst monthsFiltered = [...months.slice(0, 1), ...months.slice(4)]\nconsole.log(monthsFiltered) // ['January', 'May']\n```\n\n#### shift\n\nshift 移除数组的第一个元素然后返回它。从功能上来说，你可以使用 spread/rest 实现。\n\n```javascript\nconst numbers = [1, 2, 3, 4, 5]\n\n// With shift\nconst firstNumber = numbers.shift()\nconsole.log(firstNumber) // 1\nconsole.log(numbers) // [2, 3, 4, 5]\n\n// Without shift\nconst [firstNumber, ...numbersWithoutOne] = numbers\nconsole.log(firstNumber) // 1\nconsole.log(numbersWithoutOne) // [2, 3, 4, 5]\n```\n\n#### unshift\n\nUnshift 允许你在数组开始添加一个或者多个元素。像是 `shift`， 你可以使用展开操作符做同样的事：\n\n```javascript\nconst numbers = [3, 4, 5]\n\n// With unshift\nnumbers.unshift(1, 2)\nconsole.log(numbers) // [1, 2, 3, 4, 5]\n\n// Without unshift\nconst newNumbers = [1, 2, ...numbers]\nconsole.log(newNumbers) // [1, 2, 3, 4, 5]\n```\n\n### 太长不看版：\n\n- 无论何时你在数组上操作时，不要使用 `for-loop` 也不要重复造轮子，你想做的可能已经有一个方法在那里。\n- 大多数情况，你应该使用 `map`,`filter`,`reduce`和`展开操作符`。它们对开发者来说是最基础的工具。\n- 有许多方法需要了解像 `slice`,`some`,`flatMap`等等。记住它们并且在合适的时候使用它们。\n- 副作用导致不想要的改变。要清楚哪些方法会改变你的原始数组。\n- `slice` 和展开操作符是浅拷贝。因此，对象和子数组将会共享同一个引用，小心使用它们。\n- “旧”的改变数组的方法可以被新的替换。取决于你想做什么。\n\n作者：toddmark\n链接：https://juejin.im/post/5ca5639ef265da30d561f31e\n来源：掘金\n著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。"
categories:
  - Web开发
toc: false
date: 2019-04-25 17:28:07
---

首先做一个粗体声明：**循环经常是无用的，并且使得代码很难阅读**。
当谈到迭代一个数组的时候，无论你想去查找元素，排序或者任何其他的事，都有可能存在一个数组的方法供你使用。

然而，尽管它们有用，但其中一些仍然不被人了解。我会努力为你展示一些有用的方法。把这篇文章当做对 JavaScript 数组方法的指引吧。
注意： 在开始之前，不得不了解一件事：我比较偏爱函数式编程。所以我倾向于使用的方法不会直接改变原来的数组。这种方法，我避免了副作用。我不是说不应该改变数组，但至少要了解那些方法会改变，那些会有副作用。副作用导致不想要的改变，而不想要的改变带来bugs！
了解到这里，我们可以开始正文了。

<!-- more -->

### 必不可少的

当跟数组打交道时，有四件事你应该清楚：`map,filter,reduce`和 展开操作符。它们富有力量。

#### map

你可以在很多种情况下使用它。基本地，每次你需要修改数组的元素时，考虑使用 `map`。

它接受一个参数：一个方法，在每一个数组元素上调用。然后返回一个**新的数组**，所以没有副作用。

```javascript
const numbers = [1, 2, 3, 4]

const numbersPlusOne = numbers.map(n => n + 1) // 每个元素 +1
console.log(numbersPlusOne) // [2, 3, 4, 5]
```

你也能创建一个新数组，用于保留对象的一个特殊属性：

```javascript
const allActivities = [
  { title: 'My activity', coordinates: [50.123, 3.291] },
  { title: 'Another activity', coordinates: [1.238, 4.292] },
  // etc.
]

const allCoordinates = allActivities.map(activity => activity.coordinates)
console.log(allCoordinates) // [[50.123, 3.291], [1.238, 4.292]]
```

所以，请记住，当你需要去**转换**数组时，考虑使用**map**。

#### filter

这个方法的名字在这里十分准确的：当你想去过滤数组的时候使用它。

如同`map`所做，它接受一个函数作为它的唯一参数，在数组的每个元素上调用。这个方法返回一个布尔值：

- `true` 如果你需要在数组中保留元素
- `false` 如果你不想保留它

接着你会得到一个带有你想要保留的元素的新数组。

举个例子，你可以在数组中只保留奇数：

```javascript
const numbers = [1, 2, 3, 4, 5, 6]
const oddNumbers = numbers.filter(n => n % 2 !== 0)
console.log(oddNumbers) // [1, 3, 5]
```

或者你可以在数组中移除特殊的项：

```javascript
const participants = [
  { id: 'a3f47', username: 'john' },
  { id: 'fek28', username: 'mary' },
  { id: 'n3j44', username: 'sam' },
]

function removeParticipant(participants, id) {
  return participants.filter(participant => participant.id !== id)
}

console.log(removeParticipant(participants, 'a3f47')) //  [{ id: 'fek28', username: 'mary' }, { id: 'n3j44', username: 'sam' }];
```

#### reduce

个人认为是最难理解的方法。但是如果你一旦掌握它，很多疯狂的事情你都可以用它做到。

基本地， `reduce` 使用有值的数组然后组合成一个新的值。它接受两个参数，一个回调方法就是我们的 **reducer** 和一个可选的初始化的值（默认是数组的第一个项）。这个 reducer 自己使用四个参数：

- 累计：在你的 reducer 中累积的返回值
- 当前数组的值
- 当前索引
- 当前调用 reduce 的数组

大多数时候，你只需要使用前两个参数：累计值和当前值。

抛开这些理论。来看看常见的一个 `reduce` 的例子。

```javascript
const numbers = [37, 12, 28, 4, 9]
const total = numbers.reduce((total, n) => total + n)
console.log(total) // 90
```

在第一个遍历时，这个累计值，也就是 `total`，使用了初始化为 37 的值。它返回的值是 37 + `n` 并且 `n` 等于 12，因此得到 49.在第二次遍历时，累加值是 49，返回值是 49 + 28 = 77。如此继续直到第四次。

`reduce` 是很强大的，你可以实际使用它去构建很多数组的方法，比如 `map` 或者 `filter`：

```javascript
const map = (arr, fn) => {
  return arr.reduce((mappedArr, element) => {
    return [...mappedArr, fn(element)]
  }, [])
}

console.log(map([1, 2, 3, 4], n => n + 1)) // [2, 3, 4, 5]

const filter = (arr, fn) => {
  return arr.reduce((filteredArr, element) => {
    return fn(element) ? [...filteredArr] : [...filteredArr, element]
  }, [])
}

console.log(filter([1, 2, 3, 4, 5, 6], n => n % 2 === 0)) // [1, 3, 5]
```

根本上看，我们给 `reduce` 一个初始默认值 `[]`：我们的累计值。对于 `map`，我们运行一个方法，它的结果是累加到最后，多亏了 **展开操作符**（不必担心，后面讨论）。对于 `filter`，几乎是相似的，除了我们在元素上运行过滤函数。如果返回 true，我们返回前一个数组，否则在数组最后添加当前元素。

我们来看一个更高级的例子：深度展开数组，也就是说把 `[1, 2, 3, [4, [[[5, [6, 7]]]], 8]]` 样的数组转换成 `[1, 2, 3, 4, 5, 6, 7, 8]` 样的。

```javascript
function flatDeep(arr) {
  return arr.reduce((flattenArray, element) => {
    return Array.isArray(element)
      ? [...flattenArray, ...flatDeep(element)]
      : [...flattenArray, element]
  }, [])
}

console.log(flatDeep([1, 2, 3, [4, [[[5, [6, 7]]]], 8]])) // [1, 2, 3, 4, 5, 6, 7, 8]
```
这个例子有点像 `map`，除了我们用到了递归。我不想去解释这个用法，它超出了这篇文章的范围。但是，如果你想了解更多的关于递归的知识，请参考这篇[优质的文章](https://guillaumebogard.dev/blog/what-is-recursion/)。

### 展开操作（ES2015）

我知道这不是一个方法。但是，在处理数组时，使用展开操作可以帮助你做很多事情。事实上，你可以在另一个数组中使用它展开一个数组的值。从这一点来说，你可以复制一个数组，或者连接多个数组。

```javascript
const numbers = [1, 2, 3]
const numbersCopy = [...numbers]
console.log(numbersCopy) // [1, 2, 3]

const otherNumbers = [4, 5, 6]
const numbersConcatenated = [...numbers, ...otherNumbers]
console.log(numbersConcatenated) // [1, 2, 3, 4, 5, 6]
```
注意：：展开操作符对原数组做了一次浅拷贝。但什么是 浅拷贝？🤔

额，浅拷贝是尽可能少的复制原数组。当你有一个数组包含数字，字符串或者布尔值（基本类型），它们是没问题的，这些值被真正复制。然而，对于 **对象和数组** 而言，这是不同的。只有 **对原值的引用** 会被复制！因此，如果你创建一个包含对象的数组的浅拷贝，然后在拷贝的数组中修改了对象，它也会修改原数组的对象，因为它们是 **同一个引用**。

```javascript
const arr = ['foo', 42, { name: 'Thomas' }]
let copy = [...arr]

copy[0] = 'bar'

console.log(arr) // No mutations: ["foo", 42, { name: "Thomas" }]
console.log(copy) // ["bar", 42, { name: "Thomas" }]

copy[2].name = 'Hello'

console.log(arr) // /!\ MUTATION ["foo", 42, { name: "Hello" }]
console.log(copy) // ["bar", 42, { name: "Hello" }]
```

所以，如果你想去“真正地”靠谱一个包含对象或者数组的是维护组，你可以使用 lodash 的方法 cloneDeep。但是不要觉得必须做这样的事。这里的目标是 意识到事情是如何运作的。

### 最好了解的

下面你看到的方法，是最好了解一下的，同时它们能帮助你解决某些问题，比如在数组中搜索一个元素，取出数组的部分或者更多。

你曾经尝试用过 `indexOf` 去查找一个数组中是否存在某个东西吗？这是一个糟糕的方式对吧？幸运的是，`includes` 为我们做到了这些。给 `includes` 一个参数，然后会在数组里面搜索它，如果一个元素存在的话。

```javascript
const sports = ['football', 'archery', 'judo']
const hasFootball = sports.includes('football')
console.log(hasFootball) // true
```
#### concat

concat 方法可以用来合并两个或者更多的数组。

```javascript
const numbers = [1, 2, 3]
const otherNumbers = [4, 5, 6]

const numbersConcatenated = numbers.concat(otherNumbers)
console.log(numbersConcatenated) // [1, 2, 3, 4, 5, 6]

// You can merge as many arrays as you want
function concatAll(arr, ...arrays) {
  return arr.concat(...arrays)
}

console.log(concatAll([1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12])) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```

#### forEach

无论何时你想为数组的每个元素执行一些事情时，可以使用 forEach。它使用一个函数作为参数，然后给它三个参数：当前值，索引，和当前数组。

```javascript
const numbers = [1, 2, 3, 4, 5]
numbers.forEach(console.log)
// 1 0 [ 1, 2, 3 ]
// 2 1 [ 1, 2, 3 ]
// 3 2 [ 1, 2, 3 ]
```

#### indexOf

这个用来在给定的数组中找出第一个被发现的元素的索引。 `indexOf` 也广泛用于检查元素是否在一个数组中。不过老实说，我如今已经不这样使用了。

```javascript
const sports = ['football', 'archery', 'judo']

const judoIndex = sports.indexOf('judo')
console.log(judoIndex) // 2
```

#### find
`find` 方法十分类似于 `filter` 方法。你必须提供一个函数用于测试数组的元素。然而，`find` 一旦发现有一个元素通过测试，就立即停止测试其他元素。不用于 `filter`，`filter` 将会迭代整个数组，无论情况如何。

```javascript
const users = [
  { id: 'af35', name: 'john' },
  { id: '6gbe', name: 'mary' },
  { id: '932j', name: 'gary' },
]

const user = users.find(user => user.id === '6gbe')
console.log(user) // { id: '6gbe', name: 'mary' }
```

所以使用 `filter`，当你想去过滤整个数组时。使用 `find` 在当你确定在数组中找某个唯一元素的时候。

#### findIndex

这个方法完全跟 find 相同除了它返回第一个发现元素的索引，而不是直接返回元素。

```javascript
const users = [
  { id: 'af35', name: 'john' },
  { id: '6gbe', name: 'mary' },
  { id: '932j', name: 'gary' },
]

const user = users.findIndex(user => user.id === '6gbe')
console.log(user) // 1
```

你或许认为 `findIndex` 跟 `indexOf` 是相同的。额……不完全是。`indexOf` 的第一个元素是基本值（布尔，数字，字符串，null，undefined或者一个 symbol）而`findIndex`的第一个元素是一个回调方法。
所以当你需要搜索在数组中的一个元素的基本值时，使用 `indexOf`。如果有更复杂的元素，比如object，使用 `findIndex`。

#### slice

当你需要取出或者复制数组的一部分，可以使用 `slice`。但是注意，像展开操作符一样， slice 返回部分的浅拷贝！

```javascript
const numbers = [1, 2, 3, 4, 5]
const copy = numbers.slice()
```

我在文章的开始谈到，循环是没有什么用的。来用一个例子说明你如何摆脱它。

假设你想去从 API 中去除一定量的聊天记录里，然后展示它们中的 5 条。有两种方式实现：一种是循环，另一种是 `slice`。

```javascript
// 传统方式
// 用循环来决定消息的数量
const nbMessages = messages.length < 5 ? messages.length : 5
let messagesToShow = []
for (let i = 0; i < nbMessages; i++) {
  messagesToShow.push(posts[i])
}

// 假设 arr 少于 5 个元素
// slice 将会返回原数组的整个浅拷贝
const messagesToShow = messages.slice(0, 5)
```

#### some

如果你想测试数组中 至少有一个元素 通过测试，那么可以使用 `some`。就像是 `map`,`filter`,和 `find`，`some` 用回调函数作为参数。它返回 `ture`，如果至少一个元素通过测试，返回 `true` 否则返回 `false`。

当你处理权限问题的时候，可以使用 `some`：

```javascript
const users = [
  {
    id: 'fe34',
    permissions: ['read', 'write'],
  },
  {
    id: 'a198',
    permissions: [],
  },
  {
    id: '18aa',
    permissions: ['delete', 'read', 'write'],
  },
]

const hasDeletePermission = users.some(user =>
  user.permissions.includes('delete')
)
console.log(hasDeletePermission) // true
```

#### every

类似 `some`，不同的是 `ever` 测试了所有的元素是否满足条件（而不是 至少一个）。

```javascript
const users = [
  {
    id: 'fe34',
    permissions: ['read', 'write'],
  },
  {
    id: 'a198',
    permissions: [],
  },
  {
    id: '18aa',
    permissions: ['delete', 'read', 'write'],
  },
]

const hasAllReadPermission = users.every(user =>
  user.permissions.includes('read')
)
console.log(hasAllReadPermission) // false
```
#### flat(ES2019)
这是一个即将到来的招牌方法， 在JavaScript 世界中。大致而言，`flat` 创建一个新数组，通过组合所有的子数组元素。接受一个参数，数值类型，代表你想展开的深度。
```javascript
const numbers = [1, 2, [3, 4, [5, [6, 7]], [[[[8]]]]]]

const numbersflattenOnce = numbers.flat()
console.log(numbersflattenOnce) // [1, 2, 3, 4, Array[2], Array[1]]

const numbersflattenTwice = numbers.flat(2)
console.log(numbersflattenTwice) // [1, 2, 3, 4, 5, Array[2], Array[1]]

const numbersFlattenInfinity = numbers.flat(Infinity)
console.log(numbersFlattenInfinity) // [1, 2, 3, 4, 5, 6, 7, 8]
```
#### flatMap(ES2019)

猜猜这个方法干什么？我打赌你可以做到顾名思义。

首先在每个元素上运行一个 mapping 方法。接着一次性展示数据。十分简单！

```javascript
const sentences = [
  'This is a sentence',
  'This is another sentence',
  "I can't find any original phrases",
]

const allWords = sentences.flatMap(sentence => sentence.split(' '))
console.log(allWords) // ["This", "is", "a", "sentence", "This", "is", "another", "sentence", "I", "can't", "find", "any", "original", "phrases"]
```
这个例子中，数组里有一些句子，然而我们想得到所有的单词。不使用 `map` 去把所有的句子分割成单词然后展开数组，你可以直接使用 `flatMap`。
与 `flatMap` 无关的，你可以使用 `reduce` 方法来计算单词的数量（只是展示另一种 `reduce` 的用法）

```javascript
const wordsCount = allWords.reduce((count, word) => {
  count[word] = count[word] ? count[word] + 1 : 1
  return count
}, {})
console.log(wordsCount) // { This: 2, is: 2, a: 1, sentence: 2, another: 1, I: 1, "can't": 1, find: 1, any: 1, original: 1, phrases: 1, }
```
flatMap 经常用于响应式编程，这里有个[例子](https://link.juejin.im/?target=http%3A%2F%2Freactivex.io%2Fdocumentation%2Foperators%2Fflatmap.html)。

#### join
如果你需要基于数组元素创建字符串，`join` 正是你所寻找的。它允许通过链接数组元素来创建一个新的字符串，通过提供的分割符分割。

举个例子，你可以使用 `join` 一眼展示活动的参与者：

```javascript
const participants = ['john', 'mary', 'gary']
const participantsFormatted = participants.join(', ')
console.log(participantsFormatted) // john, mary, gary
```
下面的例子更真实，在于你想先过滤参与者然后得到他们的名字。

```javascript
const potentialParticipants = [
  { id: 'k38i', name: 'john', age: 17 },
  { id: 'baf3', name: 'mary', age: 13 },
  { id: 'a111', name: 'gary', age: 24 },
  { id: 'fx34', name: 'emma', age: 34 },
]

const participantsFormatted = potentialParticipants
  .filter(user => user.age > 18)
  .map(user => user.name)
  .join(', ')

console.log(participantsFormatted) // gary, emma
```

#### from

这是一个静态方法，从类数组中创建新的数组，或者像例子中的字符串一样遍历对象。当处理 dom 时，这个方法十分有用。

```javascript
const nodes = document.querySelectorAll('.todo-item') // 这是一个 nodeList 实例
const todoItems = Array.from(nodes) // 现在你能使用 map filter 等等，就像在数组中那样!
```
你曾经见到过我们使用 `Array` 代替数组实例吗？这就是问什么 `from` 被称作静态方法。

接着可以愉快处理这些节点，比如用 `forEach` 在每个节点上注册事件监听：

```javascript
todoItems.forEach(item => {
  item.addEventListener('click', function() {
    alert(`You clicked on ${item.innerHTML}`)
  })
})
```
### 最好了解突变

是的，sort 修改了原数组。事实上，在这里进行了数组元素排序。默认的排序方法把所有的元素转换成字符串，然后按照字母表排序它们。

```javascript
const names = ['john', 'mary', 'gary', 'anna']
names.sort()
console.log(names) // ['anna', 'gary', 'john', 'mary']
```

如果你有 Python 背景的话，要小心了。使用 sort 在数字数组中不会得到你想要的结果。

```javascript
const numbers = [23, 12, 17, 187, 3, 90]
numbers.sort()
console.log(numbers) // [12, 17, 187, 23, 3, 90] 🤔
```
那么如何对一个数组排序？额，`sort` 接受一个函数，一个比较函数。这个函数接受两个参数：第一个元素（我们称呼为 `a`）和第二个元素作比较（`b`）。这两个元素之间的比较需要返回一个数字。

- 如果为负，`a` 排序在 `b` 之前。
- 如果为正，`b` 排序在 `a` 之前。
- 如果是0，没有任何改变。

那么你可以使用下面的方式排序数组：

```javascript
const numbers = [23, 12, 17, 187, 3, 90]
numbers.sort((a, b) => a - b)
console.log(numbers) // [3, 12, 17, 23, 90, 187]
```

或者通过最近时间排序：

```javascript
const posts = [
  {
    title: 'Create a Discord bot under 15 minutes',
    date: new Date(2018, 11, 26),
  },
  { title: 'How to get better at writing CSS', date: new Date(2018, 06, 17) },
  { title: 'JavaScript arrays', date: new Date() },
]
posts.sort((a, b) => a.date - b.date) // Substracting two dates returns the difference in millisecond between them
console.log(posts)
// [ { title: 'How to get better at writing CSS',
//     date: 2018-07-17T00:00:00.000Z },
//   { title: 'Create a Discord bot under 15 minutes',
//     date: 2018-12-26T00:00:00.000Z },
//   { title: 'Learn Javascript arrays the functional way',
//     date: 2019-03-16T10:31:00.208Z } ]
```
#### fill
`fill` 修改或者填充了数组的所有元素，从开始索引到结束索引，使用一个静态值。`fill` 最有用的作用是使用静态值填充一个新数组。

```javascript
//  Normally I would have called a function that generates ids and random names but let's not bother with that here.
function fakeUser() {
  return {
    id: 'fe38',
    name: 'thomas',
  }
}

const posts = Array(3).fill(fakeUser())
console.log(posts) // [{ id: "fe38", name: "thomas" }, { id: "fe38", name: "thomas" }, { id: "fe38", name: "thomas" }]
```
#### reverse

这个方法名在这里显而易见。然而，像留意 sort 那样，reverse 会反转数组的位置。

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.reverse()
console.log(numbers) // [5, 4, 3, 2, 1]
```

### 你可以替换的方法

终于，在这个最后的部分，你将发现改变原数组的方法，同时可以很容易替换其中一些。我不是说你应该抛弃这些方法。只是想要你意识到一些数组方法有副作用，并且这里有可选择的其他方法。

#### push

处理数组时这是使用最多的方法。事实上，push 允许你在数组中添加一个或者多个元素。它也通常基于一个旧数组构建一个新数组。

```javascript
const todoItems = [1, 2, 3, 4, 5]

const itemsIncremented = []
for (let i = 0; i < items.length; i++) {
  itemsIncremented.push(items[i] + 1)
}

console.log(itemsIncremented) // [2, 3, 4, 5, 6]

const todos = ['Write an article', 'Proofreading']
todos.push('Publish the article')
console.log(todos) // ['Write an article', 'Proofreading', 'Publish the article']
```
如果你需要像 `itemsIncremented` 一样构建一个数组，很多方法都是机会，像我们的朋友 `map`,`filter`或者`reduce`。事实上我们可以使用 `map` 同样做到：

```javascript
const itemsIncremented = todoItems.map(x => x + 1)
```

并且如果你需要使用 `push`，当你要添加新元素的时候，展开操作符为你撑腰。

```javascript
const todos = ['Write an article', 'Proofreading']
console.log([...todos, 'Publish the article']) // ['Write an article', 'Proofreading', 'Publish the article']
```

#### splice

`splice` 常常用于作为移除某个索引元素的方法。你可以同样使用 `filter` 做到。

```javascript
const months = ['January', 'February', 'March', 'April', ' May']

// With splice
months.splice(2, 1) // remove one element at index 2
console.log(months) // ['January', 'February', 'April', 'May']

// Without splice
const monthsFiltered = months.filter((month, i) => i !== 3)
console.log(monthsFiltered) // ['January', 'February', 'April', 'May']
```

你可能会想，如果我需要移除多个元素呢？额，使用 `slice`：

```javascript
const months = ['January', 'February', 'March', 'April', ' May']

// With splice
months.splice(1, 3) // remove thirds element starting at index 1
console.log(months) // ['January', 'May']

// Without splice
const monthsFiltered = [...months.slice(0, 1), ...months.slice(4)]
console.log(monthsFiltered) // ['January', 'May']
```

#### shift

shift 移除数组的第一个元素然后返回它。从功能上来说，你可以使用 spread/rest 实现。

```javascript
const numbers = [1, 2, 3, 4, 5]

// With shift
const firstNumber = numbers.shift()
console.log(firstNumber) // 1
console.log(numbers) // [2, 3, 4, 5]

// Without shift
const [firstNumber, ...numbersWithoutOne] = numbers
console.log(firstNumber) // 1
console.log(numbersWithoutOne) // [2, 3, 4, 5]
```

#### unshift

Unshift 允许你在数组开始添加一个或者多个元素。像是 `shift`， 你可以使用展开操作符做同样的事：

```javascript
const numbers = [3, 4, 5]

// With unshift
numbers.unshift(1, 2)
console.log(numbers) // [1, 2, 3, 4, 5]

// Without unshift
const newNumbers = [1, 2, ...numbers]
console.log(newNumbers) // [1, 2, 3, 4, 5]
```

### 太长不看版：

- 无论何时你在数组上操作时，不要使用 `for-loop` 也不要重复造轮子，你想做的可能已经有一个方法在那里。
- 大多数情况，你应该使用 `map`,`filter`,`reduce`和`展开操作符`。它们对开发者来说是最基础的工具。
- 有许多方法需要了解像 `slice`,`some`,`flatMap`等等。记住它们并且在合适的时候使用它们。
- 副作用导致不想要的改变。要清楚哪些方法会改变你的原始数组。
- `slice` 和展开操作符是浅拷贝。因此，对象和子数组将会共享同一个引用，小心使用它们。
- “旧”的改变数组的方法可以被新的替换。取决于你想做什么。

作者：toddmark
链接：https://juejin.im/post/5ca5639ef265da30d561f31e
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。