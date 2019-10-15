# 代码概览

喾语言编译器ku工程的代码组织结构如下：

```bash
├── main.go     # 主程序入口
├── args.go     # 命令行参数
├── runtime.go  # 运行时环境
├── README.md   # README
├── lexer       # 词法分析
├── parser      # 语法分析
├── ast         # AST树
├── semantic    # 语义分析
├── codegen     # 代码生成
├── doc         # 文档生成
└── util        # 辅助功能
```

要了解编译器工作的流程，可以从`main.go`入口函数开始，然后循着编译的流程逐步阅读各个模块：

```
main.go -> lexer -> parser -> ast -> semantic -> codegen
```

一次编译的流程如下：

1. main.go读入需要进行编译的文件（包括标准库和依赖），输送给词法分析器lexer
2. lexer读入文件，进行词法分析，得到一个符号列表（tokens）
3. parser读入Tokens，进行语法分析，得到一个语法分析树（ParseTree）
4. ast读入ParseTree，并调用constructor来构建AST语法树（ASTTree）
5. sementic模块对AST树进行语义分析，检查是否有语义错误
6. codegen将AST树转化为LLVM代码（LLIR），并调用LLVM后端，生成为可执行文件（或静态库）

可以看到，ku编译器基本是按照编译原理的标准流程进行的。如果对词法分析、语法分析、语义分析、AST树等编译原理概念有所疑问，请参考[《编译原理》](https://book.douban.com/subject/3296317/)一书。

我会在下面对这几个模块分别进行描述。

注：阅读文档之外，还可以参阅代码中的注释。我已经在代码中对所有关键流程都添加了注释。