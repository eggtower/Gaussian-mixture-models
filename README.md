# Gaussian-mixture-models README

## 訓練資料
#### 兩張足球轉播畫面

![](https://github.com/eggtower/Gaussian-mixture-models/blob/master/soccer1.jpg)
![](https://github.com/eggtower/Gaussian-mixture-models/blob/master/soccer2.jpg)

## 使用說明
安裝相依套件
```
conda create --name <env_name> --file requirements.txt
```
啟動虛擬環境
```
conda activate <env_name>
```
執行 jupyter notebook 
```
jupyter notebook 
```
執行 `GMM_M1.ipynb`、`GMM_M2.ipynb` 即可

## 評估
使用 `sklearn.metrics` 的 `silhouette_score()` 方法，將測試結果與場地 mask 進行比對

#### silhouette_score
> Compute the mean Silhouette Coefficient of all samples.
>The Silhouette Coefficient is calculated using the mean intra-cluster distance (a) and the mean nearest-cluster distance (b) for each sample. The Silhouette Coefficient for a sample is (b - a) / max(a, b).
> The best value is 1 and the worst value is -1.

[sklearn.metrics.silhouette_score - scikit-learn 1.0.2 documentation](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html#sklearn.metrics.silhouette_score)

#### 場地mask
![](https://github.com/eggtower/Gaussian-mixture-models/blob/master/soccer1_mask.png)
![](https://github.com/eggtower/Gaussian-mixture-models/blob/master/soccer2_mask.png)

#### 效能比較
<table>
   <tr>
      <td>Scenario</td>
      <td>1</td>
      <td>2</td>
      <td colspan="2">3</td>
   </tr>
   <tr>
      <td>Training</td>
      <td>soccer1.jpg</td>
      <td>soccer1.jpg</td>
      <td colspan="2">soccer1.jpg & soccer2.jpg</td>
   </tr>
   <tr>
      <td>Testing</td>
      <td>soccer1.jpg</td>
      <td>soccer2.jpg</td>
      <td>soccer1.jpg</td>
      <td>soccer2.jpg</td>
   </tr>
   <tr>
      <td>silhouette_score</td>
      <td>0.873</td>
      <td>0.854</td>
      <td>0.76</td>
      <td>0.406</td>
   </tr>
</table>

## 訓練結果
#### Scenario 1
![Segmentation1](https://raw.githubusercontent.com/eggtower/Gaussian-mixture-models/master/result/Segmentation1.jpg)

可以觀察到以一張圖片建構 GMM model,以相同的圖片進行測試，判別是否為場地的效果不錯，人物與場地能區隔出來。與 mask 做比較，僅有場地上的線無法區分出來以及場外後方的景物有些許的錯誤判別。
#### Scenario 2
![Segmentation2](https://raw.githubusercontent.com/eggtower/Gaussian-mixture-models/master/result/Segmentation2.jpg)

使用一張圖片建構 GMM model,以不同的圖片進行測試，人物與場地上的線，相較於 Scenario 1 能辨別的更清楚，但是在場外後方的觀眾區辨別的結果就比較差了，可觀察到有不少看板或是觀眾群被錯誤判斷呈白色區塊。
#### Scenario 3-1
![Segmentation3-1](https://raw.githubusercontent.com/eggtower/Gaussian-mixture-models/master/result/Segmentation3-1.jpg)
#### Scenario 3-2
![Segmentation3-2](https://raw.githubusercontent.com/eggtower/Gaussian-mixture-models/master/result/Segmentation3-2.jpg)

使用兩張圖片建構 GMM model，分別將兩張照片進行測試，模型可能是因為經過兩張圖片的訓練有點 overfitting，兩個測試結果表現較差: 
Scenario 3-1 或許是 soccer1.jpg 圖片的內容較為單純(大部分是場地，其餘的內容佔比較小，且為單純的色塊為主)，因此判斷的準確度沒有下滑太多，對於場地上的線的判斷有提升；場上的人物判斷效果則變差，而場外牆板後方的區域也是大部分判斷錯誤。

Scenario 3-2 可以觀察到不僅觀眾區的判斷大量錯誤，連場地靠近觀眾區的區塊也判斷錯誤。經過觀察可以發現，與 soccer1.jpg 不同的是 soccer2.jpg 的觀眾區顏色較為繽紛，而且場地內還有陰影的變化(有照到陽光與沒照到陽光)，因此可以斷定影響 Model2 準確度的訓練資料是 soccer2.jpg，另外也可以得知若要使用顏色較多的圖片來進行 Gaussian Mixture Model 的訓練，其結果較差。
