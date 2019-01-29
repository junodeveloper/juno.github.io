profiling과 thresholdings의 개선

고려사항

- downsampling? 이 잘 안되는 것 같음 (5픽셀 범위 내에서 최댓값만 남겨놓는 것)
- mergeDividing에서 블록의 개수를 단순히 vecPosForRect의 size로 정하면 안될 것 같음. [캡처 이미지 참고] thresholding이 아주 잘 된다면 상관없겠지만.
- threshold를 automatically determine하는 방법에 대해 고민





#### Thresholding

profile 값이 전체적으로 작을 경우 (symmetryThreshold보다도 작을 경우)

- scaling을 해 주었을 때 구분선 위치가 명확하게 드러날 수도 있지 않을까.

- mx / 3 이상인 위치들의 개수가 유일하거나 2~3개 정도로 적을 경우에는 구분선일 확률이 높지 않을까



#### Profiling

- 2개의 conv 필터를 상하 혹은 좌우로 이어붙이고, 각 필터에 속한 픽셀들의 차 diff를 계산
  같은 행 또는 열에 diff가 연속으로 큰 값이 나타난다면 해당 행 또는 열이 구분선일 확률이 높아진다고 볼 수 있지 않을까 (근데 예외가 많을 것 같다)
- 



#### Preprocessing

- Edge 검출률을 높이기 위해 상하좌우 1픽셀씩 확장한다.



#### Issues



이 정도 오차는 허용하기 어렵나? - movie_no_border 14.jpg, 32.jpg

육안으로도 엣지 구분 어려움 - entertain_no_border 33.jpg

엣지가 절반만 보임(left * right = 0) - movie_no_border 15.jpg

총체적 난국 - movie_no_border 17.jpg && movie_no_border 19.jpg