# 2020-2021学年第2学期
# **实 验 报 告**
![zucc](./img/zucc.png "ZUCC")

- 课程名称:编程语言原理与编译
- 实验项目:期末大作业
- 专业班级_计算2002__                      
- 学生学号_32001080__                        
- 学生姓名_吴卓霖__                        
- 实验指导教师:郭鸣

## 实验内容


1. 评分方式，以下为起始评分     
    - 按 及格 中 良 优 参考计分，最终看完成水平。
          60 70 80 90
  - [参考任务](https://gitee.com/sigcc/plzoofs/blob/master/microc/task.md)
    - 优秀
        - 参与实际的开源编译器项目
          - v rust julia 等
        - 自行完整实现一个较复杂的语言 
          - 生成  中间语言、汇编 
          - 实现高级的语言特性
            - 按特性实现难度给分
    - 良 完整实现一个简单语言   
        - 词法/语法/语义检查，中间代码IR,栈式虚拟机  
    - 中 实现一个简单语言
        - 实现 词法/语法/语义（类型检查，中间语言AST,IR）
    - 及格  修改实现一个简单语言
        - 修改 现有项目 简单的词法/语法，必须理解原来的代码
        - 实现需要包括解释器和编译器
1. - 完成文档为 md格式，请预览并确保格式正确。 
        - 缩进，代码，图片，突出显示 的正确markdown语法
        - 翻译专业词汇标准，请到图书馆查找相关翻译的编译书籍，部分书籍有专业术语表。 
    - columbia 
    - PLC
    - ssw
    - yale
   
1. 提交内容
    - 项目报告
        - 文档结构 [参考项目结构](https://bb.zucc.edu.cn/bbcswebdav/users/j04014/PLC/final/columbia.microc.llvm.proj.rar)
    - 项目源代码（测试代码）
    - [参考项目](https://bb.zucc.edu.cn/bbcswebdav/users/j04014/PLC/final21)
1. 建议改进列表（可选择其中部分加以）
    - 词法部分
        - 修改注释的表示方式，如：
            - (*  *) 
            - //
            - /* */
        - 修改常量定义
            - 字符串常量  （参考python语法）
                - 单引号' ' 双引号 ""  三引号 '''
            - 数值常量 （参考c语法）
                - 二进制  0b0101， 八进制0o777  十六0xFFDA
        - 修改标识符定义
            - 类型名称 以大写开头
            - 变量名以小写开头
            - 两个下划线开头的名字__是内部保留，不允许
    - 语法部分
        - if的多种方式 
            - switch case
        - 循环的多种方式 
            - for / while / do while/ until 
        -  ? : 表达式  (C语言)
        - for  in 表达式
        - 匿名 lambda 函数（参考Python 或 Javascript）

    - 语义部分
        - 类型检查，类型推理
        - 支持嵌套函数
        - 动态作用域，静态作用域
        - 闭包支持
        - 模式匹配支持
        - 中间代码生成 AST，四元式，三元式，llvm
        - 生成器 generator, yield
        - 协程 coroutine



请根据实际情况填写下表

## 以下的内容 放到项目文档中

| 姓名 |学号 |班级 |任务|权重|
| :----------- | :-----------: |  ---: |   ---: |   ---: |
| Name   |      No.     | Class |Task|Factor|
| 张魏忠|   32001230 | 计算机2002 |编码，测试|0.9|
| 吴卓霖|   32001080 | 计算机2002 |设计，编码|1.0|

 成员代码提交日志

![提交信息](./img/commits.png)

1. 项目自评等级:(1-5)
请根据自己项目情况填写下表

| 词法                                       | 评分 | 备注 |      |
| ------------------------------------------ | ---- | ---- | ---- |
| char.float.string声明定义                  |   |      |      |
| ++.--.+=.-=                           |     |      |      |
| print/printf                          |     |      |      |

| 语法                                       | 评分 | 备注 |      |
|------------------------------------------|--------|------|-----|
| switch-case                        |     |      |      |
| 循环  for / while / do while       |     |      |      |
| break continue                            |      |      |      |
| return函数结果                            |      |      |      |
| 三目运算符?                            |      |      |      |




1. 项目说明

    - 项目 是基于现有的plzoofs-master-microc代码

            Absyn.fs抽象语法设计
            CLex.fsl词法分析
            Contcomp.fs编译器
            CPar.fsy语法分析
            interp.fs业务逻辑层
            interpc.fsproj解释器运行
            microcc.fsproj编译器运行
            StackMachine.fs堆栈指令

2. 解决技术要点说明
    - 定义声明(char.float.string)
      - 实现char、float、string的声明定义，
      ```f#
      //under Evaluating micro-C expressions
      | CstI i -> (i, store)
      | ConstChar c    -> ((int c), store)
      | ConstString s  -> (s.Length,store)
      | ConstFloat f ->
            //类型转换
           let bytes = System.BitConverter.GetBytes(float32(f))
           let v = System.BitConverter.ToInt32(bytes, 0)
         (v, store)
      ```
    - print
      - 仿c语言格式,如：(print("%d",i))
      ```f#
        //under Evaluating micro-C expressions
        | Print (op , e1) ->    let (i1,store1) = 
                                eval e1 locEnv gloEnv store
                            let res = 
                                match op with
                                | "%c"   -> (printf "%c " (char i1); i1)
                                | "%d"   -> (printf "%d " i1; i1)  
                                | "%s"   -> (printf "%s " (string i1); i1)
                                | "%f"   -> (printf "%f " (float i1) ;i1)
                                | _ -> failwith ("unknown primitive " + op)  //Error info 
                            (res, store1)
      ```
    - ++.--.+=.-=
      - c语言自运算
      ```f#
        //Evaluating micro-C expressions
        | SimpleOpt (ope,acc,e) ->
        let  (loc, store1) = access acc locEnv gloEnv store // 取acc地址
        let  (i1)  = getSto store1 loc
        let  (i2, store2) = eval e locEnv gloEnv store
        let  res =
            match ope with
            | "Z++" -> i1 + i2
            | "++Z" -> i1 + i2
            | "Z--" -> i1 - i2
            | "--Z" -> i1 - i2
            | "+="  -> i1 + i2
            | "-="  -> i1 - i2
            | "*="  -> i1 * i2
            | "/="  -> i1 / i2
            | "%="  -> i1 % i2
            | _ -> failwith ("unknown primitive " + ope)
        (res, setSto store2 loc res)
      ```
    - switch case
      - 选择
      ```f#
        //under exec
        | Switch (e,body) ->  
                let (res, store0) = eval e locEnv gloEnv store
                let rec loop store1 controlStat = 
                    match controlStat with
                    | Some(Break)           -> (store1, None)          // 如果有遇到的break，结束该次循环并清除break标记
                    | Some(Return _)        -> (store1, controlStat)   // 如果有未跳出的函数，
                    | _                     ->                         // continue或者没有设置控制状态时，先检查条件然后继续运行
                        let rec pick list =
                            match list with
                            | Case(e1,body1) :: tail -> 
                                let (res2, store2) = eval e1 locEnv gloEnv store1
                                if res2=res then exec body1 locEnv gloEnv store2 None
                                            else pick tail
                            | [] -> (store1,None)
                            | Default( body1 ) :: tail -> 
                                let (res3,store3) = exec body1 locEnv gloEnv store1 None
                                pick tail
                        (pick body)
                loop store0 controlStat
        | Case (e,body) -> exec body locEnv gloEnv store controlStat
      ```
    - for
      - 循环
      ```f#
        | For ( dec,e1,opera,body ) ->
        let (res , store0) = eval dec locEnv gloEnv store
        let rec loop store1 controlStat = 
             match controlStat with
             | Some(Break)           -> (store1, None)          // 如果有遇到的break，结束该次循环并清除break标记
             | Some(Return _)        -> (store1, controlStat)   // 如果有未跳出的函数，
             | _                     ->                         // continue或者没有设置控制状态时，先检查条件然后继续运行
                let (ifValue, store2) = eval e1 locEnv gloEnv store1
                if ifValue<>0 then 
                    let (store3,c) = exec body locEnv gloEnv store2 None
                    let (oneend ,store4) = eval opera locEnv gloEnv store3
                    loop store4 c
                        else (store2,None)
        loop store0 controlStat
      ```
    - return
      - 返回函数的值
      ```f#
        //under callfun
        match c with
        | Some(Return res)  -> 
            if res.IsSome then 
                let retVal = fst (eval res.Value locEnv gloEnv store3) in
                    (retVal, store3) 
            else (-111, store3) // interupt by return
        | _                 -> (-112, store3)  // end with normal stmt
      ```
    - ?
      - 三目运算符
      ```f#
        //under eval
        | Prim3 (e1, e2, e3) ->
        let (i1, store1) = eval e1 locEnv gloEnv store
        if i1 <> 0 then
            eval e2 locEnv gloEnv store1
        else
            eval e3 locEnv gloEnv store1
      ```
3. 心得体会（结合自己情况具体说明）

     - 大项目开发过程心得
        - 遇到哪些困难，经历哪里过程，有哪些收获
        - 。。。
        - 。。。
     - 本课程建议
         - 课程难度方面，进度方面，课程内容，授课方式等，给出你的意见
         - 。。。
         - 。。。
