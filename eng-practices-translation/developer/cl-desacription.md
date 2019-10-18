#撰写好的 CL 描述

 ​  一个好的 CL 描述公开记录了它做了哪些改变和为什么做这些改变。它会成为我们的版本控制历史中永久存在的一部分，在未来会被除了你的 reviewer 以外的成百上千的人阅读。

 ​  未来的开发者会根据描述查找你的 CL。未来的某些人可能因为一些微弱的关联却没有方便的细节来查看你的更改。如果所有的重要信息都在代码里，而不在描述里，那么对他们来说会很难定位到你的 CL。

## 第一行（PR 的 title）

 - 简短总结做了什么
 - 完整的句子，要写的像命令一样
 - 紧接着空行

 ​	一个 CL 的描述的__第一行__应该是对该 CL 所做的具体工作的总结摘要，然后是空行。这是大多数未来开发者浏览一段代码的版本控制历史时会看的东西，所以第一行应该提供足够的信息，以便他们不用为了大致了解你的 CL 做了什么，而去阅读你的 CL 或者整个描述。

 ​	按照传统，CL 描述的第一行是一个完整的句子，写得像一个命令（命令式句子）。例如，用 “__Delete__ the FizzBuzz RPC and __replace__ it with the new system.”  而不是 “__Deleting__ the FizzBuzz RPC and __replacing__ it with the new system.”。不过，不用把其他句子写成命令式的句子。

## Body 内容翔实

 ​	描述的剩余部分应该内容翔实。它可能包含一个简短描述，介绍解决了什么问题，为什么这是最好的解决方式。如果这个解决方式有什么不足，应该在描述中提到。如果相关，应包含背景信息，比如 bug 号，基准测试结果，和设计文档的链接。

 ​	即使很小的 CL 也应注意细节，把它放在上下文中。

## 糟糕的 CL 描述

 ​	`"Fix bug"` 是一个不恰当的 CL 描述。什么 bug？你做了什么去修复它？其他相似的糟糕的描述包括：

 - `“Fix build.”`
 - `“Add patch.”`
 - `“Moving code from A to B.”`
 - `“Phase 1.”`
 - `“Add convenience functions.”`
 - `“kill weird URLs.”`

 ​	这些有一部分是真实的 CL 描述。它们的作者可能坚信它们提供了有用的信息，但是它们并没有满足一个 CL 描述的目标。

## 优秀的 CL 描述

这里有一些优秀描述的例子。

### 功能性更改

```
  rpc: remove size limit on RPC server message freelist.

  Servers like FizzBuzz have very large messages and would benefit from reuse. Make the freelist larger, and add a goroutine that frees the freelist entries slowly over time, so that idle servers eventually release all freelist entries.
```

 ​	前面几个词描述了这个 CL 到底做了什么。描述的剩余部分表明了解决的什么问题，为什么这是个好的解决方法，以及关于具体实现的更多细节信息。

### 重构

```
  Construct a Task with a TimeKeeper to use its TimeStr and Now methods.

  Add a Now method to Task, so the borglet() getter method can be removed (which was only used by OOMCandidate to call borglet’s Now method). This replaces the methods on Borglet that delegate to a TimeKeeper.

  Allowing Tasks to supply Now is a step toward eliminating the dependency on Borglet. Eventually, collaborators that depend on getting Now from the Task should be changed to use a TimeKeeper directly, but this has been an accommodation to refactoring in small steps.

  Continuing the long-range goal of refactoring the Borglet Hierarchy.
```

 ​	第一行描述了这个 CL 做了什么，以及这与过去的变化。描述的剩余部分讨论了具体的实现，CL 的上下文，该方案并不理想，以及未来可能的解决方向。它还解释了为什么要做这些更改。

### 需要一些上下文的小型 CL

```
  Create a Python3 build rule for status.py.

  This allows consumers who are already using this as in Python3 to depend on a rule that is next to the original status build rule instead of somewhere in their own tree. It encourages new consumers to use Python3 if they can, instead of Python2, and significantly simplifies some automated build file refactoring tools being worked on currently.
```

 ​	第一句描述了具体做了什么。剩余部分解释了为什么要这样更改，并给了 reviewer 很多的上下文。

### 提交 CL 之前先 review 描述

 ​ CLs 在 review 期间可能会有巨大的改变。所以在提交 CL 之前 review 描述是很有价
 值的，会确保描述依旧反映了这个 CL 做了什么。
