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


4.Haralick texture

  -mahotas.features.haralick
  
  -用於描述圖像的紋理，紋理是指表面的外觀的一致性或感官雷同。紋理的描述包括“粗糙”與“順滑”。
    Haralick功能的潛在應用包括確定道路是否舖有礫石
    
  -使用Gray-Level Co-occurrence Matrix (GLCM) : 查看相鄰像素對，並記錄彼此相鄰出現的次數
  
    -我們可以指定四個不同的鄰接方向：從左到右，從上到下，從左上到右下和從右上到左下，進而產出四個GLCM矩陣
  
    -最後透過統計學等觀念可以萃取出13~14個一維的特徵向量
  
  -若要分類 : 
    
    - 可善用 : SVM

5.Local Binary Patterns

  -主要是針對灰階影像，在每一個灰階pixel的影像，會在鄰近8個pixel進行閾值判定，小於中心的為1大於中心的為0
  
  -8個0與1個組合以逆時鐘剛好可以排序為一個2進制的機械碼，因此範圍會為0~255，計算出來的值在賦予給中心像素
  
  -為了擴展原本僅能3*3，
    
    新增了兩個參數 : 
    
    p : 要考慮的圓對稱鄰域中的像素數
    
    r : 要抓取的多外圍
    
    p不再受限8個後，中心像素定義的值會變成依造周圍像素1,0分布狀況而判定種類定義給p，因此p數越大，p可能判定的種類也就越多種
    
    隨著p值的增加，所得直方圖的維數也將增加。
    
    p採樣的點越多，您可以編碼的模式就越多，但同時又增加了計算成本。另一方面，如果您增加半徑大小r，則可以在圖像中捕獲更大的紋理細節。但是，如果您同時
    
    增加r而沒有增加p，那麼您將失去LBP描述子的局部判別能力。通常，您將希望同時增加或減少r和p。
   
  -若要分類 : 
    
    - 可善用 : x**2 distance
    
    
6.Histogram of Oriented Gradients

  -功能 : 使用局部強度梯度的分佈來表達局部對象的外觀和形狀，同時也具有紋理的判斷力

  -步驟1 : 標準化影像陣列(平方根標準化尤佳)
  
  -步驟2 : 計算影像梯度(水平與垂直,會產出梯度大小矩陣以及角度方向矩陣)
  
  -步驟3 : 
    
    -在影像(梯度大小矩陣以及角度方向矩陣)上拆分成單位小格矩陣(pixels_per_cell)，假設我們影像為128*128 , pixels_per_cell = 4*4 , 那拆分的cell會為32*32
    -計算每個cell的直方圖分布，orientations為定義在角度矩陣上，要分成幾個方向做值方圖描述(0~180)
    
    -然後透過Cell上每個pixel的梯度的大小同時搭配Cell上每個pixel的角分類來建立x軸為角度分類y軸為梯度家權總和的直方圖
    
    -因此以上述為例，就產生了32*32個直方圖陣列，每個cell都包含描述著9個方向的梯度
    
  -步驟4 : block 標準化
    
    -使用block當作滑動視窗，cells_per_block參數會決定block每次框選幾個cell，cells_per_block假設為2*2，我們會得到31*31矩陣，每格block的內容為2*2*9個方向性的描述(因為4個cell各描述著9個梯度大小的方向矩陣)，先將2*2*9的資料做L1標準準化，最終輸出的結果 : (31*31)*(4*9)的一維矩陣
    
  -三大重要參數:
    
    -orientations
    
    -pixels_per_cell
    
    -cells_per_block


