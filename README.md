# sound-loss-calculation
程序要求当前工作目录下存在四个文件夹：
文件夹     声场类型	    数学符号
ideal	     目标声场	    \(I_1\)
simu	     理论仿真声场	\(I_2\)
discrete	 离散仿真声场	\(I_3\)
MAT_data	 扫描实验声场	\(I_4\)
例如，程序发现四个文件夹都有 field01.mat，就自动提取这四个文件并进行六组比较。
如果某个文件夹不存在，程序直接停止。
如果某个文件夹没有对应文件，则跳过这一组，不影响其他文件组的计算。
普通文件：【1 1 1 1】
DASA文件：【1 255 1 1】
固定误差尺度：255
精度：double
只有涉及第四组扫描实验数据的比较，才使用扫描实验的有效区域掩膜：无效位置将替换为0（目标与扫描实验比较时，应排除这个位置；
离散仿真与扫描实验比较时，也应排除这个位置；目标与理论仿真比较时，不应该因为扫描实验缺少数据而排除这个位置。）
   组别	        比较对象	    比较模式
1	目标 vs       理论仿真	   RAW原始幅值
2	目标 vs       离散仿真	   PEAK峰值归一化
3	目标 vs       扫描实验	   PEAK峰值归一化
4	离散仿真 vs    扫描实验	 PEAK峰值归一化
5	理论仿真 vs    离散仿真	 RAW原始幅值
6	理论仿真 vs    离散仿真	 PEAK峰值归一化
12个声场评价指标；
2个数据覆盖率指标。
'PSNR','SSE','Uniformity','MaxAE','MSE','Correlation','Efficiency','MAE','SSIM','NRMSE', 'EntropyDiff','RMSE','Coverage','ROICoverage'
1. MSE：均方误差 \(\frac1{K_v}\sum(\frac{p-r}{S})^2\) 越小越好 
2. MAE：平均绝对误差 	\(\frac1{K_v}\sum|\frac{p-r}{S}|\)与MSE相比，MAE对极大误差不那么敏感 越小越好
3. RMSE：\(\sqrt{MSE}\)	均方根误差	越小越好
4. SSE:	\(\sum(\frac{p-r}{S})^2\)	总平方误差	越小越好
5. MaxAE	\(\max|\frac{p-r}{S}|\)	最大局部误差	越小越好
6. PSNR	\(10\log_{10}(1/MSE)\)	峰值信噪比	越大越好
7. NRMSE	\(\frac{RMSE}{r_{\max}-r_{\min}}\)	相对动态范围误差	越小越好
8. Correlation	皮尔逊相关系数	变化趋势相关性	越接近1越好
9. SSIM	结构相似性公式	图像结构相似程度	越接近1越好
10. Uniformity	\(1-\frac{\sigma}{\mu}\)	目标区域均匀性	越大越好
11. Efficiency	\(\frac{\sum_{\Omega}P_n^2}{\sum_Mp_n^2}\)	目标区域平方幅值集中度	越大越好
12. EntropyDiff	\(|H_R-H_P|\)	灰度统计差异	越小越好
13. Coverage	\(\frac{K_v}{MN}\)	整体有效覆盖率	越大越好
14. ROICoverage	\(\frac{|ROI\cap M|}{|ROI|}\)	目标区域有效覆盖率	越大越好
