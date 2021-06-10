
# MP3: A Unified Model to Map, Perceive, Predict and Plan

## Personal Info

ID: R08922A20  
NAME: 洪筱慈 

## Basic information

- Paper link: https://arxiv.org/abs/2101.06806
- Conference: CVPR'21


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

下圖中，第一列利用顏色深淺表示某一個位置中有車的機率，當車子在移動，機率高的區域就慢慢轉移。車子的移動則是用 velocity vector 表示。

<div align="left">
<img src=img/16-1-3.png width=500x>
</div>



3. Probabilistic Model:

路上的車子、行人，以及腳踏車，此刻的位置是確定的，但是他們接下來的移動卻是有很高的不確定性，為了將這個不確定性納入考量，作者將這些 object 的移動用機率模型表示。每個 drivable area 中的 BEV grid cell 、每個移動中的 object ，都是 Bernoulli random variables，而從時間 t 到 t+1，某個 object 從位置 i_1 移動到位置 i_2 的機率就表示為：

<div align="left">
<img src=img/16-1-4.png width=500x>
</div>


### Motion planning

1. retrieval-based trajectory sampler: 在上一階段收集完資訊之後，接下來就是要根據這些資訊，選出一條路徑來走。 作者使用 sample-based motion-planner，也就是從一大堆預先已經生好的軌跡中，選出最安全、最舒適可到達目的地的軌跡（果然如果要短時間內做出決策，還是得要 sample-based 呀）。 選擇的基準是 learned scoring function，根據前面得到的機率模型，預防跟其他車子相撞，並且確保車子開在車道裡面。所以 planner 會 evaluate 所有的軌跡，並從裡面挑出一個 cost 最小的軌跡：

<div align="left">
<img src=img/16-1-5.png width=500x>
</div>

2. routing: 除了 raw data 之外，另一個駕駛中的元素是導航。假如有 HD map，那導航的資訊就是一連串的車道資訊，車子只要跟著開就可以了。但是現在沒有 HD map，我們沒有車道資訊，只有一連串的指令：“請繼續直行”、“請向右轉”等等的。 作者將這些指令也用 Bernoulli random variables 表示，給定某一個指令，和 predicted map M, routing network(由 3 CNNs 組成) 會針對 BEV predict 一個 dense probability 。

3. Trajectory scoring: 分別從 trajectory sampler 跟 routing predictor 得到資訊之後，在各種方面使用 score function 來 encourage 或 penalize ，得到想要的軌跡：

* 要能夠執行給定的 high level 指令
* 不可能開到路外面去
* 不可以闖紅燈
* Safety: 例如，當 SDV 軌跡跟其他有 object 的區域有重疊時，就給予 panelty。或者為了和前車保持安全距離，先搜集前方的 grid-cells，如果這些 cell 裡面有 object 正在劇烈減速，就趕快計算安全距離。
* Comfort

### Learning

在上述的方法中，各個模型訓練裡面，值得一提的是 scoring 的訓練，因為這些 score 是不可微的，所以作者使用了 max-margin loss 來 panelize 那些 loss 比較低但是和人類作法不同，或者不安全的選擇。

model train 在 URBANEXPERT dataset 上，是真實世界的畫面，收集了一些比較具有挑戰性的駕駛情境，每個情境都是 25 秒。

## Evaluation
在這篇 paper 中，作者使用了兩種方法來評估 proposed method 和其他方法的 performance： closed-loop 和 open-loop。 

1. closed-loop: 用 LiDAR 模擬器建構一個虛擬世界，並且從一小段真實駕駛情境開始，接下來讓 SDV 來做決策。評估的指標有：Success, OffRoute(有多少機率 SDV 駛離導航路線), 在 撞車/駛離道路 等等的 event 之前開了多遠 (progress per event)，以及一些衡量 comfort 的指標。

2. Open-loop: 在剛剛提到的 URBANEXPERT test set（和 train set 地域上沒有重疊） evaluation，這個 dataset 是真實世界的畫面。利用 progress 來衡量在 5 秒內 SDV 沿著指定路徑走的距離， L2 則衡量 SDV 走的路徑和人類駕駛真實路徑的差距。

不管在 closed-loop 還是 open-loop ，各項指標都顯示， proposed method 都 out-perform 其他方法。

從下圖中可以看到，當下了 commend (Keep Lane, turn left, turn right) 之後， SDV 能夠準確預測出正確的路徑。並且從 occupancy 圖可以看到， model 可以準確抓出其他車輛動向。

<div align="left">
<img src=img/16-1-6.png width=1000x>
</div>







