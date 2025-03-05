# paper_GSA_compare

Title: Comparison of global sensitivity analysis methods for a fire spread model with a segmented characteristic


Author: Shi-Shun Chen, Xiao-Yang Li


School of Reliability and Systems Engineering, Beihang University, Beijing 100191, China

Code: All of our source codes are publicly available at https://github.com/dirge1/GSA_segmented.

# Abstract
Global sensitivity analysis (GSA) can provide rich information for controlling output uncertainty. In practical applications, segmented models are commonly used to describe an abrupt model change. For segmented models, the complicated uncertainty propagation during the transition region may lead to different importance rankings of different GSA methods. If an unsuitable GSA method is applied, misleading results will be obtained, resulting in suboptimal or even wrong decisions. In this paper, four GSA indices, i.e., Sobol index, mutual information, delta index and PAWN index, are applied for a segmented fire spread model (Dry Eucalypt). The results show that four GSA indices give different importance rankings during the transition region since segmented characteristics affect different GSA indices in different ways. We suggest that analysts should rely on the results of different GSA indices according to their practical purpose, especially when making decisions for segmented models during the transition region.

Keywords: Global sensitivity analysis; Piecewise model; Variance-based method; Moment-independent method; Decision making
# Introduction
Computational models are always adopted to describe real systems. However, due to data sparsity, environmental noise or lack of knowledge, uncertainties are always involved in model inputs, resulting in uncertain model outputs [1]. At this point, global sensitivity analysis (GSA) is a powerful tool to assess the impact of the uncertainty in each input variable on the uncertainty of model output [2]. With the help of GSA, analysts can find out effective ways of model calibration, uncertainty reduction and risk control [3-5]. As a result, GSA has received extensive attention in practical applications, such as pandemic progression [6], environmental dynamics [7], and engineering design [8, 9].


Among all the GSA indices in the literature, variance-based methods are the most widely used and well-proven approaches [10-12]. Nevertheless, they rely on the assumption that variance is sufficient to describe the output uncertainty. Since variance-based methods only consider the second-order moment, some information is inevitably lost when output uncertainty is solely represented with variance [13]. To overcome this limitation, numerous moment-independent GSA methods have been proposed. The first representative moment-independent method is the delta index developed by Borgonovo [14]. The delta index is a normalized index that quantifies the sensitivity based on the area difference between the conditional and marginal probability density function (PDF) of output. The mutual information derived from information theory is an alternative moment-independent GSA approach [15], which can quantify the reduction in the uncertainty of the target variable (i.e., model output) when controlling the uncertainty of a source variable (i.e., model input) based on the conditional and marginal PDF of output. Further, Pianosi and Wagener [16] proposed another moment-independent GSA measure based on the conditional and marginal cumulative distribution function (CDF) of output, called PAWN index, to avoid the estimation of PDFs, thus improving the robustness of the index and cutting down the computation time. For detailed introduction of other GSA approaches, interested readers are referred to [17].


Due to the existence of numerous GSA methods, the pros and cons of different GSA methods are of great interest. In this regard, scholars have conducted many comparative studies. For instance, Ikonen [18] and Li et al. [19] compared the variance-based method, delta index and the elementary effects method by application to fuel behavior modeling and thermal hydraulic phenomena, respectively. Iooss and Prieur [20] and Vuillod et al. [21] compared the Shapley effect with the variance-based method for models with independent and correlated inputs, respectively. They found that different GSA methods had similar results for variables with higher importance. Rabitti and Borgonovo [22] compared the delta index with the Shapley effect for a mathematical annuity model. They demonstrated that both methods could give reliable answers in complicated case with correlated inputs. Upreti et al. [23] and Khorashadi et al. [24] compared the PAWN index with the variance-based method for the Aquacrop model and SWAT model, respectively. They concluded that when the output PDF was highly-skewed, both methods could identify the non-influential variables, and the PAWN index could quantify the relative importance of the influential variables more effectively than the variance-based methods.


In the above studies, scholars mainly focus on the computational efficiency and convergence of the GSA indices and different GSA methods get similar importance rankings. Whereas, in some cases, the results obtained by different GSA methods can be quite different. For example, Borgonovo [25] compared the delta index with the variance-based method for a risk assessment model. He revealed that the influential variables identified by the variance-based methods were not always consistent with the ones that influenced the entire output distribution the most. Therefore, applying moment-independent methods, like the delta index, was necessary for decision making. In [26], the authors compared the entropy-based importance measure with the variance-based method, mutual information and delta index for the Ishigami function. They found that different GSA methods get different importance rankings due to the undesirable characteristics of the output PDF.


The above research shows that different GSA methods may get different importance rankings. However, in practical applications, most analysts tend to use the GSA method that they are comfortable with, rather than the method that best suits the purpose and problem at hand [17]. If different GSA indices give different importance rankings for the target model and an unsuitable GSA index is applied, misleading results will be obtained, which will lead to suboptimal or even wrong decisions. Therefore, it is crucial for analysts to know the applicability of different GSA methods to their target model.


In this study, we focus on the implementation of GSA on a special class of mathematical models, i.e. segmented models. Segmented models are commonly adopted to describe the behavior of some special systems whose input-output relationship may be changed when certain indicator exceeds the corresponding critical threshold. For example, many scholars suggested that with the increase in wind speed, the influence of atmospheric conditions on fire spread would change [27, 28]. As a result, the critical threshold of wind speed should be identified and different models need to be developed for different stages.


For segmented models, due to the uncertainty in the indicator or threshold, there is a transition region where the behavior of the system cannot be accurately determined. During the transition region, the complicated uncertainty propagation may lead to some undesirable characteristics in the output PDF (e.g., highly-skewed or heavily-tailed). Hence, it is important to discuss and compare the results of different GSA methods on the segmented model critically, so as to provide credible information for decision making. Indeed, some researchers have conducted GSA for segmented models. However, most of the relevant studies only utilized the variance-based method. For instance, Spiessl and Becker [29] compared three computationally efficient variance-based methods and three sampling schemes by application to a final repository model with quasi-discrete behavior. Liang et al. [30] conducted GSA for four representative nitrogen uptake models of crops by the variance-based method. In particular, the DAISY model considered in their research was a three-stage segmented model, in which the nitrogen uptake corresponded to different models at different accumulated temperature levels. Lamboni et al. [31] employed the variance-based method to perform GSA for a dynamic crop model, where the denitrification process was a two-stage segmented model determined by temperature. In [32], the authors developed a simple model for the establishment of tick-borne pathogens where the growth mechanism of larvae and nymphs was changed at different periods, corresponding to different models. Then, they carried out GSA for the model by the variance-based approach.


