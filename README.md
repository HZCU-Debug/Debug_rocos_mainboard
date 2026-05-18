# Debug_rocos_mainboard

基于 [NBUT RoboCup主控板](https://gitee.com/fan-jiaming123/main_board) 二次开发

## 主要修改点

1. src/action.c：修改吸球力度，原仓库三档力度都为 0.5，本仓库修改为 0.7/0.8/0.9
2. inc/cfg.h：关闭蜂鸣器
3. inc/typedef.h：修改中断写法适配 EIDE 编译

## 乐博车嵌入式烧录流程

需要的工具

​1.stlinkv2
​2.ph2.0 转杜邦线 5pin 双头排母一根
​3.MDK-Keil5 软件及 stm32f4xx 芯片库
5.STM32 ST-LINK Utility 软件：用于解除车上的烧写代码的保护机制