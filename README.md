# SCHOOL VIOLENCE FREE

SOFTEER BOOTCAMP 4th - Mini Project

```
# Summary

- 학교폭력 피해 경험이 있는 자녀를 둔 부모를 대상으로 한 Data Product
- 450(+α)여개의 서울의 초,중,고등학교의 학교폭력 심의 결과 데이터를 수집, 처리 및 정제
```

## 구체화 대상

### 누구의 ?

- 학교폭력 피해 경험이 있는 자녀를 둔 부모
- 현재 인천에 거주 중
- 이직을 이유로 서울로 이사를 가게 됨

### 어떤 문제 ? 

 - 학교 폭력으로부터 안전한 거주 지역을 찾고 싶은데 관련 정보를 찾아서 필터링 하기 어려움
 - 자녀가 다른 학교로 전학을 가야하는 상황에서, 자녀가 학교 폭력에 대해 최대한 신경쓰지 않을 수 있는 지역을 찾기에는 정보가 부족함

### 어떻게 해결 ?

- 학교의 기본정보(학생 수, 교원 수, 성별 비율 등)과 학교폭력 심의 사항(심의 건수, 심의 결과, 가해학생 선도 조치 등)을 비교
- 특정 학교와 그 학교가 위치한 지역의 다른 학교들과도 비교하여, 해당 학교 뿐 아니라 지역에 대한 판단 또한 가능하게끔

## Product Detail

### Data Transformation

#### Directory

```
+ data_transformation
    - trans_gu.ipynb
    - trans.ipynb
    - school_info.csv
    - school_names.txt
    - middle_school_names.txt
    + school_html_files
        - XX학교_recent_one_year.html
        - OO학교_recent_one_year.html
        ...        
```

- trans_gu.ipynb
    - 구별로 초, 중, 고등학교 별 10개씩 산출
    - 산출된 학교의 리스트를 school_names.txt, middle_school_names.txt에 저장
        - school_names.txt 
        - middle_school_names.txt  
        - 위 두 개 파일은 특정 학교의 정보를 추출할때 사용

- trans.ipynb
    - html에서 관심있는 대상(학교 이름, 학교폭력 심의 결과, 학교폭력 유형, 가해학생 선도 교육 조치 현황 등)을 추출
    - 구별 중학교 학교폭력 심의 결과
    - 1년간 학교 폭력 심의 건수 0건인 학교 조회 및 해당 학교가 위치한 지역구의 평균 학교폭력 심의 건수를 비교
        - 해당 학교 뿐 아니라, 주위의 환경까지 판단할 수 있게끔 하기 위한 취지

### Web Crawling

#### Directory

```
+ web_crawling
    - crawling_recent_one_year.ipynb
    - school_info.csv
    - done.txt
    - notdone.txt
    + school_violence_htmls
        - XX학교_recent_one_year.html
        - OO학교_recent_one_year.html
    ...
    (+ captchas) 
```

- crawling_recent_one_yaer.ipynb
    - 학교알리미(https://www.schoolinfo.go.kr/Main.do)에서 정보를 수집
    - 특정 학교의 가장 최근 1년의 학교폭력 심의 결과를 크롤링
    - 정보 수집이 완료된 학교는 done.txt에 로그로 저장
    - 해당 정보를 공시한 페이지의 전체 html을 파일형식(XX학교_recent_one_year.html)로 저장
    - 심의 결과 공시가 없거나, 특정 에러로 크롤링 실패한다면 에러 로그와 함께 notdone.txt에 로그로 저장

- school_info.csv - columns
    - school_code (학교 코드)
    - school_name (학교 이름)
    - doro_zipcode (도로명 우편번호)
    - doroname (도로명 주소)
    - doroname_detail (도로명 상세주소)
    - si (학교가 위치한 시)
    - gu (학교가 위한 구)

- ###### captchas ? -> 혹시 모를 이후의 captcha breaking을 위해 레이블과 함께 수집 !

## 회고

### 데이터 수집 전략

- 초기 단계에서는 학교 리스트 데이터를 기반으로 단순히 순차적 수집
    - 특정 구에 치중되어 데이터가 수집됨, 데이터의 편향성 문제가 발생 가능함을 확인
- 이후 구별, 초·중·고등학교 별로 균등하게 학교리스트를 작성하고, 이에 따라 정보를 수집하는 방법으로 전략을 수정

- 표본은 항상 모집단의 특성을 완벽히 반영할 수 없음
    - 따라서, 해결하고자 하는 문제의 특성을 최대한 반영하는 방법으로 데이터를 수집해야함

### Actor의 설정, 분석

- 프로젝트 초기, 학부모가 이사를 고려할 때 매물 가격대 등을 상정하고 구별로 정보를 우선적으로 검토한다는 가정과 함께 프리토타이핑을 진행

- 그러나, 실제 그러한 배경을 가지고, 해결하려는 문제에 의해 큰 고통을 받는 대상이라면, 학교 선택이 우선일 수 있음을 피드백을 통해 깨달음

- Actor에 대해 더 깊이 생각해야하고, 이러한 생각이 바탕이 되어야 더 효과적인 해결책을 제시할 수 있음

### 크롤링에 너무 과한 투자는 오히려 독

- 데이터 수집 과정에서 크롤링 대상 데이터에 캡챠 보안이 설정되어있어 접근이 제한됨

- 해당 접근 제한을 우회할 수 있는 방법이 있는지, 해당 캡챠 인증 과정을 인공신경망 모델링을 통해 해결할 수 있는지 등을 탐색하며 너무 많은 시간을 보냄

- 하지만, 데이터를 조금씩 골고루 수집하고, feedback iteration frequency를 늘려 여러번 프리토타이핑을 진행했다면, 짧은 시간에도 불구하고 더 퀄리티 높은 결과물을 산출할 수 있었을 것이라 생각함

- 이후에는 데이터 수집 과정에 매몰되지 않고 짧은 주기의 feedback iteration을 자주 경험하는 방향으로 프로젝트를 진행해야겠음
