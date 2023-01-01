---
title: Algorithmic Trading Using Python Notes
date: 2023-01-01
updated: 2023-01-01
tags: [quantitative finance]
categories: [tools]
description: study notes https://youtu.be/xfzGZB4HhEE.
---

## Section 1: Algorithmic Trading Fundamentals

Algorithmic trading means using computers to make investment decisions.

#### Algorithmic Trading Landscape

- Renaissance Technology
- AQR Capital Management
- Citadel Security

#### Algorithmic Trading and Python

- Python is most popular for algorithmic trading
- Python can be slow, call functions from libraries written in other languages (e.g., Numpy)

#### Algorithmic Trading Process

1. collect data
2. develop a hypothesis for a strategy
3. backtest that strategy 
4. implement the strategy in production

backtest: formulate the strategy, see how it would have performed historically over time
- take the strategy back as far in time as one can
- across as many markets as one can 
- example: hypothesis is that the largest firm outperforms. Should test this back in time in the U.S. and also in other countries such as Canada and Japan.
 
## Section 2: Course Configuration & API (Application Programming Interface) basics

API is a way to let your software interact with others' software. This class will use the IEX Cloud API (only GET requests).

- GET: receive data from API
- POST: push data to API
- PUT: add and overwrite data to API
- DELETE: delete data from API


[list of public APIs](https://github.com/public-apis/public-apis)




