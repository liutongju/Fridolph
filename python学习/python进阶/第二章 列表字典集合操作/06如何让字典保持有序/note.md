实际案例： 某编程竞赛中，对参赛选手编程解题进行计时，选手完成题目后，把该选手解题用时记录到字典中，以便赛后按选手名查询成绩。
答题用时越短，成绩越优

{
  'LiLei': (2,43),
  'HanMei': (5, 52),
  'Jim': (1, 39),
  ... 
}

比赛结束后，需按排名顺序依次打印选手成绩，如何实现？


# 解决方案

使用 collections.OrderdDict
以OrderedDict替代内置字典Dict，依次将选手成绩存入OrderedDict