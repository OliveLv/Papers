# A novel method for identifying behavioural changes in animal movement data
###**词语翻译**
* movement data 移动（运动）数据
* gappiness 间隙
* BCPA 行为变化点分析
* compass orientation 方位
* turning angles 转角
* persistence velocity 持久速率
* MLCP 极大似然变化点  

###**相关定义**
- 平稳随机过程  
指统计特征不随时间平移而变化，即统计特征与时间起点的选择无关。  
现实生活中，很难会有严格满足平稳随机过程条件的情况，因此引入了广义平稳随机过程。对于时间序列{X(t)}，称其为广义平稳随机过程，当且仅当：  
   
   <img src="http://www.forkosh.com/mathtex.cgi?  E(X(t))=m_x">  
   <img src="http://www.forkosh.com/mathtex.cgi?  R_x (t_1,t_2 )=R_x (\tau)=E[X(t_1 )X(t_2 )]">  
其中，<img src="http://www.forkosh.com/mathtex.cgi?  m_x">为常数，<img src="http://www.forkosh.com/mathtex.cgi?  \tau">表示时间间隔，<img src="http://www.forkosh.com/mathtex.cgi?  R_x (t_1,t_2 )">为自相关函数，反映随机过程X(t)在任意两个不同时刻之间的相关程度。

- 平稳高斯过程  
根据中心极限定理可知，大量独立同分布的随机变量之和，其分布区域高斯分布。该方法称为随机过程的高斯化。

- 一阶自回归模型AR(1)
假设<img src="http://www.forkosh.com/mathtex.cgi?  {X_t}">是平稳、正态、零均值的时序，有  

   <img src="http://www.forkosh.com/mathtex.cgi?  X_t=\phi_1*X_{(t-1)}+\varepsilon_t,\varepsilon_t\sim NID(0,\sigma^2)">  
其中，<img src="http://www.forkosh.com/mathtex.cgi?  \phi_1"> 指一阶自相关函数，<img src="http://www.forkosh.com/mathtex.cgi?  \varepsilon_t">为t时刻的随机误差，它满足正态分布。

- **AR(1)与一元线性回归模型的区别**  
一元线性回归模型表达了在相同时间t时，一个随机变量与其他随机变量之间的关系，是一个静态模型，即对随机变量的静态描述；而AR(1)表达了在不同时间t时，一个随机过程本身的观测数据之间的关系，即表达了时间序列内部的相关关系，是一个动态模型，即对随机变量的动态描述。

###**正文**
####**Abstract**
论文中将移动数据看作来自于一个连续随机过程的子采样，并介绍了BCPA算法，一种基于似然的方法，它可以分析出显著性结构变化。BCPA算法对存在(时空)间隙和测量误差都具有鲁棒性，同时可以找出那些不易被察觉的结构。

####**Introduction**
####**Methods**
+ Orthogonal decomposition of movement data(正交分解)
原始移动数据包括n+1个观测数据，表示在时间T时的绝对位置为Z={X,Y}。将原来的绝对位置Z和绝对方位<img src="http://www.forkosh.com/mathtex.cgi?  \phi">转换为速度<img src="http://www.forkosh.com/mathtex.cgi?  V">和转角<img src="http://www.forkosh.com/mathtex.cgi?  \Psi">：   

    <img src="http://www.forkosh.com/mathtex.cgi?  V(T_i)=||Z_i-Z{(i-1)}||/||T_i-T_{(i-1)}||">  
    <img src="http://www.forkosh.com/mathtex.cgi?  \Psi(T_i)=\phi_i-\phi_{(i-1)}">   
 再将两者进行正交分解，得到持久速率<img src="http://www.forkosh.com/mathtex.cgi?  V_P (t)">和转角速率<img src="http://www.forkosh.com/mathtex.cgi?  V_t (t)">  
 
	<img src="http://www.forkosh.com/mathtex.cgi?  V_P (T_i )=V(T_i)cos(\Psi(T_i)) ">   
	<img src="http://www.forkosh.com/mathtex.cgi?  V_t (T_i )=V(T_i )sin(\Psi(T_i))">  
    
 其中，<img src="http://www.forkosh.com/mathtex.cgi?  V_P"> 捕捉在一个给定方向上持续移动的倾向和大小，<img src="http://www.forkosh.com/mathtex.cgi?  V_t"> 捕捉在给定时间区域内朝垂直方向移动的趋势。此外，这些变量易于构建平稳的高斯自回归时间序列模型。通过直方图或Q-Q图展示移动数据得到经验型结果显示两个变量可有混合正态分布表示，且的<img src="http://www.forkosh.com/mathtex.cgi?  V_t">均值接近为0。
 虽然进行正交分解，但实际中两者不是相互独立的。在论文中，在相互独立的情况下分别对两个变量进行分析。

