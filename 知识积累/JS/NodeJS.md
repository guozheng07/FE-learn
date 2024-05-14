# process.cwd()和path.resolve()
process.cwd() 和 path.resolve() 都可以用来获取路径，但是它们的用途和返回结果有所不同。
- process.cwd()：这个方法返回的是 Node.js 进程的当前工作目录，也就是你在命令行中执行 node 命令时所在的目录。
- path.resolve()：这个方法用于将相对路径解析为绝对路径。它会把路径或路径片段的序列解析为绝对路径，解析过程从右到左，直到构造出一个绝对路径，如果处理完所有给定的路径段还未能创建出绝对路径，则当前工作目录会被用上。

简单来说，process.cwd() 返回的是运行 Node.js 进程时的工作目录，而 path.resolve() 是用来解析和构造绝对路径的。