With the development of the moment-independent methods, recently, Kc et al. [33] analyzed a segmented fire spread model using the PAWN index and compared the GSA results with the variance-based ones. Their results illustrated that the PAWN index should be prioritized when computational resources are limited, while the variance-based approach was able to provide more detailed information about uncertainty propagation. Nonetheless, their study only focused on the second stage of the fire spread model rather than the transition region between the two stages, which means that they did not consider the impact of segmented characteristics on the GSA results. Table 1 provides a summary of the above literature reviews. As a result, to the best of our knowledge, none of the existing studies discuss and compare the results of different GSA methods on segmented models, especially in the transition region.


Table 1  Summary of the literature reviews.
Literature	Sobol index	Delta index	Mutual information	PAWN index	Focus on segmented models	Consider the transition region
Ikonen [18]
√	√				
Li et al. [19]
√	√				
Upreti et al. [23]
√			√		
Khorashadi et al. [24]
√			√		
Borgonovo [25]
√	√				
Tang et al. [26]
√		√			
Spiessl and Becker [29]
√				√	
Liang et al. [30]
√				√	
Lamboni et al. [31]
√				√	
Dunn et al. [32]
√				√	
Kc et al. [33]
√			√	√	
This work	√	√	√	√	√	√

Focusing on this critical issue, in this paper, four GSA indices, i.e. the Sobol index, mutual information, delta index and PAWN index, are employed for the aforementioned segmented fire spread model introduced by Kc et al. [33]. It is an empirical fire spread model established by Cheney et al [34] called Dry Eucalypt model, which is widely used for Australian eucalypt forests. We primarily concentrate on the importance rankings in the transition region of the segmented model. Then, as can be seen in Section 3, we find that the results of these four GSA methods are not the same during the transition region. Motivated by this phenomenon, a specific case is chosen and the reasons for the inconsistent results are further analyzed by fixing variables to different values in turn and comparing conditional PDF and CDF to the original ones. Furthermore, we conduct comparisons between the GSA methods and discuss how to choose appropriate GSA methods. The main aim of this paper is to demonstrate the specificity of conducting GSA on segmented models and guide the choice of GSA methods.
# Materials and methods
## GSA methods
In order to facilitate the description of subsequent GSA methods, the following generic model is introduced. Assume that the output response of a system is determined by a multitude of input random variables, which can be expressed as
	 	(1)
where "X = (" "X" _"1"  "," "X" _"2"  ",⋯," 〖" X" 〗_"n"  ")"  is an n-dimensional vector of the input variables with random uncertainties; Y represents the output response with uncertainty propagated by X via the function g(∙). It should be noted that the inputs and output represented in Eq. (1) are random variables. The goal of GSA is to quantify the uncertainty importance of each input variable in respect to the output variable from their samples. In the following subsections, we will introduce the GSA methods based on Eq. (1). The numerical implementations of these four GSA indices are presented in the Appendix.
Sobol index
The Sobol index is based on the total variance decomposition given by [35]
	 	(2)
where V(∙) stands for the variance; Vi is the variance contributed by the input variable Xi to the output variance individually; Vi,j is the variance contributed by the interaction between variable Xi and Xj and V1,2,…,n is the variance caused by the interaction among all variables. 
According to [35], the total variance of Xi considering its own variance and all its interactions with other variables can be derived as
	 	(3)
where E(∙) stands for the expectation; X~i denotes all the input variables except Xi. Then, two normalized sensitivity indices for Xi can be defined as [35]
	 	(4)
	 	(5)
where "S" _"i"  and "S" _"i" ^"T"  represents the main effect and the total effect of Xi on Y, respectively.
Mutual information
Shannon defined the differential entropy of continuous variables as [15]
	 	(6)
where "Ω" _"Y"  is the range of variation of Y; and fY(y) is the marginal PDF of Y. The mutual information "I (Xi; Y)"  between two continuous random variables Xi and Y is defined by
	 	(7)
where
	 	(8)
is the conditional entropy of Y given knowledge of Xi; f (xi, y) is the joint PDF of Xi and Y; f (y|xi) is the conditional PDF of y given xi; "f" _("X" _"i"  ) "(" "x" _"i"  ") " is the marginal PDF of Xi; "Ω" _"Xi"  is the range of variation of Xi.
For continuous variables, there is a normalized version of mutual information given by [36]
	 	(9)
However, when the mutual information is large, this normalized approach may invisible the change in the index. Therefore, the mutual information given by Eq. (7) is straightforwardly considered as a sensitivity index in this paper, denoted as ηi.
Delta index
To investigate the impact of the input variable uncertainties on the PDF of model output, Borgonovo proposed the delta index as [14]
	 	(10)
where s(Xi) is the area difference between the marginal output PDF fY(y) and the conditional one f(y|xi), i.e.
	 	(11)
PAWN index
Unlike the aforementioned moment-independent approaches, the PAWN index uses the cumulative distribution function (CDF) of the output to quantify the sensitivity of input variables. The PAWN index was defined as [16]
	 	(12)
where FY(y) and F(y|xi) are the unconditional and conditional CDF of y; and stat is a statistic operator (e.g. maximum, median or mean) defined by the user. In this paper, the mean is chosen as a typical operator since it can share an information value interpretation [37].
## Fire spread model
Fire spread models are commonly employed by fire behavior analysts to predict how fast a wildfire will spread [38]. An accurate fire spread model can help analysts develop community evacuation strategies and avoid potentially disastrous consequences. Since uncertainties are inevitable in model inputs, many scholars have conducted GSA for different fire spread models to enhance the understanding of fire behavior, such as GSA for the Rothermel model [39], the FARSITE model [40], the SPITFIRE model [41] and the Spark model [42].
In this work, we consider an empirical fire spread model established by Cheney et al [34] called Dry Eucalypt model, which is widely used for Australian eucalypt forests. The model has only four input variables, which enables a clear study on the effect of segmented characteristics on GSA results. In the model, the fire spread rate R (m/h) can be expressed as follows:
	 	(13)
	 	(14)
	 	(15)
	 	(16)
	 	(17)
