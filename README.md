# DACON_BitcoinTrader2
💰 DACON's AI bitcoin trader competition season 3

## 프로젝트 개요
- 프로젝트 과정 : 데이콘, [인공지능 비트 트레이더 시즌 3](https://dacon.io/competitions/official/235740/overview/description)
- 프로젝트 명 : 머신러닝 및 매수, 매도 전략을 통한 비트코인 트레이딩 모델 설계
- 프로젝트 기간 : 21.06.01 ~ 21.07.15

## 프로젝트 수행목적
본 프로젝트는 지난 시즌 2 프로젝트에서의 아쉬움이 남아 추가적으로 진행한 프로젝트입니다. 기존 접근방식은 다양한 모델로 가격을 잘 예측하려고 하는 방식으로 접근하였지만 결과적으로 비트코인 가격과 같은 변동성이 큰 데이터에 대해서는 제대로 예측하는 것 자체가 불가능하다라는 판단 하에, 간단한 머신러닝 모델를 설계하고 매수, 매도의 트레이딩 알고리즘에 집중하여 모델링해보는 것을 목적으로 하였습니다.

## 프로젝트 설명
본 프로젝트의 진행방향은 시즌2에서 간단한 모델이지만 결과가 좋았던 [2위, skyepodium 팀](https://dacon.io/competitions/official/235740/codeshare/2813?page=1&dtype=recent)의 방식을 Baseline으로 하여 이번 프로젝트를 연구하였습니다.

우선, 2위팀의 진행방식을 이해하고 이후 Basic-ARIMA인 ARIMA(4, 0, 1)이 아닌 이외의 fractional ARIMA, Prophet, Neural Prophet 모델을 같은 방식으로 진행하였습니다.  이후 결과를 바탕으로 이번 프로젝트에서는 어떤 모델를 중심으로 진행할 지를 결정하였으며, 선택된 모델을 가지고 기존 시즌 2팀의 트레이딩 방식을 좀 더 연구하여 내부 파라미터를 조절하였으며 추가적으로는 볼린져 밴드 알고리즘 및 Smoothing과 같은 데이터 핸들링을 시도하였습니다.

(시즌 2팀의 VWAP 전략이나 실수차분이나 스무딩이나 모두 기존 데이터에 정상성을 부여하는 과정이다라는 것을 아래 그래프를 통해 확인할 수 있습니다)
![Diff비교](./images/Diff비교.png)

추가로, 지난 시즌 2 프로젝트에서의 결과로 가격을 예측하는 것은 불가능하다라는 판단하에 차라리 가격이 가장 높은 지점을 확률적으로 분류하는 접근도 시도하였습니다. 이에 사용한 모델은 Conv1d를 사용하였으며 모든 120분의 지점이 아닌 압축한 20분의 class 중 highest price class를 classification 하는 문제로 변형한 것입니다. 프레임워크는 simple한 keras 프레임워크를 사용하였습니다.

## 프로젝트 코드
해당 프로젝트 내 노트북 구성은 아래와 같습니다. (여러 모델 실행결과 ARIMA(4,0,1)이 가장 준수하게 나와서 해당 모델을 위주로 진행하였습니다)

1. Baseline line ARIMA(시즌2위 팀 review) : [baseline-ARIMA-Experiment](./codes/baseline-ARIMA-Experiment.ipynb)
2. 베이스라인 코드에 실수 차분 추가 : [Fractional ARIMA](./codes/Fractional-ARIMA_Experiment.ipynb)
3. 베이스라인 코드에 Prophet 적용 : [Prophet](./codes/Prophet_Experiment.ipynb)
4. 베이스라인 코드에 Neural Prophet 적용 : [Neural Prophet](./codes/Neural-Prophet_Experiment.ipynb)
5. 볼린져 밴드 전략 + ARIMA : [Add Ballinger Band Strategy](./codes/BallingerBand+ARIMA_Experiment.ipynb)
6. 볼린져 밴드 전략 + Smoothing + ARIMA : [Add Ballinger Band Strategy with Smoothing](./codes/Smoothing+BallingerBand+ARIMA_Experiment.ipynb)
7. 실수 차분을 적용한 ARIMA : [Fractional ARIMA](./codes/Fractional-ARIMA_Experiment.ipynb)
8. Conv1d-Classification 시도 : [Conv1d](Conv1d_Experimnet.ipynb)

**9. Final code** : [Add Ballinger Band Strategy, parameter tunings](./codes/_INU_IME팀_PUBLIC-4위_Private-31위_$1351.64013_ARIMA.ipynb)

## 프로젝트 결과 및 세부 연구과정
Public 대회에서는 4위로 나름의 성과를 거두었으나, Private 대회에서는 31위로 처참한 결과를 거두었습니다. 예상되는 이유는 최근 비트코인 가격의 큰 급락이 원인이 아닐까 추정됩니다. 그래도 Public 데이터만을 따졌을 때 나름의 의미있는 트레이딩 전략을 취했다라는 판단하에 프로젝트 최종 결과물을 정리하도록 합니다. 최종 모델은 ARIMA(4,0,1)를 사용하였습니다. 이후, 매수 매도 전략에 있어서 세밀한 조정을 하였습니다. 

다만 아쉬운 점은 fractional ARIMA 를 시도하였을 때 매수 시도 자체가 적게 나와 Public에서 점수가 높지 않게 나왔는데 어찌보면 좀 더 안정적으로 매수할 수 있도록하는 핸들링 기법이지 않았나 싶습니다. 

끝으로 아래의 세부 연구과정을 서술하고 마칩니다.

<br>

1. ARIMA 모델에 input이 되는 VWAP를 구하는 과정에서 아래와 같이 변경하였습니다. 
```
시즌2 2위팀 방식 : open하나만을 사용하기 보다는 open(시가), high(고가), low(저가) 3개의 평균을 price로 사용
df['volume_price'] = ((df['open'] + df['high'] + df['low']) / 3) * df['volume_tb_base_av']


1차 수정 방식 :   (open + close)/2 = price 로 사용, 이유는 high와 low를 포함하는 값은 오차의 범위가 너무 커짐
df['volume_price'] = ((df['open'] + df['close']) / 2) * df['volume_tb_base_av']
        

2차 수정 방식 :   high + low /2를 추가 가격 데이터로 잡아 변동성을 반영하게 바꿈(매수 횟수를 늘리기 위해)
df['volume_price'] = ((((df['high'] + df['low'])/2) +  df['open'] + df['close']) / 3) * df['volume_tb_base_av']
 

최종 방식 : df['volume_price'] = ((((df['high'] + df['low'])/2) +  df['open'] + df['close']) / 3) * df['volume_tb_base_av']
```
<br>

2. RSI 파라미터 수정
변동성을 강하게 취하기 위해, RSI 의 값이 75 보다 큰 경우 greedy 하다고 판단하여 약 60분간 투자하지 않습니다.(60분의 기준은 RSI 그래프를 기준으로 임의적인 폭을 잡았습니다)

<br>

3. 볼린저 밴드 추가
볼린저 밴드는 일반적인 20일 기준의 단순이동평균값 및 k값을 2로 주어 upper과 lower 밴드를 만드는 방식을 취했습니다. 이후 안정성을 추구하기 위해 일반적인 밴드 터치보다는 BBW가 Squuze되는 경우에 lower 밴드 터치시 매수를 하지 않도록 하는 전략을 취했습니다.
```
 BBW 가 P10 인 상태에서는 상단 터치시 매수로 변경(추세변환), 하단 터치시 매도로 변경(추세변환)
    '''
    # 볼린저 밴드 전략. 1
    threshold_indices = np.argwhere(bbw_series[-20:] <= bbw_threshold)
    
    if (len(threshold_indices) > 0):
        '''
        bbw가 squeeze되는 구간이 있는 경우, 상하단 터치 확인 후 매수, 매도
        '''
        for idx in threshold_indices:
            # get squuezed term  index
            buy_idx = idx[0]
            
            # 하단 터치 확인 => 하락 추세 변환
            if (lower_touch_series[buy_idx] == True):
                # 하락이 예상되므로 매도
                buy_quantity = 0
                '''
                이 스퀴지 전략으로 매수를 바꿔주는게 좋을지 말지.. 모르겠는데 1의 개수를 세어봐야 겠다는 생각
                21.07.13 전략 - BBW 스퀴지 전략에서 안정적으로 하단 터치시 급락인 것만 방지
                21.07.13 전략2 - 매수량이 너무 적다면, 아래 상승이 예상되는 케이스를 매수로 변경
                21.07.14 전략 수정 - 하단터치시에만 buy quantity = 0 으로 변경
                '''
```
![볼린저 밴드 전략](./images/Ballinger-Band-image.png)

4. 볼린저 밴드 threshold 
볼린저 밴드의 스퀴지 전략을 취하기 위해서는 BBW의 Squuze threshold를 잘 정하는 것이 중요한데 이건 경험적인 전략을 취해 P5로 정하였습니다.
![BBW-image](./images/BBW-image.png)

___

### 참조
1. [시즌 2위, skyepodium 팀 포스트](https://dacon.io/competitions/official/235740/codeshare/2813?page=1&dtype=recent)
2. [실수 차원의 차분 시계열](https://m.blog.naver.com/chunjein/222072460703)
3. [INU_IME 팀 데이콘 포스트](https://dacon.io/competitions/official/235740/codeshare/2950?page=1&dtype=recent)
