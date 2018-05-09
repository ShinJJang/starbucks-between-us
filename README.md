# 우리 사이 스벅(starbucks-between-us)

> 약속이나 스터디 모임 시, 거리 상 중간 지점 근처 스타벅스를 찾아보고자 합니다. (교통 최적은 아님)

## TODO

- [ ] Jupyter로 기본 아이디어 검증
- [ ] 스타벅스 점포 API 분석, 저장
- [ ] 적정 기술로 Application 구현

## Ideation

### 현재 아이디어

1. 출발 위치 주소를 위도 경도로 만듬 ex) [([#A 위도, #A 경도], 'tag'), ([#B 위도, #B 경도], 'tag'), ...] 
2. 무게 중심의 좌표를 구함
3. 반경 N meter 의 원을 그려, 거기에 속하는 스타벅스만 보여주거나 마커를 다르게 표시

### 초기 아이디어

1. 출발 위치 주소를 위도 경도로 만듬 ex) [([#A 위도, #A 경도], 'tag'), ([#B 위도, #B 경도], 'tag'), ...] 
2. 임의의 단위로 거리를 늘려가며 아슬아슬하게 모두 intersection이 이뤄지는 원을 그림
    - 반지름을 가지는 좌표는 'Circle marker'라고 함
    - 원의 반지름은 'R'로 표기
    - (필수) circle intersection 판단 알고리즘 필요
3. 해당 지역의 스벅 리스트를 가져옴(위도, 경도 포함됨)
4. R의 반지름을 가지는 Circle marker의 원과 100m 반지름의 스타벅스의 circle marker의 intersection을 구함
5. 각 스벅 좌표에 대해 ['#A의 circle marker에 intersect 여부', '#B의 circle marker에 intersect 여부', ...] 와 같이 구하며 모두 True라면 그 좌표만 표시하거나 다른 색으로 표기함

## Troubleshoot

1. 단순히 원그려서 intersection 구하려 햇더니 엄청 큰 구는 휘는거 고려해야 됨
    - https://en.wikipedia.org/wiki/Haversine_formula
    - 구의 표면에서 원은 타원이 됨
    - 위 문제의 파이썬에서 [방법](https://stackoverflow.com/questions/27431528/find-the-intersection-between-two-geographical-data-points)
    - 그런데 shapely, pyproj 설치에서 배보다 배꼽이 커짐(3.6 미지원, C++ build tool 2014 등, 배포 환경 만들기가 번거로울 듯함)
    - anaconda에서 python 3.4 환경 설치가 느림 https://github.com/conda/conda/issues/1700 
3. 지인의 제안에 따라 문제를 간단히 생각하기로 함
    - 너무 엄하게 같은 거리 지점이 아닌, 무게 중심에서의 원을 그려 근처 스타벅스를 찾기
    - (가정) 시군구 내에서 그리는 원은 `Haversine_formula`를 고려하지 않더라도 큰 오차가 없을 것. 어디까지나 가벼운 인사이트를 주기 위함

## 서비스 시, 고려사항

1. 매번 스타벅스 점포 정보를 API로 가져오기에는 신규 지점 외에 큰 변화가 없음. 정식 API도 아님. => 일 혹은 주 단위 배치로 스타벅스 목록 업데이트
2. 필요해 보이는 필터 제공
    - 리저브
    - 드라이브 스루
    - 지하철 인접

## 더 하면 좋은거

- 브라우저에서 현재 위치 정보 제공