where T is the air temperature; RH denotes the relative humidity; U is the average 10-m open wind speed; and FA represents the fuel age. Detailed explanations of other parameters can be found in [34]. In this model, R corresponds to different models depending on the indicator U, and the critical threshold of U is 5km/hr.
## Parameter settings
The normal or uniform distribution is always arranged for uncertainty analysis when no information is available [30]. In this analysis, the ith input variable is assumed as a random variable following the normal distribution with mean μi and standard deviation σi, i = T, RH, U, and FA. In practice, there may be correlations between variables, especially for the variable T and RH. However, in order to investigate the effect of segmented characteristics on GSA results without other potential influence, all the variables are assumed to be independent of each other in this paper.
The uniqueness of uncertainty analysis for segmented models is the abrupt model change at the critical threshold. Since the indicator U is uncertain, the overall output is jointly determined by both stages during the transition region. In order to explore the significant impact of segmented characteristics on GSA results, hereby, μU is treated as a varying parameter. The main purpose of varying μU is to (a) alter the output percentage of two stages during the transition region and examine the variation of GSA results and (b) examine the GSA results when the model is not in the transition region and not affected by the segmented characteristic.
Table 2 lists the corresponding distribution parameters. Since U is a random variable following the normal distribution, we define the transition region according to the threshold (i.e., 5km/hr) and ± 3-sigma of U. For simplicity, we denote the phase where 2km/hr < μU ≤ 3.5km/hr as Stage 1, the phase where 6.5km/hr ≤ μU < 8km/hr as Stage 2, and the phase where 3.5km/hr < μU < 6.5km/hr as Stage 3. In other words, μU is a varying parameter with a variation range of 2-8km/hr and a variation interval of 0.1km/hr. It is clear that Stage 3 corresponds to the transition region and its overall output is composed of the output of two different stages. 

Table 2  Basic random variables and the distribution parameters for the fire spread model.
Random variable (unit)	Distribution	Mean	Standard deviation	Acceptable range
T (°C)	Normal	25	4	10 - 40
RH (%)	Normal	20	2	14 - 26
U (km/hr)	Normal	μU	0.5	0.5 – 9.5
FA (yr)	Normal	4	0.8	1.5 – 6.5
Note: When the sampled value is out of the acceptable range, it will be resampled.

Remark 1. The applicable temperature range for the model is 10°C - 40°C [33], which is almost satisfied in the parameter settings. Besides, according to the experimental data for the dry eucalypt forest in the literature [34, 43], we choose the range of the RH, U and FA to be 14% - 26%, 0.5km/hr – 9.5km/hr and 1.5yr – 6.5yr, respectively. The range of variables chosen in this paper is essentially within the parameter range of the experimental data. When the sampled value is out of the given range, it will be resampled. It should be noted that the range and the distribution assigned to each input variable could easily be changed if required for other analysis [34].
Remark 2. It should be noted that GSA analysis has been carried out for the same fire spread model in [33]. However, the wind speed U follows a uniform distribution which is consistently greater than 5km/hr in their study. Since the fire spread behavior will have an abrupt change when U exceeds 5km/hr, they did not consider the effect of segmented characteristics (i.e., the abrupt model change) on the GSA results, which is the main contribution of this paper.
## Estimation details
The numerical implementations of these four GSA indices are presented in the Appendix. For a single estimation, 4,000, 10,000, 5,000 and 2,000,000 samples are generated by Latin hypercube sampling [44] to calculate the Sobol index, mutual information, delta index and PAWN index, respectively. To ensure the robustness of the estimation results, the corresponding 95% confidence intervals are calculated. Specifically, the Jackknife resampling [45] is applied for the mutual information and the bootstrap resampling [46] is applied for the Sobol, delta and PAWN index. The number of Jackknife and bootstrap replication is set as 1000, which is determined empirically by the previous study [47].
Remark 3. This paper focuses on different importance rankings obtained by different GSA methods. Therefore, we ensure that the numerical implementations of the GSA indices are valid and the generated samples are sufficient to get convergence, without making the sample size of each method comparable. The convergence validation and the computation cost discussion can be found in the Appendix.
Remark 4. In the literature, the total effect "S" _"i" ^"T"  is always chosen to be compared with the moment-independent index [48]. Hence, we choose "S" _"i" ^"T"  to compare with the moment-independent approaches in this paper.
Remark 5. According to [45], resampling data with replacement produces duplicate data points. The k-nearest neighbor algorithm interprets these duplicates as detailed, high-information features, which causes an overestimation of mutual information in the bootstrapped samples. Therefore, jackknife resampling is used to estimate the confidence level of the mutual information rather than the bootstrap resampling, as suggested in [45].
# Results and analysis
Following the numerical implementations in the Appendix and estimation details in Section 2.4, the importance rankings and corresponding 95% confidence intervals of different GSA methods under different μU are calculated. The results are illustrated in Fig. 1.
   
(a)                                                                             (b)
   
(c)                                                                           (d)
Fig. 1  GSA results under different μU: (a) Sobol index (b) Mutual information (c) Delta index (d) PAWN index. The three stages are divided by the black dashed line. At Stage 1 or Stage 2, these four GSA indices yield the same importance rankings. Whereas at Stage 3, the importance rankings can be different.

