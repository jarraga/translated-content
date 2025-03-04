---
title: PerformanceTiming.redirectStart
slug: Web/API/PerformanceTiming/redirectStart
---
{{APIRef("Navigation Timing")}}

## 概要

**`PerformanceTiming.redirectStart`** 是一个返回代表一个时刻的 `unsigned long long` 型只读属性，为第一个 HTTP 的重定向开始的时刻的 Unix 毫秒时间戳。如果重定向没有发生，或者其中一个重定向非同源，则该值返回 `0。`

## 语法

```plain
time = performanceTiming.redirectStart;
```

## 规范

因为 [Navigation Timing 规范](https://w3c.github.io/navigation-timing/#obsolete)已被弃用，此特性不再有望成为标准。请使用 {{domxref("PerformanceNavigationTiming")}} 接口代替。

## 浏览器兼容性

{{Compat}}

## 参见

- 它属于 {{domxref("PerformanceTiming")}} 接口。
