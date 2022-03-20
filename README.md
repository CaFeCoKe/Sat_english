# Sat_english
 Using Pytorch-LSTM

## 1. 사용 라이브러리
- Pytorch-torch
- Pytorch-torchtext
- NLTK
- numpy
- matplotlib
- scikit-learn

## 2. 알고리즘 순서도

## 3. 네트워크 구성도

## 4. 결과
- CoLA 사전학습 모델과 CoLA 사전학습+SAT 튜닝 모델의 AUROC 값 비교
![result](https://user-images.githubusercontent.com/86700191/159162497-7971483b-a38e-464f-9d71-a2bffebcf41d.PNG)
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
  
  - 3가지의 네트워크 모델 중 마지막 모델을 채택 하였고, 이 모델에서의 overfitting이 일어나는 구간인 15~20회의 epoch 중 최대치인 20을 선정하였다. 또한, 검증데이터에 비해 훈련데이터의 비율이 많다고 판단하여 16 : 1에서 5: 1 의 비율로 조정하였다.
  ![final](https://user-images.githubusercontent.com/86700191/159161929-1d49e099-a1ab-455e-8d7b-7dd0fd33e41c.PNG)


## 6. 참고자료(사이트)
- [PyTorch 공식 설명](https://pytorch.org/docs/stable/index.html)
- [NLTK 공식 설명](https://www.nltk.org/api/nltk.html)
- [Base Code & data](https://github.com/bjpublic/DeepLearningProject/tree/main/08_%EC%88%98%EB%8A%A5_%EC%98%81%EC%96%B4_%ED%92%80%EA%B8%B0)
- [Padding 설명](https://everywhere-data.tistory.com/66)
- [ROC curve, AUC](https://koreapy.tistory.com/897)