As shown in Fig. 1, when the model is not in the transition region (i.e., at Stage 1 or Stage 2), these four GSA indices yield the same importance rankings. At Stage 1, RH has a decisive impact on the output uncertainty, followed by T, while U and FA have almost no influence. During Stage 2, in the beginning, U contributes most to the uncertainty of model output, followed by FA, while T and RH have little impact. As μU increases, the importance of U decreases and the importance of FA grows. When μU exceeds 7.8 km/hr, the importance rank of U and FA is exchanged.
Nonetheless, when the model is at Stage 3, the importance rankings obtained by these four GSA indices can be different. Specifically, for the variables RH and U, as μU increases, the importance of RH first decreases and then increases, while the importance of U first increases and then decreases. For the Sobol index, mutual information, delta index and PAWN index, the importance of U outranks RH when μU is up to 4.2, 4.8, 4.6 and 4.9 km/hr, respectively. Besides, U achieves the highest importance percentage when μU is up to 5, 5.4, 5.4 and 5.8 km/hr, respectively.
Fig. 1 illustrates the significant impact of segmented characteristics on the GSA results. In order to further reflect the differences in the importance rankings obtained by different GSA methods, we choose the GSA results when μU=4.7km/hr as a specific case. Table 3 and Fig. 2 illustrate the importance rankings and corresponding 95% confidence intervals when μU=4.7km/hr. From Table 3 and Fig. 2, we can see that the ranking results of these four GSA methods are not the same. For the mutual information and PAWN index, the importance ranking is RH>U>T>FA. For the delta index, the importance ranking is U>RH>T>FA. Whereas, for the Sobol index, the importance ranking is U>RH>FA>T.

Table 3  Global sensitivity results of the fire spread model.
Variable	"S" ^"T" 	95% confidence interval	η	95% confidence interval
T	0.0090 (4)	[0.0081,0.0100]	0.0282 (3)	[0.0159,0.0413]
RH	0.0987 (2)	[0.0889,0.1082]	0.8011 (1)	[0.7868,0.8145]
U	0.9203 (1)	[0.8680,0.9741]	0.6106 (2)	[0.5973,0.6254]
FA	0.0222 (3)	[0.0178,0.0272]	0.0113 (4)	[0.0033,0.0210]
Variable	δ	95% confidence interval	κ	95% confidence interval
T	0.1091 (3)	[0.1016,0.1169]	0.2066 (3)	[0.1805,0.2374]
RH	0.1927 (2)	[0.1810,0.2050]	0.6050 (1)	[0.5830,0.6269]
U	0.3536 (1)	[0.3386,0.3694]	0.5019 (2)	[0.4796,0.5174]
FA	0.1003 (4)	[0.0929,0.1077]	0.0766 (4)	[0.0593,0.0976]
Note: The number in the parentheses represents the importance ranking of the variable for a specific GSA method.

 
Fig. 2  Global sensitivity results of the fire spread model.

In order to provide an intuitive interpretation for the importance rankings of different GSA methods, we fix each variable to different values in turn and keep other parameter settings unchanged. Then, the corresponding conditional PDF and CDF are obtained and compared with the original PDF and CDF. Specifically, if the uncertainty of a variable has a great impact on the output uncertainty, then fixing it to different values will lead to significant variation in the output PDF and CDF. Conversely, if there is little effect, the output PDF and CDF will barely change. By this analysis, the properties of the each GSA measure can be further explored.
To make a reasonable comparison, since the input variables all follow the normal distribution in this analysis, we determine five fixed values based on the mean and standard deviation of each variable as follows: μi-2σi, μi-σi, μi, μi+σi and μi+2σi. Following the above settings, the conditional PDFs and CDFs of fixing T, RH, U, and FA are illustrated in Fig. 3- Fig. 6. In the following, we will analyze the importance rankings obtained by these four GSA methods in detail.
Remark 6. Choosing PDF and CDF for comparison is aimed to illustrate the reasons for the different importance rankings of four GSA methods since they can display the variation of output uncertainty visually and intuitively. This does not imply that the PDF or CDF based GSA method is more appropriate for uncertainty analysis in this model. How to choose appropriate GSA methods will be discussed later in Section 4.3.
Remark 7. The analytical approach used in this section is similar to the idea of the one-at-a-time method where the sensitivity is analyzed by varying one input variable at a time while keeping all others constant [49]. However, we adopt this idea here in order to interpret the importance rankings by different GSA methods, rather than quantify the importance of input variables directly like the one-at-a-time method.
   
(a)                                                                           (b)
Fig. 3  PDF and CDF of R fixing T at different values. Fixing T affects the output range slightly.
   
(a)                                                                           (b)
Fig. 4  PDF and CDF of R fixing RH at different values. Fixing RH aggregates the output and affects the output range.
   
(a)                                                                           (b)
Fig. 5  PDF and CDF of R fixing U at different values. Fixing U eliminates the heavily-tailed characteristic and affects the output range significantly.

   
(a)                                                                           (b)
Fig. 6  PDF and CDF of R fixing FA at different values. Fixing FA affects the tail characteristics of the distribution.

Sobol index: Firstly, in order to understand the results of the Sobol index intuitively, we calculate the variance of the original output and the output when the variables are fixed at different values, respectively. The results are shown in Table 4. Then, as can be seen from Fig. 5, the original output PDF possesses a heavily-tailed characteristic, while fixing U will eliminate this characteristic. This is because the heavily-tailed characteristic is caused by two points: the uncertainty of U and the segmented characteristic of the fire spread model. When U is fixed, the model is determined, thus eliminating the heavily-tailed characteristic. Since the heavily-tailed characteristic has a great impact on the variance, when it is eliminated, the output variance is greatly reduced. Therefore, the Sobol index supports that the variable U is the most influential variable. From Fig. 4, it can be shown that although the output is more aggregated after fixing RH, the heavily-tailed characteristic still exists. Besides, when RH is fixed to a small value, the heavily-tailed characteristic is more serious, making the output variance even larger than the original one. As a result, for the Sobol index, RH is less important than U. As for the variables T and FA, it is clear from Fig. 3 and Fig. 6 that fixing FA has a more significant effect on the heavily-tailed characteristic of the output than fixing T. Due to the great influence of the heavily-tailed characteristic on the variance, the Sobol index considers FA to be more important than T.

Table 4  Variance of the original output and the output when the variables are fixed at different values.
Original variance	Variable	Fix variable at μi-2σi	Fix variable at μi-σi	Fix variable at μi	Fix variable at μi+σi	Fix variable at μi+2σi
246.12	T	220.75	231.86	243.72	256.43	270.05
	RH	305.74	257.95	219.65	188.58	163.12
	U	19.69	19.69	19.69	42.91	144.59
	FA	112.21	178.43	246.70	310.79	367.48