+ Autocorrelated time-series model（自相关时间序列模型）  
假设持久速率<img src="http://www.forkosh.com/mathtex.cgi?  V_P">是来自一个连续时空，平稳的高斯过程<img src="http://www.forkosh.com/mathtex.cgi?  W(t)">的样本，同时<img src="http://www.forkosh.com/mathtex.cgi?  W(t)">具有以下属性：   

	<img src="http://www.forkosh.com/mathtex.cgi?  W(0)=W_0,">  
	<img src="http://www.forkosh.com/mathtex.cgi?  E[W(t)]=\mu,">  
	<img src="http://www.forkosh.com/mathtex.cgi?  Var[W(t)]=\sigma^2,">  
	<img src="http://www.forkosh.com/mathtex.cgi?  Corr[W(t),W(t-\tau)]=\rho^\tau">  
其中<img src="http://www.forkosh.com/mathtex.cgi?  0<\rho<1">是时滞1(即两事件间隔为1)的一阶自相关。  
对于来自连续过程<img src="http://www.forkosh.com/mathtex.cgi?  W=\{W_1,\ldots,W_n \}">上在时刻<img src="http://www.forkosh.com/mathtex.cgi?  T=\{t_1,\ldots,t_n \}">上的n个观测点，则<img src="http://www.forkosh.com/mathtex.cgi?  W_i"> 可描述为  

    <img src="http://www.forkosh.com/mathtex.cgi?  W_i=\mu+\rho^{\tau_i}(W_{(i-1)}-\mu)+\varepsilon_i">,  
其中<img src="http://www.forkosh.com/mathtex.cgi?  i\in\{1,\ldots,n \},\tau_i=t_i-t_{(i-1)},\varepsilon_i\sim N(0,\sigma^2)(1-\rho^{(2\tau_i)})">。  
论文中所给的<img src="http://www.forkosh.com/mathtex.cgi?  \varepsilon_i"> 的方差感觉有错  
对转角速率<img src="http://www.forkosh.com/mathtex.cgi?  V_t (t)">的分析过程与持久速率<img src="http://www.forkosh.com/mathtex.cgi?  V_P ">相同

+ Estimating irregular time-series parameters（估计不规则的时间序列参数）  
采用极大似然法估计参数<img src="http://www.forkosh.com/mathtex.cgi?  \hat{\mu},\hat{\sigma},\hat{\rho} ">  

   <img src="http://www.forkosh.com/mathtex.cgi?  \hat{\mu}=\overline{X}">  
   <img src="http://www.forkosh.com/mathtex.cgi?  \hat{\sigma}=S ">  
   <img src="http://www.forkosh.com/mathtex.cgi?  \hat{\rho}=arg max_\rho L(\rho|W,T,\hat{\mu},\hat{\sigma})">  
   <img src="http://www.forkosh.com/mathtex.cgi? L(\rho|W,T,\hat{\mu},\hat{\sigma})=\prod\limits_{(i=1)}^nf(W_i|W_{(i-1)},\tau_i,\rho,\hat{\mu},\hat{\sigma})">  
   <img src="http://www.forkosh.com/mathtex.cgi?  f(W_i|W_{(i-1)})\sim \varepsilon_i">            

+ Identifying structural shifts  
定义CP：
在时间<img src="http://www.forkosh.com/mathtex.cgi?  0<t<T">上的一个连续随机过程<img src="http://www.forkosh.com/mathtex.cgi?  X(t)">，定义参数集合<img src="http://www.forkosh.com/mathtex.cgi?  \Theta(t)">，在未知点处<img src="http://www.forkosh.com/mathtex.cgi?  T^*"> 发生变化  
<img src="http://www.forkosh.com/mathtex.cgi?  \Theta(t)=\begin{cases}\Theta_1&\text{$0<t\leq T^*$}\\\,\Theta_2&\text($T^*<t\leq T$)\end{cases}">  
	从连续过程<img src="http://www.forkosh.com/mathtex.cgi?  X(t)">在时间<img src="http://www.forkosh.com/mathtex.cgi?  T_i"> 上选择一个时间序列<img src="http://www.forkosh.com/mathtex.cgi?  X_i"> 。设n为第一区域的测量值，满足<img src="http://www.forkosh.com/mathtex.cgi?  T_n\equiv max(T_i<T^*)">，则  
	<img src="http://www.forkosh.com/mathtex.cgi?  L(\Theta|X,T)=\prod\limits_{(i=1)}^nf(X_i|X_{(i-1)},\Theta_1)\prod\limits_{(j=i+1)}^Nf(X_j|X_{(j-1)},\Theta_2)">  
