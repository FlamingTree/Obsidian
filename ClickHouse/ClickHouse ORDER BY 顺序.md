#ClickHouse

# 如何选择 ORDER BY 顺序

假设有两列 column1, column2 的基数分别为 $C_1$、$C_2$，其中 $C_1 \lt C_2$，索引粒度 index_granularity 值为 $G$，查询条件分别为 `column1 = val1`, `column2 = val2`，如何选择 ORDER BY 顺序，使查询的成本最低？

使用需要读取的 Block 数量衡量查询成本。

|                | (column1, column2) | (column2, column1) |
| -------------- | ------------------ | ------------------ |
| column1 = va1  |                    |                    |
| column2 = val2 |                    |                    |

1. 如果 $C_1 \times C_2 \le G$，每次查询都需要读取所有的 Block（只有 1 个 Block），查询成本相同

   |                | (column1, column2) | (column2, column1) |
   | -------------- | ------------------ | ------------------ |
   | column1 = va1  | $1$                | $1$                |
   | column2 = val2 | $1$                | $1$                |

2. 如果 $C_1 \times C_2 \gt G, C_1 \lt C_2 \le G$，查询成本相同

   |                | column1, column2                         | column2, column1                         |
   | -------------- | ---------------------------------------- | ---------------------------------------- |
   | column1 = va1  | $1 \le v \le 2$                          | $\lceil \frac{C_1 \times C_2}{G} \rceil$ |
   | column2 = val2 | $\lceil \frac{C_1 \times C_2}{G} \rceil$ | $1 \le v \le 2$                          |

3. 如果 $C_1 \times C_2 \gt G, C_1 \le G \lt C_2$，(column1, column2) 查询成本较低

   |                | column1, column2              | column2, column1                         |
   | -------------- | ----------------------------- | ---------------------------------------- |
   | column1 = va1  | $\lceil \frac{C_2}{G} \rceil$ | $\lceil \frac{C_1 \times C_2}{G} \rceil$ |
   | column2 = val2 | $C_1$                         | $1 \le v \le 2$                          |

   推导过程：

   假设 $C_2 = k*G, k \gt 1$
   
   $$
   \begin{aligned}
   Cost_{(column1, column2)} &= {\lceil \frac{C_2}{G} \rceil + C_1} \approx {k + C_1}
   \\
   Cost_{(column2, column1)} &= {\lceil \frac{C_1 \times C_2}{G} \rceil + 1} \approx {k*C_1 + 1}
   \\
   Cost_{(column1, column2)} - Cost_{(column1, column2)} &= (k + C_1) - (k * C_1 + 1)
   \\&= (k-1) - (k-1)*C_1
   \\&= (k-1)*(1-C_1) \lt 0
   \end{aligned}
   $$

4. 如果 $C_1 \times C_2 \gt G, G \lt C_1 \lt C_2$，(column1, column2) 查询成本较低

   |                | column1, column2              | column2, column1              |
   | -------------- | ----------------------------- | ----------------------------- |
   | column1 = va1  | $\lceil \frac{C_2}{G} \rceil$ | $C_2$                         |
   | column2 = val2 | $C_1$                         | $\lceil \frac{C_1}{G} \rceil$ |

   推导过程：

   假设 $C_1 = k_1*G, C_2= k_2*G, k_2 \gt k_1 \gt 1$
   
   $$
   \begin{aligned}
      Cost_{(column1, column2)} &= {\lceil \frac{C_2}{G} \rceil + C_1} \approx {k_2 + k_1*G}
      \\
      Cost_{(column2, column1)} &= {\lceil \frac{C_1}{G} \rceil + C_2} \approx {k_1 + k_2*G}
      \\
      Cost_{(column1, column2)} - Cost_{(column1, column2)} &= (k_2 + k_1*G) - (k_1 + k_2*G)
      \\&= (k_2-k_1)-(k_2-k_1)*G
      \\&= (k_2-k_1)*(1-G) \lt 0
      \end{aligned}
   $$

