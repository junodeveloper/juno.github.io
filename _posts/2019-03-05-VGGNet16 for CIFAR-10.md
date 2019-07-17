# VGGNet16 for CIFAR-10

논문(링크)의 16-layers 구조를 CIFAR-10 이미지 분류에 적용해보았다.



## 1. 단순 구현

모든 컨벌루션 레이어는 논문과 동일하고, FC 레이어의 개수는 3개에서 2개로 줄였다. 그리고 FC의 노드 개수를 4096개에서 512개로 줄였다. 입력 이미지의 사이즈가 32x32이므로 5번까지 pooling을 거쳐도 괜찮다. (stride=2) batch size는 128로 주었다.

그 외에 별다른 dropout이나 regularization은 적용하지 않고 학습했을 때에는 2000번 이상의 iteration으로도 학습이 전혀 되지 않았다. (예측률이 기댓값인 0.1에 불과)

## 2. dropout

dropout을 추가해도, 거의 20000번 이상의 iteration으로도 학습이 전혀 안 되었다. (rand() % 10 이랑 동급;)

## 3. Batch normalization

BN을 마지막 FC 레이어를 제외한 모든 레이어에 추가해주었다. 2000번의 iteration만으로 60%대의 정확도를 찍었다.

20000번으로 75.2%를 찍었다.

로그를 남기는게 더 편할듯;

학습을 좀 더 시켜봤는데 73.7%에 수렴했다.



## 4. 데이터셋 분할

위에서는 50000장의 train 셋 + 10000장의 test 셋으로 구성했는데, 여기에 validation 셋을 도입해 비율을 약간 수정했다.

(참고: https://stackoverflow.com/questions/51125266/how-do-i-split-tensorflow-datasets)

prefetch를 적용했다. (GPU가 열심히 학습돌릴 동안 CPU가 다음 batch를 미리 로드하는 함수.)

**분할은 시도만 해봤고, 이전처럼 50000장의 이미지로 학습을 계속 진행했다.**



## 5. L2 regularization

L2 정규화 코드 추가 후 학습 진행중.. () 이미 3000번 정도로 33% 찍은 상황 (iteration: 20000)

accuracy: 0.627700 (50 epochs)

왜 학습률이 더 떨어지지. 내가 잘못 구현했나..



## 6. Input normalization

keras 코드를 보니 train input과 test input을 각각 normalize하는 부분이 있었다. 이 부분을 한번 추가해보고 VGGNet 실험은 마무리하자. (dataset에 apply 메서드를 활용하면 되지 않을까.)



keras 코드에서 normalization을 **빼고** 돌려봤는데 거의 변화 없음.



## 7. Training accuracy vs Validation accuracy

위에서 언급한 정확도는 모두 Validation accuracy인데, training set에 대한 accuracy도 계산해봤다.

train acc: 0.979628

validation acc: 0.751483

training accuracy가 상당히 높았다. Keras 코드보다도. 그만큼 overfitting이 심하게 이루어졌음을 의미한다.

첫 실험이기 때문에 굳이 이 부분은 고치지 않기로 했다.

## 소감

최근에는 BN이 거의 필수적으로 사용된다고 하던데, 왜 그런지를 몸소 실감할 수 있었다. BN을 사용하지 않으면 VGGNet과 같은 deep net이 아예 학습이 안되는 상황도 발생할 수 있다는 걸 깨달았다.



Iteration 2000번까지는 이전에 구현한 BasicCNN과 정확도가 비슷하게 나타났다. (대략 60% 초반)

 지금은 20000번 돌려보고 있다. => 대략 70%대 중반정도 나온다.



## Notes

- iteration 단위 학습 -> epoch 단위 학습
- 학습하는데 걸린 시간 측정
- Validation accuracy를 log 찍어서 TensorBoard로 시각화