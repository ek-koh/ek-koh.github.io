---
title:  "Crawling, Scraping"
excerpt: "빅데이터 수집 / 전처리 : Crawling, Scraping"
toc: true
toc_sticky: true

categories:
  - Data Collection
tags:
  - [Data Collection, Crawling, Scraping]
last_modified_at: 2020-07-21 09:48:06
---

## 1. Crawling
- **crawler**: spider, bots, web crawler 등 다양한 이름으로 불린다.
- Web indexing 을 목적으로 한다.
- 처음 URL 리스트에서 시작해서 하이퍼링크들을 찾고 fetching 한다. 

## 2. BeautifulSoup vs. Scrapy
- BeautifulSoup: Parsing 목적
- Scrapy: 편하게 봇을 만들어주는 Framework


## 3. Scraping vs. Crawling

|Scraping|Crawling|
|:-------|:-------|
|웹 포함 다양한 소스에서 데이터 추출|웹에서 페이지 다운로드|
|규모 관계 없음|주로 대규모|
|중복 제거 필수 아님|중복 제거 필수|
|crawl agent + parser 필요|crawl agent 필요|

## 4. Crawling + Scraping 아키텍처
![image](https://user-images.githubusercontent.com/58713684/88469877-ccd34880-cf30-11ea-83c9-f2334e8740af.png)  

- 바깥쪽이 Crawling
- 가운데 데이터로 저장하는 부분이 Scraping

