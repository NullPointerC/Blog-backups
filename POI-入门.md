---
title: POI-入门
date: 2021-05-28 18:40:59
categories: Java
tags: [Java,backend,SpringBoot]
---

## 前言

最近接触到一个项目,这个项目要求比较特殊,需要从Excel传入数据保存到数据库再回调给前端, 所以去学习了一下怎么使用Java进行读取Excel操作

## 1. 什么是POI

### 1-1 简介

由Apache公司提供的,用`Java`编写的跨平台的Java API,提供了API给Java程序对`Microsoft Office格式`的文档读和写的功能

### 1-2 使用前提

pom.xml

```xml
<dependencies>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>3.15</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>3.15</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>
</dependencies>
```

### 1-3 POI包结构

HSSF--读写Microsoft Excel XLS

XSSF--读写Microsoft Excel OOXML XLSX

HWPF--读写Microsoft Word DOC

HSLF--读写Mircosoft PowerPoint

### 1-4 优劣势

Jxl: 消耗小,图片和图形支持有限

Poi: 功能更加完善

<hr/>

## 2. POI基本使用

### 2-1 POI封装的基本对象

XSSFWorkbook: 工作簿,指的是整个Excel文件

XSSFSheet: 工作表, 指的是一个Excel文件里的每一张sheet表

Row: 行

Cell: 单元格

### 2-2 从Excel文件中读取数据

1. 创建工作簿
2. 获取工作表
3. 遍历工作表获得行对象
4. 遍历行对象获取单元格对象
5. 获得单元格对象中值

## 3. 自己封装的一个ExcelUtils

### 3-1 ExcelUtils工具类

```java
import org.apache.poi.hssf.usermodel.HSSFDateUtil;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.FormulaEvaluator;
import org.springframework.util.StringUtils;

import java.text.DecimalFormat;
import java.text.SimpleDateFormat;

/**
 * @author Ziqiang CAO
 * @email 1213409187@qq.com
 */
public class ExcelUtils {
    private static FormulaEvaluator evaluator;
    /**
     * 获取单元格各类型值，返回字符串类型
     */
    public static String getCellValueByCell(Cell cell) {
        //格式化日期字符串
        SimpleDateFormat sdf = new SimpleDateFormat("HH:mm");
        //判断是否为null或空串
        if (cell==null || cell.toString().trim().equals("")) {
            return "";
        }
        String cellValue = "";
        int cellType=cell.getCellType();
        if(cellType==Cell.CELL_TYPE_FORMULA){ //表达式类型
            cellType=evaluator.evaluate(cell).getCellType();
        }

        switch (cellType) {
            case Cell.CELL_TYPE_STRING: //字符串类型
                cellValue= cell.getStringCellValue().trim();
                cellValue= StringUtils.isEmpty(cellValue) ? "" : cellValue;
                break;
            case Cell.CELL_TYPE_BOOLEAN:  //布尔类型
                cellValue = String.valueOf(cell.getBooleanCellValue());
                break;
            case Cell.CELL_TYPE_NUMERIC: //数值类型
                if (HSSFDateUtil.isCellDateFormatted(cell)) {  
                    //判断是否日期类型
                    cellValue = sdf.format(cell.getDateCellValue()).toString();
                } else {  //否
                    cellValue = new DecimalFormat("#.######").format(cell.getNumericCellValue());
                }
                break;
            default: //其它类型则取空串
                cellValue = "";
                break;
        }
        return cellValue;
    }
}
```

