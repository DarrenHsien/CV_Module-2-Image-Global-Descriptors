# Module-2-Image-Descriptors

1.Color Channel Statistics

  -拆分RGB各層影像
  
  -針對每層影像矩陣計算統計信息(均值 ; 標準差 ; 偏態SKEW ; 峰度kurtosis
  
  -集結統計信息 : 當成該張影像的特徵向量
  
  -若要分類 : 
  
    -在比較兩張影像相似度即可使用兩張影像之特徵向量進行 : 歐幾里得距離方式計算相似度
    
      from scipy.spatial import distance as dist

2.Color Histogram
  
  -轉換影像描述格式 ->Lab
  
  -使用cv2.calcHist : 從一張影像萃取出三維空間描述矩陣(bins,bins,bins)
  
  -將特徵空間矩陣標準化並攤平 : 萃取影像彩色直方圖特徵矩陣

  -若要分類 : 
    
    - 可善用 : K-means clustering
    
      將n個數據點劃分為k個集群。 n個數據點中的每一個將被分配給具有最均值的聚類。每個簇的平均值稱為其“質心”或“中心”


3.Hu Moments
  
  -透過影像輪廓中的質心，計算出輪廓對質心生成的7種矩的計算
    
    moments = cv2.HuMoments
  
  -通常若要使用此方法作為辨識，必須要在影像雜訊較低且影像輪廓質心抓取確實的情況下才具意義
  
  -透過7個矩的值我們可以計算出7個一維的特徵向量
  
  -若要分類 : 
    
    - 可善用 : pairwise_distances (如果我們要辨識同一的輪廓之外的異常項)
    
   
  



