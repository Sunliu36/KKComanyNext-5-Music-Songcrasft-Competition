# KKBOX datagame 

注意！merge太多檔案的dataframe會out of memory

# train_source
* 有~57萬個session
* 有~65萬首歌曲
    * 很多歌曲是重複的
* 對於同一個session_id, 會對應到同一個login type
* 時間分組？
    * 日期 (年/月/日) 可能沒有用處
    * 取小時
    * 分成更粗略的時間區段：凌晨/上午/中午/下午/傍晚
* 提取播放時間
    * 假設一個session當中，兩首歌曲的unix_played_at相減就是聆聽時間
    * 可以看到有一些歌曲的聆聽時間只有幾秒鐘
    * 聆聽率：歌曲長度 / 聆聽時間
    * 如果聆聽率太小，代表點了但沒有聽
    * 但是有些歌曲只有30秒，或甚至是10秒以下
    * 注意！第20首歌曲實際上無法得知聆聽時間，所以正式訓練的時候還要再考慮。

播放次數統計


|     | 聆聽次數     |
|:---:| ------------ |
| std | 1.916292e+02 |
| min | 0.000000e+00 |
| 25% | 1.477833e-02 |
| 50% | 8.151659e-01 |
| 75% | 1.004444e+00 |
| max | 2.190945e+05 |

聆聽時間大於等於30秒的數據約有59%

![image](https://hackmd.io/_uploads/Syg0wTuET.png)


# meta_song
* 總共有~103首歌曲

# 可以參考的做法
* content_based recommandation
* Model-based Callaborative Filtering
    * scikit-surprise
* hybrid
* implicit-feeback recommandation
* Learning2Rank
    * 我們要如何設定"正確的排序結果"(因為我們只有用戶接下來的5首歌曲)
* NeuralCF
* session_based recommandation
    * [論文筆記-A Survey on Session-based Recommender Systems](https://www.twblogs.net/a/5cfefa73bd9eee14029f568a)
    * 給定一個session，共20首歌曲，推薦(Rank)接下來的5首歌曲
    * 我們似乎可以把一個session當成一個用戶來處理，因為我們無法得知用戶的興趣及喜好

資料當中沒有**rating**這個欄位，我們要怎麼使用這些方法？
* 將聆聽紀錄轉換為評分
* 參考spotify(沒有評分機制)的音樂推薦系統


https://blog.csdn.net/john_hongming/article/details/106665628

https://zhuanlan.zhihu.com/p/351493402

https://zhuanlan.zhihu.com/p/554317156

implicit feedback
https://blog.reachsumit.com/posts/2022/09/explicit-implicit-cf/#challenges-with-using-implicit-data

### Implicit Feedback Data CF

sparse matrix
https://zhuanlan.zhihu.com/p/36122299

### session-based recommandation 
GRU網路架構：https://zhuanlan.zhihu.com/p/32481747

NARM架構：
https://blog.csdn.net/qq_35564813/article/details/90144174