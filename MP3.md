
# MP3: A Unified Model to Map, Perceive, Predict and Plan

## Personal Info

ID: R08922A20  
NAME: 洪筱慈 

## Basic information

- Paper link: https://arxiv.org/abs/2101.06806
- Conference: 


## Key contribution

假如 SDV(Self-Driving Vehicle) 需要高解析度的 map 才能駕駛，那當 map 有誤差、或者沒有即時更新的時候，就很容易做出錯誤決策，發生危險。因此，本篇提出在沒有 HD maps 的狀況下，使用收到的 raw sensor data 和 high level 指令，得到可以解釋的 representation，並做出更安全的駕駛決定。


## Methods

<div align="left">
<img src=img/16-1-1.png width=1000x>
</div>

如上圖所示，方法分為兩大部分： Scene representation、 Motion planning。

### Scene representation

在這個部分中，是將收到的 raw data，也就是 LiDAR 訊號，透過 backbone network 抽出可用資訊。
在前處理部份，將 BEV(Bird-Eye View) LiDAR 做 Voxelized 處理，並且將時間軸放在 channel dimension，這樣就可以不用算 3D convolution。
將處理過的 data 送進 backbone 之後， backbone 的工作是從 data 中 predict 有用的資訊。為了模擬人類駕駛在開車時，透過觀察路況，以及自己的先備知識，來做出判斷，backbone 將抽出下圖中的各種資訊：
<div align="left">
<img src=img/16-1-2.png width=500x>
</div>

1. online map: 包含 Drivable area, Reachable lanes(車道，盡量開在車道中線上), Intersection(十字路口，要看紅綠燈)
2. Dynamic occupancy field: 辨識目前環境中**現在**哪裡有車，以及這些車**接下來**會怎麼開。其中，因為車子面對不同種類的 object 應有不同的反應，因此將車子、行人、腳踏車視為不同的 class。

<div align="left">
<img src=img/16-1-3.png width=500x>
</div>

上圖中，第一列利用顏色深淺表示某一個位置中有車的機率，當車子在移動，機率高的區域就慢慢轉移。車子的移動則是用 velocity vector 表示。

3. Probabilistic Model:

路上的車子、行人，以及腳踏車，此刻的位置是確定的，但是他們接下來的移動卻是有很高的不確定性，為了將這個不確定性納入考量，作者將這些 object 的移動用機率模型表示。每個 drivable area 中的 BEV grid cell 、每個移動中的 object ，都是 Bernoulli random variables，而從時間 t 到 t+1，某個 object 從位置 i_1 移動到位置 i_2 的機率就表示為：

<div align="left">
<img src=img/16-1-4.png width=300x>
</div>


### Motion planning

在上一階段收集完資訊之後，接下來就是要根據這些資訊，選出一條路徑來走。 作者使用 sample-based motion-planner，也就是從一大堆預先已經生好的軌跡中，選出最安全、最舒適可到達目的地的軌跡。（果然如果要短時間內做出決策，還是得要 sample-based 呀）    

選擇的基準是 learned scoring function，根據前面得到的機率模型，預防跟其他車子相撞，並且確保車子開在車道裡面。所以 planner 會 evaluate 所有的軌跡，並從裡面挑出一個 cost 最小的軌跡：

<div align="left">
<img src=img/16-1-5.png width=300x>
</div>

### Trajectory scoring

選出軌跡之後，並不是馬上就採用了。