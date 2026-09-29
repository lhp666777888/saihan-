# 基于多源生态环境数据的塞罕坝生态修复效应测度、区域推广与提升路径研究

> 2026 年（第十二届）全国大学生统计建模大赛参赛作品配套仓库  
> 本仓库用于整理论文、数据、代码、图表及可复现实验流程。

## 1. 项目简介

本项目以塞罕坝生态修复过程为研究对象，整合土地利用、NDVI、北京气温与空气质量、区域空间、能源利用及内蒙古气象网格等多源数据，构建“生态修复效应评价—北京抗沙作用分析—区域推广筛选—稳健性检验”的统计建模框架。

研究主要包括四部分：

1. **生态修复效应评价**：利用生物丰富性指数、NDVI、植被覆盖率等指标刻画塞罕坝生态恢复趋势；
2. **北京环境响应分析**：结合气温、AQI 等数据，使用线性回归、Pearson 相关分析、多元回归和滞后回归分析统计关联；
3. **区域推广评价**：综合生态脆弱性、沙化风险、防风固沙需求、资源开发压力、碳汇潜力和治理可行性，对候选区域进行综合评分；
4. **稳健性检验**：通过权重扰动、评价方法替换和指标剔除检验区域推广结论的稳定性。

---

## 2. 建议的仓库结构

```text
Saihanba-Ecological-Restoration/
├── README.md
├── paper/
│   └── paper.pdf
├── data/
│   ├── Beijing.xlsx
│   ├── Saihanba.xlsx
│   └── other_data/
├── src/
│   ├── 01_data_preprocessing.py
│   ├── 02_biodiversity_index.py
│   ├── 03_ndvi_analysis.py
│   ├── 04_pearson_regression.py
│   ├── 05_region_scoring.py
│   └── 06_robustness_test.py
├── figures/
├── results/
└── requirements.txt
```

## 3. 数据说明

本研究使用的数据主要包括：

- 塞罕坝土地利用与生态恢复数据；
- NDVI 植被指数数据；
- 北京气温数据；
- 北京空气质量指数（AQI）数据；
- 候选推广区域的生态与能源数据；
- 内蒙古 2025 年气温和降水网格数据。

数据来源包括公开统计资料、国家林业和草原局、生态环境相关公开信息、天气历史数据以及 Open-Meteo Historical Weather API / ERA5-Land 再分析数据等。

> **注意**：如果原始数据、地图底图或比赛附件存在版权、授权或赛事公开限制，请不要直接上传受限原始文件，可仅上传经过整理的可公开数据、字段说明和数据来源链接。

## 4. 数据预处理

### 4.1 正向指标标准化

$$
Z_{ij}=\frac{X_{ij}-\min(X_j)}{\max(X_j)-\min(X_j)}
$$

### 4.2 负向指标标准化

$$
Z_{ij}^{-}=\frac{\max(X_j)-X_{ij}}{\max(X_j)-\min(X_j)}
$$

## 5. 核心算法与公式

### 5.1 NDVI 植被指数

$$
NDVI=\frac{NIR-R}{NIR+R}
$$

其中，$NIR$ 为近红外波段反射率，$R$ 为红光波段反射率。

### 5.2 生物丰富性指数

$$
W_t=\sum_{k=1}^{K}\alpha_k A_{k,t}
$$

其中，$A_{k,t}$ 为第 $t$ 年第 $k$ 类土地利用面积，$\alpha_k$ 为生态权重。具体权重以论文及数据处理文件中的设定为准。

### 5.3 线性回归

$$
y_i=\beta_0+\beta_1x_i+\varepsilon_i
$$

### 5.4 Pearson 相关系数

$$
r_{XY}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y}
$$

### 5.5 多元回归与滞后回归

$$
AQI_t=\beta_0+\beta_1NDVI_t+\beta_2W_t+\beta_3T_t+\varepsilon_t
$$

$$
AQI_t=\beta_0+\beta_1NDVI_{t-1}+\beta_2W_{t-1}+\beta_3T_t+\varepsilon_t
$$

> 本研究将上述结果解释为统计关联，不将相关性直接等同于因果关系。

### 5.6 熵权法

$$
p_{ij}=\frac{Z_{ij}}{\sum_{i=1}^{n}Z_{ij}}
$$

$$
e_j=-k\sum_{i=1}^{n}p_{ij}\ln p_{ij},\qquad k=\frac{1}{\ln n}
$$

$$
d_j=1-e_j
$$

$$
w_j=\frac{d_j}{\sum_{j=1}^{m}d_j}
$$

### 5.7 综合生态修复指数

$$
S_i=\sum_{j=1}^{m}w_jZ_{ij}
$$

### 5.8 区域推广适宜度评分

$$
G_i=w_EE_i+w_DD_i+w_FF_i+w_RR_i+w_CC_i+w_PP_i
$$

其中：

- $E_i$：生态脆弱性；
- $D_i$：沙化风险；
- $F_i$：防风固沙需求；
- $R_i$：资源开发压力；
- $C_i$：碳汇潜力；
- $P_i$：治理可行性。

### 5.9 稳健性检验

1. **权重扰动**：对各指标权重分别进行 $\pm10\%$ 和 $\pm20\%$ 扰动并重新归一化；
2. **方法替换**：比较基准加权法、等权法、熵权法和 TOPSIS 的排序结果；
3. **指标剔除**：逐一删除单个评价指标，并重新计算候选区域综合得分。

## 6. 主要研究结论

- 塞罕坝生物丰富性与 NDVI 整体呈上升趋势；
- 植被恢复、生物丰富性与北京部分气温、空气质量指标之间存在统计关联；
- 在候选区域中，内蒙古在生态脆弱性、沙化风险、防风固沙需求和碳汇潜力等方面表现突出；
- 权重扰动、评价方法替换和指标剔除后，核心区域排序总体保持稳定。

## 7. 运行环境

建议使用 Python 3.10 及以上版本。

```bash
pip install pandas numpy scipy matplotlib scikit-learn openpyxl
```

如涉及空间数据与地图绘制，可额外安装：

```bash
pip install geopandas shapely requests
```

## 8. 快速运行

```bash
python src/01_data_preprocessing.py
python src/03_ndvi_analysis.py
python src/04_pearson_regression.py
python src/05_region_scoring.py
python src/06_robustness_test.py
```

## 9. 项目说明

本仓库主要用于全国大学生统计建模大赛作品的研究复现、方法展示与学术交流。

如果需要引用本项目，请注明论文题目：

> 《基于多源生态环境数据的塞罕坝生态修复效应测度、区域推广与提升路径研究》

## 10. License

若比赛规则和数据授权允许公开，代码部分可选择 MIT License；数据、地图和论文中的第三方材料仍应遵循其原始来源的版权与使用要求。
