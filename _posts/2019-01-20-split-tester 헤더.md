## split-tester 헤더 ##

### 1. 개요 ### 

  테스트 데이터 셋을 바탕으로 알고리즘의 정확도를 판단하는 작업은 split-tester.h에 정의된 함수들이 담당한다. 이미 crocodile의 정확도 측정에 관한 글을 썼지만, 이 헤더 파일과 그 함수의 구체적인 구현 내용이 부족하여 이 글에서 보충하고자 한다.

  또한, 아직 100% 확정된 부분이 아니기 때문에 추후 수정/보완 작업이 이루어질 수 있다.

### 2. 구성 ###

  Split-tester는 데이터셋을 로드하는 **loadDataset** 함수, 알고리즘 성능을 테스트한 뒤 결과를 출력하는 **splitTest** 함수, 두 사각형 영역의 겹침 정도(IOU)를 판단하는 **calcIOU** 함수로 구성되어 있다.

  **loadDataset** 함수는 카테고리 이름을 인자로 받아 해당 카테고리의 데이터셋을 로드한다. 결과는 imgList(Mat vector), correctRectList(Rect 2d vector), fileNames(string vector)에 저장된다.

  **splitTest** 함수는 테스트 데이터의 올바른 영역 리스트와 알고리즘이 판단한 영역 리스트를 인자로 받는다. 그 후 두 영역을 비교하여 정확도를 측정하고, 표준 출력으로 결과를 보여준다. 

  **calcIOU** 는 두 Rect를 인자로 받는다. 그 후 Intersection / Union의 비율을 계산한 값을 double 형으로 리턴한다.

### 3. 사용법 ###

  새로운 알고리즘의 정확도를 split-tester로 측정하는 방법에 대해 알아보겠다. 먼저 DataCreator를 이용해 테스트 데이터를 준비할 필요가 있다. 테스트 데이터는 용량이 커질 수 있으므로 하나의 절대 경로에만 저장하고 프로젝트에서 해당 폴더를 참조할 것이다.

1. split_tester.cpp와 split_tester.h를 프로젝트에 추가한다.
2. 테스트를 진행할 위치에 split_tester.h를 include하고, splitTest를 호출한다.
   이때 테스트를 위한 데이터셋은 당연히 로드가 된 상태여야 한다. (loadDataset 이용)
3. 결과를 확인한다.



추후 내용 추가 예정.

