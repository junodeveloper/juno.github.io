### 1. 개요

Crocodile의 실행 속도를 단계별로 측정하고 결과를 기록했다. 측정은 단순히 평균값을 계산하는 방식으로 진행되었다.

### 2. 결과

CrocodilePfi:findSubRectList : 105ms

- Crocodile::findSubRectList : 105ms
  - getCombinedEdgeImage : 80ms
    - cvtColor : 1.7ms
    - splitting : 0.5ms
    - L channel proc : 24ms (= a, b channel)
      - sobel(vert + hori) : 15ms
      - allocating GrayImageMat variables (L, LVert, LHori) : 9ms
    - combining edge(L, a, b) : 1ms
    - thresholding : 0.3ms
  - _spliteImageVerticaly : 25ms
    - profilingHorizontal : 5ms
    - thresholdingHori : 0.000000ms
    - mergingSmallBlocks : 0.000000ms
    - splitHorizontally (total) : 18ms
    - Etc (assigining, ...) : 2ms

- _mergeChildrenWhenSiblingIsNotSplitedWithVideoAspectRatio : 0.005ms

- _mergeVerticalTooFlatBlockByVideoAspectRatio : 0.001ms