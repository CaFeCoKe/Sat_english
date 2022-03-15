# Sat_english
 Using Pytorch-LSTM

## 1. 사용 라이브러리

## 2. 알고리즘 순서도

## 3. 네트워크 구성도

## 4. 결과

## 5. 유의점
- torchtext를 업그레이드를 하여 v0.12.0으로 한다면 v0.9.0부터 legacy로 지원해주었던 마이그레이션 클래스들이 지원이 안될 것이다.
![legacy](https://user-images.githubusercontent.com/86700191/158297203-bb789adb-664d-4af7-90d9-e4674a80e956.PNG)

  - 해결방법 1. torchtext를 v0.9.0에서 v0.11.0까지 어느 버전이든 다운그레이드하여 사용한다. (현재 사용한 방법)
  
  - 해결방법 2. [torchtext 공식 마이그레이션 튜토리얼](https://github.com/pytorch/text/blob/master/examples/legacy_tutorial/migration_tutorial.ipynb) 을 보고 새로 공부하여 적용한다.

## 6. 참고자료(사이트)
- [PyTorch 공식 설명](https://pytorch.org/docs/stable/index.html)
- [Base Code & data](https://github.com/bjpublic/DeepLearningProject/tree/main/08_%EC%88%98%EB%8A%A5_%EC%98%81%EC%96%B4_%ED%92%80%EA%B8%B0)