采用极大似然法估计下列参数  

    <img src="http://www.forkosh.com/mathtex.cgi?  \hat{n}=arg max_n L(\Theta|X,T)">  
	<img src="http://www.forkosh.com/mathtex.cgi?  \hat{\mu}_j=\overline{X}_j">  
	<img src="http://www.forkosh.com/mathtex.cgi?  \hat{\sigma}_j=S_j">  
    <img src="http://www.forkosh.com/mathtex.cgi?  \hat{\rho}_j=arg max_\rho L(\rho|X_j,T_j,\hat{\mu}_j,\hat{\sigma}_j)">  
	其中，<img src="http://www.forkosh.com/mathtex.cgi?  j=1,2">，表示所属的区域。称<img src="http://www.forkosh.com/mathtex.cgi?  T^*=\hat{t}_n"> 为极大似然变化点(MLCP)

+ Identifying models  
不同参数的变化对应于不同的行为解释。对于持久速率<img src="http://www.forkosh.com/mathtex.cgi? V_P">，<img src="http://www.forkosh.com/mathtex.cgi?  \mu">的增加对应更快更有方向的移动，<img src="http://www.forkosh.com/mathtex.cgi?  \sigma">的增加显示更多变的移动，高<img src="http://www.forkosh.com/mathtex.cgi?  \rho">显示更强的相关性移动。对于转角速率<img src="http://www.forkosh.com/mathtex.cgi?  V_t">，<img src="http://www.forkosh.com/mathtex.cgi?  \mu">的增加显示更多转角，高<img src="http://www.forkosh.com/mathtex.cgi?  \rho">显示更大的转角直径。  
在分析一个CP时，需要考虑8中可能的模型，记为M0-M7，其中M0指三个变量没有发生变化，M1，M2，M3指一个变量发生变化，M4，M5，M6指两个变量发生变化，M7指三个变量都发生变化。
采用信息准则AIC和BIC选择模型。

    <img src="http://www.forkosh.com/mathtex.cgi?  I_A(X,T)=-2n\log(L(\Theta|X,T))+2d">  
    <img src="http://www.forkosh.com/mathtex.cgi?  I_B(X,T)=-2n\log(L(\Theta|X,T))+d\log(n)">  
    
  其中，d为M0-M7的参数个数，M0对应d=3，M7对应d=6（论文中感觉d的值不正确，如下图所示）

+ Simulation Study
+ Multiple change points
考虑存在多个变化点（CP）的情况，对参数的估计是一个非平凡问题，比较复杂的部分是在一个复杂的过程中确定变化点的数目。论文中采用一个固定大小的窗口遍历整个时间序列，每个窗口中只包含一个变化点。完整的处理过程为：
	1.  选择窗口长度 <img src="http://www.forkosh.com/mathtex.cgi?  30\leq l<N">
	2. 在窗口中找到MLCP
	3. 根据参数<img src="http://www.forkosh.com/mathtex.cgi?  \mu,\sigma,\rho">，使用BIC准则确定是否接受具有显著性变化
	4. 根据3的结果，记录行为变化点的位置和最终估计得参数值
5. 移动窗口，重复以上步骤

####**Application to data: Northern fur seal**
####**Discussion**
BCPA不需要先验假设，可以检测出显著的行为变化，并以参数值的方式表示逐渐变化的情况。具有较强的鲁棒性，只要变化比噪声显著，该方法就可以从充满错误的数据中发现行为变化点，且对具有测量时间的不规律或者数据中存在间隙的情况下也适用。  
<p>BCPA中，**需要对连续时间序列模型的假设进行检验**，同时需要人工**确定窗口大小**，窗口越大，模型选择在辨别更小范围的行为变化所需的成本越大。
该方法提供了一个通用的、鲁棒性、高效的框架来研究时间上存在异质的自相关过程。</p>