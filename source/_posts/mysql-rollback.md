---
title: mysql事务回滚问题
tags:
  - mysql
  - mysql事务
  - mysql事务回滚
  - php
  - sql事务回滚
id: 972
categories:
  - Mysql
  - PHP
date: 2013-01-19 22:35:50
---

在事务中，每个正确的原子操作都会被顺序执行，直到遇到错误的原子操作，此时事务会将之前的操作进行回滚。回滚的意思是如果之前是插入操作，那么会执行删 除插入的记录，如果之前是update操作，也会执行update操作将之前的记录还原。因此，正确的原子操作是真正被执行过的。是物理执行。

在当前事务中确实能看到插入的记录。最后只不过删除了。但是AUTO_INCREMENT不会应删除而改变值。

1、为什么auto_increament没有回滚？
因为innodb的auto_increament的计数器记录的当前值是保存在存内 存中的，并不是存在于磁盘上，当mysql server处于运行的时候，这个计数值只会随着insert改增长，不会随着delete而减少。而当mysql server启动时，当我们需要去查询auto_increment计数值时，mysql便会自动执行：SELECT MAX(id) FROM 表名 FOR UPDATE;语句来获得当前auto_increment列的最大值，然后将这个值放到auto_increment计数器中。所以就算 Rollback MySQL的auto_increament计数器也不会作负运算。

2、MySQL的事务对表操作的时候是否是物理操作？
MySQL的事务是有redo和undo的，redo操作的所有信息都是记录到 redo_log中，也就是说当一个事务做commit操作时，需要先把这个事务的操作写到redo_log中，然后再把这些操作flush到磁盘上，当 出现故障时，只需要读取redo_log,然后再重新flush到磁盘就行了。
而对于undo就比较麻烦，MySQL在处理事务时，会在数据共享 表空间里申请一个段叫做segment段，用保存undo信息，当在处理rollback，不是完完全全的物理undo，而是逻辑undo,就是说会对之 前的操作进行反操作，但是这些共享表空间是不进行回收的。这些表空间的回收需要由mysql的master thread进程来进行回收。