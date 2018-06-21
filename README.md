# 穴道照護小幫手

### Introduction  簡介

- 為了協助人們提升自我照護的能力，在生活中遇到小病小痛時，可以快速且正確地進行穴道按摩，以緩解症狀，因此，本作品運用智慧型手機(iPhone)及其軟體開發環境(Xcode)，調用iOS內建的人臉辨識與機器學習之開發套件，提供一個具備即時辨識使用者臉部特徵點並顯示臉部穴道位置之功能的行動應用軟體(App)。

### Problem Description 問題說明

- 生活中偶爾會有小病小痛(眼睛疲勞等等)，遇到這種狀況時，若能自行做簡易的照護處理，緩解症狀，不僅可以減低醫療系統的壓力與負擔，也可減少個人金錢與時間的花費。而所謂的自我簡易處理，可以包括傳統中醫的經絡療法，其中最簡便的方式當屬穴道按摩。然而，對於未曾接觸過中醫經絡相關訓練的人們，要在短時間學會準確地認穴有其困難，因此，若能以電子科技提供即時且準確的穴道位置與對應症狀等資訊，將是一個方便且有助於自我照護的服務。

### Methodology 方法

##### Vision Framework

- WWDC 17 發表的 iOS 11 內建了功能強大的機器學習與電腦視覺應用套件，即 CoreML 和 Vision framework。Vision 提供了一個圖像處理的 pipeline 架構，讓開發者可以輕鬆地開發圖像處理的相關應用。與舊版的 CIDetector 的最大差別是，Vision 介接了 CoreML，運用深度學習訓練的模型來進行圖像處理，因此，在臉部辨識的結果表現上可以更加細緻。Face landmark detection 可標示出臉部輪廓、眼睛、眉毛、鼻樑、鼻翼、嘴唇內緣、嘴唇外緣、瞳孔、中軸等位置，共有65個特徵點。 圖片解析的流程是，將 Requests 提供給一個 RequestHandler，Handler 存有圖片資訊，並將處理結果發送到 Request 的 completion Block 中，就可以從 results 屬性中得到 Observations 陣列。

![face landmark - points](https://drive.google.com/open?id=18lNSSne6XCIKoc9QxNll3dL_LIE5t31Y)
![face landmark - lines](https://drive.google.com/open?id=18lNSSne6XCIKoc9QxNll3dL_LIE5t31Y)

- 取得臉部特徵點的方式則是，從 VNFaceObservation 的 landmarks 屬性中取出allPointsf、aceContour、leftEye、rightEye、leftEyebrow、rightEyebrow、nose、noseCrest、medianLine、outerLips、innerLips、leftPupil、rightPupil 13個屬性，共包含65個特徵點的座標值。

- 取穴測量
利用傳統中醫取穴的長度標準『同身寸』，亦即以患者本身體表的某些標誌作為測量單位，例如第二至五指合併（即四橫指）之長度為三寸。

- 穴位座標計算
依據上述原理，以 landmarks 作為兩端端點，並以同身寸的比例值計算出臉部座標。以水溝穴(人中)為例，水溝穴鼻唇溝內，鼻下三分之一處。計算方式為，
pos.x = nose[4].x
pos.y = (nose[4].y - outerLips[2].y) / 3 。

### Future Work 未來研究

目前礙於 Vision framework 所搭配使用的預先訓練模型只限於臉部的辨識，並沒有辨識身體其他部位(例如手掌、四肢等)的功能。因此，將來為了完善本作品的功能，以達到設計的初衷，將自行訓練深度學習的模型，再使用 CoreML 套件接入模型文件，並搭配全身360個穴道的相關資訊，提供完整的穴位按摩輔助功能。