Mutual information: Similarly, to make the results of mutual information intuitive, we calculate the entropy of the original output and the output when the variables are fixed at different values, respectively. The results are listed in Table 5. Then, as can be seen in Fig. 4 the output is more aggregated after fixing RH. Since the entropy pays attention to the aggregation of the entire PDF, fixing RH reduces the output entropy greatly. Therefore, the mutual information supports RH as the most important variable. From Fig. 5, it can be shown that fixing U contributes little to the aggregation level of the output PDF, leading to a limited reduction in the output entropy. In addition, when U is fixed to a large value, the output entropy even increases. Hence, for mutual information, U is less important than RH. As for the variables T and FA, it is clear from Fig. 3 and Fig. 6 that fixing T contributes more to the aggregation level of the output PDF than fixing FA. Since the aggregation level of the output has a large effect on the output entropy, the mutual information considers T to be more important than FA.

Table 5  Entropy of the original output and the output when the variables are fixed at different values.
Original entropy	Variable	Fix variable at μi-2σi	Fix variable at μi-σi	Fix variable at μi	Fix variable at μi+σi	Fix variable at μi+2σi
3.6475	T	3.5599	3.5919	3.6243	3.6572	3.6908
	RH	3.1275	3.0088	2.8969	2.7909	2.6907
	U	2.9043	2.9043	2.9043	3.3018	3.9162
	FA	3.4689	3.5810	3.6554	3.7066	3.7429

Delta index: It can be shown from Fig. 5 that fixing U causes a considerable change in the output range. Specifically, when U is fixed to a large value, the output range is completely different from the original one. Since the delta index evaluates the area difference between the original PDF and the conditional PDF, the change in output range greatly increases the area difference. Therefore, the delta index supports that U is the most important variable. For the variable RH, as can be seen in Fig. 4, although fixing RH also changes the output range, the change is limited and the heavily-tailed characteristic still exists. As a result, for the delta index, variable RH is less important than U. As for the variables T and FA, it is clear from Fig. 3 and Fig. 6 that fixing T has a greater effect on the output PDF than fixing RH, so the delta index considers T to be more important than FA.
PAWN index: From Fig. 5, although fixing U to a large value changes the output CDF significantly, when U is fixed to a value less than 5, it has limited change on the output CDF. On the other hand, as can be seen in Fig. 4, no matter whether RH is fixed at a large or small value, it will have a notable effect on the output CDF. Considering that the PAWN index chosen in this paper quantifies the mean value of the maximum absolute difference between CDFs across conditioning points, U is considered to be less important than RH. As for the variables T and FA, it is clear from Fig. 3 and Fig. 6 that fixing T changes the output CDF to a much greater extent than fixing FA. Therefore, the PAWN index supports that T is more important than FA.
From the above analysis, due to the segmented characteristics during the transition region, the combination of the output of two stages will lead to undesirable characteristics in the overall output distribution. Since GSA indices measure the uncertainty importance from different perspectives, when there are undesirable characteristics in the output distribution, it will lead to different importance rankings.
# Discussions
In Section 3, we analyzed the importance rankings of different GSA methods for the segmented fire spread model. In this section, we firstly compare the results of the variance-based method and moment-independent methods in response to the prevalence of applying variance-based methods for segmented models in existing studies. Then, it is worth noting that both mutual information and delta index originate from PDFs but their importance ranking results are inconsistent. This interesting phenomenon is discussed in Section 4.2. Finally, how to choose an appropriate GSA method, which is of the most concern to analysts, is discussed in Section 4.3.

## Comparison of variance-based and moment-independent methods
As mentioned in the introduction, most of the existing studies only use the variance-based method for conducting sensitivity analysis on segmented models. However, as can be seen from Table 3, there is a situation where none of the moment-independent methods obtain the same importance rankings as the variance-based method in the transition region of the segmented model. Moreover, if we denote the symbols  ∼ ,  >  and  ≫  as differences in sensitivity between 0% and 10%, between 10% and 50%, and larger than 50%, respectively [37], then the Sobol index supports that U≫RH, which is quite different from the importance rankings obtained by the moment-independent methods (the mutual information and the PAWN index support that RH>U, and the delta index supports that U>RH).
Such a result arises because the output PDF has a heavily-tailed characteristic, while variance only focuses on the second-order moment of the output and is very susceptible to the heavily-tailed characteristic. As seen in Fig. 5, fixing U can eliminate the heavily-tailed characteristic and greatly reduce the output variance. What’s more, since the Sobol index is a normalized index, it will support that fixing U controls nearly all of the output uncertainty. Whereas, for the moment-independent methods, since they focus on the whole distribution and are finitely influenced by the heavily-tailed characteristic, they will not get radical results. Therefore, if the variance-based method is adopted without considering the actual requirements, a misleading importance ranking may be obtained, leading to suboptimal or even wrong decision making.
## Comparison of mutual information and delta index
Mutual information and delta index are both GSA indices based on the output PDF. However, as can be seen from Table 3, these two methods obtain different importance rankings for the segmented fire spread model. Specifically, mutual information supports RH to be more important than U, while the delta index supports that U is the most influential variable. The same situation occurs in [26] where these two GSA indices give different importance rankings. In essence, the reason for the different rankings lies in the different focus of the two indices. Mutual information quantifies the difference in Shannon entropy between the original output and the conditional output. Shannon entropy is an uncertainty measure for random variables and is not related to the change in the output range. On the other hand, the delta index quantifies the area difference between the original PDF and the conditional one. It is a distance-based uncertainty importance measure that cannot quantify the uncertainty of random variables. Meanwhile, it is highly influenced by the variation of the output range.
As for the segmented fire spread model, the output range of the two stages has considerable differences. Since the model stage is determined by the value of the indicator U, different U can greatly affect the output range, as illustrated in Fig. 5. When analyzing the contribution of the uncertainty of U, the mutual information compares the output entropy without considering the change in the output range, so it supports that U is less important than RH. In contrast, the definition of the delta index makes it subject to the change in the output range, so it supports that U is the most influential variable. Therefore, these two GSA methods get different importance rankings.
## How to choose an appropriate GSA method for the segmented model
Through the results and analysis in Section 3, segmented characteristics indeed lead to different importance rankings for different GSA metrics indices. Then, an intuitive and essential question is which GSA index should be chosen as the basis for decision making. As argued in [37], the choice of a GSA method should be tied to the analyst’s anticipated audience or specific requirements. For example, if analysts are concerned with the change in the output distribution, then the moment-independent approaches are preferred. On the other hand, if the desired report includes some measure of central tendency, the variance-based method may be more appropriate [37].


