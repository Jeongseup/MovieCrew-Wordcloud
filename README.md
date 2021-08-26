# MovieCrew's Wordcloud
🎥 맥도날드 크루들의 영화모임, 무비크루의 1년간의 영화이야기

## 미니 프로젝트 개요
- 프로젝트 과정 : 개인 프로젝트
- 프로젝트 명 : 무비크루's 워드클라우드 만들기
- 프로젝트 기간 : X

<br>

## 미니 프로젝트 목적 및 설명
본 프로젝트는 맥도날드 알바생들끼리 만든 영화 소모임 'Movie Crew'에서 활동한 데이터들을 기념하기 위해 만든 개인 프로젝트입니다. 약 1년 동안 영화 소모임을 하면서 나누었던 이야기들을 한 데 모아 자연어 처리 이후 영화 아이콘에 맞게 워드클라우딩 합니다.

<br>

## 미니 프로젝트 세부 수행과정
본 프로젝트는 [konlpy](https://konlpy.org/ko/latest/)와 [wordcloud](https://amueller.github.io/word_cloud/) library를 사용하여 만들었습니다. 수행과정은 다음과 같습니다.

1. 무비크루 모임에서 나눴던 이야기들을 따로 MS word로 저장한 파일을 불러와 텍스트만을 추출합니다.
2. 특수문자 및 오타와 같은 것들을 제거하기 위해 데이터를 parsing 합니다.
3. 파싱이 완료된 텍스트 데이터는 konlpy, 형태소 분석기를 통해 텍스트에서 한글 형태소(nouns)값들을 추출합니다.
4. 추출된 형태소들 중 불필요할 것 같은 단어(불용어)등을 제거합니다. (불용어 기준은 제 임의대로 준비했습니다)
5. 원하는 영화 이모티콘 형태의 워드클라우드 mask를 만들기 위해 영화 아이콘 이미지를 불러온 뒤 전처리합니다.
6. 적절한 파라미터를 통해, 워드클라우드를 생성 후 저장합니다.

자세한 설명은 [코드](./wordcloud.ipynb)를 확인해주시면 됩니다.

<br>

## 미니 프로젝트 결과
<div align='center'>
    <img style="width:20rem;" src="./result.png">
    <p>MovieCrew's Wordcloud</p>
</div>

### 참고자료
[Wordcloud 참고자료](https://lovit.github.io/nlp/2018/04/17/word_cloud/)

[한국어 자연어처리 참고자료](https://mkjjo.github.io/python/2019/07/09/korean_preprocessing.html)

[ModuleNotFoundError: No module named 'wordcloud.query_integral_image' 솔루션](https://github.com/amueller/word_cloud/issues/491)

[konlpy jpype error 솔루션](https://daewonyoon.tistory.com/386)