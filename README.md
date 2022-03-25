# Sat_english
 Using Pytorch-LSTM

## 1. 사용 라이브러리
- Pytorch-torch
- Pytorch-torchtext
- NLTK
- numpy
- matplotlib
- scikit-learn
- dill

## 2. 알고리즘 순서도

## 3. 네트워크 구성도
- Sat_pre_training.ipynb 의 Network
![SAT_Network](https://user-images.githubusercontent.com/86700191/159911376-2a614c20-7cdb-4464-b76b-7a91f70eb7ae.png)
- Sat_advanced_model.ipynb 의 Network
![SAT_Advanced_Network](https://user-images.githubusercontent.com/86700191/159911382-615a2277-18a0-4f06-b8e7-1cfdf9878c5b.png)
## 4. 결과
- CoLA 사전학습 모델과 CoLA 사전학습+SAT 튜닝 모델의 AUROC 값 비교
![result](https://user-images.githubusercontent.com/86700191/159204296-9d1b8455-5352-4426-bfe1-847f8dc1f3a9.PNG)


- MaxPool을 쓰는 발전된 네트워크의 CoLA 사전학습 모델과 CoLA 사전학습+SAT 튜닝 모델의 AUROC 값 비교
![result](https://user-images.githubusercontent.com/86700191/159485868-407ed1e7-1396-4523-b759-a7a513da1a41.PNG)


- demo 테스트 결과 (고3 수능 및 모의고사 5문제, 고1 모의고사 3문제)
![demo](https://user-images.githubusercontent.com/86700191/159657328-65210bb4-fb13-4b3c-b4db-19fa474005ef.PNG)
  <table border ="0">
      <th align ="center">Model 이름</th>
      <th align ="center">맞춘 문제 수</th>
    <tr>
      <td>Advanced_after_tuning_model</td>
      <td align ="center">4</td>
    </tr>
    <tr>
      <td>after_tuning_model</td>
      <td align ="center">1</td>
    </tr>
  <tr>
      <td>Advanced_before_tuning_model</td>
      <td align ="center">1</td>
    </tr>
  <tr>
      <td>before_tuning_model</td>
      <td align ="center">0</td>
    </tr>
  </table>
## 5. 유의점
- torchtext를 업그레이드를 하여 v0.12.0으로 한다면 v0.9.0부터 legacy로 지원해주었던 마이그레이션 클래스들이 지원이 안될 것이다.
![legacy](https://user-images.githubusercontent.com/86700191/158297203-bb789adb-664d-4af7-90d9-e4674a80e956.PNG)

  - 해결방법 1. torchtext를 v0.9.0에서 v0.11.0까지 어느 버전이든 다운그레이드하여 사용한다. (현재 사용한 방법)
  
  - 해결방법 2. [torchtext 공식 마이그레이션 튜토리얼](https://github.com/pytorch/text/blob/master/examples/legacy_tutorial/migration_tutorial.ipynb) 을 보고 새로 공부하여 적용한다.


- Overfitting(과잉적합)
  - 네트워크 구성에 있어 Dropout을 0.5로 설정하였고, epoch는 30으로 하여 3가지의 네트워크 모델을 학습시켰다. (사용 데이터: CoLA, 훈련, 검증 데이터의 비율 약 16 : 1)
  <table border ="0">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/86700191/159026474-96caa311-fa8b-4b7b-9fa6-4105996b455d.PNG" width="100%" height="30%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/159026478-8b952d9d-923d-45b3-bc04-2265e8d5d40a.PNG" width="100%" height="30%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/159026462-cd858b87-a883-460b-a50c-f24944732dbe.PNG" width="100%" height="30%"></td>
    </tr>
    <tr>
      <td>num_layer = 2, hidden_size = 200</td>
      <td>num_layer = 4, hidden_size = 100</td>
      <td>num_layer = 4, hidden_size = 200</td>
    </tr>
  </table>
  
  - 3가지의 네트워크 모델 중 마지막 모델을 채택 하였고, 이 모델에서의 overfitting이 일어나는 구간인 15~20회의 epoch 중 최대치인 20을 선정하였다. 또한, 검증데이터에 비해 훈련데이터의 비율이 많다고 판단하여 16 : 1에서 8 : 1 의 비율로 조정하였다.
![final](https://user-images.githubusercontent.com/86700191/159204298-b9ea3731-ca1a-409b-beb2-35a7a5ad2591.PNG)
  
  - MaxPool Layer를 이용한 발전된 모델에서 CoLA데이터를 이용한 사전학습시 overfitting이 일어난다. 하지만 AUROC 값이 잘 나오는 것을 보면 테스트 데이터에 대해 예측이 잘 되는 것이라고 봐도 되는데 수능 문제로 추가 학습 후 실제 데모를 보면 새로운 문제에 대해 가장 예측이 잘된다.
![over](https://user-images.githubusercontent.com/86700191/159486908-d333871c-2933-4454-8466-8bed959c5460.PNG)

  
- 정확도에 대한 고찰
  - CoLA 사전학습 모델과 사전학습후 SAT 튜닝 모델의 차이 <br>
  같은 네트워크 구성을 가진 모델에서 튜닝 한 모델이 더 좋은 결과를 보여준다. CoLA데이터와 SAT 영어 문제의 문장에 사용되는 단어나 문장의 길이 같은 차이점이 있으며,CoLA와 SAT 훈련 데이터의 비율은 약 80 : 1로 훈련 모델이 CoLA 데이터쪽으로 치우칠 수 있다.
  - Max Pool의 유무 <br>
  Bi-LSTM의 입력을 임베딩할때 패딩과정을 걸친다. 따로 패딩값을 지정하지 않고 default값인 'pad'(encoding시 1)이 들어가게 될텐데 짧은 문장들은 의미가 없는 값이 많아진다. 유의미한 값을 뽑기 위하여 Max Pool을 걸치게 되고 이로 인해 더욱 좋은 결과를 나타냈다.
  - 더 좋은 결과를 위한 보완점<br>
  사전 학습 데이터에 대해 좀 더 많은 데이터나 아니면 수능이라는 특정 분야가 있으니 모의고사, EBS 문제집같은 것들에서 데이터를 만들어 쓰면 어떨까 싶다.

## 6. 참고자료(사이트)
- [PyTorch 공식 설명](https://pytorch.org/docs/stable/index.html)
- [NLTK 공식 설명](https://www.nltk.org/api/nltk.html)
- [Base Code & data](https://github.com/bjpublic/DeepLearningProject/tree/main/08_%EC%88%98%EB%8A%A5_%EC%98%81%EC%96%B4_%ED%92%80%EA%B8%B0)
- [Padding 설명](https://everywhere-data.tistory.com/66)
- [ROC curve, AUC](https://koreapy.tistory.com/897)
- [수능 및 모의고사 자료](https://legendstudy.com/)