In terms of the segmented fire spread model during the transition region, the impact of the segmented characteristic on the GSA indices stems from the following points:


The output uncertainty varies at different stages. Then, due to the uncertainty in the indicator U, the overall output is jointly determined by both stages, and the combination of the output of the two stages leads to heavily-tailed characteristics in the overall output PDF.


The importance of different variables varies at different stages. Therefore, the heavily-tailed characteristic of the output PDF may be more significant after fixing certain variables (e.g., fixing RH).


Fixing the indicator U will eliminate the heavily-tailed characteristic of the output PDF and significantly affect the output range.


Due to the above particularities of the segmented model, different GSA indices have different features when applied to the segmented model, which are summarized as follows:


For the Sobol index, as discussed in Section 4.1, a radical importance ranking is obtained, and fixing the indicator U controls nearly all of the output uncertainty. At this point, variance-based methods may not be recommended as the basis for decision making.


For the mutual information, it evaluates the variation in the aggregation level of output without being affected by the undesirable characteristics arising from segmented characteristics. If analysts are concerned about the variation of the aggregation level of output PDF, mutual information should be preferred.


For the delta index, as discussed in Section 4.2, the variation of the output range is concerned, and the contribution of the indicator U enlarges. If the variation of the output range is of concern for analysts, the delta index should be preferred.


For the PAWN index chosen in this paper, it obtains a robust importance ranking result, and the influence of the undesirable characteristics arising from segmented characteristics is relatively small. If analysts are concerned about the variation of output CDF, the PAWN index should be preferred.


Although the above findings do not uniquely identify the appropriate GSA approach under all circumstances, they do narrow the field and provide reasonable justification for the final choice of method. In addition, the above analysis can enhance our understanding of different GSA methods and provide guidance in choosing appropriate GSA methods for other segmented models.
# Conclusion
In this paper, we focus on the implementation of GSA on a special class of mathematical models, i.e. segmented models. Sobol index, mutual information, delta index and PAWN index are used to carry out GSA for a segmented fire spread model called Dry Eucalypt model in the transition region. Some conclusions could be drawn as follows:


We demonstrate that the inconsistent GSA results stem from the segmented characteristic. During the transition region, these four GSA methods get different importance rankings. In contrast, when not in the transition region, these four GSA methods yield the same GSA results regardless of the value of μU.


The variance-based approach may not be suitable for dealing with the segmented model during the transition region, as it will yield a radical importance ranking result, i.e., fixing the indicator controls nearly all of the output uncertainty.


For moment-independent methods, the mutual information is concerned with the aggregation level of the output PDF, the delta index takes into account the variation of the output range, and the PAWN index is concerned with the variation of output CDF. All these GSA indices can be used as the basis for decision making when dealing with segmented models, depending on the specific requirements of analysts.


For the Dry Eucalypt model, during its transition region, U is always an influential variable and T always plays a minor role; when close to the Stage 1, RH has significant impact and FA has little impact; when close to the Stage 2, FA is more influential than RH.
The work in this paper clarifies the necessity for applying appropriate indices when performing GSA on a segmented fire spread model, especially during the transition region. Analysts should value the uniqueness of the segmented model and consider the results of different GSA indices according to their practical purpose, so as to provide credible information for decision making. All of our source codes are publicly available at https://github.com/dirge1/GSA_segmented.


Although this work carries out a detailed analysis for the segmented fire spread model, the generalization of the findings needs to be verified on a more extensive set of segmented models. In the supplementary material, the GSA results of another segmented model during the transition region are compared to illustrate the generalizability of the findings in this paper. Furthermore, we will work on providing a mathematical basis for the impact of segmented characteristics on different GSA approaches in future research, not limited to case studies.
# Acknowledgements
This work was supported by the National Natural Science Foundation of China [grant number 51775020], the Science Challenge Project [grant number. TZ2018007], the National Natural Science Foundation of China [grant numbers 62073009].
# Appendix
	Numerical implementations for GSA methods
The implementation methods of these four GSA indices are summarized in Table 6 and expatiated in the following.
Sobol index: Since computing variance using analytical integrals is always challenging, Monte Carlo integration is applied to get a numerical solution [50]. Following that, "S" _"i"  and "S" _"i" ^"T"  can be rewritten as
	 	(18)
	 	(19)
where
	 	(20)
	 	(21)
	 	(22)
