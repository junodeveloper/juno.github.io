profiling과 thresholdings의 개선

고려사항

- downsampling? 이 잘 안되는 것 같음 (5픽셀 범위 내에서 최댓값만 남겨놓는 것)
- mergeDividing에서 블록의 개수를 단순히 vecPosForRect의 size로 정하면 안될 것 같음. [캡처 이미지 참고] thresholding이 아주 잘 된다면 상관없겠지만.
- threshold를 automatically determine하는 방법에 대해 고민

