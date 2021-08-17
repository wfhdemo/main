# 测试 NPM 依赖重复问题

## 描述

> main 应用存在依赖 @wfhdemo/lib1@1.0.1
>
> @wfhdemo/lib2 依赖 @wfhdemo/lib1@1.0.0
>
> main 应用依赖 @wfhdemo/lib2

## 问题

> main 中的依赖@wfhdemo/lib1 重复了，但是版本不同
>
> npm(node package manage)为依赖同样维持了依赖树，所以会安装所有的依赖
>
> 依赖树如下

```js
/*
    main
      --@wfhdemo/lib1@1.0.1
      --@wfhdemo/lib2@1.0.0
                  --@wfhdemo/lib1@1.0.0

    生成的目录：
    main
      --node_modules
                --@wfhdemo/lib1@1.0.1
                --@wfhdemo/lib2@1.0.0
                        --node_modules
                                --@wfhdemo/lib1@1.0.0
*/
```

## 测试结果

> @wfhdemo/lib1@1.0.1会打印版本号 1.0.1
>
> @wfhdemo/lib@1.0.0会打印版本号 1.0.0

## 结论

> npm 会为所有的依赖维持依赖树，可以处理不同的版本的仓库
>
> npm 优化，将公共依赖或不重复依赖移到根目录下的 node_modules，并不影响 npm 包管理模型

## 遗留问题

- `同一项目，依赖不同版本的antd，依然会报错？待查明原因`