are three sample vectors obtained from the kth row of two independent sample matrices (i.e., x and x') for X; N is the number of rows of the sample matrix; and V ̂(∙) is the variance estimation. Notably, the Sobol index is called moment-dependent because the estimation of variance is based on the second-order moment given as:
	 	(23)
where
	 	(24)
Mutual information: The k-nearest neighbor (KNN) algorithm is applied to calculate the mutual information [51]. Compared to density estimation techniques, the KNN method utilized the distribution of kth nearest neighbor distance to represent the real density, thus avoiding the challenge of estimating the joint density directly. Besides, this computation technique is fast and efficient, and only requires a single hyperparameter k which has a relatively small effect on the results. In this paper, k is set as 3.
Delta index: We adopt an efficient estimation method introduced in [52]. Kernel-density estimation is applied to estimate fY(y) and f(y|xi). The method can not only give a robust result from given data, but also achieve a notable reduction in computational burden, making the estimation cost independent of the number of input variables.
PAWN index: The approximation method introduced in [53] is employed. This method can be performed directly from a generic dataset without tailored sampling. Besides, it only requires a single hyperparameter n corresponding to the conditioning intervals and is robust against the choice of n. In this paper, n is set as 10.


Table 6  Implementation methods and tools for the four GSA indices.
GSA index	Implementation method	Implementation tool
Sobol index	Monte Carlo integration [50]
Matlab: authors’ code
Mutual information	KNN algorithm [51]
Python: ennemi [54]

Delta index	Kernel-density estimation [52]
Python: SALib [55]

PAWN index	Generic sampling [53]
Matlab: SAFE [56]

	Convergence validation and computation costs
In this section, we validate the estimation convergence in Table 3 through a specific case that μU=4.7km/hr. Convergence analysis is conducted by examining the stability of the importance rankings with increasing sample size. For each sample size, we conduct 100 replications of the estimation and take the mean value. The results are illustrated in Fig. 7. It can be seen that the importance rankings are stable with increasing sample size, which demonstrates that the estimation of GSA indices using the sample size chosen in this paper are convergent.
   
(a)                                                                           (b)
  
(c)                                                                           (d)
Fig. 7  Importance rankings of different GSA methods under different sample size when μU=4.7km/hr: (a) Sobol index (b) Mutual information (c) Delta index (d) PAWN index. The results indicate that the estimations obtained using the sample size chosen in this paper are convergent.

Besides, the computation time of each index under different sample sizes is shown in Fig. 8. It can be viewed that under the same sample size, calculating the delta index is the most time-consuming, followed by mutual information and PAWN index, while computing the Sobol index requires the least computational resources.
  
(a)                                                                             (b)
  
(c)                                                                           (d)
Fig. 8  Computation time of different GSA methods under different sample size: (a) Sobol index (b) Mutual information (c) Delta index (d) PAWN index.
# References
[1]Shang X., Su L., Fang H., et al. An efficient multi-fidelity Kriging surrogate model-based method for global sensitivity analysis. Reliability Engineering & System Safety. 2023, 229: 108858.
[2]Saltelli A., Ratto M., Andres T., et al. Global sensitivity analysis: the primer. John Wiley & Sons, 2008.
[3]Ballester-Ripoll R., Paredes E.G., Pajarola R. Sobol tensor trains for global sensitivity analysis. Reliability Engineering & System Safety. 2019, 183: 311-322.
[4]Hübler C. Global sensitivity analysis for medium-dimensional structural engineering problems using stochastic collocation. Reliability Engineering & System Safety. 2020, 195: 106749.
[5]Nagel J.B., Rieckermann J., Sudret B. Principal component analysis and sparse polynomial chaos expansions for global sensitivity analysis and model calibration: Application to urban drainage simulation. Reliability Engineering & System Safety. 2020, 195: 106737.
[6]Lu X., Borgonovo E. Global sensitivity analysis in epidemiological modeling. European Journal of Operational Research. 2023, 304(1): 9-24.
[7]Jamous M., Marsooli R., Ayyad M. Global sensitivity and uncertainty analysis of a coastal morphodynamic model using Polynomial Chaos Expansions. Environmental Modelling & Software. 2023, 160: 105611.
[8]Wang Y., Lu Q., Yao T., et al. Global sensitivity analysis of a semi-submersible floating wind turbine using a neural network fitting method. Ocean Engineering. 2023, 285: 115351.
[9]Cardoso-Fernández V., Bassam A., May Tzuc O., et al. Global sensitivity analysis of a generator-absorber heat exchange (GAX) system’s thermal performance with a hybrid energy source: An approach using artificial intelligence models. Applied Thermal Engineering. 2023, 218: 119363.
[10]Pohya A.A., Wicke K., Kilian T. Introducing variance-based global sensitivity analysis for uncertainty enabled operational and economic aircraft technology assessment. Aerospace Science and Technology. 2022, 122: 107441.
[11]Song J., Wei P., Valdebenito M.A., et al. Data-driven and active learning of variance-based sensitivity indices with Bayesian probabilistic integration. Mechanical Systems and Signal Processing. 2022, 163: 108106.
[12]Piano S.L., Ferretti F., Puy A., et al. Variance-based sensitivity analysis: The quest for better estimators and designs between explorativity and economy. Reliability Engineering & System Safety. 2021, 206: 107300.
[13]Helton J.C., Davis F.J. Latin hypercube sampling and the propagation of uncertainty in analyses of complex systems. Reliability Engineering & System Safety. 2003, 81(1): 23-69.
[14]Borgonovo E. A new uncertainty importance measure. Reliability Engineering & System Safety. 2007, 92(6): 771-784.
[15]Shannon C.E. A mathematical theory of communication. The Bell system technical journal. 1948, 27(3): 379-423.
[16]Pianosi F., Wagener T. A simple and efficient method for global sensitivity analysis based on cumulative distribution functions. Environmental Modelling & Software. 2015, 67: 1-11.
[17]Razavi S., Jakeman A., Saltelli A., et al. The Future of Sensitivity Analysis: An essential discipline for systems modeling and policy support. Environmental Modelling & Software. 2021, 137: 104954.
[18]Ikonen T. Comparison of global sensitivity analysis methods - Application to fuel behavior modeling. Nuclear Engineering and Design. 2016, 297: 72-80.
[19]Li D., Jiang P., Hu C., et al. Comparison of local and global sensitivity analysis methods and application to thermal hydraulic phenomena. Progress in Nuclear Energy. 2023, 158.
[20]Iooss B., Prieur C. Shapley effects for sensitivity analysis with correlated inputs: Comparisons with sobol’ indices, numerical estimation and applications. International Journal for Uncertainty Quantification. 2019, 9(5): 493-514.
[21]Vuillod B., Montemurro M., Panettieri E., et al. A comparison between Sobol's indices and Shapley's effect for global sensitivity analysis of systems with independent input variables. Reliability Engineering and System Safety. 2023, 234.
[22]Rabitti G., Borgonovo E. Is mortality or interest rate the most important risk in annuity models? A comparison of sensitivity analysis methods. Insurance: Mathematics and Economics. 2020, 95: 48-58.
[23]Upreti D., Pignatti S., Pascucci S., et al. A comparison of moment-independent and variance-based global sensitivity analysis approaches for wheat yield estimation with the Aquacrop-OS model. Agronomy. 2020, 10(4): 607.
[24]Khorashadi Zadeh F., Nossent J., Sarrazin F., et al. Comparison of variance-based and moment-independent global sensitivity analysis approaches by application to the SWAT model. Environmental Modelling & Software. 2017, 91: 210-222.
[25]Borgonovo E. Measuring uncertainty importance: investigation and comparison of alternative approaches. Risk Analysis. 2006, 26(5): 1349-1361.
[26]Tang Z., Lu Z., Jiang B., et al. Entropy-based importance measure for uncertain model inputs. AIAA journal. 2013, 51(10): 2319-2334.
[27]Cheney N., Gould J., Catchpole W.R. Prediction of fire spread in grasslands. International Journal of Wildland Fire. 1998, 8(1): 1-13.
[28]Ghodrat M., Shakeriaski F., Nelson D.J., et al. Existing improvements in simulation of fire–wind interaction and its effects on structures. Fire. 2021, 4(2): 27.
[29]Spiessl S.M., Becker D.-A. Sensitivity analysis of a final repository model with quasi-discrete behaviour using quasi-random sampling and a metamodel approach in comparison to other variance-based techniques. Reliability Engineering & System Safety. 2015, 134: 287-296.
[30]Liang H., Gao S., Hu K. Global sensitivity and uncertainty analysis of the dynamic simulation of crop N uptake by using various N dilution curve approaches. European Journal of Agronomy. 2020, 116: 126044.
[31]Lamboni M., Makowski D., Lehuger S., et al. Multivariate global sensitivity analysis for dynamic crop models. Field Crops Research. 2009, 113(3): 312-320.
[32]Dunn J., Davis S., Stacey A., et al. A simple model for the establishment of tick-borne pathogens of Ixodes scapularis: a global sensitivity analysis of R0. Journal of Theoretical Biology. 2013, 335: 213-221.
[33]Kc U., Aryal J., Garg S., et al. Global sensitivity analysis for uncertainty quantification in fire spread models. Environmental Modelling & Software. 2021, 143: 105110.
[34]Cheney N.P., Gould J.S., Mccaw W.L., et al. Predicting fire behaviour in dry eucalypt forest in southern Australia. Forest Ecology and Management. 2012, 280: 120-131.
[35]Sobol' I.M. Global sensitivity indices for nonlinear mathematical models and their Monte Carlo estimates. Mathematics and Computers in Simulation. 2001, 55(1): 271-280.
[36]Joe H. Relative entropy measures of multivariate dependence. Journal of the American Statistical Association. 1989, 84(405): 157-164.
[37]Borgonovo E., Hazen G.B., Jose V.R.R., et al. Probabilistic sensitivity measures as information value. European Journal of Operational Research. 2021, 289(2): 595-610.
[38]Storey M.A., Bedward M., Price O.F., et al. Derivation of a Bayesian fire spread model using large-scale wildfire observations. Environmental Modelling & Software. 2021, 144: 105127.
[39]Salvador R., Piñol J., Tarantola S., et al. Global sensitivity analysis and scale effects of a fire propagation model used over Mediterranean shrublands. Ecological modelling. 2001, 136(2): 175-189.
[40]Cai L., He H.S., Liang Y., et al. Analysis of the uncertainty of fuel model parameters in wildland fire modelling of a boreal forest in north-east China. International Journal of Wildland Fire. 2019, 28(3): 205-215.
[41]Gomez-Dans J.L. A Sensitivity Analysis Study of the SPITFIRE Fire Model. EarthArXiv. 2018.
[42]Kc U., Garg S., Hilton J., et al. A cloud-based framework for sensitivity analysis of natural hazard models. Environmental Modelling & Software. 2020, 134: 104800.
[43]Lachlan Mccaw W., Gould J.S., Phillip Cheney N., et al. Changes in behaviour of fire in dry eucalypt forest as fuel increases with age. Forest Ecology and Management. 2012, 271: 170-181.
[44]Stein M. Large sample properties of simulations using Latin hypercube sampling. Technometrics. 1987, 29(2): 143-151.
[45]Holmes C.M., Nemenman I. Estimation of mutual information for real-valued data with error bars and controlled bias. Physical Review E. 2019, 100(2): 022404.
[46]Efron B., Tibshirani R.J. An introduction to the bootstrap. CRC press, 1994.
[47]Dubreuil S., Berveiller M., Petitjean F., et al. Construction of bootstrap confidence intervals on sensitivity indices computed by polynomial chaos expansion. Reliability Engineering & System Safety. 2014, 121: 263-275.
[48]Auder B., Iooss B. Global sensitivity analysis based on entropy. Joint ESREL (European Safety and Reliability) and SRA-Europe (Society for Risk Analysis Europe) Conference, Valencia2009:2107-2115.
[49]Yu S., Yun S.-T., Hwang S.-I., et al. One-at-a-time sensitivity analysis of pollutant loadings to subsurface properties for the assessment of soil and groundwater pollution potential. Environmental Science and Pollution Research. 2019, 26(21): 21216-21238.
[50]Mara T.A., Tarantola S., Annoni P. Non-parametric methods for global sensitivity analysis of model output with dependent inputs. Environmental Modelling & Software. 2015, 72: 173-183.
[51]Kraskov A., Stögbauer H., Grassberger P. Estimating mutual information. Physical Review E. 2004, 69(6): 066138.
[52]Plischke E., Borgonovo E., Smith C.L. Global sensitivity measures from given data. European Journal of Operational Research. 2013, 226(3): 536-550.
[53]Pianosi F., Wagener T. Distribution-based sensitivity analysis from a generic input-output sample. Environmental Modelling & Software. 2018, 108: 197-207.
[54]Laarne P., Zaidan M.A., Nieminen T. ennemi: Non-linear correlation detection with mutual information. SoftwareX. 2021, 14: 100686.
[55]Herman J., Usher W. SALib: An open-source Python library for sensitivity analysis. Journal of Open Source Software. 2017, 2(9): 97.
[56]Pianosi F., Sarrazin F., Wagener T. A Matlab toolbox for global sensitivity analysis. Environmental Modelling & Software. 2015, 70: 80-85.

