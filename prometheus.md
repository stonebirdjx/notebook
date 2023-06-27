<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [数据模型](#%E6%95%B0%E6%8D%AE%E6%A8%A1%E5%9E%8B)
  - [metric name和tag](#metric-name%E5%92%8Ctag)
  - [样本](#%E6%A0%B7%E6%9C%AC)
- [:point_right:指标类型](#point_right%E6%8C%87%E6%A0%87%E7%B1%BB%E5%9E%8B)
  - [:point_right:Counter（计数器）](#point_rightcounter%E8%AE%A1%E6%95%B0%E5%99%A8)
  - [:point_right:Gauge（测量值）](#point_rightgauge%E6%B5%8B%E9%87%8F%E5%80%BC)
  - [:point_right:Histogram（直方图)](#point_righthistogram%E7%9B%B4%E6%96%B9%E5%9B%BE)
  - [:point_right:Summary（摘要）](#point_rightsummary%E6%91%98%E8%A6%81)
- [:point_right:Query-PromQL](#point_rightquery-promql)
  - [字符串](#%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [浮点型](#%E6%B5%AE%E7%82%B9%E5%9E%8B)
  - [序列选择器](#%E5%BA%8F%E5%88%97%E9%80%89%E6%8B%A9%E5%99%A8)
    - [:point_right:即时向量选择器](#point_right%E5%8D%B3%E6%97%B6%E5%90%91%E9%87%8F%E9%80%89%E6%8B%A9%E5%99%A8)
    - [:point_right:范围向量选择器](#point_right%E8%8C%83%E5%9B%B4%E5%90%91%E9%87%8F%E9%80%89%E6%8B%A9%E5%99%A8)
    - [:point_right:偏移量](#point_right%E5%81%8F%E7%A7%BB%E9%87%8F)
  - [:point_right:子查询](#point_right%E5%AD%90%E6%9F%A5%E8%AF%A2)
  - [:point_right:@ modifier](#point_right-modifier)
  - [:point_right:注释](#point_right%E6%B3%A8%E9%87%8A)
  - [运算符](#%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [矢量匹配](#%E7%9F%A2%E9%87%8F%E5%8C%B9%E9%85%8D)
    - [:point_right:一对一向量匹配](#point_right%E4%B8%80%E5%AF%B9%E4%B8%80%E5%90%91%E9%87%8F%E5%8C%B9%E9%85%8D)
    - [:point_right:多对一和一对多向量匹配](#point_right%E5%A4%9A%E5%AF%B9%E4%B8%80%E5%92%8C%E4%B8%80%E5%AF%B9%E5%A4%9A%E5%90%91%E9%87%8F%E5%8C%B9%E9%85%8D)
  - [:point_right:聚合操作](#point_right%E8%81%9A%E5%90%88%E6%93%8D%E4%BD%9C)
- [:point_right:函数](#point_right%E5%87%BD%E6%95%B0)
  - [abs()-样本值都转换为其绝对值](#abs-%E6%A0%B7%E6%9C%AC%E5%80%BC%E9%83%BD%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%85%B6%E7%BB%9D%E5%AF%B9%E5%80%BC)
  - [absent() - 判断时间序列是否存在](#absent---%E5%88%A4%E6%96%AD%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8)
  - [:point_right:absent_over_time() -  检查某个序列的时间数据](#point_rightabsent_over_time----%E6%A3%80%E6%9F%A5%E6%9F%90%E4%B8%AA%E5%BA%8F%E5%88%97%E7%9A%84%E6%97%B6%E9%97%B4%E6%95%B0%E6%8D%AE)
  - [ceil() - 样本值四舍五入](#ceil---%E6%A0%B7%E6%9C%AC%E5%80%BC%E5%9B%9B%E8%88%8D%E4%BA%94%E5%85%A5)
  - [:point_right:changes() - 时间序列值发生变化的次数](#point_rightchanges---%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E5%80%BC%E5%8F%91%E7%94%9F%E5%8F%98%E5%8C%96%E7%9A%84%E6%AC%A1%E6%95%B0)
  - [clamp() - 指标值限制在指定的范围内](#clamp---%E6%8C%87%E6%A0%87%E5%80%BC%E9%99%90%E5%88%B6%E5%9C%A8%E6%8C%87%E5%AE%9A%E7%9A%84%E8%8C%83%E5%9B%B4%E5%86%85)
  - [clamp_max() - 将指标值限制在最大值以下](#clamp_max---%E5%B0%86%E6%8C%87%E6%A0%87%E5%80%BC%E9%99%90%E5%88%B6%E5%9C%A8%E6%9C%80%E5%A4%A7%E5%80%BC%E4%BB%A5%E4%B8%8B)
  - [clamp_min() - 将指标值限制在最小值以上](#clamp_min---%E5%B0%86%E6%8C%87%E6%A0%87%E5%80%BC%E9%99%90%E5%88%B6%E5%9C%A8%E6%9C%80%E5%B0%8F%E5%80%BC%E4%BB%A5%E4%B8%8A)
  - [day_of_month()  - 时间戳所表示的日期的月份中的天数](#day_of_month----%E6%97%B6%E9%97%B4%E6%88%B3%E6%89%80%E8%A1%A8%E7%A4%BA%E7%9A%84%E6%97%A5%E6%9C%9F%E7%9A%84%E6%9C%88%E4%BB%BD%E4%B8%AD%E7%9A%84%E5%A4%A9%E6%95%B0)
  - [day_of_week() - 时间戳中的星期几](#day_of_week---%E6%97%B6%E9%97%B4%E6%88%B3%E4%B8%AD%E7%9A%84%E6%98%9F%E6%9C%9F%E5%87%A0)
  - [day_of_year() - 提取时间戳中的第几天](#day_of_year---%E6%8F%90%E5%8F%96%E6%97%B6%E9%97%B4%E6%88%B3%E4%B8%AD%E7%9A%84%E7%AC%AC%E5%87%A0%E5%A4%A9)
  - [days_in_month() - 提取时间戳中的第几月](#days_in_month---%E6%8F%90%E5%8F%96%E6%97%B6%E9%97%B4%E6%88%B3%E4%B8%AD%E7%9A%84%E7%AC%AC%E5%87%A0%E6%9C%88)
  - [:point_right:delta() - 计算时间序列的增量](#point_rightdelta---%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E7%9A%84%E5%A2%9E%E9%87%8F)
  - [:point_right:deriv() - 用于计算时间序列的导数](#point_rightderiv---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E7%9A%84%E5%AF%BC%E6%95%B0)
  - [exp() - 计算以e的指数函数](#exp---%E8%AE%A1%E7%AE%97%E4%BB%A5e%E7%9A%84%E6%8C%87%E6%95%B0%E5%87%BD%E6%95%B0)
  - [floor() - 向下取整](#floor---%E5%90%91%E4%B8%8B%E5%8F%96%E6%95%B4)
  - [:point_right:histogram_count()  - 用于计算指定直方图（Histogram）中的样本数量](#point_righthistogram_count----%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%8C%87%E5%AE%9A%E7%9B%B4%E6%96%B9%E5%9B%BEhistogram%E4%B8%AD%E7%9A%84%E6%A0%B7%E6%9C%AC%E6%95%B0%E9%87%8F)
  - [:point_right:histogram_sum() -  用于计算指定直方图（Histogram）中的样本总和](#point_righthistogram_sum----%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%8C%87%E5%AE%9A%E7%9B%B4%E6%96%B9%E5%9B%BEhistogram%E4%B8%AD%E7%9A%84%E6%A0%B7%E6%9C%AC%E6%80%BB%E5%92%8C)
  - [:point_right:histogram_fraction() - 用于计算指定直方图（Histogram）中位于指定分位数以下的样本的比例](#point_righthistogram_fraction---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%8C%87%E5%AE%9A%E7%9B%B4%E6%96%B9%E5%9B%BEhistogram%E4%B8%AD%E4%BD%8D%E4%BA%8E%E6%8C%87%E5%AE%9A%E5%88%86%E4%BD%8D%E6%95%B0%E4%BB%A5%E4%B8%8B%E7%9A%84%E6%A0%B7%E6%9C%AC%E7%9A%84%E6%AF%94%E4%BE%8B)
  - [histogram_quantile() - 用于计算指定直方图（Histogram）中的分位数值。](#histogram_quantile---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%8C%87%E5%AE%9A%E7%9B%B4%E6%96%B9%E5%9B%BEhistogram%E4%B8%AD%E7%9A%84%E5%88%86%E4%BD%8D%E6%95%B0%E5%80%BC)
  - [holt_winters() - 该函数可用于对时间序列数据进行趋势和"季节性"分析](#holt_winters---%E8%AF%A5%E5%87%BD%E6%95%B0%E5%8F%AF%E7%94%A8%E4%BA%8E%E5%AF%B9%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E8%BF%9B%E8%A1%8C%E8%B6%8B%E5%8A%BF%E5%92%8C%E5%AD%A3%E8%8A%82%E6%80%A7%E5%88%86%E6%9E%90)
  - [hour() - 返回时间戳中的小时](#hour---%E8%BF%94%E5%9B%9E%E6%97%B6%E9%97%B4%E6%88%B3%E4%B8%AD%E7%9A%84%E5%B0%8F%E6%97%B6)
  - [:point_right:idelta() - 用于计算时间序列的瞬时增量](#point_rightidelta---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E7%9A%84%E7%9E%AC%E6%97%B6%E5%A2%9E%E9%87%8F)
  - [increase() - 用于计算时间序列的增量。](#increase---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E7%9A%84%E5%A2%9E%E9%87%8F)
  - [irate() - 用于计算时间序列的瞬时速率](#irate---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E7%9A%84%E7%9E%AC%E6%97%B6%E9%80%9F%E7%8E%87)
  - [label_join() - 将给定标签的值使用指定的分隔符连接起来，生成一个新的标签值。](#label_join---%E5%B0%86%E7%BB%99%E5%AE%9A%E6%A0%87%E7%AD%BE%E7%9A%84%E5%80%BC%E4%BD%BF%E7%94%A8%E6%8C%87%E5%AE%9A%E7%9A%84%E5%88%86%E9%9A%94%E7%AC%A6%E8%BF%9E%E6%8E%A5%E8%B5%B7%E6%9D%A5%E7%94%9F%E6%88%90%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E6%A0%87%E7%AD%BE%E5%80%BC)
  - [label_replace() - 替换标签](#label_replace---%E6%9B%BF%E6%8D%A2%E6%A0%87%E7%AD%BE)
  - [ln() - 用于计算给定表达式的自然对数](#ln---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E7%BB%99%E5%AE%9A%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%E8%87%AA%E7%84%B6%E5%AF%B9%E6%95%B0)
  - [log2() - 2 为底数](#log2---2-%E4%B8%BA%E5%BA%95%E6%95%B0)
  - [log10() - 10为底数](#log10---10%E4%B8%BA%E5%BA%95%E6%95%B0)
  - [minute() - 时间戳分钟](#minute---%E6%97%B6%E9%97%B4%E6%88%B3%E5%88%86%E9%92%9F)
  - [month() - 时间戳月](#month---%E6%97%B6%E9%97%B4%E6%88%B3%E6%9C%88)
  - [:point_right:predict_linear() - 预测未来某个时间点的数据值](#point_rightpredict_linear---%E9%A2%84%E6%B5%8B%E6%9C%AA%E6%9D%A5%E6%9F%90%E4%B8%AA%E6%97%B6%E9%97%B4%E7%82%B9%E7%9A%84%E6%95%B0%E6%8D%AE%E5%80%BC)
  - [:point_right:rate() - 计算时间序列数据的速率](#point_rightrate---%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E9%80%9F%E7%8E%87)
  - [resets() - 用于计算时间序列数据中重置（即突然归零）的次数](#resets---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E4%B8%AD%E9%87%8D%E7%BD%AE%E5%8D%B3%E7%AA%81%E7%84%B6%E5%BD%92%E9%9B%B6%E7%9A%84%E6%AC%A1%E6%95%B0)
  - [round() - 四舍五入为最接近的整数](#round---%E5%9B%9B%E8%88%8D%E4%BA%94%E5%85%A5%E4%B8%BA%E6%9C%80%E6%8E%A5%E8%BF%91%E7%9A%84%E6%95%B4%E6%95%B0)
  - [:point_right:scalar() - 获取变量值](#point_rightscalar---%E8%8E%B7%E5%8F%96%E5%8F%98%E9%87%8F%E5%80%BC)
  - [sgn() - 判断正负](#sgn---%E5%88%A4%E6%96%AD%E6%AD%A3%E8%B4%9F)
  - [sort() - 返回按样本值升序排序的向量元素](#sort---%E8%BF%94%E5%9B%9E%E6%8C%89%E6%A0%B7%E6%9C%AC%E5%80%BC%E5%8D%87%E5%BA%8F%E6%8E%92%E5%BA%8F%E7%9A%84%E5%90%91%E9%87%8F%E5%85%83%E7%B4%A0)
  - [sort_desc() - 进行降序排序](#sort_desc---%E8%BF%9B%E8%A1%8C%E9%99%8D%E5%BA%8F%E6%8E%92%E5%BA%8F)
  - [sort_desc() - 返回每个样本值的平方根](#sort_desc---%E8%BF%94%E5%9B%9E%E6%AF%8F%E4%B8%AA%E6%A0%B7%E6%9C%AC%E5%80%BC%E7%9A%84%E5%B9%B3%E6%96%B9%E6%A0%B9)
  - [time() - 获取当前服务器时间的时间戳](#time---%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%97%B6%E9%97%B4%E7%9A%84%E6%97%B6%E9%97%B4%E6%88%B3)
  - [timestamp() - 日期时间字符串转换为对应的时间戳](#timestamp---%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%AF%B9%E5%BA%94%E7%9A%84%E6%97%B6%E9%97%B4%E6%88%B3)
  - [vector() - 标量当做向量返回](#vector---%E6%A0%87%E9%87%8F%E5%BD%93%E5%81%9A%E5%90%91%E9%87%8F%E8%BF%94%E5%9B%9E)
  - [year() - 提取年份](#year---%E6%8F%90%E5%8F%96%E5%B9%B4%E4%BB%BD)
  - [:point_right:avg_over_time(range-vector) - 用于计算时间范围内时间序列数据的平均值](#point_rightavg_over_timerange-vector---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E5%B9%B3%E5%9D%87%E5%80%BC)
  - [:point_right:min_over_time(range-vector) -用于计算时间范围内时间序列数据的最小值。](#point_rightmin_over_timerange-vector--%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E6%9C%80%E5%B0%8F%E5%80%BC)
  - [:point_right:max_over_time(range-vector)-用于计算时间范围内时间序列数据的最大值。](#point_rightmax_over_timerange-vector-%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E6%9C%80%E5%A4%A7%E5%80%BC)
  - [:point_right:sum_over_time(range-vector) - 用于计算时间范围内时间序列数据的总和](#point_rightsum_over_timerange-vector---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E6%80%BB%E5%92%8C)
  - [:point_right:count_over_time(range-vector)- 用于计算时间范围内时间序列数据的计数](#point_rightcount_over_timerange-vector--%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E8%AE%A1%E6%95%B0)
  - [:point_right:quantile_over_time(scalar, range-vector) - 用于计算时间范围内时间序列数据的分位数](#point_rightquantile_over_timescalar-range-vector---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E5%88%86%E4%BD%8D%E6%95%B0)
  - [stddev_over_time(range-vector) - 用于计算时间范围内时间序列数据的标准差](#stddev_over_timerange-vector---%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E6%A0%87%E5%87%86%E5%B7%AE)
  - [stdvar_over_time(range-vector) - 是用于计算时间范围内时间序列数据的方差的函数](#stdvar_over_timerange-vector---%E6%98%AF%E7%94%A8%E4%BA%8E%E8%AE%A1%E7%AE%97%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E6%96%B9%E5%B7%AE%E7%9A%84%E5%87%BD%E6%95%B0)
  - [:point_right:last_over_time(range-vector) -用于获取指定时间范围内每个时间点的时间序列数据的最后一个值](#point_rightlast_over_timerange-vector--%E7%94%A8%E4%BA%8E%E8%8E%B7%E5%8F%96%E6%8C%87%E5%AE%9A%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E6%AF%8F%E4%B8%AA%E6%97%B6%E9%97%B4%E7%82%B9%E7%9A%84%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%95%B0%E6%8D%AE%E7%9A%84%E6%9C%80%E5%90%8E%E4%B8%80%E4%B8%AA%E5%80%BC)
  - [:point_right:present_over_time(range-vector) -指定时间范围内的时间序列是否存在值。](#point_rightpresent_over_timerange-vector--%E6%8C%87%E5%AE%9A%E6%97%B6%E9%97%B4%E8%8C%83%E5%9B%B4%E5%86%85%E7%9A%84%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%E5%80%BC)
  - [acos(v instant-vector)](#acosv-instant-vector)
  - [acosh(v instant-vector)](#acoshv-instant-vector)
  - [asin(v instant-vector)](#asinv-instant-vector)
  - [asinh(v instant-vector)](#asinhv-instant-vector)
  - [atan(v instant-vector)](#atanv-instant-vector)
  - [atanh(v instant-vector)](#atanhv-instant-vector)
  - [cos(v instant-vector)](#cosv-instant-vector)
  - [cosh(v instant-vector)](#coshv-instant-vector)
  - [sin(v instant-vector)](#sinv-instant-vector)
  - [sinh(v instant-vector)](#sinhv-instant-vector)
  - [tan(v instant-vector)](#tanv-instant-vector)
  - [tanh(v instant-vector)](#tanhv-instant-vector)
  - [deg(v instant-vector)](#degv-instant-vector)
  - [pi()](#pi)
  - [rad(v instant-vector)](#radv-instant-vector)
- [**:point_right:HTTP** **API**](#point_righthttp-api)
  - [:point_right:健康检查](#point_right%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
  - [:point_right:就绪状态检测](#point_right%E5%B0%B1%E7%BB%AA%E7%8A%B6%E6%80%81%E6%A3%80%E6%B5%8B)
  - [:point_right:重新加载配置](#point_right%E9%87%8D%E6%96%B0%E5%8A%A0%E8%BD%BD%E9%85%8D%E7%BD%AE)
  - [:point_right:退出](#point_right%E9%80%80%E5%87%BA)
  - [其他](#%E5%85%B6%E4%BB%96)
- [prometheus系统](#prometheus%E7%B3%BB%E7%BB%9F)
  - [配置文件](#%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
  - [启动 Prometheus](#%E5%90%AF%E5%8A%A8-prometheus)
  - [:point_right:命令行参数](#point_right%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
  - [:point_right:prometheus.yml](#point_rightprometheusyml)
  - [:point_right:rule.yaml规则文件](#point_rightruleyaml%E8%A7%84%E5%88%99%E6%96%87%E4%BB%B6)
- [编写一个简单的 Exporter](#%E7%BC%96%E5%86%99%E4%B8%80%E4%B8%AA%E7%AE%80%E5%8D%95%E7%9A%84-exporter)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

Prometheus  是谷歌团队2012年以开源软件的形式进行研发的系统监控和告警工具包，自此以后，许多公司和组织都采用了 Prometheus 作为监控告警工具。

Prometheus 适用于记录文本格式的时间序列，它既适用于以机器为中心的监控，也适用于高度动态的面向服务架构的监控。在微服务的世界中，它对多维数据收集和查询的支持有特殊优势。

**主要优势**

- 由指标名称和和键/值对标签标识的时间序列数据组成的多维数据模型。

- 强大的查询语言 PromQL。

- 不依赖分布式存储；单个服务节点具有自治能力。

- 时间序列数据是服务端通过 HTTP 协议主动拉取获得的。

- 也可以通过中间网关来推送时间序列数据。

- 可以通过静态配置文件或服务发现来获取监控目标。

- 支持多种类型的图表和仪表盘。


# 数据模型

Prometheus **所有采集的监控数据均以指标（metric）的形式保存在内置的时间序列数据库当中（TSDB）**：属于同一指标名称，同一标签集合的、有时间戳标记的数据流。除了存储的时间序列，Prometheus 还可以根据查询请求产生临时的、衍生的时间序列作为返回结果。

## metric name和tag

每一条时间序列由指标名称（Metrics Name）以及一组标签（键值对）唯一标识。其中指标的名称（metric name）可以反映被监控样本的含义（例如，`http_requests_total` — 表示当前系统接收到的 HTTP 请求总量）

通过使用标签，Prometheus 开启了强大的多维数据模型：对于相同的指标名称，通过不同标签列表的集合，会形成特定的度量维度实例（例如：所有包含度量名称为 `/api/tracks` 的 http 请求，打上 `method=POST` 的标签，就会形成具体的 http 请求）。

> 指标名称只能由 ASCII 字符、数字、下划线以及冒号组成，同时必须匹配正则表达式 `[a-zA-Z_:][a-zA-Z0-9_:]*`
>
> 标签的名称只能由 ASCII 字符、数字以及下划线组成并满足正则表达式 `[a-zA-Z_][a-zA-Z0-9_]*`。

## 样本

样本由以下三部分组成：

- 指标（metric）：指标名称和描述当前样本特征的 labelsets；
- 时间戳（timestamp）：一个精确到毫秒的时间戳；
- 样本值（value）： 一个 folat64 的浮点型数据表示当前样本的值。

```bash
api_http_requests_total{method="POST", handler="/messages"}
```

# :point_right:指标类型

## :point_right:Counter（计数器）

用于记录单调递增的值，例如请求数量、错误数量等。Counter 可以增加，但不能减少(除非监控系统发生了重置)，每次增加的值必须是正数。

```go
requestsTotal := prometheus.NewCounter(prometheus.CounterOpts{
    Name: "http_requests_total",
    Help: "Total number of HTTP requests",
})
```

> 用 counter 类型的指标来表示服务的请求数、已完成的任务数、错误发生的次数等。

**:point_right:CounterVec（计数器向量）：与Counter类似，但可以为每个计数器指定多个标签。**

## :point_right:Gauge（测量值）

也称之为仪表盘：用于记录可增可减的值，例如系统内存、CPU 占用率等。Gauge 的值可以随时间增加或减少，如负载、并发连接数等。

```go
cpuUsage := prometheus.NewGauge(prometheus.GaugeOpts{
    Name: "cpu_usage",
    Help: "CPU usage percentage",
})
```

:point_right:**GaugeVec（度量向量）：与Gauge类似，但可以为每个度量指定多个标签，以便在查询时可以使用标签进行过滤。**

## :point_right:Histogram（直方图)

用于度量数据的分布情况，例如请求延迟、响应大小等。Histogram 将值划分为一些桶（bucket），每个桶表示一个数值范围。可以计算某个范围内的值出现的次数（count）和占比（percentile）。

```go
httpLatency := prometheus.NewHistogram(prometheus.HistogramOpts{
    Name: "http_request_latency_seconds",
    Help: "HTTP request latency distribution",
    Buckets: prometheus.LinearBuckets(0.01, 0.01, 10),
})
```

> 在大多数情况下人们都倾向于使用某些量化指标的平均值，例如 CPU 的平均使用率、页面的平均响应时间。

**metrics 样例**

```bash
# metrics 样本数量。
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="7.5",} 2.0
/ 在总共2次请求当中。http 请求响应时间 <=10 秒 的请求次数为 2
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="10.0",} 2.0
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="+Inf",} 2.0

# metrics 所有样本值的大小总和，命名为 `<basename>_sum`
# 实际含义： 发生的2次 http 请求总的响应时间为 13.107670803000001 秒
io_namespace_http_requests_latency_seconds_histogram_sum{path="/",method="GET",code="200",}13.107670803000001

# 样本总数，命名为 `<basename>_count`。值和 `<basename>_bucket{le="+Inf"}` 相同。
io_namespace_http_requests_latency_seconds_histogram_count{path="/",method="GET",code="200",} 2.0
```

**HistogramVec（直方图向量）：与直方图类似，但可以为每个直方图指定多个标签。**

## :point_right:Summary（摘要）

类似于 Histogram，也是用于度量数据的分布情况，但是计算方式略有不同。Summary 可以在运行时计算出特定分位数的值，并跟踪 count 和 sum。

```go
httpDuration := prometheus.NewSummary(prometheus.SummaryOpts{
    Name: "http_request_duration_seconds",
    Help: "HTTP request duration summary",
    Objectives: map[float64]float64{
        0.5: 0.05,
        0.9: 0.01,
        0.99: 0.001,
    },
})
```

> 直接存储了分位数（通过客户端计算，然后展示出来），而不是通过区间来计算。

**metrics 详情**

```bash
# 样本值的分位数分布情况，命名为 `<basename>{quantile="${}"}`
#这 12 次 http 请求中有 50% 的请求响应时间是 3.052404983s
io_namespace_http_requests_latency_seconds_summary{path="/",method="GET",code="200",quantile="0.5",} 3.052404983
# 这 12 次 http 请求中有 90% 的请求响应时间是 8.003261666s
io_namespace_http_requests_latency_seconds_summary{path="/",method="GET",code="200",quantile="0.9",} 8.003261666

# 所有样本值的大小总和，命名为 `<basename>_sum`。
# 含义：这12次 http 请求的总响应时间为 51.029495508s
io_namespace_http_requests_latency_seconds_summary_sum{path="/",method="GET",code="200",} 51.029495508

# 样本总数，命名为 `<basename>_count`。
io_namespace_http_requests_latency_seconds_summary_count{path="/",method="GET",code="200",} 12.0
```

**:point_right:SummaryVec（摘要向量）：与摘要类似，但可以为每个摘要指定多个标签**

# :point_right:Query-PromQL

Prometheus 提供了一种名为 PromQL(Prometheus Query Language) 的功能查询语言，使用户可以实时选择和汇总时间序列数据。表达式的结果可以在 Prometheus 的表达式浏览器中显示为图形和表格数据

在 Prometheus 表达式的表达语言中，一个表达式或子表达式可以计算为以下四种类型之一：

- instant vector(瞬时/即时向量)：一组时间序列，每个时间序列包含一个样本，所有数据样本共享相同的时间戳。
- Range vector(范围向量)：一组时间序列，其中包含每个时间序列随时间变化的一系列数据点
- Scalar(标量)：一个简单的数字浮点值
- String(字符串)：一个简单的字符串值。目前未使用

## 字符串

```bash
# 与 go 相同
"this is a string"
'these are unescaped: \n \\ \t'
`these are not unescaped: \n ' " \t`
```

## 浮点型

标量浮点值可以表示为`[-](digits)[.(digits)]`形式

```bash
- 2.43
```

## 序列选择器

### :point_right:即时向量选择器

标签匹配操作符如下所示：

- `=`: 选择与提供的字符串完全相同的标签(精确匹配)
- `!=`: 选择不等于提供的字符串的标签(反向匹配)
- `=~`: 选择与提供的字符串进行正则表达式匹配的标签(正则表达式匹配)
- `!~`: 选择正则表达式不匹配提供的字符串的标签(反向正则表达式匹配)

```Bash
http_requests_total{environment=~"staging|testing|development",method!="GET"}
```

### :point_right:范围向量选择器

语法上，将范围持续时间附加在向量选择器末尾的方括号(`[]`)中，以指定为每个范围向量元素提取多久的时间值。

持续时间指定为数字，紧随其后的是以下单位之一：

- `ms` - milliseconds
- `s` - 秒
- `m` - 分钟
- `h` - 小时
- `d` - 天
- `w` - 周
- `y` - 年

```Bash
# 我们选择在过去 5 分钟，数据指标名称为http_requests_total且job标签为prometheus的所有时间序列记录的所有值
http_requests_total{job="prometheus"}[5m]

# 5h
# 1h30m
# 5m
# 10s
```

### :point_right:偏移量

`offset`修饰符允许更改查询中各个瞬时向量和范围向量的时间偏移。

```Bash
# 以下表达式返回相对于当前查询时间过去 5 分钟的 http_requests_total 的值：
http_requests_total offset 5m
sum(http_requests_total{method="GET"} offset 5m)
# sum(http_requests_total{method="GET"}) offset 5m // INVALID.
```

## :point_right:子查询

```bash
rate(http_requests_total[5m])[30m:1m]
max_over_time(deriv(rate(distance_covered_total[5s])[30s:5s])[10m:])
```

## :point_right:@ modifier

 @ modifier 允许更改查询中各个瞬时向量和范围向量的评估时间。 提供给@ modifier的时间是unix时间戳并用浮点文字描述。

```bash
sum(http_requests_total{method="GET"} @ 1609746000) // GOOD.
sum(http_requests_total{method="GET"}) @ 1609746000 // INVALID.
```



## :point_right:注释

PromQL 支持以`#`开头的行注释。 例：

```Bash
# This is a comment
```

## 运算符

- `+` (addition)
- `-` (subtraction)
- `*` (multiplication)
- `/` (division)
- `%` (modulo)
- `^` (power/exponentiation)

- `atan2` (based on https://pkg.go.dev/math#Atan2)
- `==` (equal)
- `!=` (not-equal)
- `>` (greater-than)
- `<` (less-than)
- `>=` (greater-or-equal)
- `<=` (less-or-equal)
- `and` (intersection)
- `or` (union)
- `unless` (complement)
- `on`
- `ignoring`
- `group_left`
- `group_right`

## 矢量匹配

### :point_right:一对一向量匹配

**一对一**从运算符每側查找一对唯一的条目。在默认情况下，遵循`vector1 <operator> vector2`格式。如果两个条目具有完全相同的一组标签和相应的值，则它们匹配。`ignoring`关键字允许在匹配时忽略某些标签，而`on`关键字允许将所考虑的标签集减少为仅考虑提供的列表中的标签集：

```Bash
<vector expr> <bin-op> ignoring(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) <vector expr>

# 样本数据
method_code:http_errors:rate5m{method="get", code="500"}  24
method_code:http_errors:rate5m{method="get", code="404"}  30
method_code:http_errors:rate5m{method="put", code="501"}  3
method_code:http_errors:rate5m{method="post", code="500"} 6
method_code:http_errors:rate5m{method="post", code="404"} 21
method:http_requests:rate5m{method="get"}  600
method:http_requests:rate5m{method="del"}  34
method:http_requests:rate5m{method="post"} 120

# 查询示例
method_code:http_errors:rate5m{code="500"} / ignoring(code) method:http_requests:rate5m

{method="get"}  0.04            //  24 / 600
{method="post"} 0.05            //   6 / 120
```

### :point_right:多对一和一对多向量匹配

**多对一**和**一对多**向量匹配指的是"一"侧的每个向量元素可以与"多"侧的多个元素匹配的情况。必须使用`group_left`或`group_right`修饰符明确表示，其中 left/right 确定哪个向量具有更高的基数。

```Bash
<vector expr> <bin-op> ignoring(<label list>) group_left(<label list>) <vector expr>
<vector expr> <bin-op> ignoring(<label list>) group_right(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) group_left(<label list>) <vector expr>
<vector expr> <bin-op> on(<label list>) group_right(<label list>) <vector expr>

#查询实例
method_code:http_errors:rate5m / ignoring(code) group_left method:http_requests:rate5m

{method="get", code="500"}  0.04            //  24 / 600
{method="get", code="404"}  0.05            //  30 / 600
{method="post", code="500"} 0.05            //   6 / 120
{method="post", code="404"} 0.175           //  21 / 120
```

## :point_right:聚合操作

Prometheus 支持如下内置的聚合运算符，这些运算符可用于聚合单个即时向量的元素，从而产生具有聚合值的较少元素的新向量：

- `sum` (在指定维度上求和)
- `max` (在指定维度上求最大值)
- `min` (在指定维度上求最小值)
- `avg` (在指定维度上求平均值)
- `stddev` (在指定维度上求标准差)
- `stdvar` (在指定维度上求方差)
- `count` (统计向量元素的个数)
- `count_values` (统计具有相同数值的元素数量)
- `bottomk` (样本值中最小的 k个值)
- `topk` (样本值中最大的 k个值)
- `quantile` (在指定维度上统计 φ-quantile 分位数(0 ≤ φ ≤ 1))

这些运算符可以用于聚合所有标签维度，也可以通过包含`without`或`by`子句来保留不同的维度。 这些子句可以在表达式之前或之后使用。

```Bash
<aggr-op> [without|by (<label list>)] ([parameter,] <vector expression>)
或
<aggr-op>([parameter,] <vector expression>) [without|by (<label list>)]
```

`label list`是未加引号的标签列表，结尾可能包含逗号。例如`(label1, label2)`和`(label1, label2,)`都是合法的。

`without`从结果向量中删除列出的标签，而所有其它标签均保留在结果向量中。`by`恰好相反，它会删除`by`子句中未列出的标签，即使它们的标签值在向量的所有元素之间都相同。

只有`count_values`, `quantile`, `topk`和`bottomk`才需要`parameter`参数。

`count_values`为每个唯一的样本输出一个时间序列，每个序列都有一个附加标签。该标签的参数由聚合参数指定，标签值为唯一的样本值。每个时间序列的值是样本值出现的次数。

`topk`和`bottomk`与其它聚合器不同之处在于将输入样本的一个子集(包括原始标签)返回到结果向量中。`by`和`without`仅用于划分输入向量。

# :point_right:函数

## abs()-样本值都转换为其绝对值

abs(v instant-vector) 返回输入向量，其中所有样本值都转换为其绝对值。

```bash
abs(-5.2)
```

## absent() - 判断时间序列是否存在

如果该时间序列不存在，则返回一个值为 1 的时间序列；如果该时间序列存在，则返回一个空的时间序列。

```bash
absent(metric_name)
absent(nonexistent{job="myjob"})
# => {job="myjob"}
absent(nonexistent{job="myjob",instance=~".*"})
# => {job="myjob"}
absent(sum(nonexistent{job="myjob"}))
# => {}
```

> `absent()` 函数仅用于判断时间序列是否存在，并不会返回实际的时间序列数据。

## :point_right:absent_over_time() -  检查某个序列的时间数据

```bash
absent_over_time(nonexistent{job="myjob"}[1h])
# => {job="myjob"}

absent_over_time(nonexistent{job="myjob",instance=~".*"}[1h])
# => {job="myjob"}

absent_over_time(sum(nonexistent{job="myjob"})[1h:])
# => {}
```

`absent_over_time()` 函数通常用于检测在一段时间内是否缺少某个时间序列的数据。它可以用于告警规则中，以便在一段时间内缺少数据时触发告警。

## ceil() - 样本值四舍五入

ceil(v instant-vector) 将 v 中所有元素的样本值四舍五入到最接近的整数。

## :point_right:changes() - 时间序列值发生变化的次数

```bash
changes(metric_name[5m])
```

`changes()` 函数返回的是一个标量值，表示值的变化次数。如果在指定时间范围内没有变化，返回值为 0。

## clamp() - 指标值限制在指定的范围内

```bash
# clamp(<expression>, <min_value>, <max_value>)
clamp(cpu_usage, 0, 100)
```

如果 `cpu_usage` 的值小于 0，返回 0；如果 `cpu_usage` 的值大于 100，返回 100。

## clamp_max() - 将指标值限制在最大值以下

```bash
# clamp_max(<expression>, <max_value>)
clamp_max(cpu_usage, 90)
```

将 `cpu_usage` 的值限制在 90 以下。如果 `cpu_usage` 的值大于 90，返回 90；否则返回原始值。

## clamp_min() - 将指标值限制在最小值以上

```bash
# clamp_min(<expression>, <min_value>)
clamp_min(cpu_usage, 0)
```

## day_of_month()  - 时间戳所表示的日期的月份中的天数

```bash
# day_of_month(<timestamp>)
day_of_month(1622400000)
```

返回1-31

## day_of_week() - 时间戳中的星期几

```bash
# day_of_week(<timestamp>)
day_of_week(1622400000)
```

返回0-6

## day_of_year() - 提取时间戳中的第几天

```bash
# day_of_year(<timestamp>)
day_of_year(1622400000)
```

返回 1 to 365 和 1 to 366

## days_in_month() - 提取时间戳中的第几月

```bash
# days_in_month(<timestamp>)
days_in_month(1622400000)
```

返回 1 to 12

## :point_right:delta() - 计算时间序列的增量

```bash
# delta(<expression>[, <time_range>])
delta(cpu_temp_celsius{host="zeus"}[2h])
```

它可以帮助您了解指标随时间的变化情况，比如某个指标在一段时间内的增长量或速率。

## :point_right:deriv() - 用于计算时间序列的导数

```bash
# deriv(<expression>[, <time_range>])
deriv(metric_name[5m])
```

它通过计算时间序列在给定时间范围内的斜率来估计时间序列的变化速率。它可以用于监控系统的性能、流量、请求速率等指标的变化情况。

## exp() - 计算以e的指数函数

`exp(x)` 的计算方式是将参数 `x` 作为指数，计算 e 的 x 次幂。

```bash
exp(2)
```

## floor() - 向下取整

```bash
floor(3.8)  # 结果是 3
```

## :point_right:histogram_count()  - 用于计算指定直方图（Histogram）中的样本数量

```bash
histogram_count(http_request_duration_seconds)
```

如计数器（Counter）或计量器（Gauge），使用 `count` 函数来获取样本数量。

## :point_right:histogram_sum() -  用于计算指定直方图（Histogram）中的样本总和

```bash
histogram_sum(http_request_duration_seconds)
```

对于其他类型的指标，如计数器（Counter）或计量器（Gauge），使用 `sum` 函数来获取样本总和。

## :point_right:histogram_fraction() - 用于计算指定直方图（Histogram）中位于指定分位数以下的样本的比例

比例

```bash
histogram_fraction(histogram_metric{label="value"}, 0.95) # 0.95是bucket的值域
```

## histogram_quantile() - 用于计算指定直方图（Histogram）中的分位数值。

值

```bash
histogram_quantile(0.95, histogram_metric{label="value"})
```

## holt_winters() - 该函数可用于对时间序列数据进行趋势和"季节性"分析

```bash
holt_winters(v range-vector, seasonality float, trend float, smooth float)

```

- `v range-vector`：要进行预测的时间序列数据，可以是一个瞬时向量或范围向量。
- `seasonality float`：季节性周期的长度，用于描述数据的季节性变化。例如，**如果数据呈现每周循环的季节性**，则可以指定为 7。
- `trend float`：趋势参数，用于调整预测结果的趋势。
- `smooth float`：平滑参数，用于控制数据的平滑程度。

```bash
holt_winters(my_metric{job="example"}, 7, 0.1, 0.1)
```

## hour() - 返回时间戳中的小时

```bash
# hour(timestamp)
```

返回0-23

## :point_right:idelta() - 用于计算时间序列的瞬时增量

```bash
idelta(my_metric{job="my_job"})
```

对于具有非递增性质的数据，如温度、压力等，使用 `idelta()` 函数可能不会得到准确的结果。

## increase() - 用于计算时间序列的增量。

它可用于估计时间序列在指定时间范围内的总增量。

```bash
# increase(vector [duration])
increase(my_metric{job="my_job"}[5m])
```

对于非递增的时间序列，如温度、压力等，使用 `increase()` 函数可能会得到不准确的结果。

## irate() - 用于计算时间序列的瞬时速率

```bash
# irate(vector [duration])
irate(my_metric{job="my_job"}[5m])
```

对于非递增的时间序列，如温度、压力等，使用 `irate()` 函数可能会得到不准确的结果。

## label_join() - 将给定标签的值使用指定的分隔符连接起来，生成一个新的标签值。

```bash
label_join(my_metric{job="my_job"}, "_", "instance", "region", "zone")
```

## label_replace() - 替换标签

```bash
label_replace(http_requests_total, "status_code", "$1", "status_code", "200")
label_replace(<result_from_previous_step>, "status_code", "Not Found", "status_code", "404")

```

## ln() - 用于计算给定表达式的自然对数

（即以 e 为底的对数）。

```bash
ln(10)
```

## log2() - 2 为底数

```bash
log2(8)
```

## log10() - 10为底数

```bash
log10(10)
```

## minute() - 时间戳分钟

```bash
minute(1629106200)
```

0 to 59

## month() - 时间戳月

```bash
month(1629106200)
```

 1 to 12

## :point_right:predict_linear() - 预测未来某个时间点的数据值

基于线性回归模型对时间序列数据进行预测

```bash
predict_linear(metric[1h], 1h)
```

## :point_right:rate() - 计算时间序列数据的速率

```bash
rate(metric[10s])
# rate(sum(metric[1m])[5m])
```

速率的单位为数据点每秒（即数据点/s）

## resets() - 用于计算时间序列数据中重置（即突然归零）的次数

```bash
resets(metric)
```

 counters and counter-like native histograms.

## round() - 四舍五入为最接近的整数

```bash
round(expression)
```

## :point_right:scalar() - 获取变量值

```bash
scalar(metric)
# scalar(rate(metric[5m]))
```

- 如果向量表达式返回多个时间序列，`scalar` 函数将只返回第一个时间序列的标量值。
- 如果向量表达式为空或没有样本值，`scalar` 函数将返回 NaN

 标量表示**值，** 矢量表示**位置，** 张量表示**整个空间。**

单值比较：`scalar` 函数还可用于将向量表达式的标量值与其他标量值进行比较。例如，您可以使用 `scalar` 函数提取一个时间序列的标量值，并与预定义的阈值进行比较，以确定是否触发警报或采取其他操作。

## sgn() - 判断正负

```bash
sgn(metric)
```

sgn(v instant-vector) 返回一个向量，其中所有样本值都转换为其符号，定义如下：如果 v 为正，则为 1；如果 v 为负，则为 -1；如果 v 等于 0，则为 0。

## sort() - 返回按样本值升序排序的向量元素

histograms are sorted by their sum of observations.

```bash
# sort(vector_expression, sort_order="asc", sort_by="value", label="label_name") 命令待测试
sort(metric)
```

## sort_desc() - 进行降序排序

```bash
sort_desc(vector_expression)
```

## sort_desc() - 返回每个样本值的平方根

```bash
sort_desc(vector_expression)
```

## time() - 获取当前服务器时间的时间戳

```bash
time()
```

## timestamp() - 日期时间字符串转换为对应的时间戳

```bash
timestamp("2022-01-01T12:00:00Z")
```

## vector() - 标量当做向量返回

vector(s scalar) 将标量 s 作为不带标签的向量返回。

## year() - 提取年份

```bash
year(timestamp) 
```

## :point_right:avg_over_time(range-vector) - 用于计算时间范围内时间序列数据的平均值

```bash
avg_over_time(metric_name[5m])
```

您可以对时间范围内的时间序列数据进行平均值计算，以便进行数据分析、趋势观察和报警等操作。

## :point_right:min_over_time(range-vector) -用于计算时间范围内时间序列数据的最小值。

```bash
min_over_time(metric_name[5m])
```

## :point_right:max_over_time(range-vector)-用于计算时间范围内时间序列数据的最大值。

 ```bash
 min_over_time(metric_name[5m])
 ```

## :point_right:sum_over_time(range-vector) - 用于计算时间范围内时间序列数据的总和

```bash
sum_over_time(metric_name[5m])
```

## :point_right:count_over_time(range-vector)- 用于计算时间范围内时间序列数据的计数

```bash
count_over_time(metric_name[5m])
```

## :point_right:quantile_over_time(scalar, range-vector) - 用于计算时间范围内时间序列数据的分位数

the φ-quantile (0 ≤ φ ≤ 1) of the values in the specified interval.

```bash
#quantile_over_time(quantile, range vector)
quantile_over_time(0.5, metric_name[5m])
```

> `[5m]` 表示过去5分钟的时间范围。`quantile_over_time` 函数将计算该时间范围内 `metric_name` 指标的中位数，并返回一个时间序列向量。

## stddev_over_time(range-vector) - 用于计算时间范围内时间序列数据的标准差

```bash
stddev_over_time(metric_name[5m])
```

## stdvar_over_time(range-vector) - 是用于计算时间范围内时间序列数据的方差的函数

```bash
stdvar_over_time(metric_name[5m])
```

## :point_right:last_over_time(range-vector) -用于获取指定时间范围内每个时间点的时间序列数据的最后一个值

```bash
last_over_time(metric_name[5m])
```

## :point_right:present_over_time(range-vector) -指定时间范围内的时间序列是否存在值。

```bash
present_over_time(metric_name[5m])
```

## acos(v instant-vector)

calculates the arccosine of all elements in v (special cases).

## acosh(v instant-vector)

calculates the inverse hyperbolic cosine of all elements in v (special cases).

## asin(v instant-vector)

calculates the arcsine of all elements in v (special cases).

## asinh(v instant-vector)

 calculates the inverse hyperbolic sine of all elements in v (special cases).

## atan(v instant-vector)

 calculates the arctangent of all elements in v (special cases).

## atanh(v instant-vector)

calculates the inverse hyperbolic tangent of all elements in v (special cases).

## cos(v instant-vector)

calculates the cosine of all elements in v (special cases).

## cosh(v instant-vector)

calculates the hyperbolic cosine of all elements in v (special cases).

## sin(v instant-vector)

calculates the sine of all elements in v (special cases).

## sinh(v instant-vector)

calculates the hyperbolic sine of all elements in v (special cases).

## tan(v instant-vector)

calculates the tangent of all elements in v (special cases).

## tanh(v instant-vector)

calculates the hyperbolic tangent of all elements in v (special cases).

## deg(v instant-vector)

converts radians to degrees for all elements in v.

## pi()

returns pi.

## rad(v instant-vector)

converts degrees to radians for all elements in v.

# **:point_right:HTTP** **API**

## :point_right:健康检查

```Bash
GET /-/healthy
```

## :point_right:就绪状态检测

```Bash
GET /-/ready
```

## :point_right:重新加载配置

```Bash
PUT  /-/reload
POST /-/reload
```

## :point_right:退出

```Bash
PUT  /-/quit
POST /-/quit
```

## 其他

在 Prometheus 服务的`/api/v1`下可以访问当前稳定的 HTTP API。

# prometheus系统

## 配置文件

prometheus.yml 

```bash
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

## 启动 Prometheus

```bash
# Start Prometheus.
# By default, Prometheus stores its database in ./data (flag --storage.tsdb.path).
./prometheus --config.file=prometheus.yml
```

## :point_right:命令行参数

```bash
-config.file: 指定Prometheus的配置文件路径，默认值为./prometheus.yml。
-web.listen-address: 指定Prometheus的监听地址和端口，默认值为localhost:9090。
-web.external-url: 指定Prometheus的外部访问地址和端口，默认值为http://localhost:9090。
-storage.tsdb.path: 指定Prometheus数据存储路径，默认值为./data。
-storage.tsdb.retention: 指定Prometheus数据保留时间，格式为<duration>s|m|h|d|w|y，默认值为15d。
-alertmanager.url: 指定Alertmanager的访问地址和端口，默认值为http://localhost:9093。
-query.lookback-delta: 指定Prometheus查询时的向前滚动时间，格式为<duration>s|m|h|d|w|y，默认值为5m。
-query.timeout: 指定Prometheus查询的超时时间，格式为<duration>s|m|h|d|w|y，默认值为2m。
```

## :point_right:prometheus.yml

```bash
global:
  scrape_interval: 15s   # 默认的抓取间隔时间
  evaluation_interval: 15s  # 默认的规则计算时间间隔

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']  # prometheus自身的地址

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']  # node_exporter的地址

  - job_name: 'blackbox_exporter'
    metrics_path: /probe
    params:
      module: [http_2xx]  # 用于http探针检查的模块
    static_configs:
      - targets:
          - http://localhost:9090
          - http://localhost:9100
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: blackbox_exporter:9115  # blackbox_exporter的地址

rule_files:
  - rules.yml   # 规则文件的路径
```

上述配置文件中包含了以下几个配置项：

- `global`：定义了全局配置项，比如默认的抓取间隔时间和规则计算时间间隔等。
- `scrape_configs`：定义了需要抓取的数据源和相应的配置信息。在该示例中，定义了三个数据源：`prometheus`自身、`node_exporter`和`blackbox_exporter`，并分别指定了相应的目标地址、抓取路径和抓取参数等信息。
- `rule_files`：定义了需要加载的规则文件路径。

## :point_right:rule.yaml规则文件

record：定义一个新的指标，通常是在原有指标的基础上进行计算、聚合等操作，最终生成一个新的指标。

```yaml
  - record: my_new_metric
    expr: sum(my_metric) by (label_name)
```

alert：定义一个报警规则，当满足某些条件时触发报警。

```yaml
  - alert: HighErrorRate
    expr: sum(rate(http_requests_total{status="500"}[5m])) by (job) / sum(rate(http_requests_total[5m])) by (job) > 0.5
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: High error rate detected
      description: "{{ $value }}% of requests to {{ $labels.job }} resulted in 5xx errors."
```

group：将多个rule分组，方便管理。

```yaml
groups:
  - name: my-rules
    rules:
      - record: my_new_metric
        expr: sum(my_metric) by (label_name)
      - alert: HighErrorRate
        expr: sum(rate(http_requests_total{status="500"}[5m])) by (job) / sum(rate(http_requests_total[5m])) by (job) > 0.5
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: High error rate detected
          description: "{{ $value }}% of requests to {{ $labels.job }} resulted in 5xx errors."
```

# 编写一个简单的 Exporter

```go
package main

import (
    "log"
    "net/http"

    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promhttp"
)

var (
    //使用GaugeVec类型可以为监控指标设置标签，这里为监控指标增加一个标签"device"
    speed = prometheus.NewGaugeVec(prometheus.GaugeOpts{
        Name: "disk_available_bytes",
        Help: "Disk space available in bytes",
    }, []string{"device"})

    tasksTotal = prometheus.NewCounter(prometheus.CounterOpts{
        Name: "test_tasks_total",
        Help: "Total number of test tasks",
    })

    taskDuration = prometheus.NewSummary(prometheus.SummaryOpts{
        Name: "task_duration_seconds",
        Help: "Duration of task in seconds",
        //Summary类型的监控指标需要提供分位点
        Objectives: map[float64]float64{0.5: 0.05, 0.9: 0.01, 0.99: 0.001},
    })

    cpuTemperature = prometheus.NewHistogram(prometheus.HistogramOpts{
        Name: "cpu_temperature",
        Help: "The temperature of cpu",
        //Histogram类型的监控指标需要提供Bucket
        Buckets: []float64{20, 50, 70, 80},
    })
)

func init() {
    //注册监控指标
    prometheus.MustRegister(speed)
    prometheus.MustRegister(tasksTotal)
    prometheus.MustRegister(taskDuration)
    prometheus.MustRegister(cpuTemperature)
}

func main() {
    //模拟采集监控数据
    fakeData()

    //使用prometheus提供的promhttp.Handler()暴露监控样本数据
    //prometheus默认从"/metrics"接口拉取监控样本数据
    http.Handle("/metrics", promhttp.Handler())
    log.Fatal(http.ListenAndServe(":10000", nil))
}

func fakeData() {
    tasksTotal.Inc()
    //设置该条样本数据的"device"标签值为"/dev/sda"
    speed.With(prometheus.Labels{"device": "/dev/sda"}).Set(82115880)

    taskDuration.Observe(10)
    taskDuration.Observe(20)
    taskDuration.Observe(30)
    taskDuration.Observe(45)
    taskDuration.Observe(56)
    taskDuration.Observe(80)

    cpuTemperature.Observe(30)
    cpuTemperature.Observe(43)
    cpuTemperature.Observe(56)
    cpuTemperature.Observe(58)
    cpuTemperature.Observe(65)
    cpuTemperature.Observe(70)
}
```

