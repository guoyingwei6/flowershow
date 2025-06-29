---
created: 2024-06-20 10:24
updated: 2025-06-10 09:55
---
## xargs is the inverse function of echo  
xargs 是 echo 的反函数](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html "Permalink to xargs is the inverse function of echo")

Published: Fri 12 April 2024

By [David Hashe](https://dhashe.com/author/david-hashe.html)

In [Blog](https://dhashe.com/category/blog.html).

`xargs` is a particularly confusing unix command. I want to share my trick for understanding how it works.  
`xargs` 是一个特别令人困惑的 UNIX 命令。我想分享我的技巧来理解它是如何工作的。

Let's look at the abbreviated [tldr](https://tldr.sh/) output for xargs:  
让我们看一下 xargs 的缩写 tldr 输出：

`$ tldr xargs xargs   - Run a command using the input data as arguments:    {{arguments_source}} | xargs {{command}}`

So, for example, if you had a file named `files_to_delete` with the contents below:  
例如，如果您有一个名为 `files_to_delete` 的文件，其内容如下：

`obsolete_note.md yucky_recipe.pdf cringe_tiktok.mov`

Then you could run the following line to delete each of the files [1](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fn:cat_abuse):  
然后您可以运行以下行来删除每个文件 [1](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fn:cat_abuse) ：

`$ cat files_to_delete | xargs rm`

Which reduces to the following command:  
这减少为以下命令：

`$ rm obsolete_note.md yucky_recipe.pdf cringe_tiktok.mov`

This is a general pattern that shows up in a lot of contexts when writing shell pipelines. And as you might expect, this is because xargs implements a necessary and fundamental operation in shell scripting: converting input data from stdin to cmdline args.  
这是编写 shell 管道时在很多上下文中都会出现的通用模式。正如您所料，这是因为 xargs 在 shell 脚本中实现了一个必要且基本的操作：将输入数据从 stdin 转换为 cmdline args。

For context, there are two separate and equally important ways to pass input to a unix program: stdin and cmdline args [2](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fn:env). Sometimes, a program's cmdline args determine what it expects on stdin. And in general, programs can have quite complex behavior with respect to their input. But the basic differences between the two input sources are:  
对于上下文，有两种独立但同样重要的方法将输入传递给 unix 程序： stdin 和 cmdline args [2](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fn:env) 。有时，程序的命令行参数决定了它在标准输入上的期望。一般来说，程序对其输入可能有相当复杂的行为。但这两种输入源之间的基本区别是：

- stdin is an infinite text stream for ongoing input  
    stdin 是用于持续输入的无限文本流
- cmdline args is a fixed text array of start-of-program input  
    cmdline args 是程序开始输入的固定文本数组

Here's a generic example of how to pass input to a command within a pipeline [3](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fn:echo_heredocs):  
以下是如何将输入传递到管道 [3](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fn:echo_heredocs) 中的命令的通用示例：

`$ echo $STDIN_DATA | $CMD $ARGS_DATA # $CMD may need to be given input via $STDIN_DATA and/or $ARGS_DATA`

For most programs, it is obvious what the cmdline args are, because they are written out in the pipeline. The tricky part of xargs is that it dynamically constructs the cmdline args for the program it calls based on its own stdin.  
对于大多数程序来说，cmdline args 是什么是显而易见的，因为它们是在管道中写出的。 xargs 的棘手部分是它根据自己的 stdin 为其调用的程序动态构造 cmdline args。

In our earlier example, xargs dynamically constructed the call `rm obsolete_note.md yucky_recipe.pdf cringe_tiktok.mov` based on its stdin. We couldn't tell what the arguments to rm would be based on the original pipeline text `cat files_to_delete | xargs rm`.  
在我们前面的示例中，xargs 根据其标准输入动态构造调用 `rm obsolete_note.md yucky_recipe.pdf cringe_tiktok.mov` 。我们无法根据原始管道文本 `cat files_to_delete | xargs rm` 判断 rm 的参数是什么。

Interestingly, the echo command implements exactly the opposite operation to xargs in shell scripting: converting input data from cmdline args to stdin:  
有趣的是，echo 命令在 shell 脚本中实现了与 xargs 完全相反的操作：将输入数据从 cmdline args 转换为 stdin：

`$ tldr echo echo   - Print a text message. Note: quotes are optional:    echo "{{Hello World}}"`

Both xargs and echo show up frequently in shell pipelines.  
xargs 和 echo 都频繁出现在 shell 管道中。  
I think that echo is less confusing because the mental model of shell pipelines is of self-contained commands passing their output forward to the next command, and echo is just a command that directly generates output from cmdline args. For example:  
我认为 echo 不那么令人困惑，因为 shell 管道的心智模型是独立的命令，将其输出转发到下一个命令，而 echo 只是一个直接从 cmdline args 生成输出的命令。例如：

`$ echo $ARGS | command_plus_args | command_plus_args # echo naturally fits into the pipeline mental model as a source of data over stdin $ command_plus_args | xargs $CMD | command_plus_args # xargs does something weird and unexpected by dynamically constructing a command in the middle of the pipeline`

See how xargs breaks the mental model by constructing a command dynamically at runtime based on the previous link in the chain, which is counter-intuitive.  
了解 xargs 如何通过在运行时根据链中的前一个链接动态构造命令来打破心理模型，这是违反直觉的。

But this is also why xargs is so powerful. Shell pipelines only pass data through stdin/stdout, but many unix programs require input via cmdline args, and xargs makes it possible to use those programs within a pipeline.  
但这也是xargs如此强大的原因。 Shell 管道仅通过 stdin/stdout 传递数据，但许多 UNIX 程序需要通过 cmdline args 输入，而 xargs 使得在管道中使用这些程序成为可能。

And so now we can state the trick for understanding xargs, which was also the title of this post: xargs is the inverse function of echo.  
现在我们可以说出理解 xargs 的技巧，这也是这篇文章的标题：xargs 是 echo 的反函数。

- `echo` is a function that maps cmdline args to stdin  
    `echo` 是将 cmdline args 映射到 stdin 的函数
- `xargs` is a function that maps stdin to cmdline args  
    `xargs` 是将 stdin 映射到 cmdline args 的函数
- `xargs echo` is an identity function because `echo $DATA | xargs echo | $CMD` is equivalent to `echo $DATA | $CMD`  
    `xargs echo` 是一个恒等函数，因为 `echo $DATA | xargs echo | $CMD` 相当于 `echo $DATA | $CMD`
- `echo $DATA | xargs $CMD` is another identity function because it is equivalent to `$CMD $DATA`  
    `echo $DATA | xargs $CMD` 是另一个恒等函数，因为它相当于 `$CMD $DATA`

In fact, without any arguments, `xargs` defaults to behaving like `xargs echo` because defaulting to an identity function is a sensible thing to do.  
事实上，在没有任何参数的情况下， `xargs` 默认的行为与 `xargs echo` 类似，因为默认使用恒等函数是明智的做法。

So, in order to remember what xargs does, just think about what echo does, and remember that xargs performs the inverse.  
因此，为了记住 xargs 的作用，只需考虑 echo 的作用，并记住 xargs 执行相反的操作。

_Thank you to [Eliot Robson](https://eliotwrobson.github.io/#about) for providing feedback on drafts of this post. All mistakes are my own.  
感谢 Eliot Robson 对本文草稿提供反馈。所有的错误都是我自己造成的。_

---

1. I am aware that this is a [useless use of cat](https://porkmail.org/era/unix/award). [↩](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fnref:cat_abuse "Jump back to footnote 1 in the text")  
    我知道这是对 cat 的无用用途。 ↩
    
2. There is also the [environment](https://en.wikipedia.org/wiki/Environment_variable), but it is uncommon to explicitly use that in shell pipelines. [↩](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fnref:env "Jump back to footnote 2 in the text")  
    还有环境，但在 shell 管道中显式使用它的情况并不常见。 ↩
    
3. Note that `$STDIN_DATA` is the cmdline args for `echo` but the stdin for `$CMD`. Also note that we could have replaced echo with a [heredoc](https://en.wikipedia.org/wiki/Here_document). [↩](https://dhashe.com/xargs-is-the-inverse-function-of-echo.html#fnref:echo_heredocs "Jump back to footnote 3 in the text")  
    请注意， `$STDIN_DATA` 是 `echo` 的命令行参数，但是 `$CMD` 的标准输入。另请注意，我们可以用定界符替换 echo。 ↩